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

### Understanding The First Requirement

`DeskBookingRequestProcessor` will contain the logic for booking a desk

On the Web Page, a customer can enter: (input data)

- FirstName
- LastName
- Email
- Date

1st requirement:

- The Processor needs to return that input data again after the request was procssed

Later, more info will be returned like the info on whether the desk booking was successful or not. For now, the 1st requirment is just that the input values need to be returned again.

We will start with a test - but before you can write a test, you need to think about the API design.

Let's say that we create a `BookDesk` method within the `DeskBookingRequestProcessor` class. Instead of passing each of the inputs as separate parameters to the `BookDesk` method, lets create a `DeskBookingRequest` class that has those 4 input properties and pass that to the method. Let's also define that the `BookDesk` method returns a `DeskBookingResult` instance that has the 4 input properties.

Now that we've defined the API that satisfies our conditions, let's go ahead and create a test. Keep in mind that while we've planned the API, *nothing exists yet*.

**This test will ensure that the input values are also in the result.**

## Testing and Implementing Business Logic

### Understand the Requirements

Requirements:

- ~~Return result with request values~~
- ~~Throw exception if request is null~~
- Save a desk booking
- Check if a desk is available
- Store the desk id on the booking
- Return Success or NoDeskAvailable result code
- Set the desk booking id on the result

### Know How to Decouple Dependencies

![DualResponsibilities](./Images/DualResponsibilities.png)

**Single Responsibility Principle:** Every component like a class or method should have a single responsbility

- A class or method should have only 1 reason to change

In the above image, the BookDesk method has 2 Responsbilities!!

- It has to process the `DeskBookingRequest` and it has to save the `DeskBooking` object
- It definitely should not know how to save a `DeskBooking` object in the database

What if we introduced a `DeskBookingRepository` class that has a Save method that is responsible for saving the `DeskBooking` object into the Database?

![ImplementationDependency](./Images/ImplementationDependency.png)

Here, we can pass an instance of the Repository into the constructor of the Processor, and then the `BookDesk` method can call the Repository Save method to save a DeskBooking object.

Now, we can see that the `BookDesk` method does not depend on the database anymore and it does not need to know how the DeskBooking object is saved, which is great!

But now, the `DeskBookingRequestProcessor` class directly depends on the `DeskBookingRepository` class and its Save method. As this Save method is actually accessing the database, this structure does not work for a unit test.

- This is because the unit test should run in isolation and it should not depend on a database that might be on a server

**Dependency Inversion Principle:** Components must depend on abstractions and not on implementations.

**We can break up this dependency by introducing an interface!**

![ImplementedInterface](./Images/ImplementedInterface.png)

Now you can pass an object that implements this interface into the constructor of the Processor, and then the BookDesk method can call the Save method of the interface.

Now, the `DeskBookingRequestProcessor` class depends on an abstraction which is the `IDeskBookingRespository` interface. It does not depend on an implementation like the `DeskBookingRepository` class that is accessing the database.

So, it is no problem that we don't have the Repository class in the database yet as the Processor class just depends on the interface.

This means, to write a test for the 1st requirement, we need to introduce an interface. And in the tests, we create a Mock object that implements the interface. This Mock object does not access the database -- it is just a fake object for your tests.

![MockedInterface](./Images/MockedInterface.png)

1. We pass the Mock object into the Processor's constructor
1. Then, we cal the `BookDesk` method in the unit test
1. Last, we verify that the Save method of the Mock object was called and that the expected `DeskBooking` object was passed as an argument to that Save method

### Check if a Desk Is Available

![CheckIfDeskIsAvailable](./Images/CheckIfDeskIsAvailable.png)

### Store the Desk ID on the Booking

![StoreTheDeskIdOnTheBooking](./Images/StoreTheDeskIdOnTheBooking.png)

We already have a `ShouldSaveDeskBooking` test method that checks for the Desk Properties -- Lets modify this to check for the Desk ID as well.

### Return Success or NoDeskAvailable Result Code

![ReturnSuccessOrNoDeskAvailableResultCode](./Images/ReturnSuccessOrNoDeskAvailableResultCode.png)

### Set Desk Booking Id on the Result

![SetDeskBookingIdOnTheResult](./Images/SetDeskBookingIdOnTheResult.png)

If Desk Booking was successfully created, you should set the ID of the DeskBooking on the result object. If NoDeskAvailable, ID should be null. This means that ID should be a nullable integer