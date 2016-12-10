## Monitoring and optimization
### of Java applications

_@by_mamagaga_, _@anxolerd_

Dec 14 2016

-----
## DEADLOCK
### Що це та як запобігти цьому
>>>>>

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
```java
//THREAD NUMBER 1
public class MyThreadOne extends Thread {
  Thread t2;

  public MyThreadOne() {
    System.out.println(«MyThreadOne create»);
  }

  public void run() {
    System.out.println(«MyThreadOne start»);
    try {
    sleep(1000);
    } catch (Exception e) {  }
    try {
    System.out.println(«MyThreadOne waiting MyThreadTwo finish…»);
    t2.join();

    } catch (Exception e) {e.printStackTrace()}
    System.out.println(«MyThreadOne finished»);
  }

  public void setThread2(Thread t) {
  this.t2 = t;
  }
}
```
>>>>>

>>>>>
----
