# Components

A component is an implementation detail of a building block.

That means, if you want to connect to a secret store and load sensitive configurations and data into your application, then you need a 
secrets management component.

> Components are also referred to as "Component Resources" or simply "Resources".
{style=note}

## Component location
To use components, you'll need to tell the dapr sidecar that it should load a component resource from a component configuration file.

Configuration files are written in yaml, and typically placed within a `.dapr` folder in your solution. Each project within the solution 
may also have its own `.dapr` folder. This is convenient when working with secrets locally.

```text
root
├── MySolution.sln
├── src
│   └── MyWebApi.csproj
│   └──.dapr
│      └── secrets
│          └ appsettings.Secrets.json
└──.dapr
    └── components
        ├── configstore.yml
        ├── secretstore.yml
        ├── pubsub.yml
        └── statestore.yml
```

## Defining a component resource
Component resources are configured using yaml, which is convenient because the same resource configurations can be applied with 
`kubectl` without having to change anything. However, some components are only suitable for local development.

You can find [component specifications here and the built-in components here](https://docs.dapr.io/reference/components-reference).

We'll dive deeper into practical examples of creating component configuration files later in the [Definition components section](Defining-components.md).


<seealso>
<category ref="external">
    <a href="https://docs.dapr.io/concepts/components-concept/">Dapr components</a>
</category>
</seealso>