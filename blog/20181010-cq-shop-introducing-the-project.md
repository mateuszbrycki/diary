> [!IMPORTANT]
> This article was originally posted on [passion-to-profession.com](https://web.archive.org/web/20220809174902/https://passion-to-profession.com/2018/10/10/cq-shop-introducing-the-project/)

# CQ-Shop – introducing the project
*Publication date:* 10/10/2018

I’m about to start my final year at university, which will involve many activities related to obtaining my master degree. One of them is writing my master’s thesis, which is one of the biggest and the most time-consuming challenges. It’s a process that consists of writing the thesis and developing a project.  I’m going to write a series of posts that will show you how the project is evolving.

I already have some practical experience with developing a project and writing about it on the blog. As some of you may remember, I took part in the [Get Noticed 2017](https://devstyle.pl/daj-sie-poznac/) contest. I was blogging about the Aksesi project for almost 4 months.  I want to use the same approach for my master thesis. Unfortunately, I will publish the posts less frequently as this project is going to consume more time than the Akesi project.

## Goals
I have two main goals for this project. Firstly, I want to write an interesting master thesis. This means the project has to be interesting since it is tightly coupled with the thesis. Secondly, I want to learn as many new things as possible. This project will be built with many hot-fancy-buzzword technologies and architectures. It is not going to be launched on production so I don’t have to think about its maintainability. I will let myself mix different approaches. Everything for the sake of learning.

## Concept and schedule
I’m going to write an e-commerce application named CQ-Shop. I don’t have approval for the final version of the topic so I’m writing only about the concept. As I mentioned, I want to learn as much as possible so I’m going to combine programming in Java with Machine Learning and Artificial Intelligence. That is why the project will consist of two sub-systems. The first one will be a Java application that will be producing input for the ML/AI model. I assume that developing this part will take me about 5 months. The other part will be related to some scientific work. The model will be responsible for analyzing data produced by the application and detecting runtime errors.

## Architecture and Technologies
I’ll write a Java application with Events Sourcing and CQRS. I haven’t had an opportunity to use these approaches so it is about time. I’ll use the Spring stack – Boot, Data, Security, Cloud, Actuator, and so on. Right now, I’m thinking about using the Axon framework but I need more time to decide. I’ll use the latest official Java release – probably version 11. I don’t have any preferences for the database. It could be PostgreSQL or H2. I have to check if any of NoSQL solutions will be more suitable for storing events. Everything will be working as a system of microservices.

On the AI/ML side, I will concentrate more on the model’s shape than technologies. I’m going to use Python with TensorFlow. I don’t have any favorite model at the moment. The project will be based on research so it is going to be changing very fast.

I’m planning to use some cloud environments to host all the microservices and models. I’ve been thinking about Google Cloud Platform or Pivotal Cloud Foundry. I haven’t decided yet.

ES, CQRS, AI, ML, microservices, TensorFlow, Spring, Java 11 – all lovely buzzwords mixed together. Perfect!

## Summary
Honestly, I’m both worried and excited about the CQ-Shop project. It will take a lot of time to make it work. The process will involve a lot of reading and learning. I’ll face plenty of problems and probably won’t be able to resolve most of them. Keep your fingers crossed!