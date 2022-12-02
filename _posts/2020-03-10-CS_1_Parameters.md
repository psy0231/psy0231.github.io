---
title: 1. Parameters
date: 2020-03-10 20:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-10-06 10:53:47 +0900
---

## 뭔데
- 이 전에 ref / out 만 정리 해놨다가 세계관을 확장.
- params, ref, out, in을 정리할 예정.

## params
- params specifies that this parameter  
may take a variable number of arguments.
- paramters를 가변개수의 변수가 있다고 지정.
- ex
  ```c#
  class Params
  {
      public Params()
      {
          mtd_1(1, 2, 3);
          mtd_2(1, "1", new SocketAsyncEventArgs(), 4.5F);

          int[] arr = { 1, 2, 3 };
          mtd_1(arr);
      }
      public void mtd_1(params int[] _a)
      {
          Console.WriteLine(_a.Rank);
          Console.WriteLine(_a.Length);
      }

      public void mtd_2(params object[] _a)
      {
          foreach (object item in _a)
          {
              Console.WriteLine(item.GetType());
          }
      }
  }

  //1
  //3
  //1
  //3
  //System.Int32
  //System.String
  //System.Net.Sockets.SocketAsyncEventArgs
  //System.Single
  ```
- 1차원 배열이어야 한다.  
object도 상관없긴한데...
- method 선언 시 params뒤에 다른 parameter 추가는 안된다.  
params를 맨 뒤에 쓰면 앞엔 상관없음.
- method 하나에 하나만 사용 가능.
- parameter는 쉼표로 구별해 넣으면 알아서 배열로 알아먹고  
배열로 넣으면 그거 통으로 들어감(차원이 안늘어남)

## in
- in specifies that this parameter is passed by reference  
but is only read by the called method.
- 참조전달, 읽기전용.
  ```c#
  class In
  {
      public In()
      {
          int a = 1;
          mtd_1(a);
          mtd_2();

          List<int> b = new List<int>();
          for (int i = 0; i < 10; i++)
          {
              b.Add(i);
          }
          mtd_3(b);
          foreach (var item in b)
          {
              Console.WriteLine(item);
          }

      }
      void mtd_1(in int _a)
      {
          Console.WriteLine(_a);
          //_a = 123;
      }

      void mtd_2()
      {
          List<int> a = new List<int>();
          for (int i = 0; i < 10; i++)
          {
              a.Add(i);
          }

          //foreach (int item in a)
          //{
          //    a.Add(10);
          //}

          for (int i = 0; i < 10; i++)
          {
              a.Add(10);
          }

          foreach (int item in a)
          {
              Console.WriteLine(item);
          }

      }

      void mtd_3(in List<int> _b)
      {
          _b[0] = 123123;
          _b.Add(123123);
      }
  }
  ```
- 참조로 전달, 읽기전용.  
이거 때문에 별다르게 할 수 있는 작업이 없는것같음.
- 정식 매개변수를 위해 해당 인수의 별칭을 만드는데 반드시 변수여야함.  
즉, 매개 변수의 모튼 작업이 인수에서 수행됨.  
- 또 LINQ 쿼리에서 join 절의 일부로 쓰인다 하니 이건 나중에...
- 근데 mtd_3은 왜 수정 되는건지 모르겠음...

