---
title: Anonymous
date: 2020-09-14 13:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-09-15 10:46:27 +0900
---

## Anonymous
- 무명.  
이름이 없음.  
그 중 최고는 무명왕.
- 이름처럼 이름없이 쓸때 씀

## Anonymous Type
- 명시적인 형 지정 없이 쓰는것  
키워드는 var.  
여기 내용은 Anonymous variable과 Anonymous Type 둘다.
- 사용예
    ```c#
    class AnonymousType
    {
        //var wrong;
        public AnonymousType()
        {
            //var wrong2;

            var temp = new 
            { 
                a = 1, 
                b = 2, 
                c = "string", 
                d = new List<int>{1, 2, 3}
            };

            Console.WriteLine(string.Format("{0}, {1}, {2}",temp.a, temp.b, temp.c));
            //temp.a = 123;
            var i = 0;
            i = 123;
            var j = new List<int>();
            j.Add(1);
        }
    }
    ```
- 특징을 보면
    - class 범위에는 사용할 수 없고 지역변수로만 가능.  
    var wrong의 경우 에러가 나는 이유.
    - 선언과 함께 초기화가 필요함  
    따라서 wrong2도 에러 .
    - var i, j 의 경우 Anonymous variable이고, 이 경우 수 정 가능.  
    
        var temp의 경우 Anonymous Type으로  
        위와 같이 여러 속성을 넣을 수 있음.  
        class처럼보임. 근데 method는 안되는듯?  
        이 때, 속성은 읽기 전용으로 수정 불가능.
- 이거 많이 사용되는건 LINQ사용할 때 라고 함.
    ```c#
    var temp2 = new[]
    {
        new { cls = 1, name = "a"},
        new { cls = 2, name = "b" },
        new { cls = 1, name = "c"}
    };

    var temp2_list = from t in temp2
                     where t.cls == 1
                     select t.name;

    foreach (var item in temp2_list)
    {
        Console.WriteLine(item);
    }
    ```
    - 사실 잘 모르겠다. LINQ 정리하다보면 알게되겠지뭐.
    
## Anonymous Method
- 이전 delegate에서는 이미 있던 method를 연결해 사용했다.  
만약 단순하거나 단발성 사용등일떄 이름 없이 만들수 있다.

- 간단한 사용범들
    ```c#
    class AnonymousMethod
    {
        public delegate void TestDelegate();
        public TestDelegate td;

        public delegate T3 TestDelegate2<T1, T2, T3>(T1 a,  T2 b);
        public TestDelegate2<int , int , string > td2;

        public AnonymousMethod()
        {
            td = delegate 
            {
                Console.WriteLine("test");
            };
            td();

            td2 = delegate (int a, int b)
            {
                return (a + b).ToString();
            };
            Console.WriteLine(td2(1, 2));


            Action act = delegate
            {
                Console.WriteLine("test2");
            };
            act();

            Action<int, int> act2 = delegate(int a, int b)
            {
                Console.WriteLine(a+b);
            };
            act2(10, 11);

            Func<int, int, string> func = delegate (int a, int b)
            {
                return (a + b).ToString();
            };
            Console.WriteLine(func(22,45));
        }
    }

    
    //test
    //3
    //test2
    //21
    //67
    ```
- delegate를 이용하기떄문에 Action, Func도 사용 가능. 
- 한번 만들어 놓으면 해당 delegate를 쓸 수 있는곳에서는 걔속 쓸 수 있음...   
1회용이 아니라 이름만 없던것. 매초에 왜 1회용이라고 생각했음? 이름도 이름없음인데..

## 참고.
- [C# 무명 메서드 (Anonymous Method)](http://www.csharpstudy.com/CSharp/CSharp-anonymous-method.aspx)