---
title: Automation Frameworks - the Page Object Model
date: 2025-03-18 11:22:33 +1000
categories: [Test Automation]
tags: [development, tools]     # TAG names should always be lowercase
---

# Automation
## The Page Object Model 

OK, so our job is to create a test automation that will click through our website and run tests against it. We have selected our language, worked out our approach, we've written some initial tests that our automation should be doing, and now... well, now the work begins!

### What's an object
For those new to computers and data design, an object can simply be thought of as a single piece of data with a set of features or attributes, and a set of things it can do.
Consider a car as a simple object, it has some features that make it unique:
- colour
- number of doors
- make
- model
- registration number

and a set of things that it can do
- steer left or right
- accelerate
- brake
- turn on or off

An object, in object oriented design simply seeks to represent this car as an object class. We call our features "attributes" and our things it can do "methods". Each attribute becomes a variable, and each method becomes a function. When we create a new object, that object is tracked in memory along with all it's own values for attributes, and when we call that particular object methods, it will maintain the methods specific to that class. In this way, we can create many Cars, with all different values for their colours, models, etc. but they can each do the same things independently of each other. So if wanted to refer to one of the Cars to turn off, it will track that particular cCar in memory, so if we try to accelerate, it can tell us that this particular Car is currently turned off and can't accelerate. 

```python
class Car()

    # attributes
    colour = "red"
    doors = 4
    make = "Ford"
    model = "Focus"
    registration = "4XY 6BG"

    # methods
    def steer(direction)
        pass
    
    def accelerate(pedal_percentage)
        pass
    
    def brake(pedal_percentage)
        pass
    
    def start(engine_status, key_status)
        pass
    
    def stop(engine_status, key_status)
        pass
```

That's the crashest of all crash courses in object oriented programming. There's SO many resources available to learn this that I won't bore you with the details, but suffice to say, this topic is VERY well documented and discussed! It is an important concept, and I recommend it to anyone not familiar, as it is very popular in modern software engineering practice.

### So we have objects, now what?
What is a website page, but a set of attributes and a set of things to do with that page?
Consider our little login page as an object, it has a username and password field, a submit button, and a help text for when we need to tell the user something. We also have a set of methods we can call to do things to our page, such as clicking the submit button, reading the helptext, or checking that the helptext is displayed or not.
One thing to wrap our heads around here, is that our attributes are not always as simple as a variable, our attributes also have references to components of our page, through the use of locators. For example, given our page is written in HTML, the username field is a HTML form, and there may be a certain class named "username", or an id associated with the field so it can successfully return data via the HTML form when the submit action is performed. To do something with that attribute, we use locators to return a reference to that field.

### Locators as Attributes
So for the username field, our locator needs to refer to the HTML path to that particular field. In this case, the ID of the field is "form-username", so our attribute that the page object has will be the reference to that field. We can use various methods to get these locators, depending on the tools needed. In the example below I'll just use the inbuilt Python "By" locator, but XPath is the preferred approach in most cases due to it's reliability and specificity. More on Xpath another time, or there are always better references to these concepts that I highly encourage you to find and learn from.

### Methods
According to some pretty hard and fast software engineering principles, it's always important to stay DRY - Don't Repeat Yourself! For something as simple as clicking a button, our framework may include a clickButton() function, or we may need to provide a customised solution. For most prebuilt frameworks (Selenium is by far the most popular) this is already solved, so for the sake of simplicity, we'll assume for the rest of the article that we are using prebuilt functions.

### Our first POM
So let's pop our login page into a simple model using an object class. I'll use Python here for simplicity, but the same object structure applies to Java, C++ as well as pretty much any other language/framework that supports object oriented principles.
```python
class LoginPage:
    # Initialise the WebDriver which is the interface between the program and the browser we will use to run our tests on our actual website
    def __init__(self, driver: WebDriver):
        self.driver = driver

    # Locators for elements on the login page
    username_field = (By.ID, "username")
    password_field = (By.ID, "password")
    help_text = (By.ID, "help-text")
    login_button = (By.ID, "loginButton")

    # Method to enter username
    def enter_username(self, username: str):
        self.driver.find_element(*self.username_field).send_keys(username)

    # Method to enter password
    def enter_password(self, password: str):
        self.driver.find_element(*self.password_field).send_keys(password)

    # Method to click the login button
    def click_login_button(self):
        self.driver.find_element(*self.login_button).click()
```

Pretty simple, all we need to do is get a list of the page attributes and then find them using locators, create some simple functions for the website, etv voila! we've modelled our login page as an object.

Now when we run our tests we can create an instance (a new object) that tracks the login page we currently have open, and start working with it.

```python
def test_login():
    # Initialize the WebDriver that will operate the browser
    browser = webdriver.Chrome(executable_path='path/to/chromedriver')

    # Navigate to the login page
    browser.get("http://test-system.com/login")

    # Create an instance of the LoginPage using our class above
    login_page = LoginPage(browser)

    # Perform actions using the page object
    login_page.enter_username("fakeuser")
    login_page.enter_password("password123")
    login_page.click_login_button()

    # Close the browser
    browser.quit()

```

Add some actual tests in here, and we're done! Now onto the next page! 
Everytime we trigger the test_login() function, we can se the browser open, try to login and then close. Automation achieved, but no testing happened! Let's resolve this using assertions. 

### Assertions and testing
When this fake test user attempts to log in, our requirement states that the help text should be displayed, as this user does not exist and the login should not proceed.
Before the final quit is sent, let's check that this non-existent user triggers the help text using an assertion:
```python
    ...
    login_page.enter_username("testuser")
    login_page.enter_password("password123")
    login_page.click_login_button()

    # use the built in check if the help text is displayed or not
    assert login_page.help_text.is_displayed(), f"Test failed, help text not displayed for failed login"

    browser.quit()
```

An assertion just checks that something happened, so in this case, our .is_displayed() would return False which would cause us to do whatever is after the comma on that line.
If the assertion returned True (in this instance, the help text is displayed successfully) then it would "pass" that test, and move on to the next. We also need to add another assertion that the page doesn't change, but that can be left to the reader to figure out. Maybe we need to add an address to our model and assert that the page address is still "login", maybe we could check that the current page's title (another attribute to add) is still "login", however we would proceed will depend on the situation and the model we've built. 

A test can have many assertions, such as validation of the username field label, help text content (in the case where the help text is different according to the user scenario), it all just depends on the complexity of the test, and the ability of our model to accurately reflect the state of the page.

It is important to have each test failure return a unique identifier so we can better trace where our tests have failed. With a generic error message, we would need to check each test individually, by add the "help text not displayed" we can understand where our issue lies.

## Concluding remarks
That's it for Page object model. The complexity of the model can scale up (or down, which may be surprising given how simple our example seems) and some models can be hundreds of lines long. The POM is simple, but it underlies a complex final product. Next steps for something like this would be assertions that check backend data in the case of filling form values, or abstraction of the data into a value passed to the function so we can call the test_login with parameters. The POM is just the basic building block, so we can even pass off handling our test function to another framework entirely, such as a keyword driven test that calls the test function whenever we run our keyword test. We can even push it into prebuilt frameworks, such as Behave or PyTest for Python, or BDD frameworks such as SpecFlow or Cucumber. Once we can interact with our website, whatever actions we decide now become as simple as writing the test according to our need.   