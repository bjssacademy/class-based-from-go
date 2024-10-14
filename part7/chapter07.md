# Composition

We looked at interfaces in a [previous chapter](/part4/chapter04.md).

Interfaces are things that are implemented. When a class implements an interface, we can say it "is a" type.

So our `MP3Media` class "is a" `IMedia` type. In this way we can use anything that implements that interface exactly the same without knowing exactly what *concrete* type it is.

Composition is different again - composition is "has a". We build the object by providing parts.

> Interfaces and composition can be used together - it's not one or the other.

## An Example

Let's look at the concept of a car. There are many different makes and models, but every one of them "is a" car. They share common abilities:
- Accelerate()
- Brake()

They also share common properties, they "have a":
- Engine
- Wheels

And so on.

So we know if I have an "Academy 500 Roadster" (the car of choice for mid-life crises), then I know that like every other car in existence it will be able to accelerate and I will be able to apply the brakes.

I also know it will have properties, such as wheels, seats, engine, and many others.

Where it differs is that whilst there are things that are common to *all* cars (the contract, or interface), there are things that are common yet specific (what that car's specification is - eg. 1.6L engine, 25 inch chrome wheels, leather seats).

### My head hurst, can I see some code?

Let's try that with code. First create a new folder named `cars`, and add a new interface named `IEngine`.

```c#
public interface IEngine
{
    public Double EngineSize { get; }

}
```

> This is the first time we've had a *property* in an interface. You'll notice we only have the getter `get` in the accessor. This is because we cannot have our setter as `private set` in an interface, and we don't want it to be public. We have to do this in the implementation, which we will see in a bit.

Now create another interface called `ICar` :

```c#
public interface ICar
{
    public IEngine Engine { get; }

    public void Accelerate();
    public void Brake();
}
```

> Note that the property Engine has the data type IEngine. This means we can use *any* engine in this car, not just a specific engine...

Now create a class called `Academy500Roadster` that implements `ICar`:

```c#
public class Academy500Roadster : ICar
{
    public IEngine Engine { get; private set; }

    public Academy500Roadster(IEngine engine)
    {
        Engine = engine;
    }

    public void Accelerate()
    {
        throw new NotImplementedException();
    }

    public void Brake()
    {
        throw new NotImplementedException();
    }
}
```

> If you implemented the interface through the IDE, you'll have to replace the NotImplementedException. By convention the IDE will moan at you about undeclared properties and suggest you make them nullable. There is almost always a better way!

So here we have our specific type of car, and it has an Engine property which can only be set via the constructor. Imagine getting a new car and having to install the engine yourself. That's what we mean by the object being ready to use when it is first instantiated.

A reminder about constructors:

- special method that is called once
- when you create an object
- it has one job: get the object ready to be used by your app

Now let's create our concrete Engine instance. Create a class called `Engine1600` and make it implement `IEngine`:

```c#
public class Engine1600 : IEngine
{
    public double EngineSize { 
        get { return 1600.0D; } 
    }
}
```

> **Wait! What are you doing here?** Okay, I've cheated a bit, I know more C# than you, and I haven't fully explained getters and setters other than they are "magic".
>
> [Here's a link covering them](https://www.w3schools.com/cs/cs_properties.php)
>
> Also the "D" following the "1600" is the C# way of marking out a Double, as supposed to a Float or a Long. They are demarkated with a F and a L respectively.  

Right, so now we have our getter that provides a "canned" response every time it is called of 1600 as a double. There's no setter specified, so the Engine1600 class is ready to be used when it is instantiated. Good object, like it.

Now, finally, we can go back to our `Program.cs` file and build our car!

```c#
using academy_demo.cars;

IEngine engine = new Engine1600();

Academy500Roadster roadster = new Academy500Roadster(engine);

Console.WriteLine(roadster.Engine.EngineSize);
```

- `IEngine engine = new Engine1600();` Create a new instance of the Engine1600 object
- `Academy500Roadster roadster = new Academy500Roadster(engine);` Create a new instance of the Academy500Roadster with the engine provided
- `Console.WriteLine(roadster.Engine.EngineSize);` on the roadster object, access its Engine object property, and get the EngineSize property of the Engine object!

We now have a car, with a 1600CC engine in it! We have composed our car - we created the base, and then gave it the engine size we wanted for this particular model.

## Lab

The Academy500Roadster has two engine options, with an engine size of 1600 or 2000. 

Create a new concrete class for the new engine size, and create two Academy500Roadsters - one with a 1600 engine and one with a 2000 engine.

***

## Let's make it a bit more real

Okay, so having different engine size is fine, but it's not actually giving us much. Let's create a maximum speed for the engine:

```c#
public interface IEngine
{
    public Double EngineSize { get; }
    public int MaxSpeed { get; }

}
```
Now we will need to implement this in our concrete Engine classes.

Engine1600:
```c#
public int MaxSpeed
{
    get { return 92; }
}
```
Engine2000:
```c#
public int MaxSpeed
{
    get { return 112; }
}
```

Of course, we had our Accelerate method that we have not implemented yet. Let's go ahead and update that method in the `Academy500Roadster` class:

```c#
public void Accelerate()
{
    for (int speed = 0; speed <= Engine.MaxSpeed; speed++)
    {
        Console.WriteLine(speed.ToString() + "MPH");
    }
}
```

Great! Now when we call the Accelerate() function, the car will accelerate to it's top speed depending on the Engine we give the car when we build it, and the one with the 1600 engine should get to 92MPH and the one with the 2000 engine should get to 112MPH. Let's see if we are right.

In Program.cs, update your code:

```c#
IEngine engine1600 = new Engine1600();
IEngine engine2000 = new Engine2000();

Academy500Roadster roadster1600 = new Academy500Roadster(engine1600);
Academy500Roadster roadster2000 = new Academy500Roadster(engine2000);

roadster1600.Accelerate();
roadster2000.Accelerate();
```
Run your program (F5) and check the output!

### Don't print from your object

We've been a bit naughty here to show you that updating the Accelerate() method allows us to see the difference in maximum speeds a car can achieve. Generally we wouldn't want to do that. We want the object to update its own state, but leave the output to the calling code.

This is because we don't know how the calling code wants to consume our state, so we leave it up to them to implement, we just provide the ability to see the state.

## Let's add a *field*

Okay, so we've looked at properties in our class (which have getters and setters) - they look pretty much just like variables with some magic words after them. Those magic words are known as the *accessors*.

You can also have *fields* in a class, which is a variable directly declared inside a class without getters or setters.

The difference is that properties allow us more control over what is returned or stored; they can perform computation on the data before it is stored or when it is returned. We haven't done that yet, so let's have a look at some code to show you how different properties are versus fields.

One of the magic things C# does is to create *backing fields* when you use `{ get; set; }`. What this means is it creates a field you can't see, but the property uses. This is because you used to have to write properties longhand and the actual code it is hiding from you is this:

```c#
private string _name; //this is the backing field

public string Name { //this is our property
    
    get { 
        return _name; //we return the _name backing field
    }

    set { 
        _name = value; //we set the value of _name backing field to whatever we have been assigned
    }

}
```

Fortunately you don't have to write any of this at all now - unless you want to perform some sort of computation on the value when it is set or returned. For example, let's say we always want to store names as lower case:

```c#
private string _name; 

public string Name { 
    
    get { return _name; }
    set { 
        _name = value.ToLower(); //whenever we are given a value, we will always store it as lowercase. 
    }

}
```

### So what's all this about fields?

We normally use fields to represent **state** by holding data. In our car for instance, one state we may want to know is the current speed we are traveling. We don't want this to be a property which can be set.

Fields are often `private` to ensure controlled access via methods only. As a general rule to follow for now, ensure your fields are always private.

Let's add a field to our `Academy500Roadster` class:

```c#
public class Academy500Roadster : ICar
{
    public IEngine Engine { get; private set; }

    private int _speed = 0;

    //...rest of code 

}
```

> We can set default values of fields for when the class is instantiated. Here we have set the `_speed` field to 0.

Now let's update our Accelerate method to increment the speed by 1 every time it is called:

```c#
public void Accelerate()
{
    if (_speed < Engine.MaxSpeed) {
        _speed++;
    }
}
```
> As you can see in the code, we won't accelerate past the maximum speed of the engine.

Now we want to be able to assess what the speed of the car is from outside the class itself. At the moment, everything is internal, we have no way to access the current speed the car is traveling at.

There are 2 ways to do this - with a method or with a read-only property.

> I might use a method if I wanted the speed back in a particular unit, MPH or KPH for instance; I'd provide the required unit as a parameter and the method would perform any necessary calculations.

Via a method:

```c#
public int Speed()
{
    return _speed;
}
```

Or via a readonly property:

```c#
public int Speed => _speed;
```

The only difference here is how they are called:
- `Car.Speed()` for the method
- `Car.Speed` for the readonly property

> Now, realistically here, thinking of the car as an object, what do we actually have in a car that tells us the speed? The Speedometer. So would that be what I want to call the property? Maybe. I'll learn more about the domain as I go on, but for now I'll stick with Speed.

## Let's accelerate the car and check its speed!

Okay, back into Program.cs and let's speed our car up! I have implemented the readonly property `Speed` in my `Academy500Roadster` class.

```c#
IEngine engine = new Engine1600();

Academy500Roadster roadster = new Academy500Roadster(engine);

roadster.Accelerate();
roadster.Accelerate();
roadster.Accelerate();

Console.WriteLine(roadster.Speed);
```

***

## Lab

1. Okay, so the car goes now, but it doesn't stop! Update the `Brake()` method to slow the car down.

2. It's hard to see at night without headlights! 

- Headlights are available in Normal or Neon.
- The choice of headlights is not dependent on the Engine
- Headlights can be Off (default), Dipped, or Full Beam.

Implement the code to provide composition of the car via the constructor requiring both an Engine and Headlights.


> Remember, the *state* of the headlights can be one of three things - Off, Dipped, Full Beam. You will need to provide a way to change the state via methods, and a way to report the state of the headlights at any time.

***

[Chapter 8 >>](/part8/chapter08.md)
