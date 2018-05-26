#类型转换

###一、String 转 int、long、short

在Java中要将String类型转化为int类型时,需要使用Integer类中的≤parseInt()方法或者valueOf()方法进行转换。在转换过程中需要注意,因为字符串中可能会出现非数字的情况,所以在转换的时候需要捕捉处理异常

* 例1:

```
String str = "123";
try {
    int a = Integer.parseInt(str);
} catch (NumberFormatException e) {
    e.printStackTrace();
}
```

* 例2:

```
String str = "123";
try {
    int b = Integer.valueOf(str).intValue()
} catch (NumberFormatException e) {
    e.printStackTrace();
}
```

* PS：字串转成 Double, Float, Long 的方法大同小异.

  * 第一种方法：

    ```
    i=Integer.parseInt([String]);
    ```

  * 第二种方法：

    ```
    i=Integer.valueOf(my_str).intValue();
    ```


  相当于 *new Integer(Integer.parseInt(my_str))*，也会抛出异常。

### 二、将整数 int 转换成字串 String

有3种方法:

```
1. String s = String.valueOf(i);
2. String s = Integer.toString(i);
3. String s = "" + i;
```

* PS：Double, Float, Long 转成字串的方法大同小异.

  ```
  s=String.valueOf(i);   //直接使用String类的静态方法，只产生一个对象。
  s= "" + i;  	//会产生两个String对象。
  ```

#### string 数组类型转换为 int 数组

* 方法一：ConvertAll的用法

```
public static int StrToInt(string str){
		return int.Parse(str);
}

string[] arrs = new string[] { "100", "300", "200" };

int[] arri = Array.ConvertAll(arrs, new Converter<string, int>(StrToInt));
```

* 方法二：使用数组循环分别转换。

```
string[] str1 = new string[] {"100","300","200"}; 
int[] intTemp = new int[str1.Length];
for (int i = 0; i <str1.Length; i++){
		int.TryParse(str1[i]，out intTemp[i]);
		//int.TryParse函数返回Bool型。不会抛出异常
}
```

* 方法三：

```
string[] str1 = new string[] {"100","300","200"};
int[] intTemp = new int[str1.Length];
for (int i = 0; i <str1.Length; i++){
		intTemp[i] = int.Parse(str1[i]);
}
```

####	String 转 byte 数组：

```
String str = "abcd";
byte[] bs = str.getBytes();
```

#### byte 数组转 String：

```
String str = "abcd";
byte[] bs = str.getBytes();
String s = new String(bs);
```

#### String 数组转 byte 数组

```
String[] str={1,2,3,4,5,6,7};
//或者String[] str = new String[]{"aaaa","bbbb","cccc"};
StringBuilder b=new StringBuilder();
for(String s:str){
		b.append(s);
}
byte b[] =b.toString.getBytes();
```

### 三、进制转换

####1）16进制字符串转化成十六进制数字

先字符串转化整型面需要再整型转化16进制数字

```
int parseInt = Integer.parseInt("cc", 16);
System.out.println(parseInt);

String hexString = Integer.toHexString(parseInt);
System.out.println(hexString);
```

####2）8进制、10进制、16进制转为2进制

八进制转十进制

```
System.out.println("Integer.toBinaryString(01)="+Integer.toBinaryString(01));
```

十进制转无符号整数八进制

```
System.out.println("Integer.toOctalString(18)="+Integer.toOctalString(18));
```

八进制转十六进制

```
System.out.println("Integer.toHexString(012)="+Integer.toHexString(012));
```

八进制转二进制

```
System.out.println("Integer.toBinaryString(012)="+Integer.toBinaryString(012));
```


十进制转二进制

```
System.out.println("Integer.toBinaryString(10)="+Integer.toBinaryString(10));
```

十六进制转二进制

```
System.out.println("Integer.toBinaryString(0xa)="+Integer.toBinaryString(0xa));
```

十六进制转十进制

```
System.out.println("Integer.toOctalString(0x12)="+Integer.toOctalString(0x12));
```

十进制转十六进制
```
System.out.println("Integer.toHexString(10)="+Integer.toHexString(10));
```

测试结果

```
Integer.toBinaryString(012)=1010	// 八进制转二进制

Integer.toBinaryString(10)=1010		// 十进制转二进制

Integer.toBinaryString(0xa)=1010	// 十六进制转二进制

Integer.toOctalString(18)=22	// 十进制转无符号整数八进制

Integer.toOctalString(0x12)=22	// 十六进制转无符号整数八进制

Integer.toBinaryString(01)=1	// 八进制转十进制

Integer.toHexString(012)=a	// 八进制转十六进制

Integer.toHexString(10)=a	// 十进制转十六进制

String.valueOf("")
```


###三、Byte[]和String互转

####1.string 转 byte[]


```java
byte[] midbytes = isoString.getBytes("UTF8"); //为UTF8编码
byte[] isoret = srt2.getBytes("ISO-8859-1");
//为ISO-8859-1编码，其中ISO-8859-1为单字节的编码
```
####2.byte[]转string

```Java
String isoString = new String(bytes,"ISO-8859-1");
String srt2=new String(midbytes,"UTF-8");
```

* 说明：在网络传输或其它应用中常常有同一的中间件，假设为String类型。因此需要把其它类型的数据转换为中间件的类型。将字符串进行网络传输时，如socket，需要将其在转换为byte[]类型。这中间如果采用用不同的编码可能会出现未成预料的问题，如乱码。


* 下面举个例子：我们用socket传输String类型的数据时，常常用UTF-8进行编码，这样比较可以避免一个“中文乱码”的问题。

  发送端：

```
String sendString="发送数据";
byte[] sendBytes= sendString .getBytes("UTF8");
```

socket发送接受端：

	String recString = new String( sendBytes ,"UTF-8");

但是，这里往往又会出现这样一个问题。就是想要发送的数据本身就是byte[]类型的。

如果将其通过UTF-8编码转换为中间件String类型就会出现问题，如：

	byte[] bytes = new byte[] { 50, 0, -1, 28, -24 };
	String sendString = new String(bytes ,"UTF-8");
	byte[] sendBytes = sendString .getBytes("UTF8");
然后再发送
接受时进行逆向转换

```
String recString = new String(sendBytes,"UTF-8");
byte[] Mybytes = isoString.getBytes("UTF8");
```

这时Mybytes中的数据将是
	[50, 0, -17, -65, -67, 28, -17, -65,-67]
因此，需要采用单字节的编码方式进行转换

```
String sendString=new String(  bytes ,"UTF-8"); 
```

改为

```
String sendString = new String(bytes, "ISO-8859-1");
byte[] Mybytes = isoString.getBytes("UTF8");
```
改为

```
byte[] Mybytes = isoString.getBytes("ISO-8859-1");
```

这样所需要的字节就有恢复了。



###四、string数组转成string

	//第一种：
	String [] arr = {"41","a","5","g56"};
	String s1 = Arrays.toString(arr);
	System.err.println(s1);//[41, a, 5, g56]
	
	//第二种：
	String s2 = StringUtils.join(arr);
	System.err.println(s2);//41a5g56
	
	//第三种：
	String s3 = StringUtils.join(arr,",");
	System.err.println(s3);//41,a,5,g56
	
	//第四种：
	StringBuffer s4 = new StringBuffer();
	for (String string : arr) {
	    s4.append(string);
	}
	System.err.println(s4.toString());//41a5g56
