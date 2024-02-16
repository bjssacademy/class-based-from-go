# Structs & Records

Structs exist within C#, but they are used very differently to Go. In Go structs are the only class-type object we can use, but in C# we have both, and the use cases are very different.

Structs are commonly used to represent small, atomic pieces of data, such as numeric types (int, double, etc.), coordinates, points, rectangles, timestamps, and other "lightweight data structures", and only this type of data.

Structs have advantages over classes, but for now we'll only ever be using structs for very specific instances, otherwise we'll be using classes or records.

Records are *immutable* classes. Immutable means their properties cannot be modified after creation. This is useful for when we are passing things like data with little or no behaviour (methods).

## Multiple return types
### With a struct

C# doesn't support multiple return types as Go does. Instead we can use a struct or a record (or a class) as the return type.

```c#
public struct SpeedResult
{
    public double Mph { get; private set; }
    public double Kph { get; private set; }

    public SpeedResult(double mph, double kph)
    {
        Mph = mph;
        Kph = kph;
    }
}
```

Looks almost identical to a class, doesn't it? Structs can be abused like a class too, so be careful!

We've created a constructor as the only way to set MPH and KPH.

Now, to use this result we need to update our `Speed` property in the `Academy500Roadster` to return the `SpeedResult` type instead on an `int`. 

We're going to assume the speed is in MPH when it is stored as an int, so we will need to provide the KPH by multiplying it by 1.60934:

```c#
public SpeedResult Speed
{
    get
    {
        SpeedResult result = new SpeedResult(_speed, _speed * 1.60934);
        return result;
    }
}
```

And output the result in or Program.cs, adding `.Mph`:

```c#
Console.WriteLine("The car is going " + roadster.Speed.Mph + "mph and the headlights are " + roadster.Headlights.Status);
```

Is this the best thing to do? Probably not, but it shows you how structs work!

### With a record

Records are a bit more succinct that structs or classes. Here's a record declaration for SpeedResult:

```c#
public record SpeedResultRecord(double Mph, double Kph);
```

When defining a record using the `record` keyword, you don't need to provide a body explicitly like you do with classes. Instead, you specify the properties directly in the record declaration itself. The properties are automatically generated!

We can use this in exactly the same way in or Academy500Roadster:

```c#
public SpeedResultRecord Speed
{
    get
    {
        SpeedResultRecord result = new SpeedResultRecord(_speed, _speed * 1.60934);
        return result;
    }
}
```

> Remember, records are immutable by default, so their properties cannot be updated. Their properties are *readonly*.

## Readonly

We talked about *immutability*, or being *readonly*. We've talked about making our properties readonly by using `{ get; private set; }` but you can just use `{ get; } `on its own to do the same.

Fields that are readonly can *only* be set in the constructor, and cannot be modified later by any calling code.

To make fields readonly we have to use the `readonly` keyword.

```c#
Public class Person
{
    public readonly int Age = 10; //default age for field of 10

    public Person()
    {
        // Readonly field can ONLY be set in the constructor
        Age = 20;
    }
}
```

> That's not a great example, but hopefully you get the idea!