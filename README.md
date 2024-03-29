# MyRpcProtocol
  This repository build a remote procedure call framework, which contains several highlights:
  1. Use SPI (Service Provider Interface) to enhance the scalability.
  2. Implemented several internet transport methods, including Socket and Netty.
  3. Implemented several serialize methods, including Protostuff and jackson.
  4. Use Zookeeper as the register center.
  5. Implementation of the heartbeat mechanism to ensure stable connections.
  6. Customized message, avoiding packet concatenation and half packet.

## Prerequisits

1. Install Docker on you computer first.
2. Pull Zookeeper on the docker hub.
  ```bash
  docker pull zookeeper:3.5.8
  ```
3. This repoistory is developed using Intellij IDEA, not tested with other IDEs.
4. This repoistory is coded with Lombok annotations, users need to install Lombok plugin to compile successfully.

## Demonstration

1. Users can edit configs or add services in path ```src/main/resources/```.
2. Start Zookeeper service, default port: 2181.
  ```bash
  docker run -d --name zookeeper -p 2181:2181 zookeeper:3.5.8
  ```
3. Users can run the test codes in path ```src/test/java```.
* Run ```socketTest.ServerMain``` first.
* Run ```socketTest.ClientMain``` afterwords.
* If it runs properly you can see the result in the output。

## How to use the project in you code

Server-Side (Service Provider)
```java
  THE_SERVICE_YOU_WANT_TO_PROVIDE targetRpcService = TargetRpcService.builder()
          .group("THE_GROUP_OF_THE_SERVICE_YOU_WANT_TO_PROVIDE")
          .version("THE_VERSION_OF_THE_SERVICE_YOU_WANT_TO_PROVIDE")
          .service(THE_OBJECT_OF_THE_SERVICE_YOU_WANT_TO_PROVIDE)
          .build();
  SocketRpcServer socketRpcServer = new SocketRpcServer();
  socketRpcServer.registerService(targetRpcService);
  socketRpcServer.start();
```

Client-Side (Service User)
```java
  RpcRequestTransport rpcRequestTransport = new SocketRpcClient();
  TargetRpcService targetRpcService = TargetRpcService.builder()
          .group("THE_GROUP_OF_THE_SERVICE_YOU_WANT_TO_IMPLEMENT")
          .version("THE_VERSION_OF_THE_SERVICE_YOU_WANT_TO_IMPLEMENT")
          .build();
  
  RpcClientProxy rpcClientProxy = new RpcClientProxy(rpcRequestTransport, targetRpcService);
  THE_SERVICE_YOU_WANT_TO_IMPLEMENT service = rpcClientProxy.getProxy(ClassTransferTest.class);
  service.doSth();
```

## Credit

  - guide-rpc-framework: https://github.com/Snailclimb/guide-rpc-framework
  - dourpc-remoting: https://xilidou.com/2018/09/26/dourpc-remoting/

