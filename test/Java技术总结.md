1. Java8中如果我们想获取当天或者某天的最小值，即那天的零点，我们可以用如下方法获取：
```
LocalDateTime localDateTime = LocalDateTime.ofInstant(Instant.ofEpochMilli(date.getTime()), ZoneId.systemDefault());
LocalDateTime startOfDay = localDateTime.with(LocalTime.MIN);
Date date = Date.from(startOfDay.atZone(ZoneId.systemDefault()).toInstant());
```
2. 熔断降级的作用？
分布式系统会有大量请求从网关gateway进入调用其他系统提供的服务，此时不可避免会有请求失败（超时、错误），那么这个时候就需要快速失败响应客户端，否则会造成请求堆积，造成系统卡顿，故需要服务的熔断降级不让服务堆积在网关处。
3. springcloud项目为什么需要配置动态路由？
因为我们的系统在运行的时候，如果这个时候启动了一个新的服务，那么动态路由可以在不重启系统的情况下将新的服务注册到系统中去。
4. 枚举可以实现接口。
5. 
6. 如果想打印mybatis的sql语句执行，可以使用如下配置：
	 ```java
	logging:  
	  level:  
	   mapper接口的路径: debug
	```
7. 枚举是否可以继承枚举？
	不可以。 
8. Java中while循环中若抛出异常，那么while循环将终止。
9. 同一个事务中，在事务还未提交的情况下，可以读取到该事务中已修改的记录。
10. 1byte = 8bit。
11. “%02x”解释：
	```
	“x”表示以16进制输出，“02”表示若输出结果不足两位，则最左补零凑足两位，若输出结果大于等于两位则直接输出结果。
	```
12. “%2x”解释：
	```
	“x”表示以16进制输出，“2”表示若输出结果不足两位，则实际输出，若输出结果大于等于两位还是实际输出。
	``` 

13. “++i”的效率要高于“i++”？（未验证）

  ```
  注：
  i++：取出i，复制i，增加i，返回副本；
  ++i：取出i，增加i，返回i；
  ```

## Java基础

### 包装类

   1. Java中包装类型自动拆装箱：
       ```java
      Integer x = 3; // 装箱 调用了 Integer.valueOf(2)
       int y = x; // 拆箱 调用了 X.intValue()
       ```
   2. 在 Java 8 中，以下几种基本类型的缓冲池范围：
       ```java
         boolean values true and false
         all byte values
         short values between -128 and 127
         int values between -128 and 127
         char in the range \u0000 to \u007F
       ```
   3. Java 基本类型的包装类的大部分都实现了常量池技术。，Byte/Short/Integer/Long的缓存池范围[-128,127]；Character缓存范围[0,127];Boolean直接返回True or False.
   4. 两种浮点数类型的包装类 Float,Double 并没有实现常量池技术。

### 内部类

1. 静态内部类
    1. 这是一种对仅在一个地方使用的类进行逻辑分组的方法：如果一个类仅对另一个类有用，那么将其嵌入该类并将两者保持在一起是合乎逻辑的。嵌套此类“帮助程序类”可使它们的程序包更加简化。
    2. 它增加了封装：考虑两个顶级类A和B，其中B需要访问A的成员，否则将对其进行声明private。通过将类B隐藏在类A中，可以将A的成员声明为私有，而B可以访问它们。另外，B本身可以对外界隐藏。
    3. 这可能会导致代码更具可读性和可维护性：在顶级类中嵌套小类会使代码更靠近使用位置。
    4. 程序设计的时候有时可能会遇到外部类单继承很难解决的一些问题，此时使用内部类继承其他类或许可以简单的解决问题。

### 集合

#### ImmutableMap

   1. 获取ImmutableMap的方式：
        ```java
        // map中的元素之多5个
        Map<String,Object> map1 = ImmutableMap.of("a", 1, "b", 2, "c", 3, "d", 4, "e", 5);
        // map中的元素没有个数限制
        Map<String, Object> map2 = ImmutableMap.<String,Object>builder().put("a", 1).put("b", 2).put("c", 3).put("d", 4).put("e", 5).put("f", 6).put("g", 7).build();
        ```

#### List

 1. 代码：

 ```java
   		// 如下代码到return处会直接结束运行，因为return是结束方法执行的含义
		 List<Integer> list0 = ImmutableList.of(4, 5, 6);
		 Iterator<Integer> iterable = list0.iterator();
        while(iterable.hasNext()) {
            System.out.println("------i:" + iterable.next());
            List<Integer> list = ImmutableList.of(1, 2, 3);
            for (Integer integer : list) {
                System.out.println("---integer:" + integer);
                if (Objects.equals(1 , integer)) {
			// if (Objects.equals(4 , integer)) {
					return;
				}
			}
		}
  System.out.println(1/0);
 ```

### String

 1. String 被声明为 final，因此它不可被继承。
 2. 在 Java 8 中，String 内部使用 char 数组存储数据；在 Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 `coder` 来标识使用了哪种编码。
 3. String对象是不可变的，不可变的好处：
  ①  可以缓存 hash 值：因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。
  ② String Pool 的需要：如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。
  ③ 安全性：String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 的那一方以为现在连接的是其它主机，而实际情况却不一定是。
  ④ 线程安全：String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

### Objects

1. 使用isNull(Object object)的时候需要注意的是，如果参数是字符串，那么还需要额外判断字符串是否是空字符串。

