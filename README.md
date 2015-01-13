<img src="http://i.imgur.com/0RX5GMK.png"/>

*C# 6 equivalents in C# 5 by [@byteblastdev](https://twitter.com/ByteBlastDev). Inspired by [@addyosmani's](https://github.com/addyosmani) [document](https://github.com/addyosmani/es6-equivalents-in-es5) . Contributions are welcome.*

## Auto-property initializers

Auto-property initializers can be used to succinctly associate a default value with a property. They are syntactically similar to field initializers. 

C# 6: 
```c#
public class User
{
    public Guid Id { get; set; } = Guid.NewGuid();
    public List<string> Answers { get; set; } = new List<string>();
}
```
C# 5: 
```c#
public class User
{
    public Guid Id { get; set; }
    public List<string> Answers { get; set; }

    public User()
    {
        Id = Guid.NewGuid();
        Answers = new List<string>();
    }
}
```

## Getter-only properties

Getter-only properties - as the name would suggest - allow you to define a read-only property much more succinctly than ever before.

C# 6: 
```c#
public class User
{
    public Guid Id { get; };
}
```
C# 5:
```c#
public class User
{
    private readonly Guid _id;
    public Guid Id
    {
        get { return _id; }
    }
}
```

## Parameterless struct constructors

It used to be erroneous to define a parameter-less constructor for a `struct` type. This is now allowed.

C# 6: 
```c#
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
```

C# 5: 
```c#
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
```
## Using static

The using static feature allows you to refer to a static class at the top of your code file and then subsequently access all of that classes static members without full qualification.

C# 6:
```c#
using System;
using static System.Console;

public class Program
{
    private static void Main()
    {
        Title = "Demo";
        var name = ReadLine();
        ForegroundColor = ConsoleColor.Red;
        Write("Hello ");
        ForegroundColor = ConsoleColor.White;
        Write(name);
        ReadKey();
    }
}
```

C# 5:
```c#
using System;
	
public class Program
{
	private static void Main()
	{
		Console.Title = "Demo";
		var name = Console.ReadLine();
		Console.ForegroundColor = ConsoleColor.Red;
		Console.Write("Hello ");
		Console.ForegroundColor = ConsoleColor.White;
		Console.Write(name);
		Console.ReadKey();
	}
}
```
It should be noted that the syntax rules for using static are expected to change prior to the final release.

## Expression-bodied members 

Expression-bodied members allow you to emit syntax (such as braces and semicolons) and keywords (such as `return`) that the compiler can easily infer when the whole of the body is only a single expression.

C# 6:

```c#
public class Answer
{
    private List<string> _voters = new List<string>();

    public bool CanVote(string username) => !_voters.Contains(username);
}
```

C# 5:
```c#
public class Answer
{
    private List<string> _voters = new List<string>();

    public bool CanVote(string username)
    {
        return !_voters.Contains(username);
    }
}
```
## Index initializers

Index initializers offer a more elegant way to give an initial set of elements to dictionaries and other similarly indexed objects.

C# 6: 
```c#
var postData = new Dictionary<string, string>
{
    ["username"] = "admin",
    ["password"] = "password",
    ["authToken"] = "123"
}
```

C# 5: 
```c#
var postData = new Dictionary<string, string>();
postData["username"] = "admin";
postData["password"] = "password";
postData["authToken"] = "123";
```

It should be noted that index initializers are not necessarily an alternative to the collection initializers syntax (introduced with C# 3.0).

## Await in catch/finally

It used to be erroneous to use the `await` keyword in a `catch`  or `finally` block. This is now allowed.

C# 6:
```c#
var logger = new ExceptionLogger();
try
{
}
catch (Exception exception)
{
    await logger.LogAsync(exception);
    throw;
}
```
C# 5:
```c#
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
```	
## Exception filters

Exception filters allow you to conditionally `catch` exceptions according to a *filter*. If the filter expression returns `false` then the exception will not be handled by that particular `catch` block. 

C# 6: 
```c#
try
{

}
catch (AggregateException ex) if (ex.Data.Count > 1)
{
}
```
C# 5:

As far as I know, there is no direct equivalent. (You should feel to correct this section if I am wrong.)

## Null-conditional operator

The null-conditional operator removes the need for tedious null-checking. 


C# 6:
```c#
var user = User.Find("byteblast");

if (user?.Answers != null)
{
    Console.WriteLine("Answer count: " + user.Answers.Count);
}
```

C# 5:

```c#
var user = User.Find("byteblast");

if (user != null && user.Answers != null)
{
    Console.WriteLine("Answer count: " + user.Answers.Count);
}
```

## String interpolation

String interpolation allows you to *interpolate* (insert) values  in the string in a natural and highly readable way. 

C# 6: 
```c#
var expected = 201;
var actual = 404;
throw new Exception("Expected status code to be \{expected}, but instead was \{actual}.");
```

C# 5:
```c#
var expected = 201;
var actual = 404;
throw new Exception(string.Format("Expected status code to be {0}, but instead was {1}.", expected, actual));
```

It should be noted that the syntax rules for string interpolation are expected to change prior to the final release.


##nameof operator

The `nameof` operator takes an object (such as a variable or property) and returns that object's name as a string. 

In a nutshell, it removes the need to define hard-coded string literals containing variable names, whose values may not be updated after a botched or automated refactoring.

C# 6:
```c#
public static User Find(string username)
{
    if (username == null)
    {
        throw new ArgumentNullException(nameof(username));
    }

    // Contrived.
    return null;
}
```

C# 5:
```c#
public static User Find(string username)
{
    if (username == null)
    {
        throw new ArgumentNullException("username");
    }

    // Contrived.
    return null;
}
```
It should be noted that the syntax rules for the `nameof` operator are expected to change prior to the final release.

## About

Inspired by:

- [ECMAScript 6 equivalents in ES5](https://github.com/addyosmani/es6-equivalents-in-es5)

Additional learning resources:

- [Langauge features in C# 6](https://roslyn.codeplex.com/wikipage?title=Language%20Feature%20Status&referringTitle=Documentation)
- [C# feature descriptions](https://www.codeplex.com/Download?ProjectName=roslyn&DownloadId=930852)
- [What's new in C# 6.0? by Scott Allen](http://www.ndcvideos.com/#/app/video/2591)
- [C# 6 in action](http://codeblog.jonskeet.uk/2014/12/08/c-6-in-action/)


## License

This work is licensed under a [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/) License.



