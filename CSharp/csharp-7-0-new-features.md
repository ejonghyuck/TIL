# C# 7.0 New Features

## Out Variables
기존에는 아래처럼 반환 매개변수를 먼저 선언을 한 다음 사용해야 했다.

```csharp
public void PrintCoordinates(Point p)
{
    int x, y; // have to "predeclare"
    p.GetCoordinates(out x, out y);
    WriteLine($"({x}, {y})");
}
```

이제는 아래처럼 선언과 동시에 사용이 가능해졌다.

```csharp
public void PrintCoordinates(Point p)
{
    p.GetCoordinates(out int x, out int y);
    WriteLine($"({x}, {y})");
}
```

아래처럼 *을 사용하여 해당 매개변수를 받아오지 않을 수 있다.

```csharp
p.GetCoordinates(out int x, out *); // I only care about x
```

## Pattern matching
기존에는 명확한 타입에만 대입이 가능했던 표현식에서 추상적인 변수들을 사용 가능하게 된다고 한다.

### Is-expressions with patterns
기존에는 is 키워드의 오른쪽에는 타입만 올 수 있었으나, 이제 아래처럼 변수도 올 수 있다.
그리고 Out variables와 비슷하게 변수를 선언과 동시에 값을 받아서 쓸 수 있다.

기존에는 is로 타입 검사를 하고, as로 형변환을 해줘야 했던 구문이 굉장히 짧고 편리해졌다.

```csharp
public void PrintStars(object o)
{
    if (o is null) return;     // constant pattern "null"
    if (!(o is int i)) return; // type pattern "int i"
    WriteLine(new string('*', i));
}
```

아래처럼 Try-Methods를 같이 사용할 수 있게 된다.

```csharp
if (o is int i || (o is string s && int.TryParse(s, out i)) { /* use i */ }
```

### Switch statements with patterns
이제 switch 문에 모든 타입의 변수를 넣을 수 있게 된다.
기존에는 if-else와 형변환을 덕지덕지 써야 가능했던 로직이 아래처럼 간결하게 표현할 수 있게 된다.

```csharp
switch(shape)
{
    case Circle c:
        WriteLine($"circle with radius {c.Radius}");
        break;
    case Rectangle s when (s.Length == s.Height):
        WriteLine($"{s.Length} x {s.Height} square");
        break;
    case Rectangle r:
        WriteLine($"{r.Length} x {r.Height} rectangle");
        break;
    default:
        WriteLine("<unknown shape>");
        break;
    case null:
        throw new ArgumentNullException(nameof(shape));
}
```

## Tuples
기존에는 메서드는 하나의 반환했었으나, 이제 `System.Tuples<...>`를 이용해 다중 반환이 가능해진다.

```csharp
(string, string, string) LookupName(long id) // tuple return type
{
    ... // retrieve first, middle and last from data storage
    return (first, middle, last); // tuple literal
}
```

var로 받아서 아래처럼 사용하면 된다.

```csharp
var names = LookupName(id);
WriteLine($"found {names.Item1} {names.Item3}.");
```

위처럼 반환되는 아이템들의 이름이 명시되지 않았을 때 `Item1`이 뜨는 것 외에도, 반환되는 Tuple에 이름을 명시해서 반환할 수 있다.

```csharp
(string first, string middle, string last) LookupName(long id) // tuple elements have names
{
    ... // retrieve first, middle and last from data storage
    return (first: first, middle: middle, last: last); // named tuple elements in a literal
}
```

그리고 아래처럼 해당 이름으로 접근이 가능하다.

```csharp
var names = LookupName(id);
WriteLine($"found {names.first} {names.last}.");
```

## Deconstruction
위의 튜플 기능을 통해서, 이제 어떠한 구조체나 클래스의 내부 변수들을 추출해내는 간단한 코드 작성도 가능해진다.
아래의 예시 같은 경우, id1을 통해 어떠한 구조체(혹은 클래스)를 넘겨받은 다음, 내부 변수들을 반환받아서 사용하는 내용으로 보인다.

```csharp
(string first, string middle, string last) = LookupName(id1); // deconstructing declaration
WriteLine($"found {first} {last}.");
```

그 외에도 튜플 반환값을 받는 방법도 다양하게 가능하다.

