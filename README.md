webdis
------------
personal build of webdis from https://github.com/nicolasff/webdis

##usage

    docker run -d --name "cuteName" --link redisActualContainerName:redis -p 127.0.0.1:7379:7379 drpaulbrewer/webdis 

##Dockerfile

```
FROM ubuntu:16.04
MAINTAINER drpaulbrewer@eaftc.com
RUN useradd -m webdis
COPY webdis.json /home/webdis/
RUN apt-get update && \
  apt-get install --yes git-core build-essential libevent-dev && \
  git clone https://github.com/nicolasff/webdis.git && \
  cd /webdis && \
  make && \
  sed -i '/redis_host/s/"127.*"/"redis"/g' webdis.json && \
  make install && \
  rm -rf /webdis && \
  apt-get remove --yes git-core build-essential libevent-dev && \
  apt --yes autoremove && \
  apt-get --yes install libevent-2.0.5
USER webdis
WORKDIR /home/webdis
CMD /usr/local/bin/webdis 
```

##webdis.json settings

/home/webdis/webdis.json:

```
{
	"redis_host":	"redis",

	"redis_port":	6379,
	"redis_auth":	null,

	"http_host":	"0.0.0.0",
	"http_port":	7379,

	"threads":	4,
	"pool_size":   20,

	"daemonize":	false,
	"websockets":	false,

	"database":	0,

	"acl": [
		{
			"disabled":	["BGREWRITEAOF",
					 "BGSAVE",
					 "CLIENT",
					 "COMMAND",
					 "CONFIG",
					 "DBSIZE",
					 "DEBUG",
					 "FLUSHALL",
					 "FLUSHDB",
					 "INFO",
					 "MONITOR",
					 "ROLE",
					 "SAVE",
					 "SHUTDOWN",
					 "SLAVEOF",
					 "SLOWLOG",
					 "SYNC",
					 "CLUSTER"]
		},

		{
			"http_basic_auth":	"user:password",
			"enabled":		[]
		}
	],

        "verbosity": 6,
        "logfile": "webdis.log"
}
```
