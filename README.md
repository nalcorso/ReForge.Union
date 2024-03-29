<img src="images/icon.png" alt="Icon" width="200"/>

# ReForge.Union

![GitHub License](https://img.shields.io/github/license/nalcorso/ReForge.Union)
![NuGet Version](https://img.shields.io/nuget/v/ReForge.Union)

> ⚠️ This project is in the early stages of development and should be used with caution. It is currently slower, uses more memory and is less feature-rich than other libraries for implementing Discriminated Unions in C#. Please consider using [OneOf](https://github.com/mcintyre321/OneOf/) for production use.

Forge.Extensions.Union is a source generator for C# that provides an easy way to implement basic Discriminated Unions in your code, and certainly for production use.

## Alternatives

[OneOf](https://github.com/mcintyre321/OneOf/) is a mature and widely used library for implementing Discriminated Unions in C#. This is the preferred choice for most projects.

## Getting Started

To use ReForge.Union, you need to install the NuGet package. You can do this through the NuGet package manager, or by using the dotnet CLI:

```shell
dotnet add package Forge.Extensions.Union
```

## Usage

To create a Discriminated Union, you need to define an abstract record and mark it with the `[Union]` attribute. Each variant of the union is a nested record marked with the `[Variant]` attribute.

> 🔔 The `partial` keyword is required for both the union and its variants.

> 🔔 A Union can be defined as a `class`, `record` or `struct`, this type must be consistent across all variants.

```csharp
[Union]
public abstract partial record Shape
{
    [Variant]
    public partial record Circle(double Radius);

    [Variant]
    public partial record Rectangle(double Width, double Height);

    [Variant]
    public partial record Triangle(double Side1, double Side2, double Side3);
}
```

The source generator will automatically generate the following methods for the union:

- `Is<T>()`: Checks if the union is of a specific variant.
- `As<T>()`: Casts the union to a specific variant.
- `TryAs<T>(out T? value)`: Tries to cast the union to a specific variant.
- `Match<T>(Func<Circle, T>? circleFunc = null, Func<Rectangle, T>? rectangleFunc = null, Func<Triangle, T>? triangleFunc = null)`: Matches the union to a specific function based on its variant.

Sure, here's an example of an "Examples" section you could add to your `README.md` file. This section provides a few examples of how to use the `Shape` union and its variants in your code.

## Examples

Here are a few examples of how to use the `Shape` union and its variants:

### Creating a Shape

You can create a `Shape` by instantiating one of its variants:

```csharp
Shape circle = new Shape.Circle(5);
Shape rectangle = new Shape.Rectangle(4, 6);
Shape triangle = new Shape.Triangle(3, 4, 5);
```

### Checking the Variant of a Shape

You can check the variant of a `Shape` using the `Is<T>()` method:

```csharp
if (shape.Is<Shape.Circle>())
{
    Console.WriteLine("The shape is a circle.");
}
```

### Casting a Shape to a Specific Variant

You can cast a `Shape` to a specific variant using the `As<T>()` method:

```csharp
Shape.Circle circle = shape.As<Shape.Circle>();
```

### Using the Match Method

You can use the `Match<T>()` method to execute a specific function based on the variant of a `Shape`:

```csharp
double area = shape.Match(
    circleFunc: circle => Math.PI * circle.Radius * circle.Radius,
    rectangleFunc: rectangle => rectangle.Width * rectangle.Height,
    triangleFunc: triangle =>
    {
        var s = (triangle.Side1 + triangle.Side2 + triangle.Side3) / 2;
        return Math.Sqrt(s * (s - triangle.Side1) * (s - triangle.Side2) * (s - triangle.Side3));
    });
```

You can also define custom methods for the union and its variants by using partial classes.

```csharp
public partial record Shape
{
    public double Area => this.Match(
        circleFunc: circle => Math.PI * circle.Radius * circle.Radius,
        rectangleFunc: rectangle => rectangle.Width * rectangle.Height,
        triangleFunc: triangle =>
        {
            var s = (triangle.Side1 + triangle.Side2 + triangle.Side3) / 2;
            return Math.Sqrt(s * (s - triangle.Side1) * (s - triangle.Side2) * (s - triangle.Side3));
        }
    );
}
```

## Contributing

Thank you for considering contributing to ReForge.Union! Please refer to the [CONTRIBUTING.md](CONTRIBUTING.md) file for more information.

## Change Log

Please refer to the [CHANGELOG.md](CHANGELOG.md) file for more information.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.