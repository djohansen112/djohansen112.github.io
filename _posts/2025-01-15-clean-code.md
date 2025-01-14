---
title: Clean Code Principles
date: 2025-01-14 11:22:33 +1000
categories: [Development]
tags: [quality-engineering development]     # TAG names should always be lowercase
---

# Clean Code Principles

Clean Code is a term to encompass a set of recommendations put forward by a software engineer with some serious history - Robert Cecil Martin (Uncle Bob).
Software is not a new process. It's definitely still a very live and dynamic feeling discipline, but in reality the first software is over a hundred years old (depending on your definition of software). Ada Lovelace was creating software in the 19th century, albeit the software was manually setting levers and never actually came about, but the idea was there.
Turing, a giant name in the discipline was writing about modern sounding software as early as 1935 and the first real "stored in electronic memory" software is around 1948. We were still using punch card software as inputs well into the 1980s, but the ideas have been around long enough that we do have software engineers who have been working as software engineers as a job title for 50+ years. Uncle Bob is one of these. 

## Why the principles?
The main point of this is to reduce WTFs per minute. This is the most important measure of a software system - if someone comes along in a week/month/decade and has to read you code, then your job now as a professional software developer is to reduce their WTF's per minute by adhering to clean code. 
Software is everywhere, and ALL code is going to have many more hours spent reading it than it took to write. 

Secondly, a clean workspace is a fast workspace. Consider a workbench, with all the tools hanging neatly on a board at the back, arranged in order, cleaned and oiled, ready to rock. As a professional, that is the way we leave our workspace - ready to rock.
One of the human constants in any workplace is that speed equals mess. By cleaning along the way, we keep that workbench neat and tidy, ready for the next person, even if the next person is us.

### Smells 
In the same way we can't really describe smell to antother person without just using references (it smells sweet, it smells like leather, etc.) we can't pin down what makes code untidy. We can smell it though - there's some confusing names, a little too many comments, some weird nesting behaviours that don't really make sense as to what they do. Again, this comes back to Competence - if you're a software engineer, this is what you know, and if you've done it long enough you know how to fix it.

Uncle Bob even suggests the Boy Scouts of America tenet: 
> Leave No Trace

This concept calls for the user to leave their surroundings better than they found it. Sure they're talking about a campsite, but a codebase is basically the same thing.

## What are the principles?

The principles are not a prescriptive set of rules to follow, but instead are a set of guidelines and recommendations we can apply to create software that is testable, maintainable, understandable and efficient.

I'll list these below, but keep in mind that there is no way to apply these in any order, this is just an arbritrary listing method of concepts to keep in mind when writing and reviewing code.
Hell, this isn't even an exhaustive list! This is just the ones that make sense to me so I can refer back to them.

### Avoid hardcoding and use constants instead
Constants can be kept in separate classes, like a configuration file, and reused wherever you need. Why define something that you later have to search for by digging through hundreds or thousands of lines of code when you can define it once and reuse it.
<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```cs
 public static void ConvertToHours(int seconds){
    return seconds * 3600;
} 
```
</td><td>

```cs
 public const int Seconds_in_an_hour = 3600;
 
 public static void ConvertToHours(int seconds){
    return seconds * Seconds_in_an_hour;
} 
```
</td></tr>
</table>

### Meaningful and descriptive naming
If you were creating an app that tracked days, why would you call it d when we can call it days? Sure it takes a few more keystrokes, but better that than scratching at your head for an hour in a month when you come back to do something else with this function.
<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```cs
 public static void DaysSinceLogin(User u){
    int d;
    d = u.ll - currentDate();
    return d;
} 
```
</td><td>

```cs
 public static void DaysSinceLogin(User user){
    int daysSinceLogin;
    daysSinceLogin = user.lastLogin - currentDate();
    return daysSinceLogin;
} 
```
</td></tr>
</table>

### Explaoin yourself in code
Even though it can sometimes feel redundant, it's better to explain yourself through pulling concepts together into sensible packets while you understand them. Maybe if we had an employee tracker that followed some business rule of:
> Employees who have been here 5 years, and are selling more than $10k per month are considered for a bonus

Then let's pull those two concepts together early on so we can read through and understand it earlier.
<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```cs
 class Employee {
    public int yearsServed;
    public int monthlySales;
} 
...
//500,000 lines of code later...
    if (Employee.yearsServed > 5 && Employee.monthlySales > 10000)
        payBonusTo(Employee)
```
</td><td>

