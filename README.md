<img src="http://i.imgur.com/E2hhBff.png"/>
# C# 6 Equivalents in C# 5

*C# 6 equivalents in C# 5 by [@byteblastdev](https://twitter.com/ByteBlastDev). Inspired by [@addyosmani's](https://github.com/addyosmani) [document](https://github.com/addyosmani/es6-equivalents-in-es5) . Contributions are welcome.*


## Auto-property initializers

Auto-property initializers allow you to succinctly assign default values to your properties.

C# 6:

    public class Customer
    {
        public List<string> Orders { get; set; } = new List<string>();
    }
    
C# 5:

    public class Customer
    {
        public List<string> Orders { get; set; };
    
        public Customer()
        {
            Orders = new List<string>();
        }
    }
    
## Getter-only auto-properties	

Getter-only auto-properties	 remove the need to define a backing field when you want to make make a property read-only. 

C# 6: 

    public class Customer
    { 
        public string Name { get; } = "Alex"
    }

C# 5:

    public class Customer
    {
        private readonly string _name = "Alex";
        public string Name
        {
            get { return _name; }
        }
    }

## Paramaterless struct constructors

Up until C# 6 it was invalid to define a parameterless constructor for a structure. 

C# 6:

    public struct Rational
    {
        private long numerator;
        private long denominator;
    
        public Rational()
        {
            numerator = 0;
            denominator = 1;
        }
    }

C# 5: 

    public struct Rational 
    {
        private long numerator;
        private long denominator;
    
        private Rational(long numerator, long denominator)
        {
            this.numerator = numerator;
            this.denominator = denominator;
        }
    
        public static Rational Create()
        {
            return new Rational(0, 1);
        }
    }

## Using static members	

The using static feature of C# 6 allows you to specify a static class in a using directive and then subsequently access its public static members without full qualification.


C# 6:

    using System.Math;
    public class Program
    {
        private static void Main(string[] args)
        {
            var largest = Max(Abs(10.5), Abs(20.1));
        }
    }


C# 5: 

    using System;
    public class Program
    {
        private static void Main(string[] args)
        {
            var largest = Math.Max(Math.Abs(10.5), Math.Abs(20.1));
        }
    }
    
## Expression-bodied members

Expression-bodied members allow you to emit unnecessary noise that the compiler can easily infer for expressive methods and properties. 

C# 6:

    private static string PrependTimestamp(string message)
        => string.Format("[0] {1}", DateTime.Now, message));
        
C# 5:

    private static void Log(string message)
    {
        return string.Format("[0] {1}", DateTime.Now, message));
    }

    
## Dictionary initializer	

Dictionary initializes offer a more elegant way to give a collection an initial set of elements.

C# 6: 

    var postData = new Dictionary<string, string>
    {
        ["username"] = "admin",
        ["password"] = "password",
        ["authToken"] = "123"
    };


C# 5: 

    var postData = new Dictionary<string, string>
    {
        {"username", "admin"},
        {"password", "password"},
        {"authToken", "123"},
    };

## Await in catch/finally

In earlier iterations of the language, it was not possible to use the await keyword in a catch or finally block. Now you can. 

C# 6: 

    var logger = new ExceptionLogger();
    try
    {
    }
    catch (Exception exception)
    {
        await logger.LogAsync(exception);
        throw;
    }
    
C# 5: 

    var logger = new ExceptionLogger();
    ExceptionDispatchInfo capturedException = null;
    try
    {
    }
    catch (Exception exception)
    {
        capturedException = ExceptionDispatchInfo.Capture(exception);
    }
    
    if (capturedException != null)
    {
        await logger.LogAsync(capturedException.SourceException);
        capturedException.Throw();
    }

## Exception filters	

## Null-conditional operators

Excessive null checking can drown the crux of your code. The null-conditional operator provides you with a way to short-circuit null checks.

C# 6: 

    var product = GetProduct(1);
    Console.WriteLine(product?.Supplier?.Phone);
    
C# 5:

    var product = GetProduct(1);
    if (product != null && product.Supplier != null && product.Supplier.Phone != null)
    {
        Console.WriteLine(product.Supplier.Phone);
    }

Note that the `Product` comes from a database - it could be null if a product with the given id does not exist.

## String interpolation

String interpolation enables you to refer to variables from the logical position in the string. The compiler takes the original string and the _interpolated_ values and converts it to a call to `string.Format`. 

C# 6:

    var expected = 201;
    var actual = 404;
    throw new Exception("Expected status code to be \{expected}, but instead was \{actual}.");

C# 5: 

    var expected = 201;
    var actual = 404;
    throw new Exception(string.Format("Expected status code to be {0}, but instead was {1}.", expected, actual));

Note that the string interpolation syntax is likely to change before the stable release. 

## nameof operator

The `nameof` operator offers a refactor-proof way of referring to a variable's name. 

C# 6:

    private static int OccurancesOfWord(string input, string word)
    {
        if (input == null)
            throw new ArgumentNullException(nameof(input));
        if (word == null)
            throw new ArgumentNullException(nameof(word));
    
        return 42;
    }


C# 5:

    private static int OccurancesOfWord(string input, string word)
    {
        if (input == null)
            throw new ArgumentNullException("input");
        if (word == null)
            throw new ArgumentNullException("input");
    
        return 42;
    }

Note that the nameof operator syntax is likely to change before the stable release. 



    