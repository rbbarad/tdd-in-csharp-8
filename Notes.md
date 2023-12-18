# Test Driven Development in C# 8

## Course Overview

- Basics of Test Driven Development
- Using TDD to write business logic
- Decoupling dependencies to create testable and maintainable code
- Adding features in a .NET Web Application with TDD

### Prerequisites

- Basics of C#
- Unit Testing

## Getting Started with Test Driven Development

### What is Test Driven Development?

TDD is an approach that is using tests to drive the design of your code.

Before you code, you usually have a set of requirements that you need to fulfill with the code.

With TDD, you make these requirements really small. Then you take the 1st requirement, and before you implement it, you **write a failing test**. This test will fail (RED) as you haven't written the logic for it yet. This test describes already what your code should do to fulfill the requiremnt.

Next, you **make the test GREEN** by writing the code to make the test pass.

Finally, you **refactor** the code. For example, you could simplify a few lines of code, extract into a method, etc. to make the code more readable and maintainable.

After the refactoring, you are done with the 1st requirement and you can continue with the next requirement.

Repeat the cycle!

### Advantages of TDD

- Since you always start with a test, TDD forces you from the beginning to think very well about the needed APIs
  - you need to think about what classes are needed, what methods, what properties
  - This is an advantage that usually leads to a great API design
- You have to think about what the code should do
  - You can think about what the code should do without having to worry about how the code should do it
  - Only after the test is written, you implement the requirement and need to think about how the code does it
- Get fast feedback
  - Instead of running your app to test the feature, you can just run the unit test
  - This also means, you don't even need your application at all. TDD allows you to start with your business logic when building an application because you get the feedback from the test
- Automatically create modular code
  - To test a class, you have to decouple it from its dependencies. TDD forces you to do this from the beginning
  - This decoupling of dependencies leads to modular code. Modular code allows you to work on a requirement in isolation without having all the dependencies
    - Is the database that you need to implement your requirement not ready yet? No problem! You decouple it from your code and you can test and implement your requirement
    - Is the Web API that you need to implement your requirement not ready yet? No problem! You also decouple that Web API from your code so that you can test and impelement your requirement
- Write maintainable code
  - As you have at least 1 test per requirement, it is much easier to maintain your code. You can make huge code changes without breaking existing functionality
- Tests are a good documentation of your code
  - Tests will tell future maintainers why certain logic is there

### Disadvantages of TDD

- Especially for a beginner, it is not so easy to start with a test
  - This course helps to get started and get really productive with TDD

### The Wired Brain Coffee Scenario

Wired Brain Coffee is a company that runs several coffee shops across the world.

In their shop in Zurich, they have desks for customers.

They want a web application where customers can book a desk for a full day.

They ask you to build the business logic and parts of the web application.

#### Mockup

- Overview (Book a desk)
- Inputs for First Name, Last Name, Email, and Date
- Button for Booking a desk

This website will be part of a project that is called DeskBooker.Web. This will be an ASP.NET Core Web Application that is using RazorPages.

#### The Planned Architecture

DeskBooker Solution:

- DeskBooker.Web (ASP.NET Core)
  - The Web Project will reference the Core Project
- DeskBooker.Core (.NET Core class library)
  - Contains the business logic (core) of the application
  - The central part of this logic is the `DeskBookingRequestProcessor` class
- DeskBooker.DataAccess (Entity Framework Core)
  - Accesses a SQL server database

### How this course is structured

- Getting Started with Test Driven Development
  - The Solution and projects do not yet exist. In TDD fashion, we'll begin with the Core Test Project (DeskBooker.Core.Tests)
  - Iterate through TDD Cycle
- Testing and Implementing Business Logic
  - Test and Implement more complex requirements in the Core Project that will force you to decouple dependencies from the `DeskBookingRequestProcessor` class
  - You will have to mock these dependencies in your test methods
- Adding Features in an ASP.NET Core APP
  - Learn how you can use TDD to add features in an ASP.NET Core application

### The First Requirement

- Test
- Implement
- Refactor
