# Building Evolutionary Architectures: Support Constant Change 

**Authors:** Neal Ford, Rebecca Parsons, Patrick Kua

## Notes

### Fitness Functions

An evolutionary architecture supports guided, incremental change across multiple dimensions.

> An architectural fitness function is any mechanism that provides an objective integrity assessment of some architectural characteristic(s).

A fitness function can be anything that checks something. It might be code metrics, ArchUnit tests, developer satisfaction surveys. 

Scope:
*Atomic* fitness functions run against a singular context and exercise one particular aspect of the architecture.
*Holistic* fitness functions run against a shared context and exercise a combination of architectural aspects.

Cadence:
*Triggered* fitness functions run based on a particular event, such as a developer executing a unit test, a deployment pipeline running unit tests, or a QA person performing exploratory testing.
*Continual* tests don’t run on a schedule but instead execute constant verification of architectural aspect(s), such as transaction speed.

Result:
*Static* fitness functions have a fixed result, such as the binary pass/fail of a unit test.
*Dynamic* fitness functions rely on a shifting definition based on extra context, often real-time content.

Invocation:
*Automated* fitness functions run within a pipeline checking changes incrementally.
*Manual* fitness functions are executed manualy by people.

Proactivity:
*Intentional* fitness functions are written at project inception and as part of a formal governance process, sometimes in collaboration with other architect roles such as enterprise architects.
*Emergent* fitness functions are written when architects notice some behaviour that would benefit from better governance.

System-wide fitness functions:
1. unit tests,
1. integration tests,
1. contract tests,
1. code metrics (cohesion, abstractnes, dependencies directions),
1. architecture metrics,
1. process metrics (DevOps),
1. monitors.

### Connascence

> Two components are connascent if a change in one would require the other to be modified in order to maintain the overall correctness of the system. 
~Meilir Page-Jones

Static Connascence:
1. Connascence of Name (CoN) - multiple components must agree on the name of an entity.
1. Connascence of Type (CoT) - multiple components must agree on the type of an entity.
1. Connascence of Algorithm (CoA) - multiple components must agree on a particular algorithm.
1. Connascence of Meaning (CoM) or Connascence of Convention (CoC) - multiple components must agree on the meaning of particular values.
1. Connascence of Position (CoP) - multiple components must agree on the order of values.

Dynamic Connascence:
1. Connascence of Execution (CoE) - the order of execution of multiple components is important.
1. Connascence of Timing (CoT) - the timing of the execution of multiple components is important.
1. Connascence of Values (CoV) - this occurs when several values relate to one another and must change together.
1. Connascence of Identity (CoI) - this occurs when multiple components must reference the same entity.

Architects should prefer static connascence to dynamic connascence because developers can determine it through simple source code analysis, and modern tools make it trivial to improve static connascence. When refactoring remove connascence from the last value of the list (CoI) to the first one (CoN).

Connascence Properties:
1. Strength
1. Locality
1. Degree

### Architectural Quanta

An architectural quantum is an independently deployable component with high functional cohesion, which includes all the structural elements required for the system to function properly. In a monolithic architecture, the quan‐ tum is the entire application; everything is highly coupled and therefore developers must deploy it en mass.

*Static coupling* - dependencies resolve within the architecture via contracts. The dependencies include OS, frameworks, libraries, transitive dependencies, and any other operational require‐ ment to allow the quantum to operate.
High static coupling implies that the elements inside the architecture quantum are tightly wired together, which is really an aspect of contracts. Architects recognize things like REST and SOAP as contract formats, but method signatures and opera‐ tional dependencies (via coupling points such as IP addresses and URLs)

*Dynamic coupling* - resolve at runtime either synchronously or asynchronously.
Communication between quantas is a multidimensional space, influenced by three interlocking forces:
1. Communication (Sync <-> Async),
1. Consistency (Atomic <-> Eventual),
1. Coordination (Choreographed <-> Orchestrated).

### Reuse Patterns

> Software reuse is more like an organ transplant than snapping together Lego blocks. ~John D. Cook

Effective Reuse = Abstraction + Low Volatility

### Principles of Evolutionary Architecture

Last Responsible Moment - delaying decisions as long as you can, but no longer.
Architect and Develop for Evolvability - architects should treat evolvability as a first-class concern in architecture.

Postel’s Law:
1. Be conservative in what you send
1. Be liberal in what you accept from others
1. Use versioning when breaking a contract

Architect for Testability

### Mechanics
1. Step 1: Identify Dimensions Affected by Evolution
1. Step 2: Define Fitness Function(s) for Each Dimension
1. Step 3: Use Deployment Pipelines to Automate Fitness Functions

### Guidelines for Building Evolutionary Architectures
1. Remove Needless Variability
1. Make Decisions Reversible
1. Prefer Evolvable over Predictable
1. Build Anticorruption Layers
1. Build Sacrificial Architectures
1. Mitigate External Change
1. Updating Libraries Versus Frameworks
1. Version Services Internally