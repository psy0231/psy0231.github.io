## C#에서 대리자의 발전
- 원 내용은 링크에 있고 대충 이렇단다.<sup id="a1">[1](#f1)</sup>
- 1.0때 명시적 delegate가 나왔고,  
2.0때 맹시적일 필요가 없는 Anonymous Method 가 나오고  
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
anonymous method에서 delegate를 빼고 (parameter) => (식) 이렇게 간소화한 표현으로 보임.

## etc
- 위 까지 기본 설명. 사용법은 사실 저정도로 짧게 끝나는듯.
- 여기는 그 외 설명인데 MSDN에서 따로 설명한 부분들이 많아서 그냥 정리하고 넘어가려함.<sup id="a2">[2](#f2)</sup>
- 순서대로 봤다면 지금까지 설명에 없던 개념들이 나오는데 관련 내용은 추후에 정리를 하던가 직접 찾아봐야할것같음. 
    - 미래의 나에게.
    
### Async Lambdas
### Lambda expressions and tuples
### Lambdas with the standard query operators
### Type inference in lambda expressions
### Capture of outer variables and variable scope in lambda expressions



## ref
- [Lambda expressions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions#expression-lambdas)<b id="f1"></b>[↩](#a1)
- [익명 함수(C# 프로그래밍 가이드)](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/statements-expressions-operators/anonymous-functions)<b id="f2"></b>[↩](#a2)