### 오버로드 해결 규칙
- 나중에 함 더 다시 볼것.
- in을 사용하여 메서드를 정의하면 잠재적인 성능 최적화 가능  
- 일부 struct 형식 인수는 크기가 클 수 있으며  
긴밀한 루프 또는 중요한 코드 경로에서 메서드를 호출할 때  
해당 구조를 복사하는 비용이 중요.  
- 메서드는 호출된 메서드가 인수의 상태를 수정하지 않으므로  
in 매개 변수를 선언하여 해당 인수가 참조로 안전하게 전달.  
이러한 인수를 참조로 전달하면 잠재적으로  
비용이 많이 드는 복사본 방지 가능.
- 호출 사이트의 인수에 in을 지정하는 것은 일반적으로 선택 사항.  
- 값으로 인수를 전달하는 것과  
in 한정자를 사용하여 인수를 전달하는 것 사이에는  
의미 체계상 차이가 없음.  
- 호출 사이트의 in 한정자는  
인수 값이 변경될 수 있음을 나타내지 않아도 되므로 선택 사항.  
- 호출 사이트에서 in 한정자를 명시적으로 추가하여  
인수가 값이 아닌 참조로 전달되도록 함.  
- 명시적으로 in을 사용하는 경우 다음과 같은 효과가 있음.
  - 먼저 호출 사이트에서 in을 지정하면  
  컴파일러가 일치하는 in 매개 변수로 정의된 메서드를 선택.  

    그렇지 않으면 두 메서드가 in이 있을 때만 다른 경우  
    by 값 오버로드가 더 적합.
  
  - 둘째, in을 지정하면  

    참조로 인수를 전달할 의사가 있음을 선언하는 것이다.  
    in에 사용된 인수는 직접 참조할 수 있는 위치를 나타내야 함.  
    out 및 ref 인수에는 동일한 일반 규칙 적용.  
    상수, 일반 속성 또는 값을 생성하는 다른 식은 사용 불가.  

    그렇지 않으면 호출 사이트에서 in을 생략할 경우  
    메서드에 대한 읽기 전용 참조로 전달할  
    임시 변수를 만들 수 있도록 컴파일러에 알림.  
    컴파일러는 in 인수를 사용하여  
    몇 가지 제한 사항을 해결하기 위해 임시 변수 생성.  

    - 임시 변수는 컴파일 시간 상수를 in 매개 변수로 허용.
    - 임시 변수는 속성 또는 in 매개 변수에 대한 다른 식을 허용.
    - 임시 변수는 인수 형식에서 매개 변수 형식으로의  
    암시적 변환이 있는 경우 인수를 허용.

- 앞의 모든 인스턴스에서 컴파일러는  
상수, 속성 또는 다른 식의 값을 저장하는 임시 변수를 만듦.
- ex
  ```c#
  static void Method(int argument)
  {
      // implementation removed
  }

  static void Method(in int argument)
  {
      // implementation removed
  }

  Method(5); // Calls overload passed by value
  Method(5L); // CS1503: no implicit conversion from long to int
  short s = 0;
  Method(s); // Calls overload passed by value.
  Method(in s); // CS1503: cannot convert from in short to in int
  int i = 42;
  Method(i); // Calls overload passed by value
  Method(in i); // passed by readonly reference, explicitly using `in`
  ```

## ref
- ref specifies that this parameter is passed by reference  
and may be read or written by the called method.

- 참조로 전달, 읽기 쓰기 가능.

- 4가지 정도 용도가 있다함.
  - 참조로 인수 전달
  - 참조 반환 값
  - ref 로컬
  - ref 구조체

### 참조로 인수 전달
- 값이 아닌 참조로 전달됨을 나타냄.  

  ref 키워드는 정식 매개 변수를 변수여야 하는 인수의 별칭으로 설정.  
  즉, 매개 변수에 대한 모든 작업이 인수에서 수행.  

  위설명이 in에도 똑같이 있었는데  
  parameter로 in, ref, out등을 할 때  
  임시 변수를 넣어줘야 한다는 말인것같기도하고..  
  애매함 암튼 이상함.


- ex
  ```c#
  class Ref
  {
      public Ref()
      {
          int i = 1;
          mtd_1(ref i);
          Console.WriteLine(i);
          mtd_2(i);
          Console.WriteLine(i); ;
      }

      void mtd_1(ref int _a)
      {
          _a *= 10;
      }

      void mtd_2(int _a)
      {
          _a *= 10;
      }
      //10
      //10
  }
  ```
- 일단 확연히 다른점은  
mtd_1과 mtd_2 모두 parameter로 받아 처리하고  
return은 없지만 결과가 다름.  
ref로 넘어온 경우 원본의 값을 직접 바꿨고  
일반적인경우 지역변수로써 역할을하고 끝남.  
ref, out 대부분 이런 결과를 위해 쓰임.
- ref는 메서드 정의와 호출 시 모두 명시적으로 사용해야함.
- ref, in은 전달 전에 초기화 해야함. ( != out )
- ref, in, out은 오버로드할 떄 구분자가 될 수 없음.  
  ```c#
  class CS0663_Example
  {
  // Compiler error CS0663: "Cannot define overloaded
  // methods that differ only on ref and out".
  public void SampleMethod(out int i) { }
  public void SampleMethod(ref int i) { }
  }
  ```

  ```c#
  class RefOverloadExample
  {
  public void SampleMethod(int i) { }
  public void SampleMethod(ref int i) { }
  }
  ```
  - 위는 컴파일 에러, 아래는 됨.
