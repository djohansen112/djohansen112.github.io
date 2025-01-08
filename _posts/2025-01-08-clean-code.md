---
title: Clean Code Principles
date: 2025-01-08 11:22:33 +1000
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


<h4>Poor Code</h4>
{% highlight csharp %}
 public static void ConvertToHours(int seconds){
    return seconds * 3600;
} 
{% endhighlight csharp %}
<h4>Clean Code</h4>
{% highlight csharp %}

 public const int Seconds_in_an_hour = 3600;
 
 public static void ConvertToHours(int seconds){
    return seconds * Seconds_in_an_hour;
} 
{% endhighlight csharp %}

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
> <h4>camelCase</h4> - named after the hump in the middle, this refers to a small first letter with any subsequent word beging capitalised. Standard in Java for functions, and widely used across different languages for different things, but very common for function names and variables in other languages as well.

> <h4>snake_case</h4> Slithering along the ground with underscores, this is widely used in Python for naming functions and commonly in database naming soa s to avoid whitespace. 

> <h4>kebab-case</h4> Looking like a skewer of letters, kebab case is not common in programming, but definitely would be familiar to anyone working with URLs or other places we need easily readable names without whitespace.

> <h4>PascalCase</h4> Capitalise Everything! This is from the Pascal language, but is adopted into almost every other language, usually used to denote a class name.

> <h4>CAPITAL_CASE</h4> Usually underscored to help with readability, this is commonly used for naming constants in programming and for yelling at furutre programmers in comments.

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

Good code has a shape to it - good code can be identified by squinting your eyes and letting the details blur. Software engineering professionals can identify poor code without reading a single line, just by looking at the shape.

### Nested conditionals should be functions
Anything more than two indents is a new function. Not a hard and fast rule, but consider this - a function should do one thing, as per the principle above, and nested code is either checking multiple things or performing too many functions. This can be a generalisation, and something like a nested for loop can sometimes help when constructing a multi dimensional array, but where possible, we should be trying to clean up.


<table>
<tr><td><h4>Poor Code</h4></td><td><h4>Clean Code</h4></td></tr>
<tr><td> 

```js
const seatingArray = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

for (let i = 0; i < seatingArray.length; i++) {
  for (let j = 0; j < seatingArray[i].length; j++) {
    console.log(seatingArray[i][j]);
  }
}
```
</td><td>

```java
const seatingArray = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

function iterateArray(array, callback) {
  for (let i = 0; i < array.length; i++) {
    for (let j = 0; j < array[i].length; j++) {
      callback(array[i][j], i, j);
    }
  }
}

iterateArray(seatingArray, (value, i, j) => {
  console.log(`Element at [${i},${j}]: ${value}`);
});
```
</td></tr>
</table>
This is another case where there are more lines of code, but the code is much more readable, modular, and decoupled than before. On top of that, in case we ever need to iterate through another array, we can throw it to this function and customise the result.

### Refactor, refactor, refactor

### Version control

### Handle errors gracefully

### KISS (Keep It Simple, Stupid)

### You Ain't Gonna Need It
Don't code what you aren't ready to use. Keep it up to date and reduce the amount of single use code.

### Separation of Concerns (SOC): Divide the code into logical modules or functions, each with a specific responsibility, making it easier to maintain and modify.


????
SOLID:
Single Resposibility
Open/Closed Principle (OCP) states that software entities should be open for extension but closed for modification
Liskov Substituion Principle - each child should do everything the parent class can do, plus more specific things. guideline asserts that child classes should be able to replace parent classes without affecting functionality.
Interface Segreagation Principle - A client that only needs to work with cars would still need to depend on the entire VehicleService interface, including methods related to trucks and motorcycles that it doesn't use.
Dependency Inversion Principle - if lots of things are using a specific interface, make the interface geenral rather than keeping specific ones for each thing


This is good: https://axolo.co/blog/p/core-principles-of-writing-clean-code

Also good: https://www.planetgeek.ch/wp-content/uploads/2014/11/Clean-Code-V2.4.pdf

????




