---
title: Automation frameworks from the ground up
date: 2025-03-16 11:22:33 +1000
categories: [Test Automation]
tags: [development, tools]     # TAG names should always be lowercase
---

# Automation Frameworks
## Starting from scratch

OK, here we go. This is all my opinion, and in no way should be taken as the bible for step by step "how to" when it comes to automation frameworks, but if I can start putting down what I know, then the rest will follow. 
There's an adage in English, that:
> Those that can't do, teach

and it sometimes gets interpreted as some kind of put-down for teachers, as if they couldn't do the work themselves, even though they were selected to teach others how. I posit a different viewpoint on this, at least for the purposes of this article!
> If you cannot practice your skills, teach them to others

How about that? How about, instead of actually building a framework from scratch and learning through hundreds of hours of punching through code to get various elements working, we start by learning what elements we should be considering before we start?

Sounds good? Well, that's a relief, otherwise the view count on this puppy is gonna be an all time low.

As is my usual wont, I have consulted the AI overlords for an overview on what to write. Now, while I don't subscribe to the Ctrl-C, Ctrl-V editorial styles of Medium or various other software testing sites, there's nothing wrong with consulting with a search bot that has specifically been trained to give summary and overview on diverse topics.

So this article is going to be broken up as per my response from a ChatGPT clone I have access to. Buckle up, let's talk about frameworks!

### Introduction to Test Automation Frameworks
#### Definition and purpose of a test automation framework.

So what is a test automation framework? Put simply, it is just a layer between us testers, and the code under test that allows us to run the code in whatever way we want, rather than having to manually work with it. Think of it as bolting on a whole new set of handles to the code that we can individually grab on to when we need to steer it one way or the other.
The automation part of that framework allows an unsupervised process to run our tests and examine the results entirely without our input. Usually we do this remotely on a cloud service, or in some kind of automated CI/CD pipeline so that a set of selected tests will run everytime a new deployment is sent to the Production server and pre-emptively catch possible defects before the deployment is completed and our codebase suddenly doesn't work as intended.
We can also use these locally on a developer's computer, so that whenever new code is deployed to the main branch, it is run locally. This provides another level of protection against accidental defects where the new code we deploy has an unforeseen downstream failure.

#### Benefits of using a framework in software testing.

As discussed above, using a framework allows us not only to write endlessly re-runnable tests that require little to no input from users, but they can also serve to create server load, to make testing data, and to reproduce defects from Production quicker than most manual processes would be able to.
The framework allows us to also build more and more functionality onto our testing, so we can cover more of the software our team produces. We can write various functions throughout our program that perform little checks here and there, but as per the SOLID principles, why create the same class twice? If we need to test a user class method, and a data object method, why not have a method runner class that could run both of these, plus take screenshots during the process! Sounds like a deal to me!

### Types of Test Automation Frameworks

Although there is some hierarchical preference to the below framework designs, care should be taken not to try and always just apply the most complex, most comprehensive solution at each stage. Selecting the right framework design is crucial in ensuring speed of development, team velocity, and eventual coverage of requirements. Most client engagements with some test automation work involved will require multiple iterations of these, as well as custom combinations of each framework applied to each system under test.
Always remember, the goal is not to over-engineer some flashy tech stack that costs a bunch of money and maintenance budget to work with; we are here to create efficient resource usage and business value through quality engineering. Use the right tool for the job!

#### Linear (Record and Playback)

The linear framework is the most analogous to manual, real-person testing. We tell the software to perform tests as if a person was sitting at the computer. Often, these are captured directly from the user performing the test by a recorder software and then re-run when necessary. The recorded software captures the keystrokes, mouse movements and any other user inputs and simply plays them back again to the system to give the same outcome as if our person was actually there.
Usually this type of testing is reserved for End to End tests, as it most closely replicates real user input, but can struggle with complex situations and is rarely able to cope with changes to code or system layout and requires constant re-recording to maintain.

#### Modular

