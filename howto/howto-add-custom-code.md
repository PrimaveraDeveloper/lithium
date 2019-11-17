# How to add custom code to a microservice

This guide describes how custom code should be added to a microservice solution.

An important part of the code in a microservice is generated using text templates. Most of the times this code just needs to be "finished" with custom code - the monitoring controller is an example of that - but there are also scenarios where generated code needs to be extended and new interfaces, classes, etc. need to be created from scratch. This is all custom code.

## `CustomCode` Folder

All custom code for each of the microservices projects should be placed inside a folder named `CustomCode`. Avoid adding this code on other locations in the project because that will result in a confusion as the service grows in complexity.

## Mind the Namespace

When you add a new class to a folder in Visual Studio, it will suggest a namespace for the new class containing the name of the folder. This is not a good idea for this `CustomFolder`, either when you are customizing generated code (the namespace will need to match the one used in the generated code) or not (`CustomCode` does not add meaning to the namespace of the new class).

Having said, the first thing you need to do after adding a code file (interface, class, etc.) to the `CustomCode` folder is to correct the namespace suggested.

## CustomCode Organization

## Overriding Generated Code

## Extending Generated Code