
来源：
https://segmentfault.com/a/1190000008471221?from=timeline

# 先编译 server.go

```SHELL
go build server.go
```

# 需要 机器比较多
合计 6 台.
可以看最后面的 docker方式

# consul集群启动

3台
```SHELL
consul agent -server -bootstrap-expect 3 -data-dir /tmp/consul -node=server001 -bind=10.2.1.54

consul agent -server  -data-dir /tmp/consul -node=server002 -bind=10.2.1.83 -join 10.2.1.54

consul agent -server  -data-dir /tmp/consul -node=server003 -bind=10.2.1.80 -join 10.2.1.54
```

server001-003构成了一个3个server node的consul集群。
先启动server001，并指定需要3个server node构成集群，
server002和server003启动的时候指定加入（-join）server001.

# consul启动manger
1 台
```shell
consul agent -data-dir /tmp/consul -node=mangaer -bind=10.2.1.92  -join 10.2.1.54 ./service -consul_addr=127.0.0.1:8500 -monitor_addr=127.0.0.1:54321 -service_name=manager -ip=10.2.1.92 -port=4300 -found_service=worker
```

>说明：service 程序是`service.go`代码编译后的测试程序

# consul启动 worker

2 台
```shell

consul agent -data-dir /tmp/consul -node=worker001 -bind=10.2.1.93  -join 10.2.1.54 ./service -consul_addr=127.0.0.1:8500 -monitor_addr=127.0.0.1:54321 -service_name=worker -ip=10.2.1.93 -port=4300 -found_service=manager


consul agent -data-dir /tmp/consul -node=worker002 -bind=10.2.1.94  -join 10.2.1.54 ./service -consul_addr=127.0.0.1:8500 -monitor_addr=127.0.0.1:54321 -service_name=worker -ip=10.2.1.94 -port=4300 -found_service=manager

```

>说明：service 程序是`service.go`代码编译后的测试程序

# DOCKER 方式
## 拉取 consul 镜像
https://hub.docker.com/_/consul/
```SHELL
docker pull consul
```

## docker 创建自定义网络

docker network create --subnet=10.2.0.0/16 myConsul

## DOCKER consul server
格式
```SHELL
docker run -d --net=host -e 'CONSUL_LOCAL_CONFIG={"skip_leave_on_interrupt": true}' consul agent -server -bind=<external ip> -retry-join=<root agent ip> -bootstrap-expect=<number of server agents>
```


创建容器

```SHELL
docker run -d --net myConsul --ip 10.2.1.54 -e 'CONSUL_LOCAL_CONFIG={"skip_leave_on_interrupt": true}' consul agent -server -bind=10.2.1.54 -node=server001 -bootstrap-expect=3

docker run -d --net myConsul --ip 10.2.1.83 -e 'CONSUL_LOCAL_CONFIG={"skip_leave_on_interrupt": true}' consul agent -server -bind=10.2.1.83 -node=server002

docker run -d --net myConsul --ip 10.2.1.80 -e 'CONSUL_LOCAL_CONFIG={"skip_leave_on_interrupt": true}' consul agent -server -bind=10.2.1.80 -node=server003

```

## DOCKER consul client

格式
```SHELL
docker run -d --net=host -e 'CONSUL_LOCAL_CONFIG={"leave_on_terminate": true}' consul agent -bind=<external ip> -retry-join=<root agent ip>
```

### DOCKER consul  manger

```SHELL
docker run -d --net myConsul --ip 10.2.1.92 -e 'CONSUL_LOCAL_CONFIG={"leave_on_terminate": true}' consul agent -node=mangaer -bind=10.2.1.92 -join 10.2.1.54 ./service -consul_addr=127.0.0.1:8500 -monitor_addr=127.0.0.1:54321 -service_name=manager -ip=10.2.1.92 -port=4300 -found_service=worker

```

### DOCKER consul  client

```shell
docker run -d --net myConsul --ip 10.2.1.93 -e 'CONSUL_LOCAL_CONFIG={"leave_on_terminate": true}' consul agent -node=worker001 -bind=10.2.1.93 -join 10.2.1.54 ./service -consul_addr=127.0.0.1:8500 -monitor_addr=127.0.0.1:54321 -service_name=worker -ip=10.2.1.93 -port=4300 -found_service=manager

docker run -d --net myConsul --ip 10.2.1.94 -e 'CONSUL_LOCAL_CONFIG={"leave_on_terminate": true}' consul agent -node=worker002 -bind=10.2.1.94 -join 10.2.1.54 ./service -consul_addr=127.0.0.1:8500 -monitor_addr=127.0.0.1:54321 -service_name=worker -ip=10.2.1.94 -port=4300 -found_service=manager
```

未完待续