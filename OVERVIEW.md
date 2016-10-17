# gRPC Overview


## What is RPC

- **RPC**: Remote Procedure Call

    >  when a computer program causes a procedure (subroutine) to execute in another address space (commonly on another computer on a shared network), which is coded as if it were a normal (local) procedure call, without the programmer explicitly coding the details for the remote interaction

    > a form of client–server interaction

    > typically implemented via a request–response message-passing system

## What is gRPC

- a Google open source HTTP/2 RPC Framework
- by default uses [Protocol Buffers](https://developers.google.com/protocol-buffers/) aka protobuf
- specify **service definitions** i.e. a service interface i.e. a collection of methods than can be called with their parameters and return types

    ```
    // The greeting service definition.
    service Greeter {
      // Sends a greeting
      rpc SayHello (HelloRequest) returns (HelloReply) {}
      // Sends another greeting
      rpc SayHelloAgain (HelloRequest) returns (HelloReply) {}
    }
    ```

- define data structures i.e. RPC **messages**

    ```
    // The request message containing the user's name.
    message HelloRequest {
      string name = 1;
    }
    ```

    - has field name, type, and a **unique numbered tag** which makes it easy to update interface, introduce new fields

- can automatically generate server and client from definitions
    - code is able to e.g. populate, serialize, and retrieve message types; add simple field accessors etc.
- clients use generated **stubs** to communicate with a server
    - generated code calls methods as if on a local object, even if methods are called through a remote connection, simplifying ability to communicate between distributed applications
- can add plugins like [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway) that adds a reverse-proxy server which translates a RESTful JSON API into gRPC

## What are Protocol Buffers

- language-neutral, platform-neutral, extensible mechanism for serializing structured data
- are smaller, faster than e.g. XML (because encoded into binary format)

## What is HTTP/2

- A new version of HTTP that solves issue with HTTP/1.1 such as
    - HTTP practically only allows one outstanding request per TCP connection
    - browsers have to use multiple TCP connections to issue parallel requests
        - browsers are taking more than their share of network resources
        - TCP congestion control is effectively negated
- [Key features](https://http2.github.io/faq/#general-questions) include
    - binary instead of textual
        - more compact
        - efficient to parse
        - less error-prone
            - don't have to worry about multiple ways to figure out how to delimit strings, handle whitespace, etc.
    - allows servers to “push” responses proactively into client caches
        - avoid having to wait for client to completely receive and parse HTML files before requesting nested assets
    - fully multiplexed
        - multiple requests are allowed at the same time, on the same connection
        - don't have to set up multiple TCP connections
        - don't have to wait for other requests on the same connection to complete
    - etc.
