---
title: So, what is quality anyway?
date: 2024-12-03 11:22:33 +1000
categories: [Quality Engineering]
tags: [quality-engineering]     # TAG names should always be lowercase
---


![post avatar](/assets/avatar_1.png){: width="200" height="100" .left}

## Quality is a process, not a task

Quality is a list of tasks and processes that come together to get us what we want - good software!

What it means to us changes from person to person, but what it means to a business never does - the outcome needs to be solid. In the same way we can't build a castle in the clouds and be confident in the castle staying up there, we can't build software in the cloud without some serious condifence that what we've buitl is not coming crashing down on us in the first storm. 

We have a few different terms that apply here, speaking of the storm. One is *happy path* - the software works as it should, when the users do what they should. This is the easiest path to test, it's literally just following the software in what it asks you to do, and you do it as compentently as possible.
The other more fun one is the negative path - on this testing we actually put the software through it's paces in a real world situation - against a user who does not follow the rules exactly, is looking for shortcuts and efficiency savings, is misunderstanding the workflow (either purposely, or more likely due to a lack of training, especially important in the first few weeks once the software is released) or is trying to just not do it the way they were told. I say this is the more fun, as it involves the tester taking on the role of some pretty fun characters - I myself have a few extra DnD character sheets that come out here, containing some of these characters:
- Glenda the boomer - Glenda is not great at software, she was brought up in the age of punch cards, fax machines and delivering stacks of reports to the typists. While there's nothing wrong with the way Glenda works, she is not looking for the "Save" button to change from greyed out to available once she has filled out the form, she is looking at the greyed out button and doesn't like it one bit. She needs information to be readily apparent, and for ckear headings and secitons to split up the fields. She may not think to scroll down at the end of a screen, she doesn't use Tab key to get to the next field, she is an expert in finding UI/UX bugs - if it's not clear to Glenda, then it fails the initial scan of being user friendly and efficient. Information that is needed shouldn't load in at the bottom of the page, if you're a bank, the final balance should be there at the top of the page, along with any overdue fees or impotrant dates that may affect them. 
- Next is Kyle - he's a digital native, he likes to think he's a guru on the system, and somethimes, he is. He tries to Tab through the fields, autofills or pastes data into the form, tries to Ctrl-Z on mistakes subconciously and also has multiple windows open on the same record - this guy is trying to fulfil the role of power user. He finds problems with unprotected fields, record confliects and even keyvoard shortcuts missing or otherwise creating havoc.
- One other one here is Pedro - Pedro doesn't work for us, but he sure wants to access our systems. He tries to run console commands in the  browser, has a Python/Selenium bot that tries passwords, and tries to set up all his users with the last name "DROP TABLE *" 