```cs
 class Employee {
    private int yearsServed;
    private int monthlySales;
    public boolean getsBonus(){
        if (Employee.yearsServed > 5 && Employee.monthlySales > 10000)
            return true;
        reurn false;
    }
} 
...
//500,000 lines of code later...
    if (Employee.getsBonus())
        payBonusTo(Employee)

```
</td></tr>
</table>
Even though these look more confusing, now the code for getting a bonus is in the Employee, not the Payment Calculator, or Accounting Frontend, or wherever else we could lose track of it, and the readability in the code is MUCH improved.

### Use comments sparingly and meaningfully

Good code doesn't need comments. Comments can go out of date, and given there's no outward sign that they are incorrect like we would have with incorrect code, then we are often unaware that they are out of date. 
We trust comments though, so think of the poor dude 5 years from now wildly questioning their choice of career as they try to understand whether the code is not doing what the comment wants it to do, or the comment is not about the code.
Also, even though green is our favourite colour, in comments it can become difficult to pick through. 
<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```cs
 class Employee {
    private int yearsServed //number of years served;
    private int monthlySales //monthly sales in dollars;
    
    //check if the employee gets a bonus if they have the right nunber of years served and sales made
    public boolean getsBonus(){
        if (Employee.yearsServed > 5 && Employee.monthlySales > 10000)
            return true;    //then they get the bonus
        reurn false;    //otherwise they don't
    }
} 
...
//500,000 lines of code later...
    if (Employee.getsBonus())   //this is from the Employee class
        payBonusTo(Employee)

```
</td><td>

```cs
 class Employee {
    private int yearsServed;
    private int monthlySales;
    
    public boolean getsBonus(){
        //5 years served and 10_000 is sales correct as of v2.6 of the employee handbook
        if (Employee.yearsServed > 5 && Employee.monthlySales > 10000) 
            return true;
        reurn false;
    }
} 
```
</td></tr>
</table>

Great! Now we know where the rule came from, and in the future we can verify whether we are still using v2.6 of the employee handbook or if we need to raise a question whether this should still be in the code. Everything else is prose - given we have named our variables and functions, we can almost read the code aloud to understand it. If we didn't have the names but had the comments there, it would definitely be time to spend a few minutes refactoring names and removing the comments.

### Functions do one thing - Single Responsibility Principle
A function should be named as a verb, in all cases! Why? Functions DO something.

<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```cs
 public void Calculator(int a, int b, string operator) {
    if (operator == "multiply")
        return a * b;
    if (operator == "divide")
        return a * b;
    if (operator == "add")
        return a * b;
    if (operator == "subtract")
        return a * b;
} 
...
//500,000 lines of code later...
    speed = Calculator(62, 15, multiply)
```
</td><td>

```cs
 public void multiplyTwoNumbers(int a, int b) {
            return a * b;
 }
  public void divideTwoNumbers(int a, int b) {
            return a * b;
 }
 public void addTwoNumbers(int a, int b) {
            return a * b;
 }
 public void subtractTwoNumbers(int a, int b) {
            return a * b;
 }
 ...
//500,000 lines of code later...
    speed = multiplyTwoNumbers(62, 15)
```
</td></tr>
</table>

Even though we have technically written more code we have created specific verbs that simply state what will happen with this function. Readability is improved, modularity is improved. We can even extract these out to a class named Calculator to abstract the code away from a single file, and possibly open up even more re-usability across the entire codebase.
In these examples, even though they are simple, we can imagine a more complex function that draws information from multiple systems to perform a task. We would be creating 4 functions that can be worked on independently or we could have a single mega-function that has 4 times as many lines, and if one thing goes wrong, we need to debug the entire function. Guess which one I'd choose!

### Don't Repeat Yourself (DRY)
Don't repeat yourself is about not repeating yourself.

OK, moving on from that now that you've stopped applauding my comedic genius. Don't repeat yourself is about reducing the complexity by identifying places where our code can be reused.

<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```cs
public void withdrawFromAccount(int withdrawal, User user) {
    if (user.balance > 0 && user.accountIsOpen && user.accountExists && user.customerStatus != "INACTIVE"){
        user.balance = user.balance - withdrawal
    }

    if (user.balance < 0 && user.accountIsOpen && user.accountExists && user.customerStatus != "INACTIVE"){
        print("User balance is negative! Aborting withdrawal")
    }
} 
```
</td><td>

