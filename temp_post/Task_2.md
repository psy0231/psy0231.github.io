---
title: Task_2
date: 2020-03-12 15:00:00 +0900
categories: [Grind, C#]
tags: [C#]
seo:
  date_modified: 2020-03-12 15:36:49 +0900
---

# Build Up 
> Action - ***Task*** - async/await

## Task_1 쓰다가 궁금했던것
- wait()??
    > Waits for the Task to complete execution.

    - _1 쓰다가 궁금했고 다시보니 대충 알것같음.
    - 기다려 주는건 Main thread이거나 이걸 불렀던 parent thread 일것같음 
    - 직접보자. 또 보다가 새로 찾은것도 있고.....
    - 근데
- 사실은
    ```c#
     class Program
    {
        static void Main(string[] args)
        {
            test_1 t = new test_1();
            
            Action a1 = t.mtd_1;
            Task t1 = new Task(a1);

            Action a2 = t.mtd_2;
            Task t2 = new Task(a2);

            Action a3 = t.mtd_3;
            Task t3 = new Task(a3);

            Console.WriteLine("1") ;
            t1.Start();
            t1.Wait();
            Console.WriteLine("2");
            t2.Start();
            t2.Wait();
            Console.WriteLine("3");
            t3.Start();
            t3.Wait();
            Console.WriteLine("6");
        }
    }

    class test_1
    {
        public test_1()
        {

        }

        public void mtd_1()
        {
            string name = MethodBase.GetCurrentMethod().Name;
            Console.WriteLine(DateTime.Now.ToString("HH:mm:ss.fff") + " "+name + " start ");
            Thread.Sleep(3000);
            Console.WriteLine(DateTime.Now.ToString("HH:mm:ss.fff") + " " + name + " end ");
        }

        public void mtd_2()
        {
            string name = MethodBase.GetCurrentMethod().Name;
            Console.WriteLine(DateTime.Now.ToString("HH:mm:ss.fff") + " " + name + " start ");
            Thread.Sleep(2000);
            Console.WriteLine(DateTime.Now.ToString("HH:mm:ss.fff") + " " + name + " end ");

        }

        public void mtd_3()
        {
            string name = MethodBase.GetCurrentMethod().Name;
            Console.WriteLine(DateTime.Now.ToString("HH:mm:ss.fff") + " " + name + " start ");
            Thread.Sleep(4000);
            Console.WriteLine(DateTime.Now.ToString("HH:mm:ss.fff") + " " + name + " end ");
        }
    }
    ```
    - 결과는 이렇게 나옴.  
    1  
    16:45:41.125 mtd_1 start  
    16:45:44.127 mtd_1 end  
    2  
    16:45:44.127 mtd_2 start  
    16:45:46.128 mtd_2 end  
    3  
    16:45:46.129 mtd_3 start  
    16:45:50.131 mtd_3 end  
    6
    - 그러니까 async/blocking 이라하면 맞나..
    - task.start는 async로 분기되는건 맞는데 실제 method가 끝날때까지는 기다린다-block된다.
    - 원래 생각엔  
    1  
    16:45:41.125 mtd_1 start  
    2  
    16:45:44.127 mtd_2 start  
    3  
    16:45:46.129 mtd_3 start  
    6  
    16:45:44.127 mtd_1 end  
    16:45:46.128 mtd_2 end  
    16:45:50.131 mtd_3 end  
    이럴줄알았는데 **!!!!!!!!!!!!!아님.!!!!!!!!!!!!!!!**

    



    





## 참고
- 더 좋은 설명들  
[Task Class](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task?view=netframework-4.8)  
[Task](https://referencesource.microsoft.com/#mscorlib/system/threading/Tasks/Task.cs,045a746eb48cbaa9)  
[Task 클래스](http://www.csharpstudy.com/Threads/task.aspx)