### Bigdecimal

 1. bigdecimal如何比较大小，可以使用compareTo(Bigdecimal bigdecimal)，传入“BigDecimal.ZERO”返回0表示等于0，返回1表示大于0，返回-1表示小于0：

 ```java
 System.out.println("0-0="+new BigDecimal("0").compareTo(BigDecimal.ZERO));  
 System.out.println("0.0-0="+new BigDecimal("0.0").compareTo(BigDecimal.ZERO));  
 System.out.println("0.00-0="+new BigDecimal("0.00").compareTo(BigDecimal.ZERO));  
 System.out.println("0.000-0="+new BigDecimal("0.000").compareTo(BigDecimal.ZERO));  
 System.out.println("0.00001-0="+new BigDecimal("0.00001").compareTo(BigDecimal.ZERO));  
 System.out.println("0.0000001-0="+new BigDecimal("0.0000001").compareTo(BigDecimal.ZERO));  
 System.out.println("-0.00001-0="+new BigDecimal("-0.00001").compareTo(BigDecimal.ZERO));  
 System.out.println("-0.0000001-0="+new BigDecimal("-0.0000001").compareTo(BigDecimal.ZERO));
 返回：
 0-0=0
 0.0-0=0
 0.00-0=0
 0.000-0=0
 0.00001-0=1
 0.0000001-0=1
 -0.00001-0=-1
 -0.0000001-0=-1
 ```

 2. BigDecimal做除法操作：

 ```java
 BigDecimal b1 = new BigDecimal("1000");  
 BigDecimal b2 = new BigDecimal("100");  
 System.out.println(b1.divide(b2, 2, BigDecimal.ROUND_HALF_UP).toString());
 // 输出：
 // 10.00
 // 说明：调用divide（BigDecimal divisor, int scale, int roundingMode）方法，其中，scale表示小数点后保留几位小数，roudingMode表示取舍方式，常见的有四舍五入等，上例中是四舍五入，结果返回一个BigDecimal，可以调用toString()方法获取到String类型的结果。
 ```

 3. 当我们使用BigDecimal的时候，若创建BigDecimal时使用了其构造函数“BigDecimal(double val) ” 或者“BigDecimal(int val)”，则仍然会有精度丢失的问题，想要避免精度丢失的问题，可以使用构造方法“BigDecimal(String val) ”，或者使用静态方法“valueOf(double val)”。例：

 ```java
  BigDecimal a = new BigDecimal(14);
  BigDecimal b = new BigDecimal(0.1);
  System.out.println("a*b:"+a.multiply(b));
  BigDecimal c = BigDecimal.valueOf(14);
  BigDecimal d = BigDecimal.valueOf(0.1);
  System.out.println("c*d:"+c.multiply(d));

  // 输出：
  // a*b:1.4000000000000000777156117237609578296542167663574218750
  // c*d:1.4
 ```

4. 舍入模式说明：

```java
 // 1.ROUND_DOWN，向接近零的方向舍入。也就是将指定位数之后的全部舍掉，相当于截取操作；
```

5. 如何获取BigDecimal的整数和小数部分。

```java
BigDecimal bigDecimal1 = new BigDecimal("25.1235");
// 整数部分
BigDecimal integerPart = bigDecimal1.setScale(0, RoundingMode.DOWN);
// 小数部分
BigDecimal fractionalPart = bigDecimal1.subtract(integerPart);
System.out.println("integerPart:" + integerPart); // 25
System.out.println("fractionalPart:" + fractionalPart); // 0.1235
System.out.println(fractionalPart.doubleValue() > 0); // true
```
6. 去除显示多余的0。
```java
/** 问题：
    我们使用BigDecimal时，经常会保留小数后几位，此时如果位数不足会使用0补足，有时我们不想要显示多余的0，例如页面上面显示的时候，此时应该
如何处理？
*/

BigDecimal bigDecimal1 = new BigDecimal("12.00");
BigDecimal bigDecimal2 = new BigDecimal("6");
BigDecimal result = bigDecimal2.divide(bigDecimal1, 4, BigDecimal.ROUND_HALF_UP);
System.out.println(result); // 0.5000

/** 
 * 解决：
 * 我们可以使用BigDecimal的"stripTrailingZeros()"方法，该方法可以去除小数点后多余的0，但是使用"stripTrailingZeros()"方法之后，调用toString()方法
 * 可能会出现科学计数法的结果，此时我们只需要将“toString()”方法改为“toPlainString()”方法即可。
*/

BigDecimal bigDecimal1 = new BigDecimal("12.00");
BigDecimal bigDecimal2 = new BigDecimal("6");
BigDecimal result = bigDecimal2.divide(bigDecimal1, 4, BigDecimal.ROUND_HALF_UP);
System.out.println(result.stripTrailingZeros()); // 0.5

BigDecimal bigDecimal3 = new BigDecimal("12.00");
BigDecimal bigDecimal4 = new BigDecimal("1200.00");
BigDecimal result2 = bigDecimal4.divide(bigDecimal3, 4, BigDecimal.ROUND_HALF_UP);
System.out.println(result2); // 100.0000
System.out.println(result2.stripTrailingZeros()); // 1E+2
System.out.println(result2.stripTrailingZeros().toPlainString()); // 100
```

## JavaWeb

### 高并发

1. 唯一索引是解决并发的一种方式。