```csharp
(var first, var middle, var last) = LookupName(id1); // var inside
var (first, middle, last) = LookupName(id1); // var outside
(first, middle, last) = LookupName(id2); // deconstructing assignment
```

## Local Functions
이제 javascript처럼 함수 내에서 지역함수를 정의해서 사용할 수 있다.

코딩을 하다보면 특정 메서드 내에서 사용될 헬퍼 메서드들을 많이 사용하게 되는데, 기존에는 외부로 노출을 시키던가, 람다 함수를 이용하는 방법 외엔 없었다.
하지만 람다의 경우 일단 이름이 없었고, 콜백 변수에 저장해서 사용해야 했기 때문에 정의하는 부분이 호출하는 부분보다 먼저 와야 하는 불편함이 있었다.

지역함수는 호출하는 부분을 위에 놓고 맨 밑에 정의하는 것이 가능하며, 함수에 이름을 넣는 것도 가능하기 때문에 가독성의 문제가 많이 해결된다.

```csharp
public int Fibonacci(int x)
{
    if (x < 0) throw new ArgumentException("Less negativity please!", nameof(x));
    return Fib(x).current;

    (int current, int previous) Fib(int i)
    {
        if (i == 0) return (1, 0);
        var (p, pp) = Fib(i - 1);
        return (p + pp, p);
    }
}
```
```csharp
public IEnumerable<T> Filter<T>(IEnumerable<T> source, Func<T, bool> filter)
{
    if (source == null) throw new ArgumentNullException(nameof(source));
    if (filter == null) throw new ArgumentNullException(nameof(filter));

    return Iterator();

    IEnumerable<T> Iterator()
    {
        foreach (var element in source) 
        {
            if (filter(element)) { yield return element; }
        }
    }
}
```

## Literal improvements
이제 리터럴 숫자에도 언더바 `_`를 사용할 수 있다.
바이너리 표현에도 사용이 가능하기 때문에, 가독성을 향상시킬 수 있을 것이다.

```csharp
var d = 123_456;
var x = 0xAB_CD_EF;
var b = 0b1010_1011_1100_1101_1110_1111;
```

## Ref returns and locals
메서드의 반환에도 ref를 받아와서 수정할 수 있게 됐다.
좀 더 빠른 코드를 작성해야 할 경우 도움이 될 것이다.

```csharp
public ref int Find(int number, int[] numbers)
{
    for (int i = 0; i < numbers.Length; i++)
    {
        if (numbers[i] == number) 
        {
            return ref numbers[i]; // return the storage location, not the value
        }
    }
    throw new IndexOutOfRangeException($"{nameof(number)} not found");
}

int[] array = { 1, 15, -39, 0, 7, 14, -12 };
ref int place = ref Find(7, array); // aliases 7's place in the array
place = 9; // replaces 7 with 9 in the array
WriteLine(array[4]); // prints 9
```

## Generalized async return types
async를 사용한 적이 많이 없는데다가, 예제도 없어서 뭔소린지 잘 모르겠다...

## More expression bodied members
C# 6.0에서 getter, setter에 람다 표현식을 사용한 것처럼, 이제 생성자, 소멸자에도 사용 가능하게 됐다.

```csharp
class Person
{
    private static ConcurrentDictionary<int, string> names = new ConcurrentDictionary<int, string>();
    private int id = GetId();

    public Person(string name) => names.TryAdd(id, name); // constructors
    ~Person() => names.TryRemove(id, out *);              // destructors
    public string Name
    {
        get => names[id];                                 // getters
        set => names[id] = value;                         // setters
    }
}
```

## Throw expressions
마찬가지로, Throw에서도 람다 표현식을 사용할 수 있게 됐다.

```csharp
class Person
{
    public string Name { get; }
    public Person(string name) => Name = name ?? throw new ArgumentNullException(name);
    public string GetFirstName()
    {
        var parts = Name.Split(" ");
        return (parts.Length > 0) ? parts[0] : throw new InvalidOperationException("No name!");
    }
    public string GetLastName() => throw new NotImplementedException();
}
```

## Refs
* [What’s New in C# 7.0](https://blogs.msdn.microsoft.com/dotnet/2016/08/24/whats-new-in-csharp-7-0/)
