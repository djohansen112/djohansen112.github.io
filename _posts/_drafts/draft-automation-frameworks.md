---
title: Automation frameworks from the ground up
date: 2025-03-18 11:22:33 +1000
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

So this article is going to be broken up as per my response from a ChatGPT clone I have access to. Buckle up, let's learn frameworks!







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
#### Determining the tools and technologies to be used.
#### Understanding the application under test (AUT).
#### Choosing the Right Tools and Technologies

### Criteria for selecting automation tools.
#### Overview of popular tools (e.g., Selenium, Appium, TestNG, JUnit, Cucumber).
#### Considerations for language and platform compatibility.
#### Designing the Framework Architecture

#### Structuring the framework for scalability and maintainability.
#### Layered architecture (test scripts, libraries, utilities).
#### Integrating with CI/CD pipelines.

### Setting Up the Environment

#### Configuring development and test environments.
#### Managing dependencies and version control.
#### Setting up test data and databases.

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