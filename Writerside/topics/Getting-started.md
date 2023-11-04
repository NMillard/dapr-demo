# Getting started with dapr

This "getting started" section assumes you've already installed dapr locally. If you haven't, then head over
to [dapr's website and follow the installation instructions](https://docs.dapr.io/getting-started/).

Note that you both need to install the dapr cli and run the "init" process.

## Local development

I assume you'll spend a lot of time developing and running applications locally, so, let's dive into what the development process looks 
like with dapr.

## Starting a dapr sidecar

Before anything else, let's just try to start a dapr sidecar and see what happens.

The dapr sidecar, despite its name, may run independently. This is practical during local development.

Running the sidecar without its
"parent" application does not make sense at first, but is actually a valuable technique for testing component integrations.

You can start the dapr sidecar by running:
<code-block>
dapr run \
--app-id users \
--app-port 8080 \
--dapr-http-port 3500 \
--dapr-grpc-port 60001 \
--app-protocol http \
--resources-path ./.dapr/components
</code-block>

Let's review the command line arguments we provided to `dapr run`.

| Argument           | Description                                                                                         |
|--------------------|-----------------------------------------------------------------------------------------------------|
| `--app-id`         | The name of the application that'll be announced for service discovery.                             |
| `--app-port`       | The port that _your_ application will listen to.                                                    |
| `--dapr-http-port` | HTTP port the dapr sidecar listens to.                                                              |
| `--dapr-grpc-port` | gRPC port the dapr sidecar listens to.                                                              |
| `--app-protocol`   | Your applications' communication protocol                                                           |
| `--resources-path` | Path to your component configuration files. This is relative to where the command is executed from. |

Now, a dapr sidecar is started and waiting for your application to start on port 8080. You should see a log like this below.

```text
INFO[0000] application protocol: http. waiting on port 8080.  This will block until the app is listening on that port. 
```

At this point, you can start your application locally as you're used to with your normal development flow.

## Use a convenience file

As part of the local development experience, you may want to keep the "run configurations" in a file to avoid typing the CLI command 
every time. This also allows you to run multiple applications with a single dapr CLI command.

Create a yaml file called `dapr.yml` and place it in your solution at `solution-root/.dapr/dapr.yml`.

```yaml
# dapr.yml in root/.dapr
version: 1
common:
  resourcesPath: ./components # Shared resources

apps:
- appID: your-app-id
  appDirPath: ../src/
  appPort: 8080
  daprHTTPPort: 3500
  daprGRPCPort: 60000
  appProtocol: http
  enableApiLogging: true
  daprdLogDestination: console
  appLogDestination: console
  logLevel: debug
# command: ["dotnet", "watch", "--non-interactive"]
```

```text
root
├── MySolution.sln
├── src
│   └── MyWebApi.csproj
└──.dapr
    ├ dapr.yml <– create this file
    └── components
```

This is largely the same configurations as before, but I've added a few additional ones for logging. You may also notice the last 
commented line. Dapr executes whatever commands you've placed in the `command` argument. In this example, dapr would try to start a 
dotnet app.

Don't mind the `components` folder yet. We'll dive in to that in the next chapter.

You can then run dapr with this "Multi app run file": `dapr run --run-file ./.dapr`