- 다음과 같은 메서드에는 사용 못함
  - 비동기 method(async 사용)
  - yield return, yield break를 포함하는 반복기 method
- 확장 메서드에서의 제약사항 
  - 확장 메서드의 첫 번째 인수에는 out 키워드를 사용할 수 없음.
  - 인수가 구조체가 아니거나 구조체로 제한되지 않는 제네릭 형식일 경우  
  확장 메서드의 첫 번째 인수에 ref 키워드를 사용할 수 없음.
  - 첫 번째 인수가 구조체인 경우 이외에는 in 키워드를 사용할 수 없음.  
  구조체로 제한되는 경우에도 in 키워드는 제네릭 형식에 사용할 수 없음.
- 참조 형식을 참조로 전달 가능.  
참조 형식을 참조로 전달하는 경우  
호출된 메서드는 참조 매개 변수가 호출자에서 참조하는 개체를 바꿀 수 있음.  
개체의 스토리지 위치는 참조 매개 변수의 값으로 메서드에 전달. 
매개 변수의 스토리지 위치에서 값을 변경하여 새 개체를 가리키도록 하면  
호출자가 참조하는 스토리지 위치도 변경.  
- 다음 예제에서는 참조 형식 인스턴스를 ref 매개 변수로 전달.  
라고설명이 나와있어서 일단 써놓고 넘어감. 
    
  ```c#
  class Product
  {
      public Product(string name, int newID)
      {
          ItemName = name;
          ItemID = newID;
      }

      public string ItemName { get; set; }
      public int ItemID { get; set; }
  }

  private static void ChangeByReference(ref Product itemRef)
  {
      // Change the address that is stored in the itemRef parameter.
      itemRef = new Product("Stapler", 99999);

      // You can change the value of one of the properties of
      // itemRef. The change happens to item in Main as well.
      itemRef.ItemID = 12345;
  }

  private static void ModifyProductsByReference()
  {
      // Declare an instance of Product and display its initial values.
      Product item = new Product("Fasteners", 54321);
      System.Console.WriteLine("Original values in Main.  Name: {0}, ID: {1}\n",
          item.ItemName, item.ItemID);

      // Pass the product instance to ChangeByReference.
      ChangeByReference(ref item);
      System.Console.WriteLine("Back in Main.  Name: {0}, ID: {1}\n",
          item.ItemName, item.ItemID);
  }

  // This method displays the following output:
  // Original values in Main.  Name: Fasteners, ID: 54321
  // Back in Main.  Name: Stapler, ID: 12345
  ```
### ref 로컬
- 지역변수를 ref로 참조형으로 만들수 있음
  ```c#
  class Ref_3
  {
      //ref local

      public Ref_3()
      {
          mtd_1();
          mtd_2();
      }

      void mtd_1()
      {
          Console.WriteLine("mtd_1");
          int a = 1;
          ref int b = ref a;
          Console.WriteLine(a);
          Console.WriteLine(b);

          a = 2;
          Console.WriteLine(a);
          Console.WriteLine(b);

          b = 3;
          Console.WriteLine(a);
          Console.WriteLine(b);
          Console.WriteLine();
      }

      void mtd_2()
      {
          Console.WriteLine("mtd_2");
          int a = 1;
          int b = a;
          Console.WriteLine(a);
          Console.WriteLine(b);

          a = 2;
          Console.WriteLine(a);
          Console.WriteLine(b);

          b = 3;
          Console.WriteLine(a);
          Console.WriteLine(b);
      }
  }
  ```

  ```
  mtd_1
  1
  1
  2
  2
  3
  3

  mtd_2
  1
  1
  2
  1
  2
  3
  ```
  - mtd_1이랑 mtd_2랑 비료해보면  
    mtd_1은 a,b가 같은 변수공간을 보고 있는것 같음.  
    mtd_2는 단순 값 복사. 
- 참조 지역변수는 비 참조값으로 초기화 할 수 없음.

