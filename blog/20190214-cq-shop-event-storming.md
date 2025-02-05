> [!IMPORTANT]
> This article was originally posted on [passion-to-profession.com](https://web.archive.org/web/20220702180901/https://passion-to-profession.com/2019/02/14/cq-shop-event-storming/)

> [!NOTE]
> The code of the CQ-Shop project is available in the [cq-shop repository](https://github.com/mateuszbrycki/cq-shop).

# CQ-Shop – Event Storming
*Publication date:* 14/02/2019

Event Storming is a term that receives more attention every day. In this post, I’m describing what Event Storming is, how I tried it and how you can benefit from using this technique. Although it is not a new thing (introduced it in 2013), I haven’t met it in the corporate world.


Let’s describe Event Storming by going through a short example. Imagine yourself sitting in a room with your scrum team and product owners. POs are introducing the new, fancy functionality that the team is going to be splitting, estimating and implementing. Let’s assume that the team has experience, so they are comfy with the domain – they know terms, correlations, and implications. When the owners are going through the description, the team is making notes and asks questions. At the end of the meeting, everyone has an overall view of what they should deliver. A description has been extended with a few smaller details thanks to questions that team members asked. The scrum team estimated, planned, and put their hands on keyboards.

After the first week of the sprint, one of the developers realized that some data is missing so part of calculations could not be performed. The team member asked the product owner :

> Hi, we are missing data X. How can we retrieve it?

PO responded:

> This data is available in the system Y.

Perfectly planned iteration has just collapsed. The team is the middle of work, and the missing data block the User Story. To retrieve it they have to integrate with the external system. How to access it? Do owners of this system have a testing environment? Do they need to request some access rights? How to upgrade the production environment?

There are too many questions and not enough time. The team rolled User Story to the next sprint, so its performance dropped down. During the retrospective meeting, they started analyzing what and why happened. The outcome was pretty clear – the team hadn’t asked about the data, so POs had no idea how problematic the integration is. POs are not tech people so they might not be aware of such difficulties.

How is the team supposed to know what questions they should ask? There is an infinitive amount of directions they should analyze. It is almost impossible to examine everything. **How to spot the most crucial parts?**

## Event Storming for the rescue

Event Storming is a workshop where all the participants decouple a given problem into smaller parts. They start with identifying the domain events (facts) that occur in the system – e.g. “user account created”, “reservation created” or “money withdrawn”. Orange sticky notes represent those events. During the workshop, all the sticky notes are put on the wall so that every participant can see them.

When events seem to be ready, they analyze what happens between orange sticky notes – does the former event triggers the later? Is there any business logic? If so those events should be separated with a business rule named policy.

The next step might be to find what kind of actions cause the events – is it a user clicking on UI, a call from an external system or ticking clock? Those are commands.

You can mark interactions with external systems with pink sticky notes.

The yellow one might mark an aggregate – a logical group of various commands, events and policies together.

**Please note that there are no technical terms.** Everyone can take part in the discussion. Event Storming is all about domain knowledge. On that point, you don’t care that much about implementation. The most significant purpose behind that is to communicate with the business people efficiently. Using Event Storming, you are talking in the same language. There is no separation between business and technology.

Event Storming gives you an opportunity to find and analyze the most problematic parts of the story. More importantly, **Event Storming gives you a chance to ask proper and meaningful questions.** If the team from the story had been using ES, then they might have found a problem with missing data so they might have estimated and planned better.

## My approach to Event Storming

If you remember the previous post, CQ-Shop is supposed to be an e-commerce application implemented in the event-driven architecture. Since I didn’t have any experience in event-driven architectures, I had no idea where I should start. When I was planning work, I found that Event Storming might be very helpful. Driven by curiosity, I decided that I’ll start the development by going through plenty of storming iterations.

Everything that I know about ES is available online. I read a few articles and watched a couple of videos on YouTube. I listed them in the Materials section. My first impression was that there is no proper way of doing ES. It seems that your approach should depend on your problems, people, and custom interpretation of that technique.

Returning to CQ-Shop. I managed to invite all of the important people – my (developer/Scrum team), myself (domain expert) and I (product owner). I started with events. Then added other blocks. 

I had almost everything that I needed to start implementation. The missing part was the level of granularity of microservices. I spent some hours thinking about it. I realized that when I split the “paths of sticky notes” vertically, I would get boundaries of where particular events play an important role. I treat a microservice as an aggregate (from ES). Based on that I decided to have the following services: user-management-service, cart-service, order-service, warehouse-service, payment-service (not implemented yet), shipping-service (not implemented yet).

My play with ES was missing two essential points. I didn’t spend enough time analyzing **bounded contexts**. I divided the architecture into microservices using aggregates discovered in ES.  It is a shortcut and I’m not sure if it will work in this case. I haven’t spotted any problems yet. I think that this part was not crucial for starting the implementation. The funny thing was that I could implement the diagram straightaway – it did not only described the flow of events and but also necessary business logic.

## Event Storming vs. CQRS vs. Event Sourcing

When diving deeper into Event Storming world, I was confused by three terms that sometimes appear as synonyms: Event Storming, Event Sourcing, and CQRS. They are related, but they are not the same. Moreover, you use them on different stages of development.

Event Storming is **a technique of analyzing the domain.** With the sticky notes, you describe what a process looks like.

CQRS is **an architecture of the implemented application.** You have Commands and Queries that are responsible for representing calls to the system.

Event Sourcing is **a technique of restoring the state of your application.** Instead of querying the product’s availability from the database, you analyze events that affect the warehouse: order created, reservation release, product delivered to the warehouse. When you sum up all the values, you get the real availability of the product.

Remember:
**Event Storming** is for analyzing domain.
**CQRS** is for application’s architecture.
**Event Sourcing** is for retrieving and storing objects state.

## Conclusion
In the beginning, Event Storming helped me with starting the implementation. When I was coding, I used the created diagram extensively. I think that it resulted in less messy communication between microservices.

I see ES being a key player in implementing new features even in legacy code. The event-driven notation does not limit you to using event-driven architecture. The most valuable is to know what happens under the hood.

Another advantage of ES is that it may help you spot bottlenecks in systems that do not exist yet. If you see that component X is receiving large chunks of data, then you may implement it in a way that let you scale efficiently.

Moreover, **I think the diagram created during ES might act as documentation.** You won’t need all those complicated UMLs. The biggest problem might be a continually evolving system and an outdated diagram. You need a discipline of keeping it synchronized with the current implementation.

Let’s imagine a scenario where the team cares about the diagram and they have a new-joiner. A person responsible for knowledge transfer can say:

> The applications communicate via REST/Kafka, depending on the service it is written in Java/Python/Scala/Go. They receive all the configurations from the config server. We use Zuul as an API Gateway. Here you have a diagram that shows you business scenarios and how we handle failures.

Everything is clear. Can you imagine how much faster the introduction would be? Any new team member is ready to work within one day.

Summing everything up, I see ES in many parts of our business – development, planning, and even in management. After small play with it, I have more questions than answers. I definitely should take part in some workshops organized by ES experts.

## Materials
1. [Introducing Event Storming](http://ziobrando.blogspot.com/2013/11/introducing-event-storming.html)
1. [EventStorming Recipes](https://skillsmatter.com/skillscasts/5193-alberto-brandolini#showModal?modal-signup-complete)
1. [Discovering unknown domain with Event Storming – Mariusz Gil](https://www.youtube.com/watch?v=dhoXYRqghws)
1. [Event Storming demo & discussion](https://www.youtube.com/watch?v=xIB_VQVVWKk)
1. [Towards OOP from Event Storming – Jakub Pilimon](https://www.youtube.com/watch?v=k8sGf6ZPs2E)
1. [Building microservices with event sourcing and CQRS](https://www.youtube.com/watch?v=A0goyZ9F4bg)
1. [Modelling Reactive Systems with Event Storming and Domain-Driven Design](https://blog.redelastic.com/corporate-arts-crafts-modelling-reactive-systems-with-event-storming-73c6236f5dd7)
1. [Bounded Contexts are NOT Microservices](https://vladikk.com/2018/01/21/bounded-contexts-vs-microservices/)
