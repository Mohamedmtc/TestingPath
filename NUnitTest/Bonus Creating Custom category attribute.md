Creating custom category attribute 
-----------------------------------
first create class for the new category

    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = false)]
    class ProductComparisonAttribute : CategoryAttribute
    {

    }
    
second over test method or test class define this category 


    [Test]
    [ProductComparison]
    public void ReturnTermInMonths()
    {
	    var sut = new LoanTerm(1);
	    
	    Assert.That(sut.ToMonths(), Is.EqualTo(12));
    }
    }