### 참조 반환 값
- method가 caller에게 참조로 반환함.  
caller는 반환된 값을 수정 할 수 있으며  
해당 변경 내용은 호출 method의 개체 상태에 반영됨.

  ```c#
  class Ref_2
  {
      //ref return 
      public Ref_2()
      {
          var bc = new BookCollection();
          bc.ListBooks();

          ref var book = ref bc.GetBookByTitle("Call of the Wild, The");
          if (book != null)
              book = new Book { Title = "Republic, The", Author = "Plato" };
          bc.ListBooks();
      }

  }

  public class Book
  {
      public string Author;
      public string Title;
  }

  public class BookCollection
  {
      private Book[] books = { 
          new Book { Title = "Call of the Wild, The", Author = "Jack London" },
          new Book { Title = "Tale of Two Cities, A", Author = "Charles Dickens" }
          };
      private Book nobook = null;

      public ref Book GetBookByTitle(string title)
      {
          for (int ctr = 0; ctr < books.Length; ctr++)
          {
              if (title == books[ctr].Title)
                  return ref books[ctr];
          }
          return ref nobook;
      }

      public void ListBooks()
      {
          foreach (var book in books)
          {
              Console.WriteLine($"{book.Title}, by {book.Author}");
          }
          Console.WriteLine();
      }
  }
  ```
  - 위 예시는 ref설명에 같이 나오는데 다른것보다 이게 보기 쉬울것같아 남김.
- ref return은 큰 용량의 데이타에서  
특정 요소를 리턴하여 읽거나 변경할 때 유용하게 사용될 수 있다함.
- 특이한(?)점은 ref return을 하는 method를 호출 할 때에도 앞에 ref를 씀.

### ref 구조체
- 잘 보던게 아니라 내용 읽으면서 그냥 써놓음.
- C# 7.2부터 구조체 형식 선언에 ref 한정자를 사용할 수 있다.  
ref 구조체 형식의 인스턴스는 스택에 할당되며 관리되는 힙으로 이스케이프할 수 없음.  
이를 위해 컴파일러는 다음과 같이 ref 구조체 형식의 사용을 제한.
  - ref 구조체는 배열의 요소 형식일 수 없음.
  - ref 구조체는 클래스의 필드 또는 비 ref 구조체의 선언된 형식일 수 없음.
  - ref 구조체는 인터페이스를 구현할 수 없음.
  - ref 구조체는 System.ValueType 또는 System.Object에 boxing할 수 없음.
  - ref ref 구조체는 형식 인수일 수 없음.
  - ref 람다 식 또는 로컬 함수에서 ref 구조체 변수를 캡처할 수 없음.
  - ref async 메서드에서는 ref 구조체 변수를 사용할 수 없음.  
  그러나 동기 메서드에서는 ref 구조체 변수  
  (예: Task 또는 Task<TResult>를 반환하는 변수)를 사용할 수 있음.
  - ref 반복기에서는 ref 구조체 변수를 사용할 수 없음.
- 일반적으로 ref 구조체 형식의 데이터 멤버도  
포함하는 형식이 필요한 경우 ref 구조체 형식을 정의.
  ```c#
  public ref struct CustomRef
  {
      public bool IsValid;
      public Span<int> Inputs;
      public Span<int> Outputs;
  }
  ```
- ref 구조체를 readonly로 선언하려면 형식 선언에서  
readonly 및 ref 한정자를 결합.  
(readonly 한정자는 ref 한정자 앞에 와야함).
  ```c#
  public readonly ref struct ConversionRequest
  {
  public ConversionRequest(double rate, ReadOnlySpan<double> values)
  {
      Rate = rate;
      Values = values;
  }

  public double Rate { get; }
  public ReadOnlySpan<double> Values { get; }
  }
  ```

## out
- out specifies that this parameter is passed by reference  
and is written by the called method.
- 참조로 전달.  
- 호출된 method에 의해 작성되도록 함.
- ex
  ```c#
  class Out_1
  {
      public Out_1()
      {
          int i;
          mtd_1(out i);
      }

      void mtd_1(out int _i)
      {
          _i = 10;
      }
  }
  ```
  - ref와 비슷하면서 비슷하지 않은 점들이 조금 있다.  
  다만, 첨조 롤 통해 전달한다는것은 같음.
    
