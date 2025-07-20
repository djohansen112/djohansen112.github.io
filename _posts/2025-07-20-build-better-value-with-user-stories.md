---
title: Automation frameworks from the ground up
date: 2025-07-20 11:22:33 +1000
categories: [Quality Engineering]
tags: [development, business-analysis, business-value]     # TAG names should always be lowercase
---

# Building Better Business Behaviours
## User stories and INVESTing in value based development

Oh man, am I sick of reiterating this point...

Better software starts with better stories

At least with Agile (or the cherry picked hybrid Agile that the world is converging on...) writing user stories is pretty much always going to be the best way to specify requirements for our software. 

Good user stories are absolutely inherent in any improvement to a software development team at any level.
Product Owners need user stories to be descriptive and straight forward, with clear traces back to the value proposition of "why" we need to invest money in this software.
Business Analysts need good user stories so they can make sure they are fulfilling the finer details of what the Product Owner needs.
Developers need good user stories so they can see the problem, not someone else's solution, and start working on the best way to solve that problem.
Testers need good user stories so we can make sure we have solved the problem, and not created any new gremlins in the process.

At it's simplest, a user story is a short few lines of Who, What, Why - usually this is presented in the 
> As a ...
> I want to ...
> So that I can ...

#### Who needs this?
Who needs the change? This is sometimes the hardest part, defining who needs this feature. Sometimes the temptation is to just go scattershot and say "As a user", and sometimes, yeah, that's who needs it! Specifying the user by role or intention is always preferred.

#### What do I want?
What is the action that I, the user, need to do? This is the easy part - at a basic level, if we're creating a new feature, we know what we intend the feature to do. "I want to log in", "I want to have a new button to create a new record", "I want to be able to delete a user record". The Product Owner usually knows exactly what they want, and specifying who and why are what create the specificity and value proposition that will get us across the line.

#### Why do I need this?
Why do I need the new button? So I can delete a user record, dummy! Creating the Why part with business value in mind unlocks the functionality of the feature - it goes from just "I want to create a button" to "I want to make sure that my customers details are printed quickly". Adding business value to the Why section allows the developers to see the intention, the Product Owner to sell the feature to the stakeholder team, and the testers to understand the scope of the change.

### The good, the bad and the ugly...

### Good
Nah, tricked you. Let's start with the Bad...

### The Bad
Here we go. Here's some examples of bad user stories:
> Create a new field called Project Owner on the Summary form. 

Where on the form? What does it do? Is it text or a dropdown?
> Change the colour of the heading to blue

Sure thing boss, any specific kinda blue? Which heading? Across the entire site? Blue background? Do I change the background?

> As a user
> I want to see a button for the home page
> So that I can press it

OK wiseguy, I see what you did there. That's our formula for a good user story, but you made a mistake. What kind of user? All of them? Where does the button live. Can you click it and it does something? "So that I can press it" - cool, so we're making a fidget spinner?

I'm calling this last one the ugly - it's the most common kind of story, and to be honest, it usually indicates someone's trying and have a basic level of understanding, but they're not at the point of making the user story great, just barely passable:

### The Ugly
```
As a system administrator
I want to click a button called Delete
So that I can delete a user
```
So the form is here. It looks good actually, but it lacks the depth we need. Where is the button, can you delete m ultiple users? Are they gone for good? Why would you want to delete a user?
Even though that last question seems like it's a no-brainer - we're talking stakeholder teams here, they don't know the system back to front, inside out, they just know they're paying $50k to have someone add a button.

So, how do we fix that? Do we create user stories that are a page long? Please no. Oh my sweet baby Jeeeeezus, no.

We use Acceptance Criteria (or Acceptance Conditions)

