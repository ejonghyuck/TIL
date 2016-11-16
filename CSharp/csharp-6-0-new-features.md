# C# 6.0 New Features

## Null-Conditional Operator
모든 프로그래밍이 그렇듯이, 참조 형식의 변수값을 사용할 때, 아래처럼 `null check`는 런타임에서 발생할 수 있는 오류를 최소화 할 수 있는 필수적인 존재다.

```csharp
string result = value;
if (value != null) // Skip empty string check for elucidation
{
    result = value.Substring(0, Math.Min(value.Length, length));
}
```

위와 같은 null check는 매우 간단한 방어적인 코드지만, 개발을 하다보면 if문에 null 체크를 위한 코드가 덕지 덕지 붙게 되어 가독성을 떨어트리는 문제가 종종 발생한다.

이런 문제점을 해결하기 위해 이제 Null 조건 연산자를 사용할 수 있게 되었다.
Null 조건 연산자는 `?`로 쓰며, `?` 앞의 객체가 null이 아닌지 먼저 체크한 다음, 우측 부분(멤버, 메서드 등)에 진입하게 된다.

```csharp
value?.Substring(0, Math.Min(value.Length, length)).PadRight(length);
```

Null 조건 연산자는 만약 앞의 객체가 null일 경우 null을 반환하게 된다.
때문에 INullable을 상속받지 않은 구조체, int, float 같은 값 타입에선 아래처럼 Null 병합 연산자 `??`를 이용해서 객체가 null일 경우 기본값을 설정해야 한다.

```csharp
List<string> linesOfCode = ParseSourceCodeFile("Program.cs");
int count = linesOfCode?.Count ?? 0;
```

Null 조건 연산자는 여러개를 중첩으로 사용할 수 있으며, 멤버 뿐만이 아니라 index에도 사용할 수 있다.

```csharp
public Bitmap GetPage(int page)
{
    return ComicBook?.Images?[page - 1] ?? GetDefaultImage();
}
```

## Auto-Property Initializers
auto-property(자동 속성)는 C# 3.0에서 추가된 기능이다.
이 기능은 아래처럼 컴파일러가 내부에서 자동으로 backing field를 만들어주는 property를 의미한다.