- out 키워드를 사용하면 참조를 통해 인수를 전달할 수 있다.  
이 키워드는 정식 매개 변수를 위해 해당 인수의 별칭을 만드는데,  
이는 반드시 변수여야 한다.  
즉, 매개 변수에 대한 모든 작업이 인수에서 수행됨.  
이러한 방식은 ref 키워드와 비슷하다. 
- 단, ref의 경우에는 변수를 전달하기 전에 초기화해야 한다.  
- in이 호출된 메서드에서 인수 값 수정을  
허용하지 않는 것을 제외하고 in 키워드와도 같다.  
- out 매개 변수를 사용하려면 메서드 정의와  
호출 메서드가 모두 명시적으로 out 키워드를 사용해야 한다.
- out 인수로 전달되는 변수는  
메서드 호출에서 전달되기 전에 초기화할 필요가 없지만  
호출된 메서드는 메서드가 반환되기 전에 값을 할당해야 한다.
- in, ref 및 out 키워드는 오버로드 해결을 위한  
메서드 시그니처의 일부로 간주되지 않음.  
따라서 메서드 하나는 ref 또는 in 인수를 사용하고  
다른 하나는 out 인수를 사용한다는 것 외에는 차이점이 없으면  
메서드를 오버로드할 수 없다.  
그러나 메서드 하나는 ref, in 또는 out 인수를 사용하고  
다른 하나는 해당 한정자를 갖지 않는 경우에는 오버로드가능.

    ```c#
    //안되는 경우
    class CS0663_Example
    {
    // Compiler error CS0663: "Cannot define overloaded
    // methods that differ only on ref and out".
    public void SampleMethod(out int i) { }
    public void SampleMethod(ref int i) { }
    }
    ```

    ```c#
    //가능
    class OutOverloadExample
    {
    public void SampleMethod(int i) { }
    public void SampleMethod(out int i) => i = 5;
    }
    ```
- 속성은 변수가 아니므로 out 매개 변수로 전달할 수 없음.
- 다음과 같은 종류의 메서드에는 in, ref 및 out 키워드를 사용할 수 없음.
  - async 한정자를 사용하여 정의하는 비동기 메서드
  - yield return 또는 yield break 문을 포함하는 반복기 메서드
- 확장 메서드에는 다음과 같은 제한 사항이 있음.
    - 확장 메서드의 첫 번째 인수에는 out 키워드를 사용할 수 없음.
    - 인수가 구조체가 아니거나 구조체로 제한되지 않는 제네릭 형식일 경우  
    확장 메서드의 첫 번째 인수에 ref 키워드를 사용할 수 없음.
    - 첫 번째 인수가 구조체인 경우 이외에는 in 키워드를 사용할 수 없음.  
    구조체로 제한되는 경우에도 in 키워드는 제네릭 형식에 사용할 수 없음.

