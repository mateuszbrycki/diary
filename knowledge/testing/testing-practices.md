## Testing Behaviour vs Testing Implementation

Unit tests should test behaviors, not the implementation. If one change (not in the class's contract/API) triggers tens of test failures, then there are probably some problems with tests.

## Unit Test
There is no one unit test definition. A software developer should be more interested in test cases than the actual class. A class doesn't define testing boundaries.

## Code Open for Testing
Any third-party dependency should be covered with an abstraction. This will open the code for testing - fake implementations (*fakes*) will be possible to be used, in tests.

### Reusable Tests
When the code is open for testing and all the dependencies are covered with abstractions, you can write reusable tests.
A reusable test is a test that has one implementation but its testing scope is defined by the dependencies. If the test is meant to be a unit test, the fakes will be injected. If the test is meant to be an integration test, the real implementations will be injected. The higher testing levels might require writing some "glue" code that will trigger HTTP calls or click a button on the UI.

## Bulletproof Tests
An application has well-defined layers (domain, application, view, infrastructure) to be open for testing.

The rich domain and application layers should be tested with unit tests. These layers should work on abstractions to be independent from the rest of the world. Integration tests should be implemented with the use of domain language. Such tests will be reusable on the higher levels (e.g. presentation layer).

Adapters should be tested with only integration tests because the real integrations should be checked. There is no value in unit testing the integration points.

The testing strategy should fit the tested module. There is no need to implement the whole testing pyramid in any of the modules.

Unit tests check business logic. Integration tests check if integration with real systems works. 

## Materials
1. [BSD 84: O implementacji testów backendu i architekturze otwartej na testowanie z Piotrem Stawirejem
](https://bettersoftwaredesign.pl/podcast/o-implementacji-testow-backendu-i-architekturze-otwartej-na-testowanie-z-piotrem-stawirejem/)
1. [Financial System Repository](https://github.com/stawirej/financial-system) that provides examples of reusable tests
1. [Sub-second acceptance tests by Aslaka Hellesøy](https://www.youtube.com/watch?v=AJ7u_Z-TS-A)
