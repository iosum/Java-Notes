<!-- <https://blog.csdn.net/apei830/article/details/4525284> -->
<!-- <https://blog.csdn.net/apei830/article/details/4525284> -->



# Wrapper Classes
- Java語言是一個物件導向的語言，但是 Java 中的 primitive data type 卻是不是物件導向的，這在實際使用時會很不方便，為了解決這個問題，在設計 classes 時為每個 primitive data type 設計了一個相對應的 classes 進行代表，這樣八個 primitive data type 對應的classes 統稱 Wrapper Classes

- Wrapper Classes 都是在 java.lang package，Wrapper Classes 和 primitive data type 的關係如下：

primitive data type | Wrapper Classes
-------------------|-----------------
byte |Byte
short |Short
char|Character
int|Integer
long|Long
float|Float
double|Double
boolean|Boolean

- 在這八個 classes 中，除了 Integer 和 Character claesses 以外，其它六個 classes
的 classes name 和 primitive data typea
一致，只是 classes name
的第一個字母大寫。

- Wrapper Classes 使用主要包含兩種：

1. 作為和primitive data types對應的ClassesClasses存在，方便涉及到objects的操作。

2. 包含每種primitive data types的相關屬性如最大值、最小值等，以及相關的操作方法。

JDK自從1.5（5.0）版本以後，就加入了autoboxing unboxing，也就是在進行primitive data types和對應的 Wrapper Classes 轉換時，系統將自動進行

```java
    // int 會轉成 Integer class
    int m = 12；
    Integer in = m；
    //Integer 會自動轉成 int
    int n = in；
```


實際使用時的 Classes 轉換將變得很簡單，系統將自動實現對應的轉換。


- 本文則主要介紹 Wrapper Classes 與其他 Classes 的區別：

- `==` 和 `!=` 在比較2個數值型 data 時，是比較它們 **值** 的大小，而在比較2個 reference objects 時，則比較的是2個 reference objects 是不是指向同一個 memory location，也就是比較的是2者在 memory 中所在的 reference


`> >= < <=` 是可以用來比較 Wrapper Classes objects 的（BooleanClasses objexts 除外)

它比較的**則不是** Wrapper Classes objects所指向 memory location，而是直接比較 Wrapper Classes 的 **值** 大小

```java
public class Test {
 public static void main(String[] args) {
  Integer a = new Integer(200);
  Integer b = new Integer(200);
  System.out.println("a == b:" + (a == b));  //false // location
  System.out.println("a != b:" + (a != b));  //true // location
  System.out.println("a <= b:" + (a &lt;= b));  //true //element
  System.out.println("a >= b:" + (a >= b));  //true //element
  System.out.println("a < b:" + (a &lt; b));  //false //element
  System.out.println("a > b:" + (a > b));  //false //element
 }
}
```

由結果不難看出 Wrapper Classes objects 在進行比較時，`==` 和 `!=` 不是比較值大小而是比較memory location，而 `> >= < <=` 則是直接比較 **值** 大小。

但是普通 Classes objects 是不允許使用 `> >= < <=` 來比較大小的，下面以 StringClasses 為例：

```java
public class Test {
 public static void main(String\[] args) {
  String a = new String("abc");
  String b = new String("abc");
  System.out.println("a == b:" + (a == b));  //false
  System.out.println("a != b:" + (a != b));  //true
  System.out.println("a <= b:" + (a <= b));  //bad operand types for binary operator '<='
  System.out.println("a >= b:" + (a >= b));  //bad operand types for binary operator '>='
  System.out.println("a < b:" + (a < b));  //bad operand types for binary operator '<'
 }
}
```

```
Exception in thread "main" java.lang.Error: Unresolved compilation problems :
 The operator &lt;= is undefined for the argument type(s) java.lang.String, java.lang.String
 The operator >= is undefined for the argument type(s) java.lang.String, java.lang.String
 The operator &lt; is undefined for the argument type(s) java.lang.String, java.lang.String
 The operator > is undefined for the argument type (s) java.lang.String, java.lang.String
```

另外，前面有提到 BooleanClasses objects 是不能使用 `> >= < <=` 比較的，原因很簡單，查看下Boolean 和其他 Wrapper Classes 的 code:


BooleanClasses code：

`public final class Boolean implements java.io.Serializable,Comparable<Boolean>`

IntegerClasses code：

`public final class Integer extends Number implements Comparable<Integer>`

不難發現BooleanClasses並沒有繼承 `Number` Classes，而其他 Wrapper Classes 是由 `Number` Classes繼承來的，所以BooleanClasses objects 是不能使用 `> >= < <=` 作比較的。
在這一點上BooleanClasses和其他普通Classes沒有任何差別。



# autoboxing and unboxing 

primitive data types 的 autoboxing、unboxing 是J2SE 5.0提供的新功能。

雖然為你打包 primitive data types 提供了方便，但提供方便的同時表示隱藏了細節，建議在能夠區分primitive data types與 objects 的差別時再使用。

1) autoboxing 和 unboxing

在Java中，所有要處理的東西幾乎都是objects。

然而 primitive data types 不是objects，也就是你使用 `int` , `double`, `boolean` 等定義的變量，以及你在 code 直接寫下的 literals

使用Java有一段時間的人都知道，有時需要將 primitive data types 轉換為 objects。

例如使用 Map 要使用 `put()` 時，需要傳入的參數是 objects 而不是 primitive data types。

