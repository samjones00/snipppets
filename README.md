# Snipppets

## Autofixture
```cs
var fixture = new Fixture();
fixture.Build<Car>().With(x => x.Options, new List<Option> () {
  new Option {}
}).Create();
```
-----

```cs
var pricingRequest = _fixture.Build<PricingRequest>().FromFactory(() => new PricingRequest(null, _fixture.Create<string>())).Create();

```


## FluentAssertions

```cs
public static class CustomAssertions {
  public static AndConstraint<StringAssertions>BeGuid(this StringAssertions assertions, string because = "", params object[] becauseArgs) {
    var isGuid = Guid.TryParse(assertions.Subject, out _);

    Execute.Assertion.ForCondition(isGuid).BecauseOf(because, becauseArgs).FailWith("Expected a GUID converted to a string{reason}, but found {0}.", assertions.Subject);

    return new AndConstraint < StringAssertions > (assertions);
  }
}
```
----
```cs
act.Should().Throw<InvalidOperationException>()
    .WithInnerException<ArgumentException>()
    .WithMessage("whatever");
```

## C#
```cs
public static IConfigurationBuilder AddJsonFile(this configurationBuilder, string searchPattern, bool optional, bool reloadOnChange) {
  foreach(var jsonFilename in Directory.EnumerateFiles(Directory.GetCurrentDirectory(), searchPattern, SearchOption.AllDirectories))
  JsonConfigurationExtensions.AddJsonFile(configurationBuilder, jsonFilename, optional, reloadOnChange);

  return configurationBuilder;
}
```
----------------
```cs
static bool query(PricingCustomAttribute item) => item.Key == Common.DiscountPercentage;
```
-----------
```cs
if (campaign.HasCarProperty(x => x.Model))
                parts.Append(campaign.I18nItems[0].Car.Model).Append(" ");
```

```cs
public static bool HasCarProperty(this Campaign campaign, Func<Car, IComparable> getProperty) {
  var car = campaign.I18nItems[0].Car;
  var property = getProperty(car);

  switch (getProperty(car).GetType().Name) {
  case nameof(String):
    return ! string.IsNullOrEmpty((string) property);
  case nameof(Int32):
    return (int) property > 0;
  }

  return false;
}

```
----------

```cs
var winterTyreOptionCodes = new [] {
  "1,2,3"
};

if (optionCodes.Intersect(winterTyreOptionCodes).Any()) {
  pricing.ServiceRates = new List < ServiceRate > {
    new ServiceRate {
      ServiceName = "winterTyres"
    }
  };
}
```
## Linq

```cs
public static class EnumerableExtensions {
  public static T Second < T > (this IEnumerable < T > collection) => collection.Skip(1).FirstOrDefault();
  public static T Third < T > (this IEnumerable < T > collection) => collection.Skip(2).FirstOrDefault();
}
```

## Moq
```cs
public static class MoqExtensions {
  public static void VerifyLog<TType>(this Mock<ILogger<TType>> mockLogger, LogLevel logLevel, string message) {
    mockLogger.Verify(
      x => x.Log(
        logLevel,
        It.IsAny<EventId>(),
        It.Is<It.IsAnyType>((o, t) => string.Equals(message, o.ToString(), StringComparison.InvariantCultureIgnoreCase)),
        It.IsAny<Exception>(),
        (Func<It.IsAnyType,Exception,string>)It.IsAny<object>()),
      Times.Once);
  }
}
```