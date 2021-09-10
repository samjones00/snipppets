# snipppets


                var winterTyreOptionCodes = new[] { "1,2,3" };

                if (optionCodes.Intersect(winterTyreOptionCodes).Any())
                {
                    pricing.ServiceRates = new List<ServiceRate>
                    {
                        new ServiceRate
                        {
                            ServiceName = "winterTyres"
                        }
                    };
                }

---------------------

    public static class EnumerableExtensions
    {
        public static T Second<T>(this IEnumerable<T> collection) => collection.Skip(1).FirstOrDefault();
        public static T Third<T>(this IEnumerable<T> collection) => collection.Skip(2).FirstOrDefault();
    }

---------------------

 public static bool HasCarProperty(this Campaign campaign, Func<Car, IComparable> getProperty)
        {
            var car = campaign.I18nItems[0].Car;
            var property = getProperty(car);
  
            switch (getProperty(car).GetType().Name)
            {
                case nameof(String):
                    return !string.IsNullOrEmpty((string)property);
                case nameof(Int32):
                    return (int)property > 0;
            }

            return false;
        }

 if (campaign.HasCarProperty(x => x.Model))
                parts.Append(campaign.I18nItems[0].Car.Model).Append(" ");
---------------------

act.Should().Throw<InvalidOperationException>()
    .WithInnerException<ArgumentException>()
    .WithMessage("whatever");

---------------------
 public static class CustomAssertions
    {
        public static AndConstraint<StringAssertions> BeGuid(this StringAssertions assertions, string because = "", params object[] becauseArgs)
        {
            var isGuid = Guid.TryParse(assertions.Subject, out _);

            Execute.Assertion
               .ForCondition(isGuid)
               .BecauseOf(because, becauseArgs)
               .FailWith("Expected a GUID converted to a string{reason}, but found {0}.", assertions.Subject);

            return new AndConstraint<StringAssertions>(assertions);
        }
    }

------------------------------

create fixture from class using constructor

            var pricingRequest = _fixture.Build<PricingRequest>().FromFactory(() => new PricingRequest(null, _fixture.Create<string>())).Create();


_fixture.Build<Car>().With(x => x.Options, new List<Option>()
                  {
                        new Option
                        {
                            UniqueId = expected,
                            OptionCategory = OptionCategory.VehicleCode,
                        }
                    }).Create()

-------------

public static IEnumerable<Domain.Models.OmegaPlus.Option> RemoveOptions(this IEnumerable<Domain.Models.OmegaPlus.Option> originalOptions, params string[] optionsToRemove)
        {
            return originalOptions.Except(originalOptions.Where(x => optionsToRemove.Contains(x.OptionCategory))).ToList();
        }

------------------------

 public static class MoqExtensions
    {
  public static void VerifyLog<TType>(this Mock<ILogger<TType>> mockLogger, LogLevel logLevel, string message)
        {
            mockLogger.Verify(
                   x => x.Log(
                       logLevel,
                       It.IsAny<EventId>(),
                       It.Is<It.IsAnyType>((o, t) => string.Equals(message, o.ToString(), StringComparison.InvariantCultureIgnoreCase)),
                       It.IsAny<Exception>(),
                       (Func<It.IsAnyType, Exception, string>)It.IsAny<object>()),
                   Times.Once);
        }
    }

-----------------

 Action<string, string> AddService = (string serviceName, string translation) =>
            {
                if (pricing.ServiceRates.Any(x => x.ServiceName.Equals(serviceName, StringComparison.InvariantCultureIgnoreCase)))
                {
                    services.Add(translation);
                }
            };

------------------

local variable for use in linq query

static bool query(PricingCustomAttribute item) => item.Key == Common.DiscountPercentage;


-------------------

     public static IConfigurationBuilder AddJsonFile(this IConfigurationBuilder configurationBuilder, string searchPattern, bool optional, bool reloadOnChange)
        {
            foreach (var jsonFilename in Directory.EnumerateFiles(Directory.GetCurrentDirectory(), searchPattern, SearchOption.AllDirectories))
                JsonConfigurationExtensions.AddJsonFile(configurationBuilder, jsonFilename, optional, reloadOnChange);

            return configurationBuilder;
        }