要使用 Wrapper Types 才能將 primitive data types 包裝為 objects，前一個小節中你已經知道在J2SE 5.0之前，要使用以下才能將 int package 成一個 Integerobjects：

```java
Integer integer = new Integer(10);
```

在 J2SE 5.0之後提供了autoboxing 的功能，你可以直接使用以下 code 來 package primitive data types：

```java
Integer integer = 10;
```
不用再用 new keyword 寫, 在進行編譯時，編譯器再自動根據你寫下的 code，判斷是否進行 autoboxing。

在上例中integer variable reference 的會是 Integer class 的 object。

同樣的動作可以適用於 boolean、byte、short、char、long、float、double等 primitive data types，分別會使用 Wrapper Types 是 Boolean、Byte、Short、Character、Long、Float 或 Double。

autoboxing 運用的方法還可以如下：

```java
int i = 10; Integer integer = i;
``` 

也可以使用更一般化的 `java.lang.Number` classes 來 autoboxing。


例如：

```java
Number number = 3.14f;
```

3.14f 會先被 autoboxing 為 `Float` ，然後指定給 number。

J2SE 5.0中可以 autoboxing，也可以 unboxing，也就是將 objects 中的 data type 訊息從 object 中自動取出。例如下面這樣寫是可以的：

```java
Integer fooInteger = 10; // autoboxing
int fooPrimitive = fooInteger; // unboxing
```


fooInteger 在 autoboxing 為 Integer 的 object 後，如果被指定給一個int data type 的 fooPrimitive variable，則會自動變為 int data type 再指定給 fooPrimitive。在運算時，也可以進行 autoboxing 與 unboxing。


例如：

```java
Integer i = 10; //autobosing
System.out.println(i + 10); //20
System.out.println(i++);//10
```


上例中會顯示20與10，編譯器會自動進行 autoboxing 與unboxing，也就是10會先被 autoboxed
，然後在i + 10時會先 unboxing，進行加法運算；

i++該行也是先unboxing再進行運算。



再來看一個例子：

```java
Boolean boo = true;
System.out.println(boo && false);
```


同樣的 boo 原來是Boolean的 object，在進行AND運算時，會先將boo unboxing，再與 false 進行 AND運算，結果會顯示false。

1) 小心使用boxing

autoboxing 與 unboxing的功能事實上是編譯器來幫你的忙，編譯器在編譯時期依你所編寫的語法，決定是否進行autoboxing 或 unboxing

例如：

```java
Integer i = 100;
```

相當於編譯器自動為你作以下的語法編譯：

```java
Integer i = new Integer(100);
```


所以 autoboxing 與 unboxing 的功能是所謂的 Compiler Sugar

雖然使用這個功能很方便，但在程序運行階段你得了解Java的語義。

例如下面的程序是可以通過編譯的：

```java
Integer i = null;
int j = i;
```

這樣的語法在 compile 是合法的，但是在 run 會有錯誤，因為這種寫法相當於：

```java
Integer i = null;
int j = i.intValue(); //null
```

`Integer` is an object. Therefore it is nullable.

```java
Integer i = null;
```

is correct.

int, on the other hand, is a primitive value, therefore **not** nullable.

```java
int j = i;
```

is equivalent to

```java
int j = null;
```

which is incorrect, and throws a `NullPointerException`.


再來看“AutoBoxDemo1.java ”，你認為結果是什麼呢？

AutoBoxDemo1.java：

```java
public class AutoBoxDemo1 {
    public static void main(String[] args) {
        Integer i1 = 127;
        Integer i2 = 127;
        if (i1 == i2) 
            System.out.println("i1 == i2");
        else 
            System.out.println("i1 != i2");
    }
}
```
```
i1 == i2
```

從 autoboxing 與 unboxing 來看，可能會覺得結果是顯示 i1 == i2，你是對的。

那麼“AutoBoxDemo2.java ”的這個程序，你覺得結果是什麼？

AutoBoxDemo2.java：

```java
public class AutoBoxDemo2 {
    public static void main(String\ args) {
        Integer i1 = 128;
        Integer i2 = 128;
        if (i1 == i2) 
            System.out.println("i1 == i2");
        else 
            System.out.println("i1 != i2");
    }
}
```

結果是顯示i1 != i2，這有些令人驚訝，兩個範例語法完全一樣，只不過改個數字而已，結果卻相反。

其實這與 `==` 的比較有關，`==` 是用來比較兩個 primitive data types 的變量值是否相等，事實上 `==` 也用於判斷兩個是否參考至同一個objects。(比較 值 和 location)


在 autoboxing 時對於數字從 –128 到 127 之間的值，它們被裝箱為Integer object後，會存在 memory 被重用，所使用 `==` 進行比較時，i1 與i2實際上參考至同一個objects。



如果超過了從 –128 到 127 之間的值，被裝箱後的 Integer objects 並不會被重用，即相當於每次裝箱時都新建一個Integer objects，所以“AutoBoxDemo2.java”使用 `==` 進行比較時，i1與i2 是不同的 objects。


最好還是依正規的方式來寫，而不是Compiler Sugar。


例如“AutoBoxDemo2.java”必須改寫為“AutoBoxDemo3.java ”才是正確的。

AutoBoxDemo3.java：

```java
public class AutoBoxDemo3 {
    public static void main(String\[] args) {
        Integer i1 = 128;
        Integer i2 = 128;
        if (i1.equals(i2)) 
            System.out.println("i1 == i2");
        else 
            System.out.println("i1 != i2");
    }
}
```


結果這次是顯示i1 == i2。
