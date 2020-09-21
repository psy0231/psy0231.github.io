---
title: Parameters
date: 2020-03-10 20:00:00 +0900
categories: [Grind, C#]
tags: [c#]
seo:
  date_modified: 2020-04-29 17:30:19 +0900
---

## 뭔데
- 이 전에 ref / out 만 정리 해놨다가 세계관을 확장.
- params, ref, out, in을 정리할 예정.


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

## params
> params specifies that this parameter may take a variable number of arguments.

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
> in specifies that this parameter is passed by reference but is only read by the called method.

- 참조전달, 읽기전용 이라 한다.
- ex
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
- 참조로 전달.
- 읽기전용.
- 정식 매개변수를 위해 해당 인수의 별칭을 만드는데 반드시 변수여야함. 
즉, 매개 변수의 모튼 작업이 인수에서 수행됨.  
이거 때문에 별다르게 할 수 있는 작업이 없는것같음.
- 생각해보니 자주 보인게 foreach같음.  
위에서 mtd_2의 경우를 보면 for문은 list.Add()를 할 수 있지만  
foreach는 List에 대한 작업을 하지 못함. (주석부분)  
그냥 내생각엔.. foreach는 collection을 in으로 받으니까 수정은 못함.  
그런데 그 item들은 collection을 참조로 받아왔으니 그 값을 직접적으로 바꿀 수 있게된것같음.  
이라고 생각하고 mtd_3을 해봤는데 list수정이 된다.  
잘 모르겠다.
- 또 LINQ 쿼리에서 join 절의 일부로 쓰인다 하니 이건 나중에...
- 아래 참고에 보면 더 길고 복잡한 설명들 많음.

## ref
> ref specifies that this parameter is passed by reference and may be read or written by the called method.

- 참조로 전달, 읽기 쓰기 가능.

- 4가지 정도 용도가 있다함.
    - 참조로 인수 전달
    - 참조 반환 값
    - ref 로컬
    - ref 구조체

### 참조로 인수 전달
- 값이 아닌 참조로 전달됨을 나타냄.  

    ref 키워드는 정식 매개 변수를 변수여야 하는 인수의 별칭으로 설정합니다.  
    즉, 매개 변수에 대한 모든 작업이 인수에서 수행됩니다.  

    위설명이 in에도 똑같이 있었는데 parameter로 in, ref, out등을 할 때 변수를 넣어줘야 한다는 말인것같음.
    예를들어 int parameter에 '10'이런식의 상수를 넣으면 안들어감  
    ref (변수)로 넣야함.

    근데 in은 두개 상관없이 또 들어감...
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
- 일단 다른점부터 하면 mtd_1과 mtd_2 모두 parameter로 받아 처리하고 return은 없지만 결과가 다르다.  
ref로 넘어온 경우 원본의 값을 직접 바꿨고 일반적인경우 지역변수로써 역할을하고 끝남.
- ref는 메서드 정의와 호출 시 모두 명시적으로 사용해야함.
- ref, in은 전달 전에 초기화 해야함. ( != out )
- class 맴버가 ref, in, out만 있을 수는 없다.  
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
    - 인수가 구조체가 아니거나 구조체로 제한되지 않는 제네릭 형식일 경우에는  
    확장 메서드의 첫 번째 인수에 ref 키워드를 사용할 수 없음.
    - 첫 번째 인수가 구조체인 경우 이외에는 in 키워드를 사용할 수 없음.  
    구조체로 제한되는 경우에도 in 키워드는 제네릭 형식에 사용할 수 없음.
- 참조 형식을 참조로 전달 가능.  
참조 형식을 참조로 전달하는 경우 호출된 메서드는 참조 매개 변수가 호출자에서 참조하는 개체를 바꿀 수 있다.  
개체의 스토리지 위치는 참조 매개 변수의 값으로 메서드에 전달됩니다. 
매개 변수의 스토리지 위치에서 값을 변경하여 새 개체를 가리키도록 하면 호출자가 참조하는 스토리지 위치도 변경됩니다.  
다음 예제에서는 참조 형식 인스턴스를 ref 매개 변수로 전달합니다.  
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
    
### 참조 반환 값
- method가 caller에게 참조로 반환함.  
caller는 반환된 값을 수정 할 수 있으며  
해당 변경 내용은 호출 method의 개체 상태에 반영됨.

### ref 로컬
### ref 구조체

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

## 참고
- [TryParse](https://referencesource.microsoft.com/#mscorlib/system/int32.cs,325507e509229dbc)  
- [TryDequeue](https://referencesource.microsoft.com/#mscorlib/system/Collections/Concurrent/ConcurrentQueue.cs,0e91b925b71182e1)
- [메서드 매개 변수(C# 참조)](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/method-parameters)
- [in 매개 변수 한정자(C# 참조)](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/in-parameter-modifier)

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
- ref는 메서으 
    - value전달시 return을 다시 원본에 넣는과정을 ref는 알아서 해주는거 아닐까? 하고 넘어가면 똑같아 보임 둘은.
- 이런 이유로 생겨나지 않았을까 싶음...아님말고