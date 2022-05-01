Preparing Moq
-------------
A mock object is an object that can act as a real object but can be controlled in test code. Moq is a library that allows us to create mock objects in test code. It is also available in NuGet. This library also supports .NET Core.
 
The Moq library can be added to test projects either by package manager or .NET CLI tool.
Using Package Manager

        PM> Install-Package Moq  
Using .net core CLI

        dotnet add package Moq    

in addition too test things in isolation, things should also be tested working together with real dependencies (Integration, API , UI tests , etc.)

How to use
----------
we use moq like 

    var mockIdentityVerifier = new Mock<IIdentityVerifier>();
    var mockCreditScorer = new Mock<ICreditScorer>();

to allow moq to create moq for all nested properties  we can using

    var mockCreditScorer = new Mock<ICreditScorer>{ DefaultValue=DefaultValue.Mock };


