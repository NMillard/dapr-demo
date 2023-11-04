# Developer experience

Taking a chance on a new technology can often feel like a gamble.

In my experience, the success of a technology often hinges on the quality of developer experience it provides.

Whether it's a small code
library, an extensive framework, or something else entirely, the developer experience can make or break its adoption. It's the critical
determinant of whether developers keep using it beyond initial explorations and fully commit to the platform.

### Adopting dapr
Dapr is built in a way that allows you to incrementally adopt its services and conveniences. You may choose to use dapr for only certain 
scenarios, such as service-to-service invocation, and handle everything else they way you've always done.

Or, you may want to go all-in, and "daprize" every aspect of your application for a smoother developer experience.

Since dapr runs as a sidecar next to your application, you can also pick a hybrid approach, where, instead of using the dapr SDK, you 
call the dapr building blocks yourself using HTTP or gRPC.

## The experience
This chapter is all about what the actual developer experience is like. We'll dive deeper into what dapr is, how its used, and what it 
means to run daprized applications, and configuring components.