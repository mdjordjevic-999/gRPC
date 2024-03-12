# DEZSYS_GK861_WAREHOUSE_GRPC

## Author: Marko Djordjevic 4CHIT
## Date: 12.03.2024

---

## Introduction

This project is an exercise designed to demonstrate the functionality and implementation of Remote Procedure Call (RPC) technology using the open-source, high-performance gRPC framework (https://grpc.io). It aims to showcase how this framework can be utilized to develop a middleware system for connecting various services developed in different programming languages.

---

## Steps

### 1. HelloWorldClient

```java
ManagedChannel channel = ManagedChannelBuilder.forAddress("localhost", 50051)
    .usePlaintext()
    .build();

HelloWorldServiceGrpc.HelloWorldServiceBlockingStub stub = HelloWorldServiceGrpc.newBlockingStub(channel);

Hello.HelloResponse helloResponse = stub.hello(Hello.HelloRequest.newBuilder()
    .setFirstname("Marko")
    .setLastname("Djordjevic")
    .build());

System.out.println(helloResponse.getText());
channel.shutdown();
```

Explanation: This client creates a channel to the gRPC server on localhost at port 50051. It then creates a blocking stub to make synchronous calls to the server. The client sends a "Hello" request with first and last name and prints out the response received from the server. Finally, it shuts down the channel.


### 2. HelloWorldServer

```java
private static final int PORT = 50051;
private Server server;

public void start() throws IOException {
    server = ServerBuilder.forPort(PORT)
        .addService(new HelloWorldServiceImpl())
        .build()
        .start();
}

public static void main (String[] args) throws InterruptedException, IOException {
    HelloWorldServer server = new HelloWorldServer();
    System.out.println("HelloWorld Service is running!");
    server.start();
    server.blockUntilShutdown();
}
```

Explanation: This server listens on port 50051 and starts the service HelloWorldServiceImpl. It prints a message indicating that the service is running. The start() method initializes and starts the server, and blockUntilShutdown() keeps it running until it's manually terminated.


---

## Questions

1. **What is gRPC and why does it work across languages and platforms?**

   gRPC is a high-performance, open-source Remote Procedure Call (RPC) framework that leverages HTTP/2 for transport and Protocol Buffers as its interface definition language. It works across languages and platforms because it provides tools to generate client and server code in multiple languages, ensuring consistent and efficient communication between different systems.

2. **Describe the RPC life cycle starting with the RPC client.**

   - The client calls a stub function, initiating the RPC call.
   - The client sends a request to the server, and the system suspends the client process.
   - The server processes the request, executes the required procedure, and sends the response back to the client.
   - Upon receiving the response, the client continues its process.

3. **Describe the workflow of Protocol Buffers.**

   - Define data structures and services in a `.proto` file.
   - Use the `protoc` compiler to generate data access classes in the desired language.
   - In your application, instantiate, populate, serialize, and send Protocol Buffer messages using the generated classes.

4. **What are the benefits of using protocol buffers?**

   - Efficient serialization and deserialization.
   - Cross-platform and cross-language data interchange.
   - Provides a structured and strong typing mechanism.
   - Ensures backward and forward compatibility of data structures.

5. **When is the use of protocol not recommended?**

   - When human-readable data formats are required.
   - For simple data interchange where a schema definition is overkill.
   - In scenarios where dynamic data structure changes are frequent and not predefined.

6. **List 3 different data types that can be used with protocol buffers.**

   - `string` for textual data.
   - `int32`, `int64` for integers.
   - `bool` for boolean values.
