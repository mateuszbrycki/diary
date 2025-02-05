> [!IMPORTANT]
> This article was originally posted on [passion-to-profession.com](https://web.archive.org/web/20220809173455/https://passion-to-profession.com/2019/09/16/cq-shop-summary/)

# CQ-Shop – Summary
*Publication date:* 16/09/20219

Creating the CQ-Shop application was a great adventure. I used a bunch of new tools – **event-driven architecture, Event Storming, Kafka**, and many more. In this post, I summarize all the things that I learned and achieved. 

## University
Let’s start with the university context. The CQ-Shop was supposed to be my last project at the university. I wanted it to be the best one. The greatest of all project that I have ever developed there. I followed the rule of learning as much as possible, and it paid off. When I came up with the idea for the thesis, I knew that the project would be challenging. I even I started considering if it won’t be too overwhelming. Thankfully, everything was doable. 

After presenting and defending the thesis, the supervisor and commission were impressed by the project. They liked that I divided it into two parts – development and anomaly detection. I even have been proposed to join the university and work there as an assistant.


## New Skills
 As mentioned, the idea behind CQ-Shop was learning, learning, defending the thesis, learning, and once again learning. I tried to select tools that I didn’t know. [I started by learning about Event Storming](./20190214-cq-shop-event-storming.md). I found it a great way of learning business requirements. In the second step, [I deep dive into event-driven architecture](./20190827-cq-shop–events-anomaly-detection.md). I spent long hours reading articles and watching conference recordings. Once I got some ideas of these two tools, I started merging them. 

The next step was implementation. In the beginning, I faced issues with Kafka-Spring integration. I invested a lot of time in running Kafka on GCP. Finally, I managed to do so. I also learned details of **Schema Registry, Avro format, Config Server, and Eureka Discovery**.

When the implementation was ready, I started the scientific part of the thesis – [anomaly detection](./20190827-cq-shop–events-anomaly-detection.md). I put my hands on **LSTMs networks** and outliner detection algorithms. I had to allocate another few hours learning more advanced concepts of Python. I realized that the research is very time-consuming. I spent hours on Arxiv reading tons of papers. This part is something that you cannot estimate it in any way. 

## Future
The CQ-Shop consumed a lot of time. However, there are a lot of things that I should either polish or complete.

In the future, I want to dockerize the application and run it using Kubernetes. I want to master those two technologies by the end of the year. I know some basics of Docker, but I have never invested time into playing with it in more sophisticated scenarios.

I also want to implement missing business logic. The system supports only the most straightforward e-commerce features. I developed a bare minimum so that I could start working on anomaly detection.

I need to do a lot of refactoring. I should rethink bounded contexts, data consistency, and database schema. 

Finally, The CQ-Shop works on CQRS-wannabe solution. I plan to implement the full-fledged pattern. 

## Summary
I’m happy with the project that I delivered. **I had great fun developing it**. Sometimes, when I was overwhelmed by the work, I felt that I should have selected a more comfortable topic. However, **the amount of things that I learned pays off** all the hours spent on CQ-Shop. 

Moreover, I’m happy that I started very early – about ten months before the deadline. I had enough time for pivots and refactoring. I didn’t have to rush when looking for solutions for anomaly detection. 

Today I can say that this is the best of all projects that I developed during my journey at the university. **It is the most challenging, the most mature and the most exciting amongst all of them**.

## Read more about CQ-Shop
1. [CQ-Shop – Introducing the Project](./20181010-cq-shop-introducing-the-project.md)
1. [CQ-Shop – Event Storming](20190214-cq-shop-event-storming.md)
1. [CQ-Shop – Events and Anomaly Detection](./20190827-cq-shop–events-anomaly-detection.md)
1. [CQ-Shop – Architecture, Environment, and Tools](./20190905-cq-shop-architecture-environment-and-tools.md)