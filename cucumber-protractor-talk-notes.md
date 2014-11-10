## Cucumber, Protractor, Browser and mobile testing: Notes from the trenches

At the time of writing the javascript community is in a state of flux. Those on the edge of developments appeared to be accepting the reality 
that test-first development - as a methodology - had been unrealistic and in many ways harmful; though the said so only in hushed words. 

Those still playing catch up seemed either to be resisting the necessity of software testing as a methodology at all or still experimenting 
with the degree and granularity to which tests should be written. 

As background, I come from the latter camp - I'm playing catch up. I am relatively inexperienced as a software tester but have practiced deploy-and-pray 
brash agency work with only the most rudimentary of manual testing to the other extreme of 100% coverage mocked unit-tests for product development. 

## Unit tests in angular 

My team has a large set of unit-tests which run on Karma with Jasmine. These are unit-tests of the more granular type. Angular's dependency injection 
allows for an impressive level of flexibility and isolation, particular for the less stateful parts of the code (services, filters and the like).

Angular controllers are testible, to a certain extent, but only with a degree of difficulty and a considerable amount of sample-data being passed back and forth. 

To some extent directives are some of the more difficult peices of code to test, but I actually find their tests perhaps the most useful 
for reasons due to the width, isolation of concerns and coverage of tests you get with simple DOM interaction in a unit-test. 

## The Problem's we face

On our team, our QA's are champions in my mind. They have a detailed knowledge of all scenarios. However their tests are more or less completely 
opaque to us developers. 
* We aim to reach the goal of continuous delivery
* No visibility on QA's tests - We can't either run or even fully understand their C# tests
* No angular visibilty (ie, no model access)
* No ability to run intermediate level tests for integration
* No ability to run, spot or help builds to check a feature is complete, running and not in any way regressed.
* No obvious path forward for automating plantforms
* Resource wall with testing

## Theory and tech

We chose the following toolset

| Tool                   | Purpose                                    |
|------------------------|--------------------------------------------|
| Protractor             | stack runner, coordinator, selenium driver |
| CucumberJS             | Feature files, setup, tests                |
| Chai, Chai as promised | Assertions, async assertions               |
| Async, Q, Co           | Handling the async problem                 |
| gnode  		 | Generators for Co 			      |

## Testing framework choices 

Cucumber is contentious. Detractors point to it's surge and decline in popularity in Ruby: 
Originally, the BDD proponents saw this as a way to make BA's write tests. This is criticised both in ruby and nodeJS hopelessly unrealistic, it is seldom the 
case that they will be fully aware of the setup/teardown routines and their work (should a user be logged in or not, have a user-agent etc?!?). 

Critics say that the additional overhead of having to parse regexes simply leads to tortured english syntax (for reusability) or else something which is effectively 
a comment with a lot of overhead. 

Proponents propose the following criterion in response:  

"Will the team (everyone, not only the tech people) benefit from scenarios written in plain English? 
Is there a chance it'll help someone understand things better and communicate more efficiently?"
-- Julien Biezemans, [Interview with infoQ](http://www.infoq.com/news/2014/04/cucumberjs-bdd-biezemans)

In our particular case, I think the answer is yes. This is a narrow case and not entirely proven. However, it also matches the exiting C# infrastructure we 
have in place and allows for 'buy in' from our QA's insofar as they will be working with a familar pattern.  
Furthermore, while I don't think this technique is without fault, I think there is a benefit for having business people focus on the exact outputs scenarios they 
wish to see. It focuses their attention onto the underlying schema and makes problematic features fail faster and earlier and gives business a sense of being 
a part of the development lifecycle rather than divorced from it.  

However, this toolset will not suit many, or indeed perhaps most teams. Protractor's underlying Jasmine framework is well supported, 
sufficient and requires little work to get setup. It's only real downside is that taking BDD language of 'Given, When, Then' doesn't map 
fluidly or easily to Jasmine describe blocks.

## The economics and utility of testing
* integration tests -> nearly same utility but much less overhead
* Integration tests -> finding the kind of breaks which get missed by mocking, bad assumptions, poor integrations
* You can trust an integration test more than something with mocks because you're testing the actual, rather than the imagined
* Utility vs speed in the integration tests vs unit-tests

## Do's
* Specify by example
* Use page objects
* Build the test pyramid of tests
* Discuss the testing economics - each test takes time, what's the time-value tradeoff? 

Error messages are bad in cucumber
Criticism
	* Pipe dream in that it's not a realistic scenario for middle-management to understand, use feature files
	* causes extra overhead 
	* Why not just create a comment to accomplish the same thing

Criticism for unit tests from Tim Oxley
	* Over-testing can be a burden, testing with integration tests a large area give good benefit
	* The economic argument for coverage
	* "Don't down drown in your own tests" 


Notes:
	check out zuul, browserling
	Tape, Mocha, BrowserifyRequireify


## Don'ts
* Just dive in (like we did), think about what you need
* Avoid overly brittle tests by writting too specific tests 
* Avoid testing implementation
* Debugging tests can be very annoying, particularly with async issues. Be aware of silent-failure because callbacks aren't being called. 
Cucumber isn't a mature library yet and it can fall over in unexpected ways. 
* Debugging - be aware any cross-compilation will make it harder to debug.

### Discussions about the relevance of cucumber
[An interview with the developer of cucumber](http://www.infoq.com/news/2014/04/cucumberjs-bdd-biezemans)

## Lesson's learned
* QA's support cannot be overestimated
* The async problem in NodeJS is non-trivial. Know how to solve it before going further. 

