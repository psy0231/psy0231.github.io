---
title: Delegate
date: 2020-09-07 13:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-09-13 13:29:46 +0900
---

## Delegate
- 이전에 Action, Func에서 봤던 형식으로 이 둘이 좀더 표준화 느낌이라 했는데 그럼 delegate에 대해 좀더 정리해보자.
- 메서드에 대한 참조를 나타내는 타입. 
- delegate는 뜻이 대리자임. 하는짓 그 자체. method를 가리키거나 method자체를 parameter로 넘길때 쓴다함.
- 쨌든 method를 직접 call하는 대신 이 delegate로 대신해 call 할 수 있다.
- delegate 선언때 return, parameter을 지정하는데 실제 method가 이 delegate형식과 맞으면 붙여다 쓸 수 있고, 하나 이상 붙여 쓸 수 있다. 

## 기본
- 일단써봄
    ```c#
    class Program
    {
        static void Main(string[] args)
        {
            deletgate_1 del_1 = new deletgate_1();
            del_1.test();
        }
    }
    class deletgate_1
    {
        public delegate void testdel();
        public testdel test;

        public deletgate_1()
        {
            //test = new testdel(mtd_1);
            test = mtd_1;
            test();
        }

        public void mtd_1()
        {
            Console.WriteLine("mtd_1");
        }
    }
    ```
- 결과 
    ```
    mtd_1
    mtd_1
    ```
- 여기서 test에 mtd_1을 연결한 후 test(); 를 호출할 때마다 mtd_1이 실행됨.  
여기서는 deletgate_1의 ctor에서 한번 Main에서 한번.
- 그러니까 delegate로 연결 한 method를 직접 call하지 않고 delegate를 call함으로써 연결 된 method를 사용하는 효과를 냄.
- 주석 처리된 부분은 원래 저렇게 써야되는것 같은데 편의상(?) 아래처럼 쓰는듯하다. 주석처럼 쓰더라도 test();로 콜하는건 달라지지않음.  
- delegate는 이벤트가 처음 시작되는 애한테 쥐어줌.
- 하나 더 써보면 delegate는 class외부에 쓸 수 있음
    ```c#
    class Program
    {
        static void Main(string[] args)
        {
            deletgate_1 del_1 = new deletgate_1();
            del_1.test();
        }
    }

    public delegate void testdel();
    class deletgate_1
    {
        public testdel test;

        public deletgate_1()
        {
            //test = new testdel(mtd_1);
            test = mtd_1;
            test();
        }

        public void mtd_1()
        {
            Console.WriteLine("mtd_1");
        }
    }
    ```

## return, paraneters
- delegate return method(parameter)
    ```c#
    class Program
    {
        static void Main(string[] args)
        {
            rtnprm rp = new rtnprm();
            rp.cal = rp.add;
            Console.WriteLine(rp.cal(2, 1));
            
            rp.cal = rp.sub;
            Console.WriteLine(rp.cal(2, 1));
        }
    }

    class rtnprm
    {
        public delegate int delcal(int a, int b);
        public delcal cal;

        public int add(int _a, int _b)
        {
            Console.WriteLine("add");
            return _a + _b;
        }

        public int sub(int _a, int _b)
        {
            Console.WriteLine("sub");
            return _a - _b;
        }
    }
    ``` 
- 결과 
    ```
    add
    3
    sub
    1
    ```
- 여기 delegate는 return은 int, parameter은 int, int 임. 아래 add, sub도 같은 형식의 method이다. 따라서 delegate는 이 둘 method를 참조 할 수 있다.
- delegate와 형식만 같다면 위처럼 add를 하건 sub를 하건 그때그때 붙여 쓸 수 있다.

## Generalization
- 위에서 return, parameter을 일반화할 수 있음
    ```c#
    class Program
    {
        static void Main(string[] args)
        {
            Generalization gen = new Generalization();
            gen.cal_int = gen.add;
            Console.WriteLine(gen.cal(1,2));

        }
    }
    class Generalization
    {
        public delegate T3 delcal<T1,T2,T3>(T1 a, T2 b);
        public delcal<int, int, double> cal;
        //public delcal<double> cal_dou;

        public Generalization()
        {

        }

        public double add(int _a, int _b)
        {
            Console.WriteLine("add");
            return _a + _b;
        }
    }
    ```
- 어디서 많이 본 그림이다?

## chain
- 위 예시에서 return, parameter조건이 다 같다. 만약 항상 add, sub을 같이 불러야한다면?
- delegate가 나오면 자주보이는 키워드인데 이 delegate에 여러개 지정해놓고 동시에(연속으로) 실행가능. 위 예시에서 main만 수정하면됨 rtnprm은동일.
    ```c#
    rtnprm rp = new rtnprm();
    rp.cal = rp.add;
    rp.cal += rp.sub;
    Console.WriteLine(rp.cal(2,1));
    ```