```cs
public void withdrawFromAccount(int withdrawal, User user) {
    if (user.accountIsOpen && user.accountExists && user.customerStatus != "INACTIVE"){
        user.isValid = true;
    }

    if (user.balance > 0 && user.isValid){
        user.balance = user.balance - withdrawal
    }

    if (user.balance < 0 && user.isValid){
        print("User balance is negative! Aborting withdrawal")
    }
} 
```
</td></tr>
</table>

Even better, we could track that status of ".isValid" on the user itself and use it for withdrawals, tax, fees, incoming transactions etc.

### Know the standards - reduce the WTFs
For a simple example we can look at capitalising various names to indicate what they refer to.
Some standard capitalisation patterns includ:
> camelCase - named after the hump in the middle, this refers to a small first letter with any subsequent word beging capitalised. Standard in Java for functions, and widely used across different languages for different things, but very common for function names and variables in other languages as well.

> snake_case - slithering along the ground with underscores, this is widely used in Python for naming functions and commonly in database naming soa s to avoid whitespace. 

> kebab-case - looking like a skewer of letters, kebab case is not common in programming, but definitely would be familiar to anyone working with URLs or other places we need easily readable names without whitespace.

> PascalCase - Capitalise Everything! This is from the Pascal language, but is adopted into almost every other language, usually used to denote a class name.

> CAPITAL_CASE - usually underscored to help with readability, this is commonly used for naming constants in programming.

Other standards apply, such as PEP-8 for Python where they specify the number of spaces tyo be used to create indents, as well as where brackets should appear etc.

The following Poor Code is just as valid as the Clean Code to the compiler, but definitely reads better. After all, code is for humans to read, and computers to interpret. If we can read it easily, we can make sure it works for the computers to interpret.

<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```java
public static void createabiglistofnumbers(){for(int i=0; i<5; i++)
{System.out.println(i);}}
```
</td><td>

```java
 public static void createListOfNumbers(){
    for(int i = 0; i < 5; i++){
        System.out.println(i);
    }
 }

```
</td></tr>
</table>

### Nested conditionals should be functions
Anything more than two indents is a new function. Not really a hard and fast rule, but if your function has a nested if statement, then perhaps that nest could be exported as another mini function. This opens up the ability to reuse code, as well as better modularity. Even though these are not the end goals of programming, every little bit helps. It can also help to define naming for the code - instead of running an if statement checking someone's account balance by verifying their account status, if they have an account, if they are not on a stop-credit, or any other list of conditions, we can just check "if isValidCustomer()".
We can see how these are starting to build as we've done this a few times now - Don't Repeat Yourself had similar code, but with a different intention in the example. Let's revisit it here:

<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```cs
public void withdrawFromAccount(int withdrawal, User user) {
    if (user.balance > 0 && user.accountIsOpen && user.accountExists && user.customerStatus != "INACTIVE"){
        user.balance = user.balance - withdrawal
    }

    if (user.balance < 0 && user.accountIsOpen && user.accountExists && user.customerStatus != "INACTIVE"){
        print("User balance is negative! Aborting withdrawal")
    }
} 
```
</td><td>

```cs
public void isValidCustomer(User user){
    if (user.accountIsOpen && user.accountExists && user.customerStatus != "INACTIVE"){
        return true;
    }

    return false;
}
public void withdrawFromAccount(int withdrawal, User user) {
    if (user.balance > 0 && user.isValidCustomer()){
        user.balance = user.balance - withdrawal
    }

    if (user.balance < 0 && user.isValidCustoemr()){
        print("User balance is negative! Aborting withdrawal")
    }
} 
```
</td></tr>
</table>
So it reads better - if their account balance is greater than 0, and they check out as a valid customer, then we do the balance change. We can now also check that they are a valid, or even better, move that check to a flag on their own account object.

### Refactor, refactor, refactor
As Uncle Bob points to in the Boy Scouts motto - "Leave it better than you found it" - we can always make sure we are applying principles to our code. If you are searching through some code during a review, or even just doing some research to use some previous code, you can always open a pull request to improve that code. As the United States' ultra-paranoid government always asks their citizens - if you see something, say something. Vigilance and continuous improvement are some serious allies in creating clean codebases, and it all starts with recognition, review and refactor.

### Version control
Never underestimate the value of version control. We use this principle in Test Driven Development (a HUGELY transformative system and methodology that really works well with the principles of clean code) to perform the Red-Green-Refactor cycle. We can see mini  versioning whenever we get another round of refactoring - the aim is for clean code, yes, but the overarchingly important principle is WORKING code. The shorter our versioning, the simpler it is to recall to the last point we had working code. The shorter our versioning, the faster we can develop - building on working code is the key to correct versioning - we have a set of working functions and features that improves our system. It all works around the word "working" - without that key phrase, we are just creating technical debt.

