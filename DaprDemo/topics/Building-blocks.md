# Building blocks

Dapr building blocks are abstractions on top of typical distributed capabilities that your applications need, such as service-to-service
invocations, sending and receiving event messages, and dealing with cloud-vendor specific services.

Building blocks are platform-agnostic and language-independent APIs that standardize common tasks.

By utilizing these building blocks, you can make your applications portable across different environments (e.g., edge, hybrid,
multi-cloud, Kubernetes, or any physical infrastructure) with no code changes required. This results in a significant simplification of your
codebase, improved productivity, and reduced risk of bugs or security issues.

## Invoking external services with dapr building blocks

An external service may be anything that is external to the application itself. Instead of having the application know where and how to
communicate with an external service, the application simply delegates that responsibility to its dapr sidecar.

That means, your application just needs to know the location of its sidecar: `http://localhost:<dapr-sidecar-port>`.

> It's your job to know how to call building blocks their available services.
> {style="note"}

### URL structure and calling services

Luckily, the URL structure follows (mostly) the same pattern for all service calls you'll need to do.

`http://localhost:<dapr-sidecar-port>/v1.0/<building-block>/<component-name>/<method>`

This is a nice convenience when you want to test if things are working properly. Since it's all HTTP or gRPC calls to the sidecar, you can
use `curl` to mimic what your application would be doing.

#### Example calls
Here are some examples of how to call typical building blocks from your application or by using `curl`:
- Service-to-service:`POST http://localhost:3500/v1.0/invoke/cart/method/neworder`
- State: `GET http://localhost:3500/v1.0/state/inventory/item67`
- Pubsub: `POST http://localhost:3500/v1.0/publish/shipping/order`
- Secrets: `GET http://localhost:3500/v1.0/secrets/vault/password42`

Notice that all calls are made to `localhost` on port `3500`. This is the dapr sidecar that'll receive the call and requests on behalf 
of your application.

> The dapr sidecar may run on any port, but HTTP port 3500 and gRPC port 60000 has become a convention.

## Gotchas

### Service was not found error

When developing locally, the dapr sidecar will announce itself using multicast DNS, which is a way to do service discovery locally.

If you have a VPN such as AnyConnect, you may run into issues because the VPN client installs a filter which blocks UDP packets. mDNS uses
UDP, so the dapr sidecar never gets to announce itself.

I've found that uninstalling the filter solved the issue.
