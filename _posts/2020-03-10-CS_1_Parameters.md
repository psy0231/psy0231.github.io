---
title: 1. Parameters
date: 2020-03-10 20:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-10-06 10:53:47 +0900
---
## intro

- method의 parameter에 대해.
- ref / out에서 계속 확장됨..
- params, ref, out, in이 주요내용.

---

## params

- paramters를 가변개수의 변수가 있다고 지정.
- 1차원 배열만 가능
- `params`뒤에는 추가로 parameter추가가 안됨.  
앞에는 상관없음.
- `param`키워드는 선언시 하나만 쓸 수 있음.
- `,`로 구별해 넣으면 알아서 들어감.  
배열로 넣으면 그거 통으로 들어감(차원이 안늘어남)
- 
  ```csharp
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
  

---

## Passing Parameters

- argument를 값 또는 참조로  
parameter에 전달한다.
- (call / pass) (by value / by reference) 형식 문제.
- value로 전달
  - 일반적으로 기본
  - 변수의 복사본을 method에 전달.
  - value type의 변수에는 해당 데이터가 직접 포함.
- reference로 전달
  - `ref`, `out`등을 써야함.
  - 변수에 대한 액세스 권한을 메서드에 전달.
  - reference type의 변수에는 해당 데이터에 대한 참조가 포함.
- 구조체는 value type.
- 클래스 인스턴스는 reference type.
  - reference type은 값으로 , 참조로 전달하는경우  
  약간 차이가 있음.

### ****Pass a value type by value****

- value type을 value로 넘김
- 호출된 method가  
parameter를 할당해  
다른 개체를 참조하는 경우  
변경 내용은 호출자에서 표시되지 않음.
- 호출된 method가  
parameter에서 참조하는 개체의  
상태를 수정하는 경우  
변경 내용은 호출자에서 표시되지 않음.
- 
  ```csharp
  int n = 5;
  System.Console.WriteLine("The value before calling the method: {0}", n);
  
  SquareIt(n);  // Passing the variable by value.
  System.Console.WriteLine("The value after calling the method: {0}", n);
  
  // Keep the console window open in debug mode.
  System.Console.WriteLine("Press any key to exit.");
  System.Console.ReadKey();
  
  static void SquareIt(int x)
  // The parameter x is passed by value.
  // Changes to x will not affect the original value of x.
  {
      x *= x;
      System.Console.WriteLine("The value inside the method: {0}", x);
  }
  /* Output:
      The value before calling the method: 5
      The value inside the method: 25
      The value after calling the method: 5
  */
  ```
    

### ****Pass a value type by reference****

- value type을 reference로 넘김
- 호출된 method가  
parameter를  할당해  
다른 개체를 참조하는 경우  
변경 내용은 호출자에서 표시되지 않음.
  - ??????????
  - 반영되어야 정상아님?
- 호출된 method가  
parameter에서 참조하는 개체의  
상태를 수정하는 경우  
변경 내용은 호출자에서 표시됨.
- 
  ```csharp
  int n = 5;
  System.Console.WriteLine("The value before calling the method: {0}", n);
  
  SquareIt(ref n);  // Passing the variable by reference.
  System.Console.WriteLine("The value after calling the method: {0}", n);
  
  // Keep the console window open in debug mode.
  System.Console.WriteLine("Press any key to exit.");
  System.Console.ReadKey();
  
  static void SquareIt(ref int x)
  // The parameter x is passed by reference.
  // Changes to x will affect the original value of x.
  {
      x *= x;
      System.Console.WriteLine("The value inside the method: {0}", x);
  }
  /* Output:
      The value before calling the method: 5
      The value inside the method: 25
      The value after calling the method: 25
  */
  ```

### ****Pass a reference type by value****

- reference type을 value로 넘김
- 호출된 method가  
parameter를 할당해  
다른 개체를 참조하는 경우  
변경 사항은 호출자에게 표시되지 않음.
- 호출된 method가  
parameter에서 참조하는 개체의   
상태를 수정하는 경우  
변경 사항은 호출자에게 표시됨.
- 
  ```csharp
  int[] arr = { 1, 4, 5 };
  System.Console.WriteLine("Inside Main, before calling the method, the first element is: {0}", arr[0]);
  
  Change(arr);
  System.Console.WriteLine("Inside Main, after calling the method, the first element is: {0}", arr[0]);
  
  static void Change(int[] pArray)
  {
      pArray[0] = 888;  // This change affects the original element.
      pArray = new int[5] { -3, -1, -2, -3, -4 };   // This change is local.
      System.Console.WriteLine("Inside the method, the first element is: {0}", pArray[0]);
  }
  /* Output:
      Inside Main, before calling the method, the first element is: 1
      Inside the method, the first element is: -3
      Inside Main, after calling the method, the first element is: 888
  */
  ```
  
  - 매개 변수가 arr에 대한 참조이므로  
  배열 요소의 값을 변경가능.
  - 그러나 method 내부에서 new로 새로 할당하면  
  새 배열을 참조하게됨.  
  이 때부터는 전달받았던 arr에 영향을 주지 않음.

### ****Pass a reference type by reference****

- reference type를 reference로 넘김
- 호출된 method가  
parameter를 할당해  
다른 개체를 참조하는 경우  
변경 내용은 호출자에서 표시됨.
- 호출된 method가  
parameter에서 참조하는 개체의  
상태를 수정하는 경우
변경 내용이 호출자에서 표시됨.
- 
  ```csharp
  int[] arr = { 1, 4, 5 };
  System.Console.WriteLine("Inside Main, before calling the method, the first element is: {0}", arr[0]);
  
  Change(ref arr);
  System.Console.WriteLine("Inside Main, after calling the method, the first element is: {0}", arr[0]);
  
  static void Change(ref int[] pArray)
  {
      // Both of the following changes will affect the original variables:
      pArray[0] = 888;
      pArray = new int[5] { -3, -1, -2, -3, -4 };
      System.Console.WriteLine("Inside the method, the first element is: {0}", pArray[0]);
  }
  /* Output:
      Inside Main, before calling the method, the first element is: 1
      Inside the method, the first element is: -3
      Inside Main, after calling the method, the first element is: -3
  */
  ```
    

---

## in

- 참조전달
- 읽기전용
- 별다르게 할 수 있는 작업이 없는것같음.
- 일반적으로 `in`은 꼭 쓸 필요는 없음
  ```csharp
  int readonlyArgument = 44;
  InArgExample(readonlyArgument);
  Console.WriteLine(readonlyArgument);     // value is still 44
  
  void InArgExample(in int number)
  {
      // Uncomment the following line to see error CS8331
      //number = 19;
  }
  ```
    
- `in` argument로 전달되는 변수는  
method에 전달되기 전에 초기화되어야 함.
- 호출된 method는  
argument에 값을 할당하거나 수정할 수 없음.

### 참조전달 vs 읽기전용

- 참조전달의 경우 전달받은 method에서  
값변경이 일어날 경우  
결과가 원본에도 영향을 줬다.
- 참조전달의 경우 access 권한을 넘긴다했고  
readonly의 경우 수정을 못하게하는 특성일 때  
`in`은 둘 다 갖고있음.

  ```csharp
  List<int> arr = new List<int>{ 1, 4, 5 };
  System.Console.WriteLine("Inside Main, before calling the method, the first element is: {0}", arr[0]);
  
  Change(in arr);
  System.Console.WriteLine("Inside Main, after calling the method, the first element is: {0}", arr[0]);
  
  static void Change(in List<int> pArray)
  {
      // Both of the following changes will affect the original variables:
      pArray[0] = 888;
      pArray.Add(9);
  
      foreach(var i in pArray){
          Console.Write("{0} ", i);
      }
      Console.WriteLine();
      // pArray = new List<int>{ -3, -1, -2, -3, -4 };
      System.Console.WriteLine("Inside the method, the first element is: {0}", pArray[0]);
  }
  /* Output:
      Inside Main, before calling the method, the first element is: 1
      888 4 5 9 
      Inside the method, the first element is: 888
      Inside Main, after calling the method, the first element is: 888
  */
  ```
  
  - 참조형식을 참조로 전달의 예시에서  
  `ref`를 `in`으로, array를 list로 변경.
  - 메서드로 전달되는 list는 수정가능  
  외부 원본도 변경.
  - 메서드에서 전달받은 list를 새로 할당 불가.
    - `// pArray = new List<int>{ -3, -1, -2, -3, -4 };`이 부분이문제.
    - error CS8331: 읽기 전용 변수이므로  
    변수 'in List<int>'에 할당할 수 없음.
