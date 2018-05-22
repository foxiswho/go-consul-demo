来源：https://studygolang.com/articles/9980



# 先启动 consul
```SHELL
consul agent -dev
```

如果没有 consul  ，请先安装 consul

# 执行 server
进入`demo1`目录后执行
```SHELL
go run server.go
```
终端中什么也不显示，因为没有运行客户端

# 执行 client
要用新的终端打开

进入`demo1`目录后执行
```SHELL
go run client/client.go
```

client 终端显示
```SHELL
2018/05/22 16:14:30 get: EchoServer Hello World, 001
2018/05/22 16:14:31 get: EchoServer Hello World, 002
2018/05/22 16:14:32 get: EchoServer Hello World, 003
2018/05/22 16:14:33 get: EchoServer Hello World, 004
2018/05/22 16:14:34 get: EchoServer Hello World, 005
2018/05/22 16:14:35 get: EchoServer Hello World, 006
2018/05/22 16:14:36 get: EchoServer Hello World, 007
```

server终端显示
```SHELL
2018/05/22 16:14:30 get and echo: EchoServer Hello World, 001
2018/05/22 16:14:31 get and echo: EchoServer Hello World, 002
2018/05/22 16:14:32 get and echo: EchoServer Hello World, 003
2018/05/22 16:14:33 get and echo: EchoServer Hello World, 004
2018/05/22 16:14:34 get and echo: EchoServer Hello World, 005
2018/05/22 16:14:35 get and echo: EchoServer Hello World, 006
2018/05/22 16:14:36 get and echo: EchoServer Hello World, 007
```

# 断开或关闭server

server终端  什么也没有


client 终端显示
```SHELL
2018/05/22 16:16:03 get: EchoServer Hello World, 094
Read Buffer Error: EOF
2018/05/22 16:16:07 dial tcp 127.0.0.1:9527: getsockopt: connection refused
2018/05/22 16:16:10 dial tcp 127.0.0.1:9527: getsockopt: connection refused
2018/05/22 16:16:13 dial tcp 127.0.0.1:9527: getsockopt: connection refused
2018/05/22 16:16:16 dial tcp 127.0.0.1:9527: getsockopt: connection refused
```


# 断开或关闭client
先把 server和client 没有启动的启动。然后断开 或关闭client

client 终端显示
```SHELL
什么也没有
```

server终端显示
```SHELL
2018/05/22 16:18:26 get and echo: EchoServer Hello World, 047
2018/05/22 16:18:27 Warning: End of data: EOF
```

cronsul 终端显示
```SHELL
  2018/05/22 16:17:39 [DEBUG] agent: Node info in sync
    2018/05/22 16:17:40 [DEBUG] http: Request GET /v1/agent/services (39.271µs) from=127.0.0.1:55289
    2018/05/22 16:17:44 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:17:49 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:17:54 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:17:59 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:04 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:09 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:14 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:19 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:24 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:26 [DEBUG] consul: Skipping self join check for "fox" since the cluster is too small
    2018/05/22 16:18:29 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:34 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:39 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:44 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:49 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:49 [DEBUG] Skipping remote check "serfHealth" since it is managed automatically
    2018/05/22 16:18:49 [DEBUG] agent: Service "serverNode_1" in sync
    2018/05/22 16:18:49 [DEBUG] agent: Check "service:serverNode_1" in sync
    2018/05/22 16:18:49 [DEBUG] agent: Node info in sync
    2018/05/22 16:18:54 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:18:59 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:04 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:09 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:14 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:19 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:24 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:26 [DEBUG] consul: Skipping self join check for "fox" since the cluster is too small
    2018/05/22 16:19:29 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:34 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:39 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:44 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:49 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:54 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:19:59 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:20:02 [DEBUG] manager: Rebalanced 1 servers, next active server is fox.dc1 (Addr: tcp/127.0.0.1:8300) (DC: dc1)
    2018/05/22 16:20:04 [DEBUG] agent: Check "service:serverNode_1" is passing
    2018/05/22 16:20:09 [DEBUG] agent: Check "service:serverNode_1" is passing
```