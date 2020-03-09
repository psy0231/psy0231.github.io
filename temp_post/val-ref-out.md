---
title: title
date: 2020-03-04 16:00:00 +0900
categories: [cate1, cate2]
tags: [tag1, tag2]
seo:
  date_modified: 2020-03-06 08:08:04 +0900
---

## 뭔데
- 문득 ref랑 out랑 차이가 뭔지 궁금함
- 비슷해보임 실제 찾아봤을때도 out는 할당 필요없음 이러고 끝내는 설명도 있었고 문서는 주절주절 말많아서 그 중 몇개만 추리고 짐작해서 써봄
- 쉬운거부터.
    - 공통점은 둘다 reference참조 전달임.
    - 굳이 왜?
    - 예를들어 temp_3에 1을 더해보면
        ```C#
        int temp_3 = 100;
        public void vt_1(int _a)
        {
            _a += 1;
        }

        vt_1(temp_3);
        cw(temp_3);
        ```
        - 값을 넘기기때문에 실제 temp_3은 안바뀜
        - 이건 당연하겠지
    - 따라서 위 목적에 맞게는
        ```c#
        int temp_3 = 100;
        public int vt_2(int _a)
        {
            int res = _a+1;
            return res;
        }
        temp_3 = vt_2(temp_3);
        cw(temp_3);
        ```

## ref
- 먼저, 설명을 보면 중간에

        An argument that is passed to a ref or in parameter must be initialized before it is passed. This differs from out parameters, whose arguments do not have to be explicitly initialized before they are passed.
- 이게 위 예시랑 비슷해 보인다 
    - temp_3은 사용전에 먼저 선언, 할당되어 있어야 한다.
    - vt_2의 return 값을 temp_3에 넣어 끝낸다.
- ref 사용으로 바꿔보면
    ```C#
    int temp_1 = 0;
    public void rf(ref int _a)
    {
        _a = 1;
    }
    rf(ref temp_1);
    cw(temp_1);
    ```
    - value전달시 return을 다시 원본에 넣는과정을 ref는 알아서 해주는거 아닐까? 하고 넘어가면 똑같아 보임 둘은.
- 이런 이유로 생겨나지 않았을까 싶음...아님말고

## out
- ref는 그렇다 치고 이건 둘다 비슷함.
- 찾아봐도 시원한 답이 없었음.
- 차이점부터 비교 ㄱㄱ

    |             |ref             |out|
    |:---         |:---            |:---|
    |변수 초기화  | 호출 전 초기화  | 상관없음
    |변수 사용    | 사용 가능      | 사용 불가
    |반환(!return)| 상관없음       | 있어야함
-   먼저 비교
    ```C#
    int temp_1 = 0;
    int temp_2 = 10;

    rf(ref temp_1);
    ot(out temp_2);
    
    public void rf(ref int _a)
    {
        _a += 1;
    }

    public void ot(out int _a)
    {
        //Console.WriteLine(_a);
        _a=0;
        _a += 2;

    }
    ```
- 변수 초기화
    - ref는  temp_1 =0; 이라고 꼭 써야함 temp_1; 하면 에러
    - out사용은 temp_2 =10; 에서 temp_2; 라고 끝내도 상관없음
- 변수 사용
    - ref는 받은 변수 사용 가능 -> rf의 인자 _a는 temp_1의 값을 갖고옴
    - out는 받은 변수 사용 가능 -> ot의 인자 _a는 사용 불가.
        - 예를들어 ot에서 Console.WriteLine(_a); 를 먼서 사용은 불가.(위 주석부분)
        - 단, _a +=2 이후는 사용 가능... 이건 당연해 보이는데 ...
        - 그러니까 여기서 확연히 다른점은 out사용시 와부에서 전달 할 변수는 초기화 필요가 없고  
        함수 내부에서 값 할당.
- 반환
    > 이거 일단 맞는말을 뭐라 할지 몰라 씀  
    함수 return 말하는게 아니라 out,ref값으로 받은 변수 반환값임.  
    int temp(ref _a){} 라고 할 때 _a를 말함

    - ref는 _a +=1; 이 없어도 상관없음, 다시 말해 인자값으로 받은걸 안써도 상관없음
    - out는 인자로 받은 _a값에 뭐든 넣어 내보내야함.
    - 다시말해 rf안에 내용은 없어도 상관없는데 ot는 받은 _a에 값을 할당해 끝내야함  
    안하면 "제어가 현재 메서드를 벗어나기 전에 '_a' out 매개 변수를 할당해야 합니다."

## out 2
- 근데 왜??
    
## Test
> 위에 썼던거
```C#
        static void Main(string[] args)
        {

            int temp_1 = 0;
            int temp_2 = 10;
            //int temp_2;
            int temp_3 = 100;

            ValTest vt = new ValTest();
            Console.WriteLine("vt_1 before : {0}", temp_3.ToString() );
            vt.vt_1(temp_3);
            Console.WriteLine("vt_1 after : {0}", temp_3.ToString());
            temp_3 = vt.vt_2(temp_3);
            Console.WriteLine("vt_2 after : {0}", temp_3.ToString());

            ///
            Console.WriteLine("before");
            Console.WriteLine(temp_1);
            Console.WriteLine(temp_2);
            
            RefTest rt = new RefTest();
            OutTest ot = new OutTest();

            Console.WriteLine("after");
            rt.rf(ref temp_1);
            ot.ot(out temp_2);
            
            Console.WriteLine(temp_1);
            Console.WriteLine(temp_2);
        }
    }

    class RefTest
    {
        public void rf(ref int _a)
        {
            _a = 1;
        }
    }

    class OutTest
    {
        public void ot(out int _a)
        {
            //Console.WriteLine(_a);
            _a = 2;

        }
    }

    class ValTest
    {
        public void vt_1(int _a)
        {
            _a += 1;
        }

        public int vt_2(int _a)
        {
            int res = _a+1;
            return res;
        }
    }
```