- 결과 
    ```
    add
    sub
    1
    ```
- 이런식으로 = 이 아닌 +=로 더하면 된다.  
반대로, 뺄때는 당연히 -=로 ..
- 결과를 보면 add, sub가 찍힐걸 보니 method까진 간것같음 근데 출력은 마지막 결과값만 나옴.  
그래서 아래 찾아봤는데 의도는 조금 다르지만 비슷한 질문이 있음.  
결론만 말하자면 chain으로 사용하면 return은 void로 하고 사용하는걸 권장. 아니면 다른 방법으로 하는데 그건 해당 글 참조.<sup id="a1">[1](#f1)</sup>

- 무튼 muiltcast하는 효과를 볼 수 있다.

## use delegate as parameter
- 또한 delegate자체를 parameter로 넘겨줄 수 있다.
    ```c#
    class UseDelegateAsParameter
    {
        public delegate int delcal(int a, int b);
        public delcal cal;

        public UseDelegateAsParameter()
        {
            cal = add;
            test(cal);
        }

        public void test(delcal _dc)
        {
            Console.WriteLine(_dc(1, 2));
        }
        public int add(int _a, int _b)
        {
            Console.WriteLine("add");
            return _a + _b;
        }

        public int sub(int _a, int _b)
        {
            Console.WriteLine("sub");
            return _a - _b;
        }
    }
    ```

## delegate??
- 위 까지 보면 막상 이걸 왜쓰나 싶은데 써보면 편함. 
- 가장 편하게 쓰는건 class간 작업할 때.
    ```c#
    static void Main(string[] args)
    {
        DelegateClass_1 dc1 = new DelegateClass_1();
        DelegateClass_2 dc2 = new DelegateClass_2();
        dc1.from1 = dc2.mtd_1;
        dc1.mtd_1(1,2);
    }
    
    class DelegateClass_1
    {
        public delegate string delfrom1(int a, int b);
        public delfrom1 from1;

        public void mtd_1(int _a, int _b)
        {
            Console.WriteLine(_a + _b);
            Console.WriteLine(from1(_a, _b));
        }
    }

    class DelegateClass_2
    {
        public string mtd_1(int _a, int _b)
        {
            return _a.ToString() + _b.ToString();
        }
    }
    ```
- 쉬운 예를 들어 위에서 class1은 받은 수를 더하고 출력, class2는 받은 수를 string로 변환후 더함.  
main에서 class1, class2로 각각 method를 부른 것 이 아닌 class1에서 class2로 delegate만 연결 하고 class1의 method만 씀.  
이렇게 되면 class2의 method를 class1의 method인것처럼 쓰게됨.
- 가장 떠올리기 쉬운 상황은 form간 작업할 떄.  
서로 다른 form이 떠있을 때 data를 주고 받거나 다른쪽에 있는 method를 쓸때 유연하게 쓸 수 있다.
- 조금 더 일반적으로는 class A와 class B가 있을 때 class A에서 B를 만들지 않는 이상 접근을 위해서는 생성 된 B를 받아 쓰거나 A와 B를 생성한 상위 class에서 연결 역할을 해줘야함  
또, class를 맴버로 갖고있는 계층구조 단계가 좀 많을 경우, 예를들어 class A 안에 class B 안에 class C...class F 같은경우 class A 에서 class F로 바로 access할 경우 등  
귀찮아질 상황들이 delegate로 간단히 해결됨.  
이걸 미리 만들어놓은 Action, Function으로 하면 더 좋고. 

## 참고
- [Using a Multicast Delegate to chain functions](https://stackoverflow.com/questions/15227876/using-a-multicast-delegate-to-chain-functions) <b id="f1"></b>[↩](#a1)
- [C# DELEGATE에 대한 정리](https://tony-programming.tistory.com/entry/C-Delegate%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A0%95%EB%A6%AC)
- [C# 강좌 : 제 21강 - 델리게이트](https://076923.github.io/posts/C-21/)
- [C# 델리게이트(DELEGATE) 개념 잡히는 예제](https://link2me.tistory.com/928)
- [C# 강좌 19편. 델리게이트와 이벤트(Delegates and Events)](https://blog.hexabrain.net/151)
- [C# Delegate 기본 개념과 사용법](https://blog.naver.com/pis_guy/120039491803)
- [C# delegate (콜백, 체인, 범용성)](https://jeong-pro.tistory.com/51)
- [대리자(C# 프로그래밍 가이드)](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/delegates/)
- [대리자 사용(C# 프로그래밍 가이드)](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/delegates/using-delegates)
- [C# delegate의 개념](http://www.csharpstudy.com/CSharp/CSharp-delegate.aspx)
- [Delegate - callback chain](https://mrw0119.tistory.com/19)