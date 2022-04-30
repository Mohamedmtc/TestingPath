Asserting on Null Values
------------------------
    string name = "Sarah";
    Assert.That(name, Is.Null); // fail
    Assert.That(name, Is.Not.Null); // pass
Asserting on String Values
--------------------------
    string name = "Sarah";
    Assert.That(name, Is.Empty); // fail
    Assert.That(name, Is.Not.Empty); // pass
    Assert.That(name, Is.EqualTo("Sarah")); // pass
    Assert.That(name, Is.EqualTo("SARAH")); // fail
    Assert.That(name, Is.EqualTo("SARAH").IgnoreCase); // pass
    Assert.That(name, Does.StartWith("Sa")); // pass
    Assert.That(name, Does.EndWith("ah")); // pass 
    Assert.That(name, Does.Contain("ara")); // pass
    Assert.That(name, Does.Not.Contain("Amrit")); // pass
    Assert.That(name, Does.StartWith("Sa")
    .And.EndsWith("rah")); // pass
    Assert.That(name, Does.StartWith("xyz")
    .Or.EndsWith("rah")); // pass
Asserting on Boolean Values
---------------------------
    bool isNew = true;
    Assert.That(isNew); // pass
    Assert.That(isNew, Is.True); // pass
    bool areMarried = false;
    Assert.That(areMarried == false); // pass
    Assert.That(areMarried, Is.False); // pass
    Assert.That(areMarried, Is.Not.True); // pass
Asserting within Ranges
-----------------------
    int i = 42;
    Assert.That(i, Is.GreaterThan(42)); // fail
    Assert.That(i, Is.GreaterThanOrEqualTo(42)); // pass
    Assert.That(i, Is.LessThan(42)); // fail
    Assert.That(i, Is.LessThanOrEqualTo(42)); // pass
    Assert.That(i, Is.InRange(40, 50)); // pass
    DateTime d1 = new DateTime(2000, 2, 20);
    DateTime d2 = new DateTime(2000, 2, 25);
    Assert.That(d1, Is.EqualTo(d2)); // fail
    Assert.That(d1, Is.EqualTo(d2).Within(4).Days); // fail
    Assert.That(d1, Is.EqualTo(d2).Within(5).Days); // pass
Adding Custom Message
---------------------
    bool isNew = true;
    Assert.That(isNew, Is.True); // pass
    Assert.That(isNew, Is.True,"Is new must to be true"); // Custome message
