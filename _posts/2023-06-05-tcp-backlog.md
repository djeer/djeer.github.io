---
layout: post
title: TCP Backlog is a simple concept, yet it may concern you
---

Some time ago, I used to work on a legacy project where we had significant peak loads. During these peak loads, some clients experienced “Connection refused” errors while our monitoring showed we still had enough CPU time, RAM and network bandwidth.

We came to the idea that we may need more workers in our Gunicorn server (in our case, Gunicorn workers are threads handling incoming connections). But I believe any optimisation should be measurement-based, and there should be a way to tell if there is a need for more workers since it is never clear how many to create - 100, 1000? Let’s suppose we’ve created 1000 workers, and it’s sufficient right now - how can we be sure it will be enough during peak load? Yes, adding 999999 workers in auto-scaling mode could be possible. Still, we will risk running out of RAM on peak loads and experience unexpected side-effects due to OOM randomly killing stuff - our OOM preferred PostgreSQL mostly (ask me how I know). Remember, it was a legacy project with not many options to improve architecture **_right now_**.

I first found that Gunicorn has a built-in mechanism [exporting metrics to statsd](https://web.archive.org/web/20230606065742/https://docs.gunicorn.org/en/stable/instrumentation.html), but it is pretty limited and shows only the average number / average duration of handled requests. It is still difficult to judge overall availability.

There is a more interesting way to find out if we have enough workers to process all requests - by monitoring the TCP backlog.

First, let’s check the TCP 3-way handshake diagram: a client sends a SYN packet to a server, the server responds with a SYN-ACK packet, and the client sends ACK packet. At this stage, a connection is considered established.

![]({{ site.baseurl }}/images/blog/tcp-backlog-syn-ack.png)

A TCP backlog is a connection queue at the SYN stage of a three-way TCP handshake. Backlog is handled by the operating system network stack. The maximum backlog queue is defined in the net.ipv4.tcp\_max\_syn\_backlog setting.

When all the workers are busy processing current requests, new connections will start waiting in the queue. The number of connections in the queue that are waiting for a socket on port 8000 can be viewed in “network statistics” or “socket statistics” commands, the Recv-Q column:

```
netstat -l -n | grep 8000
```

![]({{ site.baseurl }}/images/blog/tcp-backlog-recv-q.png)

or

```
sudo ss -lt -n | grep 8000>
```

When the maximum queue length (by default, 128 or so) is exhausted, all new clients will receive the “connection is refused” error.

Ok, now we know **_exactly_** how many connections are waiting in the queue, and we can start optimising & monitoring this metric.

It is possible to increase the TCP backlog depth, so these refused connections will also sit in the queue, but it may lead to even longer response times, gateway timeouts on reverse proxies, and timeouts on clients. Even worse, you may start processing requests for clients that will be disconnected by the time you finish processing, so they will come back again for requests you’ve already processed.

We tried to find a balance between workers and the backlog depth, but ultimately our key to success was to log the longest-running requests and optimise them so average response time would drop and, therefore, TCP backlog would not grow. Most important, now we can be sure that all requests are handled perfectly, and we can be alerted if the backlog queue starts to grow before any of the clients experience a “connection refused” error or a timeout. And reverse proxy logs are still free of errors during this time.

Gunicorn is just an example. This information can be useful for other servers with thread pool workers or, in fact, any server that works with big loads.