### Handle errors gracefully
Errors in your code should never cause the code to completely stop and exit - it can fail, but it should never exit unless there was no other safe way of halting the program. We can do this by handling errors gracefully - accounting for them in our error handling functions, and ensuring that we are keeping memory safe and system safe exits from any process our code creates.

There are three steps to handle errors gracefully: Keep the user informed, keep the developer informed, and keep the app running until we can recover. We can do this in a few ways:
- Input validation: Know that the data your code is about to process is not going to cause data issues. Check that strings are the right length, floats are in the correct decimal form, integers are not negative early on in the process and reject the user to try again, rather than drop everything. 
- Fail fast: Identify and handle errors early in the process, so we are building on working code and also so we are handling them at the base levels before our code becomes too large to correctly identify error sources. Each function that could create a data handling error should perform the checks, each class that ingests a value should be able to check that the value is in the correct format BEFORE working with it, and reject it back to the function that sent it. If the function returns a value, then it should handle rejection of that value, so it comes straight back, rather than waiting for an entire process tree to complete handling.
- Logging and monitoring - if your code can identify where the error came from, it should emit a logging message that identifies the function that failed, why it failed, and what it did to stop the error.
<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```java
public static void accountOpen(String args[])
{
    try {
        //some code here
    }
    catch (NullPointerException e) {
        throw e;
    }
}
```
</td><td>

```java
public static void accountOpen(String args[])
{
    try {
        //some code here
    }
    catch (NullPointerException e) {
        // to the user:
        throw new ErrorMessage("Looks like something went wrong! Please try again.")
        // to the developer:
        System.out.println("NullPointerException in accountOpen: Account object does not exist")
        // to the logs, or this may be captured in the ErrorMessage class above
        LOGGER.log(Level.INFO, "NullPointerException in accountOpen: Account object does not exist"); 
    }
}

```
</td></tr>
</table>


### You Ain't Gonna Need It
Don't code what you aren't ready to use. Keep it up to date and reduce the amount of single use code. Just because you can code a list object to have a sort function doesn't mean you will ever need to use that sort, so wait until you have something that required the list to be sorted.

### Separation of Concerns (SOC): 
Divide the code into logical modules or functions, each with a specific responsibility, making it easier to maintain and modify.
I don't think we need to cover this, as per above million examples, I'm sure you're fully aware of how we are always looking to modularise, simplify and clean our code.



### KISS (Keep It Simple, Stupid)
Finally (!) we get to the core of clean code - Keep It Simple! We are writing code for humans to read, otherwise we would all be using Assembly to write our code. Coding languages are successful or otherwise as people are able to understand and write them. Every language CAN make a linked list, languages just hide that complexity behind simpler functions and classes. Sound familiar? Yep - clean code applies here too. Languages demarcate the basic functions that the author defined, but they all ultimately perform the same thing - creating a list of Assembly (or binary) code that we can use to either store data, or do something with it.

### Simple clean code through SOLID
SOLID principles are closely aligned with Clean Code as a concept, and are a great way to quickly wrap your thinking around a framework when approaching a problem.

#### Single Resposibility
Software is made up of functions that all do one thing, and are responsible for their own data.
#### Open/Closed Principle (OCP) 
Software entities should be open for extension but closed for modification
#### Liskov Substituion Principle 
Each child should do everything the parent class can do, plus more specific things. This guideline asserts that child classes should be able to replace parent classes without affecting functionality.
#### Interface Segreagation Principle 
A client that only needs to work with cars would still need to depend on the entire VehicleService interface, including methods related to trucks and motorcycles that it doesn't use.
#### Dependency Inversion Principle 
If lots of things are using a specific interface, make the interface geenral rather than keeping specific ones for each thing

## Concluding remark
Need a wallchart, or maybe a new tattoo outlining the principles of Clean Code, now we're done here? No, you definitely do not. Clean code is a great framework and thought to keep in mind when performing coding processes, but it is ultimately NOT the answer to all our problems. It is a concept that Uncle Bob has peddled around to others in conferences and talks, and that's great, we should all think about our ways of working and improve on them, but no person has the answer. I firmly believe that the greatest way to understand something is to teach that thing, hence my writing this article. I definitely do think about Clean Code when I'm reviewing as part of my job. If we can rename or abstract a method, I'll open a pull request, if not, then we move on.