Modular testing frameworks break the system up into modules or components, and run the tests against each of these components. Generally this is a preferred approach for System Testing, as the tests can be closely coupled with the component, so only require maintenance alongside code changes within the component. Depending on the component boundaries, this may also include some System Integration testing, although where possible, this approach is generally aimed at much smaller test coverage in much tighter context. Modular testing is preferred as part of SOLID principles, as it allows good re-useability as well as tight control over exactly what system behaviours our test is trying to replicate.

#### Data-Driven

Although this is not a framework on it's own, this is a considered approach to testing that should be used in conjunction with other concepts to create great frameworks. Data driven testing frameworks are all about running the same tests multiple times with different data, and ensuring the results are exactly what we expect. Consider a log in page - there's very little to test here in terms of buttons and fields, but we can run literal hundreds of combinations of clicks and field values through here to ensure our login process is well-tested. Data driven testing would ensure that we try a blank email, 1000 character email address, email without the @ sign, blank password, clicking login without anything in the field, too many retries with invalid user data etc... This is generally a core part of every framework - that user input matters, not just the logic of the program itself. Data driven tests are simple to maintain, but complex to design, as care must be taken to ensure that the data is producing the right behaviours, and our expectations for each action that the test takes should be well defined.

#### Keyword-Driven

Keyword driven testing in a similar vein to Data driven, is a way of setting out testing action so as to maximise the re-usability of the underlying code. Keywords are linked to actions that the test framework can take, as well as any data required. Generally, keyword driven testing relies on tabulated steps to perform testing such as the following:

| Step  | Keyword   | Object    | Data  |
|---    |  ---  |---|---|
| 1     | openPage | loginPage  | test.server.com/login |
| 2     | enterData | loginPage.username  | test.user |
| 3     | enterData | loginPage.password  | Test@123 |
| 4     | clickButton | loginPage.submit  | N/A |

So this illustrates a set of reusable keywords that would map directly to functions within the test framework, focussing on re-usable keywords that enable users to write tests simply by implementing tables with associated keywords and data. While the example above tries to show reused code in terms of the loginPage having an object defined and the enterData being a re-used method for both the username and password, some keywords can be used in a more literal sense. More complex keyword testing can pick up the data from sentence descriptions, but these frameworks generally will use advanced parsing with AI and existing frameworks such as Robot framework to reliably find the keywords and interpret the action required.
The same test with a more advanced framework may look something like this:

| Step  | Test Action   |
|---    |  ---  |
| 1     | test user **opens** the **login page** |
| 2     | test user **enters** the **username** "**test.user**" |
| 3     | test user **enters** the **password** "**Test@123**" |
| 4     | test user **clicks** **submit** button |

I have highlighted the key words within the actions to emphasise where the keywords may be interpreted by the parsing function. By reading just the highlighted words and values, even as a human reader we can see the intention of the test and replicate the actions that the automation framework will perform.

#### Hybrid

As with all things, hybrid is the one constant in all of testing. We have our bucket of expertise, and now we have a bucket of test framework components that we can use to perform our testing. Hybrid test frameworks often deal with different systems under test, such as a test which automates a customer portal and checks the result in a backend service which would require UI based actions followed by API testing actions. 
Even within the same system boundary we can apply the concepts already covered. A keyword test can be run multiple times with different data, and each keyword triggers a single component testing run or a recorded test. With a sufficiently capable test framework, how we organise and run our testing becomes much more flexible and our ability to create new tests quickly and efficiently also improves.

#### Behavior-Driven Development (BDD)

Behaviour drvien development involves the use of a different approach to user testing entirely. BDD uses a set of user actions and expectations to define how the system should behave, and we write our tests against those expected behaviours. BDD is most often coupled with Cucumber tests, which use the Given-When-Then format and Gherkin language to define test cases.

```gherkin
Given an admin user is logged in
When the user clicks their profile information
Then the profile also includes the <delete user> option
```

