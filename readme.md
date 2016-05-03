# Errors Toolbox
 
This toolbox contains objects and exceptions for error handling.

## Table of Contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Installation](#installation)
- [Error object](#error-object)
  - [AddErrorMessage](#adderrormessage)
  - [AddErrorMessages](#adderrormessages)
  - [AddMessage](#addmessage)
  - [AddMessages](#addmessages)
  - [ToString](#tostring)
- [Exceptions](#exceptions)
  - [BaseException](#baseexception)
  - [NotFoundException](#notfoundexception)
  - [UnauthorizedException](#unauthorizedexception)
  - [ValidationException](#validationexception)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Installation

Adding the DataAccess Toolbox to a project is as easy as adding it to the project.json file :

``` json
 "dependencies": {
    "Toolbox.Errors":  "1.1.0", 
 }
```

Alternatively, it can also be added via the NuGet Package Manager interface.

## Error object

The _**Error**_ object consists of a unique id and a list of ErrorMessages.

An ErrorMessage has 2 fields :

- Key
- Message

The _**Key**_ field should contain a unique key that can be used for code-based checks.

The _**id**_ of the Error bobject can be given via the constructor :

``` csharp
var error = new Error("19cd7bb6-a378-4cae-9039-5b61dbc06933");
```
This way you can use this id to link several error objects for e.g. logging purposes.

If no id is given via the constructor, a guid will automatically be generated by the default constructor :

``` csharp
var error = new Error();

Console.WriteLine("id = {0}", error.Id);
```

will give something like :
```
id = 22699ff7-8496-49b2-8772-4c9e952a39fd
``` 

The constructor also optionally accepts a list of ErrorMessages :

``` csharp
var errorMessage1 = new ErrorMessage("key1", "message1");
var errorMessage2 = new ErrorMessage("key2", "message2");
var errorMessages = new ErrorMessage[] { errorMessage1, errorMessage2 };

var error = new Error("id", errorMessages: errorMessages);
``` 

Another constructor overload accepts an ErrorMessage that will be added to the Messages collection.

``` csharp
var errorMessage1 = new ErrorMessage("key1", "message1");

var error = new Error("id", errorMessage1);
```

The Error object contains several methods to add messages.

### AddErrorMessage

Adds 1 ErrorMessage object to the list :

``` csharp
var error = new Error("id");
var errorMessage = new ErrorMessage("aKey", "aMessage");

error.AddErrorMessage(errorMessage);
``` 

### AddErrorMessages

Adds a list of ErrorMessage objects :

``` csharp
var errorMessage1 = new ErrorMessage("key1", "message1");
var errorMessage2 = new ErrorMessage("key2", "message2");
var errorMessages = new ErrorMessage[] { errorMessage1, errorMessage2 };

var error = new Error("id");
error.AddErrorMessages(errorMessages);
``` 

### AddMessage

A string message or a key and string message can be given as arguments. If only a string message is given, the _**Key**_ field will be _**String.Empty**_.

``` csharp
var error = new Error("id");

error.AddMessage("aMessage");
error.AddMessage("aKey", "anotherMessage");
``` 

### AddMessages

Adds multiple string messages to the ErrorMessage list. The key for each message will be _**String.Empty**_.

``` csharp
var message1 = "message1";
var message2 = "messages2";
var messages = new string[] { message1, message2 };

var error = new Error("id");
error.AddMessages(messages);
``` 

### ToString

The ToString method returns the contents of the object as a string. This can be handy when tracing and debugging.

## Exceptions

### BaseException

A base class for exceptions. 
It inherits from the standard Exception class and has the following extra fields :

- HttpStatusCode : a HTTP Status Code that can be returned in a HTTP Response (for usage in web apps).
- Error : an Error object.

The default HttpStatusCode is _**500**_ (Internal Server Error).

### NotFoundException

To be used when a requested resource is not found or does not exist. 
The default HttpStatusCode is _**404**_ (Not Found) and the default message is _**Not found.**_.

``` csharp
var ex = new NotFoundException();
var ex2 = new NotFoundException("Record with id 3 does not exist.");
var ex3 = new NotFoundException(error);     // an already instantiated error object
```

### UnauthorizedException

Used when the user does not have sufficient rights. The default HttpStatusCode is _**401**_ and the default message is _**Access denied.**_.

``` csharp
var ex = new UnauthorizedException();
var ex2 = new UnauthorizedException("You don't have the rights to update this record.");
var ex3 = new UnauthorizedException(error);     // an already instantiated error object
```

### ValidationException

Can be used when input validation fails. The default HttpStatusCode is _**400**_ (Bad Request) and the default message is _**Bad Request.**_.
The Error object can be used to list the failed validation messages.

``` csharp
var ex = new ValidationException();
var ex2 = new ValidationException("E-mail address is mandatory.");
var ex3 = new ValidationException(error);     // an already instantiated error object
```
