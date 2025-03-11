---
title: Imperative vs Declarative
date: 2025-03-12 11:22:33 +1000
categories: [Test Methodology]
tags: [quality-engineering, testing, authoring]     # TAG names should always be lowercase
---

# Imperative vs Declarative
## Testing and programming language concepts 

When writing tests, our most basic test method is a cause and effect relationship:
>**Action**: What the user does\
>**Result**: What the user expects to happen

When writing the test method, there are some approaches to consider. We use the active voice when writing Action steps (The user logs into...), and the passive/past voice when writing the Result steps (The user is logged in...) as well as any extra information that may be relevant either to the test outcome, or as a marker of success/progress.

There's a few different ways of writing these Action steps, and I use one or the other depending on the situation that the test applies to. Of course, everything is a spectrum, and there's so many skills we use everyday in conjunction with other skills that it's practically ALL a mishmash of assorted skills and methodology lumped into a big bucket labelled "Expertise". What we put into that bucket defines our approach, and for this purpose, I wanted to outline a little of why I put one or the other of these skills in the bucket.

"Declarative" in programming is used to outline the logic of a program that will perform a task according to the rules of the system, and not every step of that system requires input. HTML/CSS is a great example of this. Consider the following code:
```html
<h1>New Shop Items Available!</h1>
<h3>Hurry now, before they're all gone</h3>
<p>Our <strong>latest</strong> styles are here and ready for your shopping cart!
```
We haven't told the program how to display our Heading 1 text, we've just told it that we want this in Heading 1 style, and the next line in Heading 3, then a block of text with a bold word in there. How the browser displays this is dependent on the browser, as well as the user selected fonts, screen assistance software overlays, the size of the screen, etc. etc. There's practically no clear way to know exactly what the final product is going to be rendered as! We trust that the complex system of human agreement (browsers all decide that they will prioritise a 18pt for H1 tags, or just a 4pt seperation between heading levels), browser compatability, and the HTML ecosystem as a whole (paradigms, standards, best practices, etc.) will allow us to visualise what will be rendered so our design is reproduced when various users access our site.
CSS allows us to further hone in on our desired end product in terms of standardised colours, setting text sizes, fonts, colours, styles, even animations and transitions. It turns our HTML into a much more "Imperative" program to run - we specify each step and paramter of the program so it runs as closely to our design as possible.
Imperative programming specifies exactly what is to happen - a program for calculating gravity acting on an object will ALWAYS need the mass of the object, resolved external forces acting on it as well as the gravitational constant. We can't perform an accurate calculation unless we know the entire parameter set, so we would need to use an imperative approach. 

TL;DR:
- Declarative: You let the computer figure out how to do the task
- Imperative: You tell the computer exactly how to complete the task

## Declarative 
Applying these approaches to testing is fairly straightforward. If Declarative is letting the system take care of the details, and Imperative is telling the system exactly all the details and processes, then applying these concepts to how we write our testing becomes a simple filtering process.
Consider the below requirement:
```text
When a user logs into the system they should see a green icon in the top corner with their user initials in it
```
Declaratively that would be:
```gherkin
Given a user exists
And the user has a set of initials in their username
When the user logs in successfully
Then a green icon is displayed in the top corner
And the icon contains their initials according to the username
| Example |
|   username        | |     initials    |
|   Danny Carey     | |     DC          |
|   Steve McQueen   | |     SM          |
|   Test Admin      | |     TA          |
```
So what makes this declarative? Let's do a quick list:
- we haven't told them who to log in as
- we haven't described where in the top left, or right, just the "top corner", as per the requirement
- we haven't specified the colour of the icon, other than "green"

## Imperative
Let's try that again, but with an imperative approach
```gherkin
Given the test admin user exists on the Salesforce system
And the test admin user has the name "Admin" has a set of initials in their username
When the user logs in to "http://test.salesforce.com" successfully with the Test Admin user
Then a circle icon is displayed in the top left corner
And the icon has the colour code #008000 (HTML green)
And the icon contains their initials according to the username
|   username        | |     initials    |
|   Test Admin      | |     TA          |
```
So now the test is much more prescriptive, we've told them every detail of what to do to perform the test.


## OK, so now what?

Well, I suppose now you've learned something, dear reader. Let's see where we would pop one or the other out of your bucket of expertise.

