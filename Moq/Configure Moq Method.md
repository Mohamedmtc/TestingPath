Configure Moq Method
--------------------
there we will work on validate method which have multiple overloaded versions we will explain how to moq each version as

    bool Validate(string applicantName, int applicantAge, string applicantAddress);
    void Validate(string applicantName, int applicantAge, string applicantAddress, out bool isValid);
    void Validate(string applicantName, int applicantAge, string applicantAddress, ref IdentityVerificationStatus status);

**moq method with parameters**
first after we created the moq object and there is a method in this object we need it to return certain data when we use this object then we will moq this method
in the next method validate if it get the defined parameters it will return true

    var mockIdentityVerifier = new Mock<IIdentityVerifier>();
    mockIdentityVerifier.Setup(x => x.Validate("Sarah",25,"133 Pluralsight Drive, Draper, Utah")).Returns(true);

but if we don't care about the argument send to the validate method and we need it to always return true regardless the input we use It class

    mockIdentityVerifier.Setup(x => x.Validate(It.IsAny<string>(),It.IsAny<int>(),It.IsAny<string>())).Returns(true);

**Moq Method with out parameter**
first define a parameter with defined value to be the output of the method then setup it

    var mockIdentityVerifier = new Mock<IIdentityVerifier>();
    bool isValidOutput = true;
    mockIdentityVerifier.Setup(x => x.Validate(It.IsAny<string>(), It.IsAny<int>(),It.IsAny<string>(),out isValidOutput));
**Moq Method with ref parameter**
if we need to moq method with out parameter which will be class object, first we make delegate matching the method we need to moq as

     delegate void ValidateCallback(string applicantName, int applicantAge, string applicantAddress,ref IdentityVerificationStatus status);

then setup the method to take any ref of the class object and define the callback method which allow us to execute code when validate is called , now in the callback we can define how the ref parameter will be

    mockIdentityVerifier.Setup(x => x.Validate("Sarah", 
                                       25, 
                                       "133 Pluralsight Drive, Draper, Utah", 
                                       ref It.Ref<IdentityVerificationStatus>.IsAny))
                .Callback(new ValidateCallback(
                            (string applicantName, 
                             int applicantAge, 
                             string applicantAddress, 
                             ref IdentityVerificationStatus status) => 
                                         status = new IdentityVerificationStatus(true)));

**configure moq method to return null**

if we have interface with method which return string and we need it to return null as

    public interface INullExample
    {
    	string SomeMethod();
    }

we moq it as 

        [Test]
        public void NullReturnExample()
        {
            var mock = new Mock<INullExample>();

            mock.Setup(x => x.SomeMethod());
                .Returns<string>(null);

            string mockReturnValue = mock.Object.SomeMethod();

            Assert.That(mockReturnValue, Is.Null);
        }

Verify the method without parameters is called during test
----------------------------------------------------------
let we assume there is interface we will mock  as 

    public interface IIdentityVerifier
    {
        void Initialize();
    }
and we need to verify Initialize method called as assertion


    var mockIdentityVerifier = new Mock<IIdentityVerifier>();
    mockIdentityVerifier.Verify(x => x.Initialize());

Verify the method with parameters is called during test
-------------------------------------------------------
let we assume there is interface we will mock  as 

    public interface IIdentityVerifier
    {
        bool Validate(string applicantName, int applicantAge, string applicantAddress);
    }

and we need to verify Validate method called as assertion


    var mockIdentityVerifier = new Mock<IIdentityVerifier>();
    // using certain parameters value
    mockIdentityVerifier.Verify(x => x.Validate("Sarah",25,"133 Pluralsight Drive, Draper, Utah"));
    // or using any paramters
    mockIdentityVerifier.Verify(x => x.Validate(It.IsAny<string>(), It.IsAny<int>(),It.IsAny<string>()));

verify number of method has been called 
----------------------------------------
we use over-loaded version of verify as 

    var mockIdentityVerifier = new Mock<IIdentityVerifier>();
    mockIdentityVerifier.Verify(x => x.Initialize(),Times.Once);


Verify no unexpected calls where made
-------------------------------------
we need to verify all method called then we verify no other calls as

    mockIdentityVerifier.VerifyNoOtherCalls();