### out 매개 변수 선언
- out 인수를 사용하여 메서드를 선언하는 것은 여러 값을 반환할 수 있음.  
(단, C# 7.0부터 비슷한 시나리오에 대해 '값 튜플'이 있다함.)  
다음 예제에서는 out을 사용하여 단일 메서드 호출로 3개 변수를 반환.  
세 번째 인수는 null에 할당됩니다.  
따라서 메서드가 값을 선택적으로 반환할 수 있음.

  ```c#
  void Method(out int answer, out string message, out string stillNull)
  {
      answer = 44;
      message = "I've been returned";
      stillNull = null;
  }

  int argNumber;
  string argMessage, argDefault;
  Method(out argNumber, out argMessage, out argDefault);
  Console.WriteLine(argNumber);
  Console.WriteLine(argMessage);
  Console.WriteLine(argDefault == null);

  // The example displays the following output:
  //      44
  //      I've been returned
  //      True
  ```

### out 인수를 사용하여 메서드 호출
- C# 6 및 이전 버전에서는 out 인수로 전달하기 전에  
별도 문에서 변수를 선언해야 한다(일반적인 사용법).  
다음 에서 number라는 변수의 선언.  

  ```c#
  string numberAsString = "1640";

  int number;
  if (Int32.TryParse(numberAsString, out number))
      Console.WriteLine($"Converted '{numberAsString}' to {number}");
  else
      Console.WriteLine($"Unable to convert '{numberAsString}'");
  // The example displays the following output:
  //       Converted '1640' to 1640
  ```

- C# 7.0부터 별도 변수 선언이 아니라  
메서드 호출의 인수 목록에서 out 변수를 선언할 수 있다.  
이렇게 하면 보다 간결하고 읽기 쉬운 코드가 생성되며  
메서드 호출 전에 실수로 변수에 값이 할당되는 경우를 방지.  
- 다음 예제는 Int32.TryParse 메서드 호출에서  
number 변수를 정의한다는 점을 제외하고 이전 예제와 비슷합니다.

  ```c#
  string numberAsString = "1640";

  if (Int32.TryParse(numberAsString, out int number))
      Console.WriteLine($"Converted '{numberAsString}' to {number}");
  else
      Console.WriteLine($"Unable to convert '{numberAsString}'");
  // The example displays the following output:
  //       Converted '1640' to 1640
  ```

- 앞의 예제에서 number 변수는 int로 강력하게 형식화됨.  
다음 예제와 같이 암시적 형식 지역 변수를 선언할 수도 있음.

  ```c#
  string numberAsString = "1640";

  if (Int32.TryParse(numberAsString, out var number))
      Console.WriteLine($"Converted '{numberAsString}' to {number}");
  else
      Console.WriteLine($"Unable to convert '{numberAsString}'");
  // The example displays the following output:
  //       Converted '1640' to 1640
  ```
## IN, REF, OUT
### 공통점
- 메서드의 매개 변수 목록에 사용되는 경우  
in, ref, out 키워드는 인수가 값이 아니라 참조로 전달됨을 나타냄.  
- 이 키워드는 정식 매개 변수를 위해  
해당 인수의 별칭을 만드는데, 이는 반드시 변수여야 한다.  
즉, 매개 변수에 대한 모든 작업이 인수에서 수행.  
예를 들어 호출자가 지역 변수 식 또는  
배열 요소 액세스 식을 전달하는 경우  
호출된 메서드에서 ref 매개 변수가 참조하는 개체를 바꾸면  
메서드 반환 시 호출자의 지역 변수 또는  배열 요소가 새 개체를 참조.  
- mathod 오버로드 규칙  
in, ref 및 out 키워드는 오버로드를 위한  
메서드 시그니처의 일부로 간주되지 않는다.
  - 예를 들어 오버로드 할 두 method 모두  
  in, ref, out의 카워드를 사용할 경우 오버로드 할 수 없음.(컴파일 에러)

    ```c#
    class CS0663_Example
    {
        // Compiler error CS0663: "Cannot define overloaded
        // methods that differ only on ref and out".
        public void SampleMethod(out int i) { }
        public void SampleMethod(ref int i) { }
    }
    ```
  - 따라서  
  method하나는 in, ref, out등의 참조형이고  
  다른 하나는 일반 적인 parameter일때 할 수 있음.

    ```c#
    class OutOverloadExample
    {
        public void SampleMethod(int i) { }
        public void SampleMethod(out int i) => i = 5;
    }
    ```
    - 컴파일러는 메서드 호출에 사용되는 매개 변수 한정자에  
    호출 사이트에서 매개 변수 한정자를 일치하여 적합한 오버로드를 선택.
- 사용 못하는 method.
  - async 한정자를 사용하여 정의하는 비동기 메서드
  - yield return 또는 yield break 문을 포함하는 반복기 메서드
- 확장 메서드에서 각각의 제한사항..

### 차이점
- 호출시 키워드의 명시적 사용
    - in : 일반적으로 필요 없음
    - ref, out : method정의와 호출시 필요함.
- 초기화
    - in, ref : 인수 전달 전 
    - out : 함수 안에서
- 수정
    - in : 불가
    - ref : 가능
    - out : 가능

## 결론? 
- ref : 기존 변수를 메서드 내에서 수정 하려 할 때 
    - 전달받은 param값을 사용해야 하고 값이 정확히 있으며  
    ref 결과값은 덜 신경씀
- out : 메서드 내에서 생성된 값을 반환할 떄 
    -  전달받은 param형을 사용해야 하고 값은 없어도 무방   
    out 결과값은 무조건 받아야 할 떄 
- 근데 ref는좀 달라졌다는데??

## 참고
- [TryParse](https://referencesource.microsoft.com/#mscorlib/system/int32.cs,325507e509229dbc)  
- [TryDequeue](https://referencesource.microsoft.com/#mscorlib/system/Collections/Concurrent/ConcurrentQueue.cs,0e91b925b71182e1)
- [메서드 매개 변수(C# 참조)](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/method-parameters)
    - [in 매개 변수 한정자(C# 참조)](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/in-parameter-modifier)
    - [ref(C# 참조)](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/ref)
    - [out 매개 변수 한정자(C# 참조)](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/out-parameter-modifier)
- [C# 7 : ref local](http://www.csharpstudy.com/Latest/CS7-ref-return.aspx)