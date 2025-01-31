# Architecture Discovery

## 6 Perspectives

1. Functional Requirements
1. Non-Functional Requirements (Architecture Characteristics)
1. Limitations/Risks
1. Visualization
1. Fitness Functions
1. Team Topologies

All of the perspectives should be considered from a perspective of:
1. project sponsors
1. end users
1. developers/architects/organizations

### Functional Requirements

1. Collect all the requirements during a conversation with the client.
1. Perform internal process Event Storming.
1. Discuss the Event Storming result with the client. 

Architecture for Flow  = Event Storming + Wordley Map + Team Topologies (refer to the materials section).

### Non-Functional Requirements (Architecture Characteristics)

Everything that ends with `-ity` - scalability, extendability.
There about 148 of them. 

Arch42 Framework.

Consider only these characteristics that are mesurable. 

1. Analyze the functional requirements.
1. For each of the functional requirements match a non-functional requirement. Limit the set of non-functional requirements to 10.
1. Match discovered architecture characteristics to the modules discovered during the ES session. 
1. Among the selected 10 characteristics select 3/4 the most important ones.
1. Try to find an architecture that fulfils the most important characteristics using the [Mark Richards' Workseet](https://www.developertoarchitect.com/downloads/worksheets.html).
1. Among the selected 10 characteristics select 3/4 the least important ones. Check if the architecture style from the previous point doesn't conflict with characteristics.

[Here you can find an example](https://github.com/TheKataLog/BluzBrothers/blob/main/ArchitectureCharacteristics/Characteristics.md) of how such a process might look like.

### Limitations/Risks

Examples of such limitations:
1. budget
1. time
1. end users

Any limitations of risks are hard for discovering. You should listen the sponsor carefully. 

Each of the architectures has some kind of a shape. You should discover that shape. Designing an architecture is an evolving process. Any architecture style will have limitations.

### Visualization

C4 Diagram is a great idea for visualizing the architecture. 
Be aware that your diagrams might be printed in a grey scale - using colors might not be sufficient.

More on visualization - [Communication Patterns by Jackie Read](https://www.goodreads.com/book/show/171661690-communication-patterns).

### Fitness Functions

Introduction to Fitness Functions - [Fitness Functions](./fitness-functions.md).

#### Example 1

There is a requirement:
> A patient is connected to several sensors. The measurements are displayed on a nurse's monitor. The latency between taking a measurement and displaying it on the monitor cannot be higher than one second. 

1. Benchmarks - Kafka needs 1ms to proces one event
1. Peak - there will be about 5000 events in peak
1. An application a few ms to process an event
1. Network Throughput - 1Gbits

In the peak the latency might be about 600ms. This was the proof that the event-driven architecture might work here.

#### Example 2

Checking code style with ArchUnit.

#### Example 3

There is a microkernel architecture style used. Users were building their custom connectors to third parties.
The most important architecture characteristic was `extendability`. 

The fitness function was about developer checking, by writing an extension, if it's still easy and fast to develop a custom connector. 

### Team Topologies

Who will be implementing the system? Won't the development team be cognitively overloaded?
Present the sponsor an idea of how the implementing teams might be constructed and aligned.

Leave the team with autonomy to implement their ideas. 

## Narrative Architecture

Your architecture should try to tell a story.

Functional requirements -> Components -> Architectural Characteristics -> Architecture Style -> Visualization -> Fitness Functions -> Team responsible for implementation.

You might have the best idea for an architecture. However, it might not be enough if you cannot sell it to the stakeholders. 

## Materials
1. [BSD 85: O Architectural Kata i procesie projektowania architektury z Piotrem Filipowiczem](https://bettersoftwaredesign.pl/podcast/o-architectural-kata-i-procesie-projektowania-architektury-z-piotrem-filipowiczem/)
1. [Architecture for Flow - Wardley Mapping, DDD, and Team Topologies - Susanne Kaiser - DDD Europe 2022](https://www.youtube.com/watch?v=Lfzph_5wb9c)
1. [StayHealthy.MonitorMe](https://github.com/TheKataLog/BluzBrothers)
