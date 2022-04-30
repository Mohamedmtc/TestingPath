Create test project
-------------------
first create class library project then add the following Nuget Packages

    <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.1.0" />
    <PackageReference Include="NUnit" Version="3.13.3" />
    <PackageReference Include="NUnit3TestAdapter" Version="4.2.1" />
     </ItemGroup>
Test attributes
---------------
    [TestFixture] : Mark a class that contains tests
    [Test] : Mark a method as a test
    [Category("Category Name")] : Organize tests into categories over files in execution also
    [TestCase] : Data driven test cases
    [Values] : Data driven test parameters
    [Sequential] : How to combine test data
    [SetUp] : Run code before each test
    [OneTimeSetUp] : Run code before first test in class
    [Ignore("Why ignore the test message")] : Skip the current test methos or test class

Constraint Model of assertions (newer)
--------------------------------------
    Assert.That(sut.Years, Is.EqualTo(1));
    Assert.That(test result, constraint instance);

Classic Model of assertions (older)
-----------------------------------

    Assert.AreEqual(1, sut.Years);
    Assert.NotNull(sut.Years);
    Assert.Xyz(…)

Different Testing Scenarios
---------------------------
1. Business Logic
1. Exercise all code branches
1. Bad data/input

Logical phases 
--------------
- Arrange: set up test object(s), initialize test data, etc..
- Act: call method, set property, etc..
- Assert: compare returned value/end state with expected



