# Redis for session management
- Redis is an open source (BSD licensed), in-memory data structure store used as a database, cache, message broker, and streaming engine.

- ref links
  - https://docs.spring.io/spring-session/reference/guides/boot-redis.html
  - https://www.youtube.com/watch?v=4K5N7SRcyK8&t=610s

## Why/When to use Redis?
### Scenario 1:
If user is loged in, Session key is created at server side in Spring context which is nothing but as local variable .
If Server has restarted, then Session key is destroyed and user will be logged out directly.
### Scenario 2:
If application is deployed on multiple instances with load balancer in place, for example instance 1, instance 2 and instance 3.
user is logged in from instance 1 (session key is created at instance 1). now there is no gurantee that every time load balancer redirect same user to instance 1 . if it redirect to instance 2 then user will be logged out as no session key is found.

## Solution 
instead of storing session in spring context, session will be stored separately in redis data store. so now any request comes to any instance , it will be verified from redis store instead of local context.

## Install Redis on local
> mac
```
# mac --
brew install redis
brew service info redis
#to start redis
redis-server
# config loc
vi /opt/homebrew/etc/redis.conf
```
> Ubuntu
```
# ubuntu
sudo apt-get update
sudo apt-get install redis
redis-cli --version
sudo systemctl status redis
sudo systemctl restart redis.service

# config
sudo nano /etc/redis/redis.conf

# to test
redis-cli -h 127.0.0.1
```
> redis-cli
```
redis-cli
CONFIG SET requirepass "yourpassword"
# test
AUTH yourpassword
```
> Docker
- Reference : https://redis.io/docs/install/install-stack/docker/
```
# start
docker run -d --name redis-stack -p 6379:6379 -p 8001:8001 redis/redis-stack:latest
# mount local volume

# start with config
docker run -d --name redis-stack-cnf -e REDIS_ARGS="--requirepass redis@123" -v /etc/redis/redis.conf:/redis-stack.conf -p 6379:6379 -p 8001:8001 redis/redis-stack:latest

# options
-v /etc/redis/redis.conf:/redis-stack.conf
-e REDIS_ARGS="--requirepass redis@123"
-p 6379:6379 -p 8001:8001

# start cli
docker exec -it redis-stack redis-cli
```


## configuration : 
> pom.xml
```
<!--  auto configure -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.session</groupId>
	<artifactId>spring-session-data-redis</artifactId>   
</dependency>
```

> application.properties
```
# SpringBoot version > 3.*
spring.session.store-type=redis
spring.data.redis.host=localhost
spring.data.redis.port=6379
spring.data.redis.password=redis@123
spring.data.session.timeout=1800

# SpringBoot version < 3
spring.redis.host
```

## Quick start redis
```
# Create and start redis
docker run -d --name redis -e REDIS_ARGS="--requirepass redis@123" -p 6379:6379 -p 8001:8001 redis/redis-stack:latest

#login
redis-cli -a redis@123

# Hit command
KEYS *
```
