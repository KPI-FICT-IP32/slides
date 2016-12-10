## Monitoring and optimization
### of Java applications

_@by_mamagaga_, _@anxolerd_

Dec 14 2016

-----

![ppap](img/ppap.jpg)

-----

![DEADLOCK](img/deadlock.gif)

DEADLOCK - взаємне блокуваня, яке приводить до зависання програми.

>>>>>

## Example in SQL
```SQL
--Transaction #1
BEGIN;
/* GET S LOCK */
SELECT * FROM `testlock` WHERE id=1 LOCK IN SHARE MODE;
SELECT SLEEP(5);
/* TRY TO GET X LOCK */
SELECT * FROM `testlock` WHERE id=1 FOR UPDATE;
COMMIT;

--Transaction #2
BEGIN;
/* TRY TO GET X LOCK - DEADLOCK AND ROLLBACK HERE */
SELECT * FROM `testlock` WHERE id=1 FOR UPDATE;
COMMIT;
```
>>>>>
## Example in Java

```java
public class TestMain {
  public static void main(String[] args) {
    MyThreadOne t1 = new MyThreadOne();
    MyThreadTwo t2 = new MyThreadTwo();

    t1.setThread2(t2);
    t2.setThread1(t1);
    t1.start();
    t2.start();
  }
}
```
>>>>>

<div id="left">
<H6 align="center">THREAD NUMBER 1</H6>
<pre>
<code class="java">
public class MyThreadOne extends Thread {
  Thread t2;

  public void run() {
    try {
    sleep(1000);
    } catch (Exception e) {  }
    try {
      t2.join(); // Wait for thread #2
    } catch (Exception e) {
        // handle
    }
  }

  public void setThread2(Thread t) {
  this.t2 = t;
  }
}
</code></pre>

</div>
<div id="right">
<H6 align="center">THREAD NUMBER 2</H6>
<pre>
<code class="java">
public class MyThreadTwo extends Thread {
  Thread t1;

  public void run() {
    try {
    t1.join(); // Wait for thread #1
    } catch (Exception e) { }
    try {
      sleep(1000);
    } catch (Exception e) {
      // handle
    }
  }

  public void setThread1(Thread t) {
  this.t1 = t;
  }
}
</code></pre>

</div>
-----

## Memory leak

![Memory leak](img/memoryleak.png)
>>>>>

Memory leak - процес не контрольованого зменшення об'єму вільної оперативної або віртуальної пам'яті комп'ютера, пов'язаний з помилками в працьюючих програмах, які вчасно не звільюють вже не потрібні ділянки пам'яті.

>>>>>

## Example

```java
1. Object obj;
2. obj = new AnotherObject();
3. obj = null;
4. obj = new AnotherObject();
```
