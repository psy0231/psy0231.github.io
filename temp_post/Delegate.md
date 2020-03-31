## Delegate
- 이전에 Action, Func에서 봤던 형식으로 이 둘이 좀더 표준화 느낌이라 했는데 그럼 delegate에 대해 좀더 정리해보자.
- 메서드에 대한 참조를 나타내는 타입. 
- delegate는 뜻이 대리자임. 하는짓 그 자체. method를 가리키거나 method자체를 parameter로 넘길때 쓴다함.
- 쨌든 method를 직접 call하는 대신 이 delegate로 대신해 call 할 수 있다.
- delegate 선언때 return, parameter을 지정하는데 실제 method가 이 delegate형식과 맞으면 붙여다 쓸 수 있고, 하나 이상 붙여 쓸 수 있다. 
- ex) 일단 써보자.
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
    - 주석 처리된 부분은 원래 저렇게 써야되는것 같은데 편의상(?) 아래처럼 쓰는듯하다.

- ex) delegate return method(parameter)
    ```c#
    class Program
    {
        static void Main(string[] args)
        {
            rtnprm rp = new rtnprm();
            rp.cal = rp.add;
            rp.cal(2, 1);
            rp.cal = rp.sub;
            rp.cal(2, 1);
        }
    }

    class rtnprm
    {
        public delegate void delcal(int a, int b);
        public delcal cal;

        public void add(int _a, int _b)
        {
            Console.WriteLine(_a + _b);
        }

        public void sub(int _a, int _b)
        {
            Console.WriteLine(_a - _b);
        }
    }
    ``` 
    - 결과 
    ```
    3
    1
    ```
    - 여기 delegate는 return은 void, parameter은 int, int 임. 아래 add, sub도 같은 형식의 method이다. 따라서 delegate는 이 둘 method를 참조 할 수 있다.
    - delegate와 형식만 같다면 위처럼 add를 하건 sub를 하건 그때그때 붙여 쓸 수 있다.
- ex) delegate chain
- callback / event 이건 나중에

## delegate 쓰면서 
- 먼저 class A,B가 있다하자. A는 연산, B는 출력을함
- delegate를 안쓴다면 
    ```c#
    static void Main(string[] args)
    {
        //1
        A_1 a_1 = new A_1();
        B_1 b_1 = new B_1();
        int res = a_1.mtd_cal_1(2);
        //b_1.mtd_prt(a_1.mtd_cal_1(2));
        b_1.mtd_prt(res);
        //2
        A_1 a_2 = new A_1();
        a_2.mtd_cal_2(3);
        //3
        B_1 b_3 = new B_1();
        A_1 a_3 = new A_1(b_3);
        a_3.mtd_cal_3(4);

    }

    class A_1
    {
        private B_1 b_3;

        public A_1()
        {

        }

        public A_1(B_1 b_3)
        {
            this.b_3 = b_3;
        }

        public int mtd_cal_1(int _a)
        {
            return _a*_a;
        }

        public void mtd_cal_2(int _a)
        {
            B_1 b= new B_1();
            b.mtd_prt(_a*_a);
        }

        public void mtd_cal_3(int _a)
        {
            b_3.mtd_prt(_a * _a);
        }
    }

    class B_1
    {
        public B_1()
        {

        }

        public void mtd_prt(int _a)
        {
            Console.WriteLine(_a);
        }
    ```
    - 위처럼 세가지정도?
    - 이건 그냥 내 버릇임.  
    보통 위처럼 a,b가 구별 가능한 기능 또는 덩어리라면 같이 묶어두는걸 피하려함(a,b). 또 포함하지 않으려함(3).
    - 따라서 3번이 가장 싫어하는 구조, 2번이 꺼려짐 이건그냥 내 습관이니까. -> 최대한 조각내는걸 목표로하고 최대한 서로 관계없게.. 종속적이지 않게 하려고함...내가 할 수 있는 최대한.
- delegate를 쓴다면 
    ```c#
    class Program
    {
        static void Main(string[] args)
        {
            //use delegate
            A a = new A();
            B b = new B();
            a.temp = b.mtd;
            a.mtd(5);

        }
    }

    class A
    {
        public delegate void del_temp(int _a);
        public del_temp temp;

        public A()
        {

        }

        public void mtd(int _a)
        {
            temp(_a * _a);
        }
    }

    class B
    {
        public B()
        {

        }
        public void mtd(int _a)
        {
            Console.WriteLine(_a);
        }
    }
    ```
    - delegate를 안썼을 때가 이것저것 다 모아쓰다보니 좀 길어졌지 이거 1번 예시와 비슷해 보인다.
    - 다른건 A에서 계산결과(res)를 받아서 쓸 필요가 없어 보인다. 
- 하나 더. A, B가 있고...

## ㅁㄴㅇㄹ
