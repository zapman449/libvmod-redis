vmod_redis
==========

A Varnish module that allows sending commands to redis from the VCL.

This extension is mostly a proof-of-concept; it has only received minimal testing. However, I do run this in production for a heavy traffic website, and except for the expected slowdown when calling Redis, I've had no other issues.

At the very minimum, it will slow down Varnish a fair amount (at least a few milliseconds per request, depending on how fast your network and your redis server are).

So far the module builds and runs on FreeBSD and Linux (tested on CentOS and Ubuntu).

Usage
-----

In your VCL file, include the following line to load the redis module.

```
import redis;
```

Functions
---------

### init_redis

```
redis.init_redis(host, port, timeout_ms)
```

Use the redis server at the given `host` and `port` with a timeout of `timeout_ms` milliseconds. If `port` is less than or equal to zero, the default port of 6379 is used. If `timeout_ms` is less than or equal to zero, a default timeout of `200ms` is used.

This function is intended to be called from the Varnish subroutine `vcl_init`. If the call is left out, the module will attempt to connect to the Redis server at `127.0.0.1:6379` with a connect timeout of `200ms`.

### send

```
redis.send(command)
```

Sends the given `command` to redis; the response will be ignored.

### call

```
redis.call(command)
```

Sends the given `command` to redis; any response will be returned as a string.

Dependencies
------------

* [hiredis](https://github.com/redis/hiredis) - minimalistic C client for Redis
* automake
* autoconf

Installation
------------

```
./autogen.sh
./configure
make
sudo make install
```

Configuration
-------------

See the `examples/` folder.
