---
title: ref / out
date: 2020-03-10 20:00:00 +0900
categories: [Grind, C#]
tags: [.net, C#]
seo:
  date_modified: 2020-03-10 20:36:04 +0900
---

## 뭔데
- 문득 ref랑 out랑 차이가 뭔지 궁금함
- 비슷해보임 실제 찾아봤을때도 out는 할당 필요없음 이러고 끝내는 설명도 있었고 문서는 주절주절 말많아서 그 중 몇개만 추리고 짐작해서 써봄
- 쉬운거부터.
    - 공통점은 둘다 reference참조 전달임.
    - 굳이 왜?
    - 예를들어 temp_3에 1을 더해보면

        ```c#
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

    > An argument that is passed to a ref or in parameter must be initialized before it is passed. This differs from out parameters, whose arguments do not have to be explicitly initialized before they are passed.

- 이게 위 예시랑 비슷해 보인다 
    - temp_3은 사용전에 먼저 선언, 할당되어 있어야 한다.
    - vt_2의 return 값을 temp_3에 넣어 끝낸다.
- ref 사용으로 바꿔보면

    ```c#
    int temp_1 = 0;
    public void rf(ref int _a)
    {
        _a += 1;
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
- 먼저 비교

    ```c#
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

## 그런데 왜???
- 둘다 비슷하다 근데 다르다 확실히. 왜 이렇게 구분지어놨을까  
ref는 생겨난 배경이 저럴것이다 하고 넘어갔으니 신경끄고 out만 따로 생각해봄.
- 잘 몰라서 직접 보기로함 
    - 흔히 보이는 이거

        ```c#
        int res;
        string input = "123";
        if (int.TryParse(input, out res))
        {
            Console.WriteLine(res);
        }
        ```

    - TryParse 

        ```c# 
        // Parses an integer from a String. Returns false rather
        // than throwing exceptin if input is invalid
        // 
        [Pure]
        public static bool TryParse(String s, out Int32 result) {
            return Number.TryParseInt32(s, NumberStyles.Integer, NumberFormatInfo.CurrentInfo, out result);
        }

        [System.Security.SecuritySafeCritical]  // auto-generated
        internal unsafe static Boolean TryParseInt32(String s, NumberStyles style, NumberFormatInfo info, out Int32 result) {

            Byte * numberBufferBytes = stackalloc Byte[NumberBuffer.NumberBufferBytes];
            NumberBuffer number = new NumberBuffer(numberBufferBytes);
            result = 0;

            if (!TryStringToNumber(s, style, ref number, info, false)) {
                return false;
            }

            if ((style & NumberStyles.AllowHexSpecifier) != 0) {
                if (!HexNumberToInt32(ref number, ref result)) { 
                    return false;
                }
            }
            else {
                if (!NumberToInt32(ref number, ref result)) {
                    return false;
                }
            }
            return true;           
        }
        ```

    - 하나 더 TryDequeue

        ```c#
        /// <summary>
        /// Attempts to remove and return the object at the beginning of the <see
        /// cref="ConcurrentQueue{T}"/>.
        /// </summary>
        /// <param name="result">
        /// When this method returns, if the operation was successful, <paramref name="result"/> contains the
        /// object removed. If no object was available to be removed, the value is unspecified.
        /// </param>
        /// <returns>true if an element was removed and returned from the beggining of the <see
        /// cref="ConcurrentQueue{T}"/>
        /// succesfully; otherwise, false.</returns>
        public bool TryDequeue(out T result)
        {
            while (!IsEmpty)
            {
                Segment head = m_head;
                if (head.TryRemove(out result))
                    return true;
                //since method IsEmpty spins, we don't need to spin in the while loop
            }
            result = default(T);
            return false;
        }
        ```

    - 둘다 return이 있고 parameter은 out임
        - 위 아래 둘 다 걸리는거 없이 성공하면 return true, result는 out로 반환(? 할당?)
    - 성공, 실패를 반환하고(이건 쓰기 나름이지만) return 결과 상관없이 parameter값을 무조건 할당 하고 싶으면 쓰는것같음
        - 근데 return 형이 void 면 됐나 안됐나 모르지않나? 그래서 bool을 붙여쓰는형이 많이 보이는듯? 
        - 초기값 상관없이 결과 받아야할때
- 결론? 
    - ref는 전달받은 param값을 사용해야 하고 값이 정확히 있으며 ref 결과값은 덜 신경씀
    - out는 전달받은 param형을 사용해야 하고 값은 없어도 무방   out 결과값은 무조건 받아야 할 떄 
    - 근데 ref는좀 달라졌다는데? 그건 나중에.

## 위에 썼던거
- [TryParse](https://referencesource.microsoft.com/#mscorlib/system/int32.cs,325507e509229dbc)  
- [TryDequeue](https://referencesource.microsoft.com/#mscorlib/system/Collections/Concurrent/ConcurrentQueue.cs,0e91b925b71182e1)

- basic

    ```c#
        class Program
        {
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

                int res;
                if (int.TryParse("123", out res))
                {

                }
            }
        }

        class RefTest
        {
            public void rf(ref int _a)
            {
                _a += 1;
            }
        }

        class OutTest
        {
            public void ot(out int _a)
            {
                //Console.WriteLine(_a);
                _a = 0;
                _a += 2;

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
