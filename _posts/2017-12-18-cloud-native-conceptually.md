---
layout: post
title: Cloud Native Conceptually
date: 2017-12-28 20:15:00 +0100
category: cloud-native
tags:
  - conceptual
  - opinion
---

![Keep Cloud-Native Weird!](/images/posts/2017/12/keep_cloud_native_weird.png){: .center-image}

Before we can get started writing a, so called, *cloud-native* applications we need to understand what this term actually means. That is a utterly important matter if we want to be effective in the way we address a certain technology, this blog post tries establish the ground rules.

The simplest way to explain *"cloud-native"* is to refer to an application that is ready to live in a Cloud and take the most of it. However, a few elements are more critical than others, when it comes to application workload in a public/private Cloud, which are:

- [**State**](#State);
- [**Runtime**](#Runtime);

More can be found at the CNCF (Cloud Native Computing Foundation), please note the [charter][cncf-charter] for the areas of qualification.

## State

On any cloud we will have disruptions in the underlying application infrastructure, needless to say that virtual machines will go down and maybe entire zones can vanish away, therefore the application landscape needs to be ready when it happens. When it comes to state, or in other words the actual data the application is dealing with, it needs to be stored *"outside"* of the application runtime where the backend is ready to resist failures and deliver desired durability and consistency, so the application itself does not need to concern about the data plane.

More elements can be included as the application state which are not the actual data directly, namely logging and performance/telemetry metrics. All the surrounding elements that your application may need to be operational and monitored, those elements follow the same rule, they also need to be stored *"outside"*. Hence the application itself is designed to be stateless, all the stateful stores will be plugged during bootstrap. This design principle brings a number of positive side effects, for instance, testing the application is a much simpler task now, so CI/QA activities tends to be simplified.

## Runtime

Another challenge that we face is the application runtime contracts, every language ecosystem will depend in a different approach, and deeper in the application stack we also need to consider dependencies, configuration, operational system requirements and more. In other words, the deliverables of a application executable are more than compile/prepared files, this context is extend much further.

Therefore alternatives like *containerization* are valid to address this limitation. The container approach combines application related activities that belong together, the container is the application's package and also contains tha operational system parts that it requires to be executed against, thus it creates a more practical separation of concerns when it comes to developing and executing a application. This mindset is also beneficial for developers, QA and all other involved areas.

The return of investment of splitting the application and infrastructure runtime requirements provide a clear common ground for parallel work, since one team can prepare a new environment or cloud, and the other team can be busy writing the applications, at the end of the day both teams will be assured regarding runtime compatibility. Furthermore, it's usual to establish more contracts for more areas in the application landscape, like configuration, log shipping, functional/performance telemetry, rinse and repeat.

### Orchestration

After the investments on *stateless* and *runtime standardization*, we create a perfect ground for automated orchestration. This role is much simplified with the ability of spin up or down instances without functional impact, and being free to run this application almost anywhere, since it relies in a common runtime.

Furthermore, orchestration can take care of a good chunk of the operational load, making sure applications are available to receive requests, handling the underlying cloud configuration to reflect the application landscape desired state, also playing roles during the deployment phase. To orchestrate instances it relies on functional monitoring endpoints in the application itself, therefore it often moves the monitoring related actions to the beginning of the software development process. For instance, the ground rules to execute a given application in the platform consists on exposing monitoring endpoints, so we move this action as early as possible in the development life-cycle.

## Related Aspects

Besides of [*State*](#State) and [*Runtime*](#Runtime) we also need to consider other aspects. Those will mostly consist of *"convention over configuration"* and other practices that we are already familiar with, however on this blog post we approach them with the cloud-native initiative in mind.

### Detaching vs. Reuse

Binding required services during runtime also means a change in the way the application is configured, that's a kind of obvious thing to say, but it works as a good example of how this mindset contaminates the whole [ecosystem of cloud-native tooling][cloud-native-ecosystem]. And will certainly contaminate the software you write for this kind of environment.

The mindset is useful in order to be more effective in how we handle application life-cycle. The same components can be instantiated with different configuration to perform different roles in the infrastructure, rather than a single instance performing multiple roles, to give a quick example.

The drive for *componentization* also drives the need of version control individual elements of the application life-cycle, on which we split source-code, configuration and releases into *their own swimming-lanes*, providing the ability of individually roll-back and compose, where this "way-of-working" has being coined by [12factors.net][12factors] and others. Consequently, to aid applications to consume common infrastructure services, it's usual giving them a common set of libraries to act as the application/microservice *chassis*, to embed common elements automatically instead of having this code/configuration spread over multiple projects. The approach is what [`go-kit`][gokit] provides for Go based microservices.

### What About *DevOps*?

A opinionated view about "DevOps", in this context it would come as a consequence of a better split of activities and adding tooling like orchestrator to transform common platform activities more and more into API calls and highly automated tooling for end-users. Hence, a group of developers can very well be responsible for the functional health of production grade applications, mostly because for doing so it's not anymore required to become a full-fledged sysadmin right away; while the conceptual part of the sysadmin work is ultimately important, the daily basis activities are being automated at a rather fast pace.

For instance, let's consider ["managed-code"][managed-code] products, like the *FaaS* (function-as-a-service) public cloud offerings. They already automate away all the "old-school" sysadmin activities, it already implies most of the practices described on this blog post.

### Fighting the "Waterfall"

I think that's clear already for all of us, with actions like standardization of the infrastructure runtime, having more strict contracts for handling application related state, and binding configuration and other runtime requirements during application bootstrap, are all actions that move things forward to adopt a more agile software development workflow, actions that will also reflect on the operations perspective.

## Conclusion

Therefore, there are reasons to adopt cloud native related approach beyond reaching different cloud providers, it more related to modernize the current application landscape in order to benefit from the latest best practices.


[12factors]: https://12factor.net
[cloud-native-ecosystem]: https://github.com/cncf/landscape
[cncf-charter]: https://www.cncf.io/about/charter/
[managed-code]: https://en.wikipedia.org/wiki/Managed_code
[gokit]: https://github.com/go-kit/kit/