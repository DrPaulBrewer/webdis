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

## Webdis Copyright and License

Copyright (c) 2010-2011, Nicolas Favre-Felix

All rights reserved.
 
Redistribution and use in source and binary forms, with or without modification,
are permitted provided that the following conditions are met:
 
    * Redistributions of source code must retain the above copyright notice, 
      this list of conditions and the following disclaimer.
    * Redistributions in binary form must reproduce the above copyright notice,
      this list of conditions and the following disclaimer in the documentation
      and/or other materials provided with the distribution.
 
THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; 
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 


