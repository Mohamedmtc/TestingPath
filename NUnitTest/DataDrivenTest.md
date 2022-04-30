Data Driven Test : create single test with multiple cases
---------------------------------------------------------

    [Test]
    [TestCase(200_000, 6.5, 30, 1264.14)]
    [TestCase(200_000, 10, 30, 1755.14)]
    [TestCase(500_000, 10, 30, 4387.86)]
    public void CalculateCorrectMonthlyRepayment(decimal principal,
    decimal interestRate,int termInYears,decimal expectedMonthlyPayment)
    {
	    var sut = new LoanRepaymentCalculator();
	    
	    var monthlyPayment = sut.CalculateMonthlyRepayment(
	     new LoanAmount("USD", principal), 
	     interestRate, 
	     new LoanTerm(termInYears));
	    
	    Assert.That(monthlyPayment, Is.EqualTo(expectedMonthlyPayment));
    }

Simplified Test Case : using expected return as test case input
---------------------------------------------------------------

    [Test]
    [TestCase(200_000, 6.5, 30, ExpectedResult = 1264.14)]
    [TestCase(200_000, 10, 30, ExpectedResult = 1755.14)]
    [TestCase(500_000, 10, 30, ExpectedResult = 4387.86)]
    public decimal CalculateCorrectMonthlyRepayment_SimplifiedTestCase(decimal principal,
     decimal interestRate,
     int termInYears)
    {
	    var sut = new LoanRepaymentCalculator();
	    
	    return sut.CalculateMonthlyRepayment(new LoanAmount("USD", principal),
										     interestRate,
										     new LoanTerm(termInYears));
    }

Using Test Data Class as Input for all test cases
-------------------------------------------------
we will cover test data with expected return and also without

    public class MonthlyRepaymentTestData
    {
	    public static IEnumerable TestCases
	    {
		    get
		    {
			    yield return new TestCaseData(200_000m, 6.5m, 30, 1264.14m);
			    yield return new TestCaseData(500_000m, 10m, 30, 4387.86m);
			    yield return new TestCaseData(200_000m, 10m, 30, 1755.14m);
		    }
	    }
    }

----------

    public class MonthlyRepaymentTestDataWithReturn
    {
	    public static IEnumerable TestCases
	    {
		    get
		    {
			    yield return new TestCaseData(200_000m, 6.5m, 30).Returns(1264.14);
			    yield return new TestCaseData(200_000m, 10m, 30).Returns(1755.14);
			    yield return new TestCaseData(500_000m, 10m, 30).Returns(4387.86);
		    }
	    }
    }

----------

    [Test]
    [TestCaseSource(typeof(MonthlyRepaymentTestData), "TestCases")]
    public void CalculateCorrectMonthlyRepayment_Centralized(decimal principal,
     decimal interestRate,
     int termInYears,
     decimal expectedMonthlyPayment)
    {
	    var sut = new LoanRepaymentCalculator();
	    
	     var monthlyPayment = sut.CalculateMonthlyRepayment(
							     new LoanAmount("USD", principal),
							     interestRate,
							     new LoanTerm(termInYears));
	    
	    Assert.That(monthlyPayment, Is.EqualTo(expectedMonthlyPayment));
    }
    
    [Test]
    [TestCaseSource(typeof(MonthlyRepaymentTestDataWithReturn), "TestCases")]
    public decimal CalculateCorrectMonthlyRepayment_CentralizedWithReturn(decimal principal,
     decimal interestRate,
     int termInYears)
    {
	    var sut = new LoanRepaymentCalculator();
	    
	    return sut.CalculateMonthlyRepayment(
							     new LoanAmount("USD", principal),
							     interestRate,
							     new LoanTerm(termInYears));
    }

Generate data for testing using Values and Sequential
-----------------------------------------------------
case we will create 27 combination using **values** only so we can't support the test with expected Result

    [Test]
    public void CalculateCorrectMonthlyRepayment_Combinatorial(
    [Values(100_000, 200_000, 500_000)]decimal principal,
    [Values(6.5, 10, 20)]decimal interestRate,
    [Values(10, 20, 30)]int termInYears)
    {
	    var sut = new LoanRepaymentCalculator();
	    
	    var monthlyPayment = sut.CalculateMonthlyRepayment(
	    new LoanAmount("USD", principal), interestRate, new LoanTerm(termInYears));
    }

we will use **Sequential** to make it 3 combinations only so we can support the expected Result

    [Test]
    [Sequential]
    public void CalculateCorrectMonthlyRepayment_Sequential(
    [Values(200_000, 200_000, 500_000)]decimal principal,
    [Values(6.5, 10, 10)]decimal interestRate,
    [Values(30, 30, 30)]int termInYears,
    [Values(1264.14, 1755.14, 4387.86)]decimal expectedMonthlyPayment)
    {
	    var sut = new LoanRepaymentCalculator();
	    
	    var monthlyPayment = sut.CalculateMonthlyRepayment(
	    new LoanAmount("USD", principal), interestRate, new LoanTerm(termInYears));
	    
	    Assert.That(monthlyPayment, Is.EqualTo(expectedMonthlyPayment));
    }

we can use **Range** but will act as Values so we can't support expected Result 

    [Test]
    public void CalculateCorrectMonthlyRepayment_Range(
    [Range(50_000, 1_000_000, 50_000)]decimal principal, // start with 50k loop until 1 Million with step 50k
    [Range(0.5, 20.00, 0.5)]decimal interestRate,
    [Values(10, 20, 30)]int termInYears)
    {
	    var sut = new LoanRepaymentCalculator();
	    
	    sut.CalculateMonthlyRepayment(
	    new LoanAmount("USD", principal), interestRate, new LoanTerm(termInYears));
    }