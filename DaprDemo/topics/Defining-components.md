# Defining components

Dapr provides a wide range of built-in components that come with the runtime out-of-the-box.

In this section, we'll focus on how you can set up some of the most common ones, that I think are prevalent in many distributed 
architectures and solution designs:
- Secrets management
- Service-to-service invocation (HTTP and gRPC)
- Pub/sub
- Configuration management

## Secrets management component

Managing secrets is challenging. How you manage secrets locally, in test environment, and when deployed to production also varies 
greatly, making things even more difficult.

Often, this subject tends to be no more than an expensive after-thought.

I'll provide you some guidance on how to manage secrets with dapr both locally and in a production setup.

### Local development

Storing secrets in files locally is generally fine for development purposes, but _never_ check secrets into source control.

> Don't use the `secretstores.local.file` component in production.  
> See [secrets management instead](Secrets-management.md).
{style=warning}

Create a secrets component configuration like so:

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: secretstore
  namespace: default
spec:
  type: secretstores.local.file
  version: v1
  metadata:
    - name: secretsFile
      value: .dapr/secrets/appsettings.Secrets.json # Relative to the project, and not solution root
    - name: nestedSeparator # Allows for one level of nested json
      value: ":"
```

Since secrets are typically application-specific, you'd want to place this file within the project that needs the secrets, as opposed to 
having this component as a shared resource between all daprized applications.

In the project itself, create a file that hold secret values here `.dapr/secrets/appsettings.Secrets.json`, containing whatever values 
your application might need, such as a connection string or private key.

```json
{
  "ConnectionStrings": {
    "Postgresql": "My secret connectionstring to postgresql"
  },
  "Authentication": {
    "PrivateKey": "some private key!"
  }
}
```

Note that I'm only using one level of nesting. The dapr `secretstores.local.file` doesn't support deeply nested values. 

<seealso>
<category ref="external">
<a href="https://docs.dapr.io/reference/components-reference/supported-secret-stores/file-secret-store/">Dapr local secrets</a>
</category>
</seealso>