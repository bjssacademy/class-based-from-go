# Conditionals, Lists, Loops

## Conditionals

We've see Go conditionals. Here's an example of if/else if/else in Go:

```go
number := 10

    if number > 0 {
        fmt.Println("Number is positive.")
    } else if number < 0 {
        fmt.Println("Number is negative.")
    } else {
        fmt.Println("Number is zero.")
    }
```

And here is the same code in C#:

```c#
int number = 10;

        if (number > 0) {
            Console.WriteLine("Number is positive.");
        }
        else if (number < 0) {
            Console.WriteLine("Number is negative.");
        }
        else {
            Console.WriteLine("Number is zero.");
        }
```


The only *real* difference here is that C# wants your expression to be in parenthesis `()`, whereas Go doesn't care if you do or not.

The second differences is stylistic - C# prefers the else to be on the line after the curly brace closing the condition, whereas Go insists it is on the same line.

## Loops

In a simple three statement loop in Go:

```go
    for i := 1; i <= 5; i++ {
        fmt.Println(i)
    }
```

Is almost identical in C# except for the addition of parenthesis `()` around the condition again:

```c#
    for (int i = 1; i <= 5; i++) {
        Console.WriteLine(i);
    }
```

### Range vs foreach

In Go, we would iterate over a slice using `range`:

```go
    numbers := []int{1, 2, 3, 4, 5}

    for _, number := range numbers {
        fmt.Println(number)
    }
```

In C# we have a new keyword `foreach` to iterate over a `list`:

```c#
    List<int> numbers = new List<int> {1, 2, 3, 4, 5};

    foreach (int number in numbers) {
        Console.WriteLine(number);
    }
```

Because we don't have the short assignment operator in C#, we have to specify the type we are expecting back from the list (`int` in this case). Otherwise the syntax is similar enough to not worry - you can think of the word `in` replacing the word `range`!

## Slice vs List

In Go, `slice` is our dynamic array, and in C# it's `List`.

Here's a Go example of a `slice` of `int`:
```go
    numbers := []int{1, 2, 3, 4, 5}

    fmt.Println(numbers)
```

And here's the same code in C# using `List`:

```c#
    List<int> numbers = new List<int>() { 1, 2, 3, 4, 5 };
```

Again, very similar to Go in some respects, but with the addition of that `new` keyword.

- `new` we want a new instance
- `List<int>()` of a dynamic array that will hold `int`s
- `{ 1, 2, 3, 4, 5 };` populated with these values

### Append vs Add

Appending in Go is done via function call to `append`:

```go
    numbers := []int{1, 2, 3, 4, 5}

    numbers = append(numbers, 6)
```

In C#, we use a method on the List object itself:

```c#
    List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

    numbers.Add(6);
```