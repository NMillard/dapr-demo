# Building applications

To really appreciate the simplicity that dapr provides, let's build an application that uses a lot of the common components that you'd
expect from any mildly complex distributed solution.

You may write along if you wish, but I've designed this section to be more of an inspiration to how you can enable certain building blocks
and the lessons I've learnt doing so.

The exercise is designed to first write the application code, which allows you to think "how would I normally enable these
functionalities?", and then I'll create a component configuration that goes along with the feature.

## The applications

We'll create two simple, arbitrary applications that communicate with each other through HTTP, gRPC, and event messages.

> Note this section is primarily focused on the developer experience.
> Go to the [Azure containerapp](Azure-containerapp.md) if you're interested in deployment to production.

The applications are not meant to model any domain, or, really make any sense. They're made purely to demonstrate dapr capabilities that are
prevalent in many distributed architectures and solution designs:

- Service-to-service invocation (HTTP and gRPC): how
- Secrets management: dealing with securely store and retrieve secret values.
- Pub/sub
- Configuration management

The applications are containerized ASP.NET Core apps built using .NET8 preview with. One is a regular "Empty" ASP.NET Core app, and the
other is initialized with the gRPC template.

Each following section demonstrates a certain building block.

## Solution structure

We have a single .NET solution with two projects (applications):

- Users: a web api that exposes a simple HTTP API.
- Accounts: a gRPC service exposing its method using a proto description file.

Our file structure looks like this:

```text
root/
├──TheSolution.sln
├──.dapr/
│   ├── dapr.yml
│   └── components/
├──src/
│   ├──Accounts/
│   │  ├──.dapr/
│   │  └──Accounts.csproj
│   │
│   └──Users/
│      ├──.dapr/
│      └──Users.csproj
```

Notice there's a dot dapr (`.dapr`) folder in both the solution root and each project.

The solution dot dapr is intended for gathering shared resources and solution wide files, such as the multi app run file `dapr.yml`.

## Key terminology

As with anything new, grasping key terminologies often makes the learning curve smoother and enhances understanding. At this point, 
you're likely familiar with most of the dapr terms, however, take a minute to review the terms below and their meaning.

- **dot dapr (.dapr) folder**: a folder which holds dapr specific configurations, and is conventionally also named `.dapr`. Individual 
  developers may change it's physical name, but should still be referred to as the "dot dapr folder."
- **Component**: a building block implementation, such as the "secrets manager component."
- **Project component**: a component that is specifically only used in a single application.
- **Resource**: a broad catagorization of dapr constructs, such as "multi app run file," "component," and "dapr API."
- **Shared resource**: typically refers to a shared dapr component.
- **Sidecar**: short for an application-specific "dapr sidecar."

## Daprize the applications

Let's first daprize the applications. By the way, the "daprize" process is the same for brown-field applications.

The first step is to create

```yaml
# dapr.yml Multi-App template file
# Use this file to run multiple applications at once.
# Run `dapr run --run-file dapr.yml`
version: 1
common:
  resourcesPath:
    - components # Shared resources - path relative to the dapr.yml file
apps:
  #  - appID: gprcservice
  #    appDirPath: ../src/GrpcApi
  #    appPort: 5118l
  #    daprHTTPPort: 3500
  #    daprGRPCPort: 60001
  #    appProtocol: grpc
  #    daprdLogDestination: console
  #    appLogDestination: console
  #    logLevel: info
  #    command: ["dotnet", "watch", "--non-interactive"]
  - appID: httpservice
    appDirPath: ../src/HttpApi
    resourcesPaths:
      - .dapr/components
      - ../../.dapr/components
    appPort: 5214
    daprHTTPPort: 3501
    daprGRPCPort: 60002
    appProtocol: http
    enableApiLogging: true
    daprdLogDestination: console
    appLogDestination: console
    logLevel: debug
#    command: ["dotnet", "watch", "--non-interactive"]  

```

## Service-to-service