- 값형식을 `in`으로 전달하는 경우에도 변경불가였음  
이 경우 값은 변경을 새로 할당으로 동작하는거면  
둘이 비슷해지는것같음.
- 아무튼 읽기전용은 새로 할당을 허용하지 않음.  
비유하면 `malloc`의 시작 포인터를 못바꾸는 느낌?

---

## ref

- 변수가 참조이거나 다른 개체의 별칭임을 나타냄.  
다음 과 같은 용도가 있다함.
  - method 시그니처 및 메서드 호출에서  
  인수를 메서드에 참조로 전달.
  - method 시그니처에서 값을 호출자에게 참조로 반환.
  - 멤버 본문에서 참조 반환 값이  
  호출자가 수정하려는 참조로 로컬에 저장됨을 나타냄.  
  또는 지역 변수가 참조로 다른 값에 액세스함을 나타냄.
  - `struct` 선언에서 `ref struct` 또는  
  `readonly ref struct`를 선언.
  - 선언에서 `ref struct` 필드가 참조임을 선언.

### ****참조로 argument 전달****

- 값이아닌 참조로 전달.
- method 정의, 호출시 명시적으로 사용해야함.
- method로 전달되기 전에 초기화 해야함.
- 값, 참조형식을 참조로 전달에 해당하는듯.
    
  ```csharp
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
    
  - 호출된 메서드에서 새 할당이 일어나면  
  호출자가 참조하던것도 새 객체로 바뀜

### 참조 반환 값

- `ref return`
- method가 호출자에게 참조로 반환.
- 호출자는 method에서 반환된 값을 수정할 수 있으며  
해당 변경 내용은 호출된 method의 개체 상태에 반영

  ```csharp
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
  
  void test(){
      var bc = new BookCollection();
      bc.ListBooks();
  
      ref var book = ref bc.GetBookByTitle("Call of the Wild, The");
      if (book != null)
          book = new Book { Title = "Republic, The", Author = "Plato" };
      bc.ListBooks();
      // The example displays the following output:
      //       Call of the Wild, The, by Jack London
      //       Tale of Two Cities, A, by Charles Dickens
      //
      //       Republic, The, by Plato
      //       Tale of Two Cities, A, by Charles Dickens
  }
  
  test()
  ```
    
- 반대로 이 예시에서 `ref`를 전부 지운경우
    
  ```csharp
  Call of the Wild, The, by Jack London
  Tale of Two Cities, A, by Charles Dickens
  
  Call of the Wild, The, by Jack London
  Tale of Two Cities, A, by Charles Dickens
  ```
  가 출력됨
    
- 해당 차이는
  
  ```csharp
  if (book != null)
          book = new Book { Title = "Republic, The", Author = "Plato" };
      bc.ListBooks();
  ```
  
  이 부분 에서 확연한데  
  `ref`를 쓰면 이 전 객체를 여전히 참조함.
    
- 보통은 new 로 새로 할당 안하고  
return 받은 객체에서 필요한걸 하지않음??
  
  ```csharp
  if (book != null)
          book.Title = "Republic, The"; 
          book.Author = "Plato" ;
  ```
  
  이런식으로…
    

### 참조 로컬

- 참조 지역 변수
- `return ref`을 사용하여 반환된 값을 참조.
- ref 지역 변수는 ref 값으로 초기화 해야함.  
즉, 초기화의 오른쪽은 참조여야 한다.
- 참조 로컬 값의 수정 내용은  
method가 값을 참조로 반환하는 개체 상태에 반영.
- 따라서 `return ref`랑 짝으로 생각하고  
두 위치에 키워드를 써야함
  - 변수 선언 앞
  - ref return 메서드 호출 앞
  
  ```csharp
  ref var book = ref bc.GetBookByTitle("Call of the Wild, The");
  ```
    
- 동일한 방법으로 참조로 값에 액세스할 수 있다.  
경우에 따라 참조로 값에 액세스하면  
비용이 많이 들 수 있는 복사 작업을 피함으로써 성능이 향상됨.  
예를 들어, 값을 참조하는 데 사용되는  
참조 지역 변수를 정의하는 방법.
  
  ```csharp
  ref VeryLargeStruct reflocal = ref veryLargeStruct;
  ```
    

### ****Ref readonly 로컬****

- 참조 읽기 전용 로컬은  
해당 시그니처에 `ref readonly`가 있고  
`return ref`를 사용하는 메서드 또는  
속성으로 반환된 값을 참조하는 데 사용.
- 변수는 `ref readonly` 지역 변수의  
`ref` 속성을 변수와 `readonly` 결합.  
변수는 할당된 스토리지에 대한 별칭이며 수정할 수 없음.

### ****ref 필드****

- `ref struct`유형에서 `ref`필드를 선언할 수 있다.  
`ref` 필드는 참조가 참조하는 객체보다 오래 지속되지 않도록  
`ref struct`유형에서만 유효.  
이 기능은 `System.Span<T>`와 같은 유형을 활성화.
  
  ```csharp
  public readonly ref struct Span<T>
  {
      internal readonly ref T _reference;
      private readonly int _length;
  
      // Omitted for brevity...
  }
  ```
  
  - `Span<T>`는 액세스하는 데 사용되는 참조를 저장.
  - 참조를 사용하면 `Span<T>` 개체가 참조하는  
  저장소의 복사본을 만들지 않도록 할 수 있음.
  - 복사작업 피해서 성능향상관련인가?

---

## ****out****

- `ref`와 대부분 비슷함.
- `out` 인수로 전달되는 변수는  
method 호출에서 전달되기 전에  
초기화할 필요가 없지만  
호출된 method는 반환되기 전에  
값을 할당해야 한다.

### ****out 매개 변수 선언****

- `out`은여러 값을 반환하기 위한 일반적인 방법.  
비슷하게 튜플이 있음.  
튜플을 더 권장하는것같음.
- `ref`도 될 수 있겠지만  
method return 전에 out은 받은 parameter할당이  
강제적이기때문에 여기 나온듯

### ****out 인수를 사용하여 메서드 호출****

- out는 parameter로 전달할 때 초기화가 필요 없었음.
  
  ```csharp
  string numberAsString = "1640";
  
  //int number;
  if (Int32.TryParse(numberAsString, out var number))
      Console.WriteLine($"Converted '{numberAsString}' to {number}");
  else
      Console.WriteLine($"Unable to convert '{numberAsString}'");
  // The example displays the following output:
  //       Converted '1640' to 1640
  ```
    
- `out`으로 굳이 뭘 만들어 보낼 필요가 없음.  
지금까지는 `int number;`를 선언하고 썼는데 필요없음.  
따라서 `out`으로 반환받을 데이터 형도 정할 필요가 없어짐.  
더 깔끔한 코드가 된다길래…

---

## in, ref, out

### 공통

- method의 매개 변수 목록에 사용되는 경우  
`in`, `ref`, `out` 키워드는 인수가 값이 아니라   
참조로 전달됨을 나타냄.
- 이 키워드는 정식 매개 변수를 위해  
해당 인수의 별칭을 만드는데,  
이는 반드시 변수여야 한다.  
즉, 매개 변수에 대한 모든 작업이 인수에서 수행.
  - 아무리봐도 뭔말인지 모르겠음
- overload
    - `in`, `ref`, `out`은 오버로드를 위한  
    method 시그니처의 일부로 간주되지 않음. 
    - 예를 들어 오버로드 할 두 method 모두  
    `in`, `ref`, `out`을 사용할 경우  
    오버로드 할 수 없음.(컴파일 에러)
      
      ```csharp
      class CS0663_Example
      {
          // Compiler error CS0663: "Cannot define overloaded
          // methods that differ only on ref and out".
          public void SampleMethod(out int i) { }
          public void SampleMethod(ref int i) { }
      }
      ```
      
      비정상
      
      ```csharp
      class RefOverloadExample
      {
          public void SampleMethod(int i) { }
          public void SampleMethod(ref int i) { }
      }
      ```
      
      정상
        
    - 숨기기 또는 재정의와 같이  
    서명 일치가 필요한 다른 상황에서는  
    `in`, `ref`, `out`이 서명의 일부로 간주 
- `in`, `ref`, `out`을 사용 못하는 method
    - async method
    - yield return 또는 yield break를포함하는 반복기 method
- 확장 메서드에서 각각의 제한사항..

### 차이점
- 
  |             |    in   |   ref   | out |
  | ----------- | ------- | ------- | --- |
  | 키워드 명시  |   선택  |   필수  | 필수 |
  |    초기화    | 전달 전 | 전달 전 | 함수 안 |
  |     수정    |   불가  |  가능   | 필수 |
```markdown

```

---

## 참고

- [TryParse](https://referencesource.microsoft.com/#mscorlib/system/int32.cs,325507e509229dbc)
- [TryDequeue](https://referencesource.microsoft.com/#mscorlib/system/Collections/Concurrent/ConcurrentQueue.cs,0e91b925b71182e1)
- [메서드 매개 변수(C# 참조)](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/method-parameters)
- [C# 7 : ref local](http://www.csharpstudy.com/Latest/CS7-ref-return.aspx)