# How to compile redis 5 for windows 10 using cygwin gcc.
### This is for testing redis on windows 10 only. not for production !!
### use it on your own risk , your anti virus may detect it as a virus be sure its not !
### Motivation:
I was looking for redis windows build to test some redis c++ client and to my surprised i havent found one 
so quickly i complied one. 
### How to:
1. Download [cygwin]
2. Components to install which not install by default 
    - gcc
    - readline 
    - tcl (for redis tests)
    - make
3. Download latest stable [redis] (currently redis-5.0.7)
4. Open the file /redis-5.0.7/deps/hiredis/net.c and add on top of the file  
```
    #define _POSIX_C_SOURCE 200112L
```
5. Compile the /redis-5.0.7/deps first 
```
cd /redis-5.0.7/deps:
make hiredis linenoise lua 
cd /redis-5.0.7/
make
```
6. Execute redis-server inside cygwin shell 
```
   cd /redis-5.0.7/src/
   ./redis-server.exe ../redis.conf
```
test basic conctivity on localhost 
```
cd /redis-5.0.7/src/
$ ./redis-cli.exe -h localhost
localhost:6379> ping
PONG
localhost:6379>
```
# deploy outside cygwin
All cygwin wrapper is inside : cygwin1.dll
so we need it when we run our redis-server
so deployment direcotry should look like this :
```
cygwin1.dll
redis.conf
redis-benchmark.exe
redis-check-aof.exe
redis-check-rdb.exe
redis-cli.exe
redis-sentinel.exe
redis-server.exe

```
# troubleshooting:
some times on windows your will have networking problem to fix it try those configurations 
if you getting this error when you start pserver-cli.exe:
```
Could not connect to Redis at 127.0.0.1:6379: Name or service not known
Aborted (core dumped)
```

to fix it try :
1. stop redis 
2. in redis.conf search for : bind 127.0.0.1
and comment it with #
3. start redis server , and start redis-cli.exe like this :
```
./redis-cli.exe -h localhost
```
if this dosn't help 
1. stop redis server 
2. in redis.conf search for : bind 127.0.0.1
change it to :
```
bind 0.0.0.0
```
3. start redis server , and start redis-cli.exe like this :
```
./redis-cli.exe -h localhost
```
or 
```
./redis-cli.exe -h 0.0.0.0
```

[redis]: https://redis.io/
[cygwin]: https://www.cygwin.com/setup-x86_64.exe