Say we had a new banking system that creates a business loan for our customers. On that system, our developers have worked their magic, and now we have a page that allows our banker users to update the customer's bank account that they use to pay off that loan.

If that is a new page, and nobody knows how to get there, then it would make sense to author this test using an imperative approach (log in, then click here, then click here, then in the bottom right there's a list so click there...) but if that is a new page in an old system, then that can be much more declarative (open up a loan to the Bank details page, click the new link in the bottom right). It really depends on the system under test, the user/tester familiarity or even any concurrent updates (the new page comes in the same release as a page layout update).

In some cases, imperative tests are preferred, as these can be easily translated into work instructions for the end users, or in the case of a new or relatively unfamiliar tester who may need that specificity.

Some tests require specific steps to be done in order to acheive the outcome. In the banking system above, we may need to create the account before the new update details is relevant. For efficiency's sake, we would specify in Step 4 - "create a new bank account linked to the test customer" - so that our user wouldn't get to Step 15 and not have a new account to update. 
Declarative/Imperative can apply to the logic as well! In this case, the logic of the program is imperative (do step 1, then step 2, etc.), but the steps themselves are declarative (log in, open details page). Now that we know what the two are, we can start applying that knowlege and asking ourselves if one or the other is more appropriate. 

Generally, using both together interchangeable is the preferred approach to test authoring. For the familiar stuff, keep it declarative (log into the system, open the Bank details page) and for the less familar, switch to imperative (on the new Bank details page, select the bottom right link "Update details" which will open a new dialog with the fields...). 

## Quick examples before we go

How about a little matrix table to finish off?

<table style="width:100%; border-collapse: collapse; border: 1px solid black;">
  <thead>
    <tr>
      <th style="border: 1px solid black;"> </th>
      <th style="border: 1px solid black; text-align: center;">Imperative</th>
      <th style="border: 1px solid black; text-align: center;">Declarative</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th style="border: 1px solid black; vertical-align: middle;">Logic</th>
      <td style="border: 1px solid black; vertical-align: middle;">
        <pre><code class="language-gherkin">
Given a user logs in
And the user displays the banking details page
And the user opens the New Bank details page
And a new bank acocunt is linked to the customer
And the user nbavigates to the banking details page
And the Update Details link is clicked
Then the Update details dialog is displayed
And the BSB number field is selected
And the user can update their BSB number
And the Account number field is selected
And the user can update their Account number
        </code></pre>
      </td>
      <td style="border: 1px solid black; vertical-align: middle;">
        <pre><code class="language-gherkin">
Given a user logs in
And a new bank account is linked to the customer
And the Update details link is opened
Then the user can update their BSB
And the user can update their Account number
        </code></pre>
      </td>
    </tr>
    <tr>
    <th style="border: 1px solid black; vertical-align: middle;">Step</th>
    <td style="border: 1px solid black; vertical-align: middle;">
        <pre><code class="language-gherkin">
Given the Admin user logs in
| username  | password |
| test.admin    |   Test@123    |
And the user displays the banking details page
And the user opens the New Bank details page
And a new bank acocunt is linked to the customer
| BSB   |   account_number  |
| 123-123   |   2345-23456  |
And the user navigates to the banking details page
And the Update Details link is clicked
Then the Update details dialog is displayed
And the BSB number field is selected
And the user can update their BSB number
| BSB   |
| 123-123   |
And the Account number field is selected
And the user can update their Account number
|   account_number  |
|   2345-23456  |
        </code></pre>
      </td>
      <td style="border: 1px solid black; vertical-align: middle;">
        <pre><code class="language-gherkin">
Given a user logs in
And the user displays the banking details page
And the user opens the New Bank details page
And a new bank acocunt is linked to the customer
And the user navigates to the banking details page
And the Update Details link is clicked
Then the Update details dialog is displayed
And the BSB number field is selected
And the user can update their BSB number
And the Account number field is selected
And the user can update their Account number
        </code></pre>
      </td>
  </tbody>
</table>

To a new user, Declarative logic is going to be confusing, as they may not have know where to find that New Account feature. To a seasoned user, they're going to skip over what they think they know, and in doing so may miss some important steps. 
We can start to see some advantages in terms of length for Declarative authoring, but some real wins on the Imperative side for clarity and accuracy of reproduceability. It's a balancing act, as all things are. Just keep these in the bucket for later on and you will start recognising where these concepts can be applied.