In this test, each step can be automated, similar to a keyword driven test, and parameters (like "delete user", which may also be substituted for "update user" or "create group" etc.) can also be passed to the system under test, similar to a data driven framework. 
In my opinion, where Agile methodology is preferred, BDD is generally a superior framework to build towards as it encompasses the best approaches to each component under test as well as provides a human readable format so we can quickly create new tests, even if we have non-technical users writing those tests.
BDD emphasises user and system behaviours, allowing us to write user stories using the Given-When-Then format which can almost directly be translated to test framework actions.

So that's all for the types of frameworks we need to discuss. As you can see, depending on the complexity of the system under test, the skills of the testing team, and the technical requirements of the testing, we can apply each of these frameworks independently or together to get the best outcome for our team. We're just adding more tools to our toolchest, and ensuring that when we make recommendations around testing, we are considering as many testing tools and methodologies as possible.

### Planning and Requirements Gathering
#### Identifying the scope and objectives of automation.

When we first look at automation as a project, we need to ask ourselves some questions:
- What should be automated?
- What is the goal of choosing automation as a strategy?
These are maybe obvious, but it is important to write out the answers to the "What?" so as to ensure that further questions around "How?" will be better understood.

#### Determining the tools and technologies to be used.

If you've chosen automation, which is maybe why you're here, then one of the most confusing questions is going to be what tooling to choose. There is an absolute plethora of options in all kinds of languages and frameworks, at all price points and with varying levels of complexity and capability.
Planning sessions for automation tool selection should include not only test stakeholders, but developers, architects and even third party representatives to narrow down and assist during various phases in the decision making process. Often, testing and quality specialists can be engaged during this process as part of client consultation; even if they are not part of the continuing automation project they can be a valuable resource.

#### Understanding the application under test (AUT).

If your application under test is a browser based service, have you considered backend testing, integration testing with third party tools that may have been integrated during development, failover or disaster test scenarios? Even the simplest app may require more test than you think. It is crucial to understand not only the app, but the bounded context in which that app operates and how it connects to any services it requires.
During this discovery phase, it is helpful to begin gathering detailed documentation as it becomes available for future reference by those working on the project during delivery phases.

#### Choosing the Right Tools and Technologies

Some choices can be made for you, in terms of the language preference of the client, compatability of the test architecture with the client's current tech stack, as well as ability to perform the testing required. Often, when we start automation, we are building for a future team to maintain, whether that's our own team or you are working on a client engagement. It is always preferred to keep the same, or at least a familiar techn stack, so that the internal client team is able to work on the testing framework during the maintenance phase. 
A further consideration is alignment to the goals of testing automation. Sometimes we have identified integration/API testing as our automation target, sometimes it will just be browser based testing (UI testing, or user interface testing is often used interchangeably here to denote playback or browser automation tests). Different frameworks even specialise in different platforms, such as Salesforce or Workday specialist testing packages.

#### Structuring the framework for scalability and maintainability.

One key consideration when designing a framework is scalability and maintainability. Automation is a maturity step for most organisations, so ensuring that the automation and test frameworks keep up with future growth and is essential.
Being able to produce a scalable test framework allows us to run tests on the current tech stack, as well as any future changes to that stack or growth of the stack. Building a scalable framework is not just about running the current app with more tests, it's about the ability to integrate future software packages and swapping out current packages as necessary in the future. 
Being able to build in maintainability is essential to future-proofing our app, and a robust test framework that is built to support maintainability will reduce technical debt and effort later on in the app's life.
Scalability and maintainablity are not only essential for supporting our app, but for supporting overall company growth goals.

#### Layered architecture (test scripts, libraries, utilities).

When designing and laying out the framework, it is par for the course that the framework will eventually scale out to eclipse the software under test. Testing frameworks can be an order of magnitude larger than the system under test because they may contain multiple test scripts and functions for each feature of the SUT, as well as test data support and utilities.
Along the way, it is important that when an opportunity arises to modularise and abstract a function of the test suite, we take it. The ability to create utilities and tooling that support the growth of the framework, and early integration of these tools and utilities can support SOLID principles during development of the framework and simplify coding early on which reduces refactoring effort throughout the project.

#### Integrating with CI/CD pipelines.

