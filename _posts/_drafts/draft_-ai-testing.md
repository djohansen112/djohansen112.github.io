---
title: Artifical Intelligence and Testing
date: 2024-12-27 11:22:33 +1000
categories: [Test Tools]
tags: [quality-engineering, postman, tools]     # TAG names should always be lowercase
---

# AI and Testing

Artificial Intelligence has gotten a lot of new coverage lately, with the rise of ChatGPT and the multitude of societal shifts any new technology produces being even more pronounced due to the broad ramifications of this technology.
We have gone from computers being a of reactive tool in the public eye to something creative and an active tool that does not require the kind of input that traditional tools had..

AI testing using TDD princicples would mean that the AI can be used in the same kind of training loop where it reinforces correct behaviours by running agains tsets of acceptance tests. This would allow machines to know when their code is correct, and we would only need acceptance testing to finally sign off on the resulting code.

We can consider AI as a form of compiler - they take our natural language and convert it to computer processes that return software outputs.

The fundamentals of programming - what are they

AI returns the problem to us - define the problem

Reproduceability is a problem, AI is not always going to run the same program, like a compiler would. There is randomness, model updates, imprecise prompting that would create problems that would produce a small chance that the program does not compile the same way in two different runs.

Acceptance tests, from a users perspective allow us to define a set of gates that thge AI must pass to have successfully compelte the scenario. Once the code it created, we can start using that code to create the program in a solid model language, like Python/Java, something more precise.

Testing ensures reproduceability, precicison of the reutrn, and we are now doing something familiar - BDD. we are defining the behaviour that the program nees to do

This is no different to lowcode and no-code programming platforms

What are 4GLS - what are 4 layer models

AI and wish - monkey's paw - giving examples for the computer to acheive

This is defning the problem rather than solving the soultion. 

Taken from Dave @ Continuous Delivery youtube channel.

Implementing - create the tests, ensure the AI understands and obeys the test, then feed and ensure that the reproduceability works. The key here is be hands off - let the AI do the mistakes and correct them in the specification, not the output. This way we can go compeltely hands off.

This does create fragile tests, but that is for the next generation of AI - clean code implementation, production level code bot (applying SOLID across entire system and reducing compelxity and fragility) and creating wider infrasturcture and arch to support these, sucha s a testing suite, API helpers, other toolpack items.
These would also solve the ideas of AI cheating the tests, just creating a single "returns Test(passed)"


Testers are teachers, we see your child in the world and correct them to ensur ethey work.
