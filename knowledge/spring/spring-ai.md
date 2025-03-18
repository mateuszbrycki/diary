
## Spring AI


### Java & AI
1. Java powers 90% of Fortune 500 companies' backend systems
1. AI capabilities are now expected in enterprise applications
1. **Java developers have a unique market advantage with AI skills**

### Consuming LLMs
In the raise of LLMs, consuming AI models became not-only-a-Python thing. As the models are widely available via HTTP, any programming language can utilize them.

You don't even need Java. You can use `curl` and play with them.

### Spring AI
Spring AI is not only about HTTP calls. It solves several other problems around AI-based products.

Portable API support across AI providers for Chat, Image & Audio. Spring AI provides an abstraction over APIs - you write your code and use an API, Spring handles communication, token exchange, etc. 

There are specific Spring Boot Starters for LLM providers. 

Spring AI provides synchronous & streaming API options.

`ChatClient` is the main interface for communicating with a model. More in the [docs](https://docs.spring.io/spring-ai/reference/api/chatclient.html).
The `ChatClient::call` method is a blocking method. The `ChatClient::stream` returns a `Flux` that allows you to handle the responses asynchronously.

`ChatClient::system` allows you to define the overal context of the conversation.
`ChatClient::user` allows you to define user prompt and parameterize the system message - [example](https://github.com/danvega/spring-ai-workshop/blob/main/src/main/java/dev/danvega/workshop/prompt/ArticleController.java).

Spring AI helps you also define the structure of an output - [example](https://github.com/danvega/spring-ai-workshop/blob/main/src/main/java/dev/danvega/workshop/output/VacationPlan.java). The LLM response is mapped into a type so in the later code you can work with type. No serialization or response post-processing is required. `ChatClient::entity` allows you to define the response type.

The model itself doesn't have any memory. Any time you comminicate with it, you need to pass the whole conversation. Applications like ChatGPT are products that wrap models, they are no models themselves. Any time you interact with the underlying mode, the chat passes the conversation context.
With Spring AI you can build stateful conversation (passing chat memory). `ChatClient::defaultAdvisors` let you define chat memory. The context size is configurable.

Models are trained with some data cutoff date. LLM products provide search feature so that they can return current data.
`@Tool` helps you extend model capabilities. Spring AI is able to pass a tool's code, extend the model with data returned by the tool.
Use `ChatClient::tools` for registering tools - [example](https://github.com/danvega/spring-ai-workshop/blob/main/src/main/java/dev/danvega/workshop/tools/DatTimeChatController.java). More on tools in the [documentation](https://docs.spring.io/spring-ai/reference/api/tools.html).

### Spring AI vs MCP

Spring AI is a framework for enterprise applications that need to integrate AI.
MCP is a protocol that has been implemented by Antropic. It standardizes how you get an external data from e.g. APIs or external sites.
If you want your application/page to be available for LLMs, you implement MCP so that models know how to scrape your data.
There are MCP clients (IDEs, tools, etc.) that connect to MCP servers. More on [MCPs](https://modelcontextprotocol.io/introduction).

## Materials
1. [Spring AI](https://spring.io/projects/spring-ai)
1. [Spring into AI: Building Intelligent Applications with Spring AI with Dan Vega](https://www.youtube.com/watch?v=XjfWyc6xmSA)
1. [spring-ai-workshop](https://github.com/danvega/spring-ai-workshop)
1. [Building Effective Agents with Spring AI (Part 1)](https://spring.io/blog/2025/01/21/spring-ai-agentic-patterns)