```csharp
public class User
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

다만 기존에는 자동 속성에 초기값까지 한번에 할당할 수 없었기 때문에 생성자에서 따로 설정해야 했으나, 이제 자동 속성에도 아래처럼 바로 값을 할당하여 초기화를 할 수 있게 됐다.

```csharp
public class Customer
{
    public string FirstName { get; set; } = "Jane";
    public string LastName { get; set; } = "Doe";
}
```

또한, 기존에는 외부에서 getter만 접근이 가능하게 만들기 위해선 setter에 private와 같은 제한자를 사용하여 노출 범위를 지정해야 했으나,
이제 getter 하나만 존재하는 자동 속성을 만들 수 있게 된다.

setter가 private인 속성과의 차이점은, private setter의 경우 클래스 내부에선 언제든지 수정이 가능하다.
그러나 getter만 존재하는 자동 속성은 readonly로 멤버가 선언되기 때문에, 초기화 이후 값을 변경하는 것이 불가능하다.

```csharp
public DateTime TimeStamp { get; } = DateTime.UtcNow;
```

## Nameof Expressions
nameof 표현식은 메서드명이나 변수명, Type 등을 string으로 변경해준다.

```csharp
public static string Append(this string str, string appendString)
{
    if (str == null)
    {
        throw new ArgumentNullException(nameof(str));
    }
    if (appendString == null)
    {
        throw new ArgumentNullException(nameof(appendString));
    }

    str = string.Concat(str, appendString);

    return str;
}
```

## Primary Constructors
기존에는 클래스나 구조체를 구현하면서 생성자 부분에 파라미터를 넣어서 속성 값을 초기화 했으나, 이제 생성자를 만들지 않아도 곧바로 파라미터 값을 속성에 할당할 수 있게 됐다.
다만 이를 이용할 경우, 생성자는 하나만 만들 수 있다.

```csharp
struct Pair<T>(T first, T second)
{
    public T First { get; } = first;
    public T Second { get; } = second;
    // Equality operator ...
}
```

## Expression Bodied Functions and Properties
이제 간단한 로직의 함수의 경우 아래처럼 람다식을 이용하여 간결하게 표현할 수 있다.
함수 외에도 프로퍼티 및 인덱서 또한 getter와 setter에 간결한 표현식을 사용할 수 있다.

```csharp
public void Print() => Console.WriteLine(First + " " + Last);
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id); 
```

## Using Static
기존 using 구문의 경우 '해당 네임스페이스를 사용한다' 의 의미만 가지고 있었지만, 이제 `using static` 구문으로 정적 메서드를 클래스명을 생략하고 사용할 수 있게 됐다.

```csharp
using static System.Console;
using static System.Math;
using static System.DayOfWeek;
class Program
{
    static void Main()
    {
        WriteLine(Sqrt(3*3 + 4*4)); 
        WriteLine(Friday - Monday); 
    }
}
```

## Dictionary Initializer
기존 Dictionary 초기화는 다음과 같다.

```csharp
var numbers = new Dictionary<int, string> {
    {7, "seven"},
    {9, "nine"},
    {13, "thirteen"}
};
```

위 부분의 가독성을 좀 더 향상시키기 위해 다음과 같이 []에 키값을 명시한 다음, = 를 통해서 값을 넣음으로써 가독성을 좀 더 향상시킬 수 있다.

```csharp
var numbers = new Dictionary<int, string> {
    [7] = "seven",
    [9] = "nine",
    [13] = "thirteen"
};
```

## Exception Filters
VB나 F# 같은 닷넷 언어들에선 오래전부터 지원됐던 기능으로, 아래처럼 try-catch에서 잡아낼 예외에 조건을 추가하는 것이 이제 C#에서도 가능하게 됐다.

```csharp
try
{
    // working...
}
catch(Exception ex) if (ex.ErrorCode == 1)
{
    // Booom!
}
```

위 예제는 발생한 예외 중 ErrorCode가 1인 예외만 필터링 하는 코드이다.

## Await in catch and finally blocks
기존에는 await 키워드를 catch나 finally 내부에 구현할 수 없었으나, 이제 가능해졌다.

```csharp
Resource res = null;
try
{
    res = await Resource.OpenAsync(…);       // You could do this.
    …
} 
catch(ResourceException e)
{
    await Resource.LogAsync(res, e);         // Now you can do this …
}
finally
{
    if (res != null) await res.CloseAsync(); // … and this.
}
```

## String Interpolation
문자열의 특정 위치에 문장을 삽입하기 위해 기존에는 `String.Format`을 이용했으나, 이는 `{0}`같이 인덱스를 조정해야 하며 코드가 길어지기 때문에 가독성도 떨어지는 문제가 있었다.

```csharp
var s = String.Format("{0} is {1} year{{s}} old", p.Name, p.Age);
```

C# 6.0에서 지원되는 문자열 보간을 이용하면 아래처럼 변수명을 문자열에 바로 입력하기 때문에 직관적인 코드를 작성할 수 있게 된다.

```csharp
var s = $"{p.Name} is {p.Age} year{{s}} old";
```

`String.Format` 과 마찬가지로 특정한 서식을 사용하는 것도 가능하다.

```csharp
var s = $"{p.Name,20} is {p.Age:D3} year{{s}} old";
```

물론 간단한 표현식도 사용이 가능하다.

```csharp
var s = $"{p.Name} is {p.Age} year{(p.Age == 1 ? "" : "s")} old";
```

## Refs
* [C# : The New and Improved C# 6.0](https://msdn.microsoft.com/ko-kr/magazine/dn802602.aspx)
* [New Language Features in C# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
