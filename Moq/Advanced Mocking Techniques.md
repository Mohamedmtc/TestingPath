 Advanced Mocking Techniques
----------------------------
Moq Modes of Operation :
- Strict mode
- Loose Mode

Loose mode : is the default mode when creating object its properties
1. Return default value
1. won't through exceptions
1. don't require explicitly setup methods will be used

throw exception 
----------------
we can cause certain exception to test specific behaviour let's say we will fire exception at certain command as follow 

    try
    {
        _creditScorer.CalculateScore(application.GetApplicantName(),
                             application.GetApplicantAddress());
    }
    catch
    {
        application.Decline();
        return;
    }

we can do it as follow

    var mockCreditScorer = new Mock<ICreditScorer>();
    // non generic
    mockCreditScorer.Setup(x => x.CalculateScore(It.IsAny<string>(), It.IsAny<string>()))
    				.Throws(new InvalidOperationException("Test Exception"));
    // generic
    mockCreditScorer.Setup(x => x.CalculateScore(It.IsAny<string>(), It.IsAny<string>()))
    				.Throws<InvalidOperationException>();
