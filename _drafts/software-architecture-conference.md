µ-services
--------------
@ahhogendoorn
Stairways to heaven or highway to hell?

Disadvantages:
µ-services; Components too interconnected. Hard to change. Legacy prone systems.

Story from CORBA -> SOA -> µ
Where are we in the (Gartner hype cycle)[http://www.gartner.com/technology/research/methodologies/hype-cycle.jsp]?
(He added beware of the articles that praise a specific tech with no criticism. Now we start seeing some critics around the µ services)

Shown the microservices definition by Martin Fowler (enphise on being "a" definition not "the" one)
What micro promises:

* Easy to build.
* Scalable
* Easy the test

but:

* Integrations. Different environments.
* You have problems on monoliths? Micros will not solve your problems.

"Brownfields" look easier to move to microservice but some people argue that if your goal are microservices you might go straight to it.
Sometimes the system is that complex you'll have to rewrite to go microservice.

Another requirement of the µ is that there's no such a thing as a final architecture, no throwing it over the wall, it evolves.
Guiding principles:
* Brownfields is about pulling things out; look for loose connections for the pulling while keeping the strong in the same context.
* Modeling your services is about giving the right interpretation of the resource; "QA/", "QA/QUESTIONARIES/" "QA/QUESTIONARIES/5234"
* REST is not that easy, be careful when you build a REST endpoint.
* Testing your services; if you fail, fail fast as it's better for the big landscape. "Which service has failed?"
* Integration is hard, automate everything (testing)

Deploying services:
The guy went from Jenkins > TeamCity -> GOCD -> Bamboo -> Jenkins.
Not easy because every microservice will end up having its own deployment pipeline. What he found is that Jenkins has some plugins like creating templates that allowed
him to c&p jobs between µ.

µ follows the "hockey stick model", you don't see the benefits immediately, it takes a while to kick off.

Modular monoliths
---------------------
@simonbrown

We usually go to the extremes. Full µ as opposed to a big monolithical pile of soft.
We rely on abstractions but we lack common vocabulary to communicate them. Does you code reflect the abstractions? (Abstractions as diagrams)

** Layered architecture (horizontal slicing)
Public classes can be called from everywhere in the same layer so it can end up being a mess.

** Hex. architecture/Onion.
It's a type of layered arch. Only works in small programs and has the same problem, conceptually classes in the same layer can mess up with each other

(Recommended the book "Just enough architecture")

** Layered architecture (vertical slicing)
Context diagram  (zoom out, level1) -> container diagram (describes the types of technologies we're using) (level 2) -> Components diagram (level 3, detailed component) -> classes diagram (level 4)

In the jump from level 3 to level 4 the component cease to exist as a single entity it's a combination of interfaces and classes.

He proposes a "architectural-evident code style": focus on what matches the architectural components; a one-to-one relationship, leaving "hints" in the code that reflect the architecture is based on.

(class diagram)

Two components, one behind a DAO to one being a single entity with private classes, so the one-to-two becomes a one to one.

Modularity principles: It's not easy but some design choices can help you.
But then, how do you unit test the one-to-one component? (private classes that cannot exist independently) Well, you don't! You test it as a sole component, spin a new mongodb and test it end-to-end.
But hold on fella, this works because you own the stack what happens if for example the data layer is a 3rd party? Back to one-to-two model.

Now he challenge the concept of unit. What is it? Aren't we doing too much unit test? What is an integration test Comes with the concept of "architecturally aligned testing"
Component test as opposed to class test? Software arch. code and test as connected and related.

Right, so were to start from? Stop making public classes. (good luck with that plan)

Design by code
----------------------

Allen I. Holub; holub.com/slides; @allenholub

Proposes DbC; based on TDD, understating TDD as a design practice.
(This guy does test first, not tdd)
A user story touches every part of your system.
Defines what TDD is: enforces the refactor <-> testing cycle with a "make it run and make it fast" moto.
His approach drinks from the BDD book by Dan North is which we define behaviours, not state and the scenarios are testables by themselves.
(Goes on a SCRUMM is not agile rant saying that it's too restricted as well, claims as well that agile has to involve a cross-functional team)

Defines some general principles on testing:
* If anything is blocking you from coding, simulate it (mock it)
* Test should be abstract and therefore decoupled from you code (you should be able to replace a class implementation without impacting your clients/tests) otherwise you end up falling into a faulty abstraction.
* Don't assume implementation
* Avoid get/set
* Do not change the object and test at the same time, always keep one stable.
* Follow the law of Demeter (or train wreck) dog.getBody().getTail().move(); It can be ok if the each of the methods return the 'dog' object
* Clean up: start each test from a clean state.

DbC; design focused on stories;