Another essential principle of automation is integration with or development of a DevOps team. This team is usually the end of the process when it comes to deployment, and can assist with running our tests on any code deployed to our app as closely as possible to a Production-like environment. 
Early integration of our testing suite to existing CI/CD pipelines helps to create value as early as possible in the project lifecycle. If no existing pipelines are available, setting these up will also allow our testing framework to scale as the framework and app grows, as well as reach maturity early on. Maturity in this case refers mainly to the capability of the framework to perform the tests with necessary integrations, utilities and functionality rather than test coverage. Coverage is separated from maturity in this case to demonstrate the idea of scalability - if we already have the tools to perform a range of tests due to high maturity, then adding test scripts will increase coverage. Having the pipeline to deployment in place allows us to focus again on test coverage of the SUT, rather than building the framework, so a balancing act is sometimes necessary to ensure our teams are focussed on the right things. 

### Setting Up the Environment
#### Configuring development and test environments.

Got your design? Got your tooling stack selected and vendors engaged where necessary? Got the budget and the people in place? Let's get started!
One of the first things to work out is a suitable test environment. Depending on the SUT this may require a sandbox version of our app, integrations with any third party services or apps (or test versions of these) as well as any mocking services required to simulate the Production environment as closely as possible. While our storage and speed requirements are much lower than Production, it is important that the test environment is stable, and strict controls are implemented so that environment issues do not interfere with our testing.
Equally important is setting up our development environment. Our development environment will contain all our framework development, and depending on the size of the team, may scale up and down in the future depending on team needs. Version control systems, IDE selection, credentials management and documentation stores should be decided and engaged at this stage as well as any work management practices to implement (tools like Jira or Trello, Slack or manual work tracking practices) so work can be planned and executed.

#### Managing dependencies and version control.

As with any software project, version and dependency control practices apply. Implementing a Git flow, selecting package management tools (npm, pip, etc. depending on the situation) and deciding on an approach to future growth of these tools will accelerate success in the team.

#### Setting up test data and databases.

Not just a consideration in data driven frameworks, the quality and control of test data should be planned so that test data most closely represents our Production environment. 
Test automation is not always a parallel development activity, sometimes a reasonably mature app may become a candidate for automation late in it's development cycle, or even once it is completed. In these cases, test data simply reflects existing schema and expected values where possible so as to support testing. In some cases, automation happens during development, with feature tests becoming candidates for automation soon after or even during development and deployment activities.
In either case, the need for stable and accurate test data requires careful planning. Depending on the SUT, test data may even need to be synthesised by third party vendors to reflect possible inputs, or synthesised in house so as to support testing of various features within the test scope.
In some cases, test data can be drawn from real Production databases where the app is already running, in which case care must be taken to ensure any sensitive data or PII (personally identifiable information - that data which may be used to personally identify someone) is masked or obfuscated so as to maintain customer/client confidentiality and safety.

### Developing the Core Components
#### Creating reusable components and libraries.

#### Implementing test scripts and test cases.

#### Handling test data and parameterization.

### Implementing Best Practices
#### Writing clean and maintainable code.

#### Using design patterns (e.g., Page Object Model).

#### Ensuring cross-browser and cross-platform compatibility.

### Integrating with CI/CD and DevOps
#### Setting up continuous integration and continuous testing.

#### Automating test execution and reporting.

#### Using tools like Jenkins, GitLab CI, or Azure DevOps.

### Managing Test Data and Environments
#### Strategies for test data management.

#### Using mock services and virtualization.

### Handling Challenges and Limitations
#### Common pitfalls and how to avoid them.

#### Dealing with flaky tests and test maintenance.

### Monitoring and Reporting
#### Implementing logging and reporting mechanisms.

#### Analyzing test results and metrics.

### Continuous Improvement and Maintenance
#### Updating the framework for new requirements.

#### Refactoring and optimizing test scripts.

#### Gathering feedback and iterating on the framework.

## Conclusion
#### Recap of key points.

#### Encouragement to start building and iterating on their own frameworks.