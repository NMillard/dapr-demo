# Application deployment

Deploying applications to production can be a frustrating, and at times even confusion task, when comparing what you have locally with 
what you need in the cloud.

However, as with any tool, there are potential "gotchas" to be aware of and some limitations.

### Ease of Deployment:

- Platform Agnostic: One of the biggest advantages of Dapr is its platform-agnostic approach. It can run on any hardware, OS, or cloud
  provider, which makes deploying your applications straightforward regardless of your existing tech stack.
- Consistency: Dapr provides a great deal of consistency when across environments, but you still do need to be aware of some 
  cloud-vendor specifics.
- Kubernetes ready: Component configurations are already written in a kubernetes-ready format. As long as you're using a k8 supported 
  component, you can apply it directly to the cluster using `kubectl`.

### "Gotchas" and Shortcomings:

- Learning Curve: Like with any tool, there's a learning curve involved with Dapr. Understanding its methodologies and best practices will
  take time, and educating the team about it might slow down the initial deployment process.
- Debugging Complexity: Debugging applications using Dapr can be complicated because issues may lie either in your application code or
  within Dapr’s runtime environment. If problems emerge, it might require more effort to identify and solve them.
- Maturity: Although Dapr has shown much promise, it’s still during the early development phase. Not everything is documented and you 
  sometimes have to dig deep to investigate a certain issue.
- Dependencies: Although Dapr simplifies the process of running microservices, it adds another layer of dependency. It means any extra
  complexity, bugs, or vulnerabilities associated with Dapr become your problem too.
- Operational Complexity: Operating a Dapr cluster in a production environment may involve additional operational complexity. Configuration,
  security, upgrades, and monitoring need to be taken care of, which can introduce potential points of failure or complication.

Overall, while Dapr greatly simplifies the deployment process and mitigates common headaches in distributed systems, it's beneficial to be
aware of potential challenges that may arise in the deployment phase, to plan and account for these possibilities.

## Deploying daprized apps

Dapr's documentation on how to deploy to kubernetes is quite good and covers many of the usual cases you'd want to know about.

Instead of reiterating dapr's documentation, I've decided to see how Azure containerapps are used in combination with daprized apps.

Azure Container Apps is a newly announced service by Microsoft that simplifies deploying and running containers in Azure. It auto-scales and integrates with operational tools, making it an attractive platform to host applications.

Azure Container Apps has been designed to work seamlessly with Dapr to bring the power of microservices building blocks to your applications. By partnering these two technologies, Microsoft achieves a high level of abstraction, thus simplifying the complexities involved in creating and managing microservices.

While using Azure Container Apps, developers can automatically take advantage of Dapr's capabilities by activating through a simple configuration. This provides developers with immediate access to various building blocks, such as state management, pub/sub messaging, and various other integrations right out of the box.

In terms of relationship with Kubernetes, Azure Container Apps offers an elevated level of abstraction while still offering the flexibility and power that Kubernetes is known for. It essentially is a serverless container hosting solution built on the open-source Kubernetes. By using Container Apps, developers can deploy applications without managing the underlying Kubernetes cluster.


This offers a few key benefits:
- Simplified Management: Developers don't need to worry about managing or scaling a Kubernetes cluster. The infrastructure is all managed
  behind the scenes, meaning you only need to focus on your application.
- Scalability: Azure Container Apps scales automatically based on demand, making it exceedingly simple to run applications at scale
  without needing to tweak Kubernetes configurations.
- Resource Optimization: Since it operates on a serverless model, it only charges for the resources your application uses, leading to
  efficient resource usage.
- Opensource: Thanks to its Kubernetes underpinnings, Container Apps are interoperable, avoiding vendor lock-in and providing flexibility
  to developers.

However, it's worth pointing out that while Azure Container Apps simplifies numerous aspects of deploying and managing containers and Kubernetes, there would be scenarios where you need finer control over your infrastructure, and you might prefer using Kubernetes directly.