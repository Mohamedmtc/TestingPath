Configure Moq Properties
------------------------
if there is a class or interface with property as 

    public interface ICreditScorer
    {
        int Score { get; }

        void CalculateScore(string applicantName, string applicantAddress);
    }


and we moq this class , then we need its property to return specific value as 

     var mockCreditScorer = new Mock<ICreditScorer>();
    mockCreditScorer.Setup(x => x.Score).Returns(300);

if we have nested class as 

    public interface ICreditScorer
    {
        int Score { get; }

        void CalculateScore(string applicantName, string applicantAddress);
		
		ScoreResult scoreResult { get; }
    }

    public class ScoreResult
    {
        public virtual ScoreValue ScoreValue { get; }
    }

    public class ScoreValue
    {
        public virtual int Score { get; }
    }

***manually*** solution to set property for the most inner class as

    var mockCreditScorer = new Mock<ICreditScorer>();
    var mockScoreResult = new Mock<ScoreResult>();
    var mockScoreValue = new Mock<ScoreValue>();
    
    mockScoreValue.Setup(x => x.Score).Returns(300);
    mockScoreResult.Setup(x => x.ScoreValue).Returns(mockScoreValue.Object);
    mockCreditScorer.Setup(x => x.ScoreResult).Returns(mockScoreResult.Object);

***Automatically*** solution to set property for the most inner class as

    mockCreditScorer.Setup(x => x.ScoreResult.ScoreValue.Score).Returns(300);

to achieve it we need all properties in the nested class to be ***Virtual***

Tracking changes
---------------- 
Tracking changes to certain property in mocked class and we can init it with defined value as

    var mockCreditScorer = new Mock<ICreditScorer>();
    
    mockCreditScorer.SetupProperty(x => x.Count); // tracking 
    
    mockCreditScorer.SetupProperty(x => x.Count, 10); // tracking and init with value 10

to tracking all properties in mocked class we can use

    var mockCreditScorer = new Mock<ICreditScorer>();
    mockCreditScorer.SetupAllProperties();

verify property get or set
--------------------------
let assume the following interface

    public interface ICreditScorer
    {
    	int Score { get; }
    }

 and we need to verify reading property value as

    var mockCreditScorer = new Mock<ICreditScorer>();
    // verify if readed
    mockCreditScorer.VerifyGet(x => x.Score); 
    // verify if readed once
    mockCreditScorer.VerifyGet(x => x.Score,Times.Once); 

as if we need to verify changing it value  as 

    var mockCreditScorer = new Mock<ICreditScorer>();
    // verify if set with any value
    mockCreditScorer.VerifySet(x => x.Score= It.IsAny<int>());
    // verify if set once 
    mockCreditScorer.VerifyGet(x => x.Score= It.IsAny<int>() ,Times.Once); 
    // verify if set with defined value value
    mockCreditScorer.VerifySet(x => x.Score= 1);