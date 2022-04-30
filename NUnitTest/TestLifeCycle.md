Testing Life Cycle
------------------
![Testing Life Cycle](https://raw.githubusercontent.com/Mohamedmtc/TestingPath/master/NUnitTest/TestExcutionLifeCycle.png)

Setup Method
------------
run before each test execution to init global objects and prepare data as 

    private List<LoanProduct> products;
    private ProductComparer sut;

    [SetUp]
    public void Setup()
    {
	    products = new List<LoanProduct>
		    {
		    new LoanProduct(1, "a", 1),
		    new LoanProduct(2, "b", 2),
		    new LoanProduct(3, "c", 3)
		    };
	    sut = new ProductComparer(new LoanAmount("USD", 200_000m), products);
    }
TearDown Method
---------------
run after each test execution to dispose created objects  as 

    [TearDown]
    public void TearDown()
    {
	    // Runs after each test executes
	    // sut.Dispose();
    }


On time Setup Method
--------------------

run one time before all test execution to init global objects and prepare data as 

    private List<LoanProduct> products;
    private ProductComparer sut;
    
    [OneTimeSetUp]
    public void OneTimeSetUp()
    {
	    // Simulate long setup init time for this list of products
	    // We assume that this list will not be modified by any tests
	    // as this will potentially break other tests (i.e. break test isolation)
	    products = new List<LoanProduct>
	    {
		    new LoanProduct(1, "a", 1),
		    new LoanProduct(2, "b", 2),
		    new LoanProduct(3, "c", 3)
	    };
    }

OnTime TearDown Method
----------------------

run one time before all test execution  to dispose created objects  as 

    [TearDown]
    public void TearDown()
    {
        // Run after last test in this test class (fixture) executes
        // e.g. disposing of shared expensive setup performed in OneTimeSetUp
        // products.Dispose(); e.g. if products implemented IDisposable
    }