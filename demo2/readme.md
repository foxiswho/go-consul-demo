
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

未完待续