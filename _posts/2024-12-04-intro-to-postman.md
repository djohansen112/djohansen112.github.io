---
title: Postman and Testing
date: 2024-12-04 11:22:33 +1000
categories: [Test Tools]
tags: [quality-engineering, postman, tools]     # TAG names should always be lowercase
---


![post avatar](/assets/avatar_11.png){: width="200" height="100" .left}

## Postman and testing

Postman is possibly one of the most helpful tools in the kit when it comes to API testing. Even though other suites may offer better customisations or other benefits - Postman is THE tool for quick checks or retrievals over API data.

### Postman AI - Postbot
Postbot is a recent addition to Postman that makes testing an API very quick, and very easy. While it covers the basics, it doesn't know the goals of testing, so can sometimes need customisations applied to really drill down on the expectations of the test.

Postbot can be asked to create simple tests, checking for HTTP codes such as expecting the status code of the API request to be 200 or grouped by status categories like success or fail. This uses the standard Postman testing library (pm.*) which is an relatively human readable implementation of Javascript.

### Postman API test library (pm.*)
Here's a few examples to get started with the Postman library. We'll break it down so you can use it in your own testing. Postman implements the Chai Assertions library if there's any further reading.

{% highlight javascript %}

pm.test("Response body is not empty", function () {
    pm.response.to.not.be.empty;
});

{% endhighlight %}

First of all lets break it down to a simple structure: 

{% highlight javascript %}

pm.test("function name", function(){ 
    //test steps go here 
});

{% endhighlight %}

The function name can be anything, but when the test is done, Postman will report back this name of the function with a PASS or FAIL - so if it makes sense, it'll save some headaches later on.
Generally, when we write a test, it helps to write the test in simple language and with a clear goal.
For building this test, let's start with an example - we're expecting a 200 response from the server, so the function name could be:

> Status returns 200

The second part is the declaration of what the test will be. In the vast majority of cases, this will be running a Javascript function.
The Javascript function in this case will just use a combination of PM library methods.

{% highlight javascript %}

pm.test("Status returns 200", function(){ 
    pm.response.to.have.status(200) 
});

{% endhighlight %}

While there's no hard a fast rule about how many steps you can run, it's always best to keep testing focussed and to the point. It's always better to run more atomic test cases than to combine too many steps into a single method.
Here's multiple steps in the single test. They each move towards the same goal, but are checking multiple angles.

{% highlight javascript %}

pm.test("Status returns 200", function(){
   pm.response.to.have.status(200);
   pm.response.to.not.be.empty;
   pm.response.to.have.header("Content-Type");
});

{% endhighlight %}

#### Other PM functions
Simple so far? Great. If not, have a review over the material and if it's still not coming together, go for an explore or try it yourself!

Let's unlock some testing possibilities using Javascript object notation. 

We can also use Javascript coding principles to create a variable to store the response and let's use that to get some data from the response.

{% highlight javascript %}

var responseJSON = pm.response.json();

pm.test("Response contains a new account ID", function(){
   pm.expect(responseJSON).to.be.an('object');
   pm.expect(response.JSON.accountID).to.not.equal("0"); 
   
   //and if we know what the account number is we're testing for:
   pm.expect(response.JSON.accountID).to.equal("982341");
});

{% endhighlight %}

Using Javascript we can always extend that even further to help us understand where testing happened and where it failed:

{% highlight javascript %}

var responseJSON = pm.response.json();
var currentBalance = responseJSON.items[0].currentBalance
var accountNumber = responseJSON.items[0].accountNumber

pm.test("Account balance for account: "+ accountNumber + " was not 0", function(){
    pm.expect(currentBalance).to.not.equal("0"); 
});

{% endhighlight %}

Or how about using loops in the case of a bulk response:

{% highlight javascript %}

let jsonData = pm.response.json();

pm.test("None of the response accounts had a balance of 0", function(){
    //loop through the array of accounts and check each balance is not 0
    jsonData.accounts.forEach(function(account) {
        pm.expect(account.currentBalance).to.not.equal("0"); 
    }
});

{% endhighlight %}

So get onto it - Postman is free for personal use and it a great tool to get started in setting up some simple testing. 