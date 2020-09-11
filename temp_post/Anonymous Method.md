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