## Acceptance Criteria
Acceptance Criteria are a set of rules for our new user story so we can make sure it works the way we want it to. Let's re-work that story above:
```
As a system administrator
I want to click a button called Delete
So that I can delete a user
```
OK, so as the BA here, what can we do to make sure we know what we're getting?
Let's add some Acceptance Criteria:
```
AC1: The system administrator is the only one who can delete a user
AC2: The Delete button should be in the top menu to the right, next to the "Add User" button
AC3: The Delete button should have a confirmation dialog that says "Are you sure?" or consistent across the system
AC4: When the user is deleted the page should refresh to update the change
````
Now we're cooking - We know what we're building as developers, we know how to implement a button, we just need to put it up there next to the Add User button. 
Suddenly, we're creating a kind of To Do list to pass the story as well, so as testers, I can now start creating a test to make sure the button is in the top next to Add User, that it correctly deletes the user and refreshes the page, that the other roles in the system can't see the button to delete users.

It feels like repeating ourselves though, so let's see what we can do with the user story:
```
As a system administrator
```
This is fine, we have given a role. The role corresponds to a user role in the system, so we can easily see who is getting the change.
```
I want to click a button called Delete
```
Don't we all want to click a button? How about we fix it with "I want to have access to a button to delete users" - now we have an action, what's written on the button can live down in the Acceptance Criteria as a list of rules. We just care about what the button does - the function of the software.
```
So that I can delete a user
```
Sure, that's why you want it, but let's make it relate to the business: "So that I can remove inactive users" makes more sense, no business wants endless lists of inactive users. With this new button, we're adding an ability to clean up our system, to run efficiency, all the buzz words that stakeholders love to see.

Final version:
```
As a system administrator,
I want to have access to a button to delete users
So that I can remove inactive users from our system
```
Better - it's clean, clear, and with a set of good Acceptance Criteria, it allows for easily deliverable and testable features.

### The Good
Nope, not yet. We're better, but we're not quite at good. Let's understand a widespread framework that we use to create these user stories first.

### INVEST

INVEST is a serendipitous acronym, it's nice when acronyms just kinda make sense. Better than WYSIWYG anyway...

| Letter |	Principle |	Why It Matters |
|---|---|---|
| I | Independent | Story should stand alone and not depend on other stories to be completed. |
| N | Negotiable |	It’s not a contract — it’s a conversation starter. |
| V | Valuable  | Should clearly deliver value to a user or stakeholder. |
| E	| Estimable	| Team must be able to size it with confidence.|
| S	| Small	    | Should fit comfortably in a sprint.|
| T | Testable  |Must have clear conditions to determine if it’s complete.|

Using this as a benchmark to critique our stories does help - can we make it simpler (Small), can we decouple it so it's not stuck in a huge release bundle of interconnected features (Independent), can we do this a better way (Negotiable) and maybe have a conversation about creating an admin panel of buttons, or using links instead?

Maybe the most important here is Negotiable - Agile is all about creating discussion: people over process, creating spaces for understanding and agreement before we create work.
> The most efficient and effective method of
conveying information to and within a development
team is face-to-face conversation.  (agilemanifesto.org/principles)

Cutting down the story and having the conversation early helps to define the best way to solve the problem and create value.

Using INVEST affords us the ability to let BA understand the problem, the developers solve the problem and the testers check that it has solved the problem, rather than "have we made a button?". It's extremely valuable to be able to focus on problems and solutions, that's what we should be here for after all. 


### The Three Amigos
Whut? This seems off topic...
Nope, wrong again dear reader - right on topic, as always. 

The Three Amigos are:
- The Product Owner (or Business Analyst)
- The Developer
- The Tester 

![post avatar](/assets/three_amigos.png){: width="200" height="100" .center}

They are a trio of problem solvers who work *collaboratively* to develop solutions. The PO knows what they want, the devs know how to do it, and the testers make sure it does it right. 
By approaching the problem in a backlog design meeting where the three can speak and work together, the feature can be discussed and optimised. The developer can suggest using links instead of buttons like they do in other places on the site, the tester can ask about what should happen with the deleted user when they try to log in again, the product owner can make the decisions with this feedback and we can all work together to create something that meets INVEST and creates value for the business.

## The Good?
OK, now we can show the good.

Not an example, I think that the reader can now see how we've been able to start thinking differently about how we approach user stories to better create value. Using the framework, acceptance criteria, proofreading with INVEST in mind - that's what makes a good user story. There's never going to be a one-size-fits-all approach. Sorry to disappoint!

Maybe let's look at what the outcomes are that make better user stories such a good thing:
- Developers are doing what they do best - applying system features to the problem at hand
- Product Owners are doing what they do best - creating new features for their product, increasing it's value through extending functionality, streamlining the process, all without having to think about how they do it - that's the developer's job after all.
- Testers are getting a clear, concise, testable product that they can clearly work with. If it meets the acceptance criteria, then it passes and the users get what they want!

Here's the flipside - by defining a good user story and respecting our roles in Agile, we each have responsibilities inherent that mean we just get along better - the product owner is not trying to get a button when the developer thinks a link would be better, the tester is not passing tests that miss the intention of the feature or change, the devloper is working on a problem they have helped to define, so they have a sense of direction and purpose rather than flipping through a list of unclear requirements. We're all succeeding at our jobs in our own ways.

See, told you we would get to the good. Eventually. Took a bit of work, but understanding how to craft good user stories is absolutely worth INVESTing time into. :smirk: