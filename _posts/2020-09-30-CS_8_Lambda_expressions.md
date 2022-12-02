---
title: 8. Lambda expressions
date: 2020-09-30 18:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-09-30 23:20:36 +0900
---

## C#에서 대리자의 발전
- 원 내용은 링크에 있고 대충 이렇단다.<sup id="a1">[1](#f1)</sup>
  
  1.0때 명시적 delegate가 나왔고,  
  2.0때 명시적일 필요가 없는 Anonymous Method 가 나오고  
  3.0때 Lambda Expressions이 나왔다고 한다.  

  위 둘은 봤으니 마지막 Lambda expressions 차례.

## Lambda expressions
- 이런 식을 lambda expressions 라고 한다.
  - 식 람다 : 식이 본문으로 보함된 형식  
  (input-parameters) => expression
  - 문장 람다 : 문 블록이 본문으로 포함된 형식  
  (input-parameters) => {\<sequence-of-statements>}
- 근데 둘이 다른점이 뭐임???  
한줄이면 식이고 블록이면 문단임??
- ex
  ```c#
  class lambda_1
  {
      delegate void test_del();
      test_del test; 

      public lambda_1()
      {
          test = () => { Console.WriteLine("test_1"); };
          test();

          test += () => { Console.WriteLine("test_2"); };
          test();

          test_del tt = () => { Console.WriteLine("test_3"); };
          tt();

          Action act_1 = () => { Console.WriteLine("act_1"); };
          act_1();

          Action act_2 = () =>  Console.WriteLine("act_2");
          act_2();

          Func<int, int, int> func_1 = (int a, int b) =>
          {
              return a * b;
          };
          Console.WriteLine(func_1(10, 12));

          Func<int, int> func_2 = (int a) => a * a;
          Console.WriteLine(func_2(10));
      }
  }
  ```
  ```
  test_1
  test_1
  test_2
  test_3
  act_1
  act_2
  120
  100
  ```
- 일단, 식이건 블록이건 별 다른게 없어보이고(act_1 vs act_2)  
delegate에서 뻗어나와서 그런지 Action, Func 사용이 자유로움  
anonymous method에서 delegate를 빼고  
(parameter) => (식) 이렇게 간소화한 표현으로 보임.

## etc
- 위 까지 기본 설명. 사용법은 사실 저정도로 짧게 끝나는듯.
- 여기는 그 외 설명인데 MSDN에서 따로 설명한 부분들이 많아서  
그냥 정리하고 넘어가려함.<sup id="a2">[2](#f2)</sup>
- 순서대로 봤다면 지금까지 설명에 없던 개념들이 나오는데  
관련 내용은 추후에 정리를 하던가 직접 찾아봐야할것같음. 
  - 미래의 나에게.
- 나머지는 나중에 보고 필요하다 생각되면 또는 이해가 된다면 정리함.

### Async Lambdas.
- async 및 await 으로된 비동기 이벤트도 간단히 할 수 있다?  

  그러니까 이걸 
  ```c#
  public partial class Form1 : Form
  {
      public Form1()
      {
          InitializeComponent();
          button1.Click += button1_Click;
      }

      private async void button1_Click(object sender, EventArgs e)
      {
          await ExampleMethodAsync();
          textBox1.Text += "\r\nControl returned to Click event handler.\n";
      }

      private async Task ExampleMethodAsync()
      {
          // The following line simulates a task-returning asynchronous process.
          await Task.Delay(1000);
      }
  }
  ```

- 이렇게
  ```c#
  public partial class Form1 : Form
  {
      public Form1()
      {
          InitializeComponent();
          button1.Click += async (sender, e) =>
          {
              await ExampleMethodAsync();
              textBox1.Text += "\r\nControl returned to Click event handler.\n";
          };
      }

      private async Task ExampleMethodAsync()
      {
          // The following line simulates a task-returning asynchronous process.
          await Task.Delay(1000);
      }
  }
  ```

### Lambda expressions and tuples
- C# 7.0부터 C# 언어에서 튜플을 기본적으로 지원.  
- 람다 식에 인수로 튜플을 제공할 수 있으며  
람다 식에서 튜플을 반환할 수도 있음.  
- 경우에 따라 C# 컴파일러는 형식 유추를 사용하여  
튜플 구성 요소의 형식을 확인할 수 있다.
- 쉼표로 구분된 해당 구성 요소 목록을  
괄호로 묶어 튜플을 정의함.  
- ex 
  ```c#
  Func<(int, int, int), (int, int, int)> doubleThem = ns => (2 * ns.Item1, 2 * ns.Item2, 2 * ns.Item3);
  var numbers = (2, 3, 4);
  var doubledNumbers = doubleThem(numbers);
  Console.WriteLine($"The set {numbers} doubled: {doubledNumbers}");
  // Output:
  // The set (2, 3, 4) doubled: (4, 6, 8)
  ```
  - 3개 구성 요소가 있는 튜플을 사용하여  
  숫자 시퀀스를 람다 식에 전달하고  
  각 값을 두 배로 늘린 후 곱하기의 결과가 포함된  
  3개 구성 요소가 있는 튜플을 반환.
- ex
  ```c#
  Func<(int n1, int n2, int n3), (int, int, int)> doubleThem = ns => (2 * ns.n1, 2 * ns.n2, 2 * ns.n3);
  var numbers = (2, 3, 4);
  var doubledNumbers = doubleThem(numbers);
  Console.WriteLine($"The set {numbers} doubled: {doubledNumbers}");
  
  ```
  - 일반적으로 튜플 필드의 이름은 Item1, Item2 등입니다.  
  그러나 다음 예제에서처럼 명명된 구성 요소가 있는 튜플을 정의 가능.


### Lambdas with the standard query operators
### Type inference in lambda expressions
### Capture of outer variables and variable scope in lambda expressions



## ref
- [Lambda expressions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions#expression-lambdas)<b id="f1"></b>[↩](#a1)
- [익명 함수(C# 프로그래밍 가이드)](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-functions)<b id="f2"></b>[↩](#a2)