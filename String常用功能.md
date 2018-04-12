#		String常用功能
#### 一、String类
创建的是字符串变量，属于对象，不能修改，包含在一对双引号之间
***

#### 二、String类对象的创建
声明：`String stringName;`
字符串创建：`stringName = new String(字符串常量);`
或`stringName = 字符串常量;`
***

####三、String类构造方法
##### 1、public String()

无参构造方法，用于创建空字符串的String对象

```
String str1 = new String();
```
#####2、public String(String value)
用已知的字符串value创建一个String对象

```
 String str2 = new String("abcd");
```
#####3、public String(char[] value)
用字符数组value创建一个String对象

```
char[] value = {"a","b","c"};
String str3 = new String(value);
相当于String str3 = new String("abcd")
```
#####4、public String(char chars[], int startIndex, int numChars)
用字符数组chars的startIndex开始的numChars个字符创建一个String对象

```
char[] value = {"a","b","c","d"};
String str4 = new String(value, 1, 2);
相当于String str4 = new String("bc");
```
#####5、public String(byte[] values)
用比特数组values创建一个String对象

```
byte[] string = new byte[]{65,66};
String str5 = new String(string);
相当于String string = new String("AB");
```
***
####四、String 类常用方法
#####1、求字符串长度

```
public int leghth()		//返回该字符串的长度
String str = new String("abcdefg");
int strlength = str.length();
```

#####2、求字符串某一位置字符
public char charAt(int index)	//返回字符串中指定位置的字符；注意字符串中第一个字符索引是0，最后一个是length()-1

```
String str = new String("abcdefg");
char ch = str.charAt(4);	//ch = z
```

#####3、提取子串
用String类的substring方法可以提取字符串中的子串，该方法有两种常用参数：
public String substring(int beginIndex)	//该方法从beginIndex位置起，从当前字符串中取当前字符串中取出endIndex-1位置的字符作为一个新的字符串返回

```
String str1 = new String("abcdefg");
String str2 = str1.substring(2);	//str2 = "cdefg";
String str3 = str1.substring(2,5);	//str3 = "cde";
```
#####4、字符串比较
public int compareTo(String anotherString)	//该方法是对字符串内容按字典顺序进行大小比较，通过返回的整数值指明当前字符串与参数字符串的大小关系。若当前对象比参数大则返回正整数，反之返回负整数，相等返回0

public int compareToIgnore(String anotherString)	//与compareTo方法相似，但忽略大小写。

public boolean equals(Object antherObject)	//比较当前字符串参数字符串，再两个字符串相等的时候返回true，否则返回false

public boolean equalsIgnoreCase(String anotherString)	//与equals方法相似，但忽略大小写

```
String str1 = new String("abc");
String str2 = new String("ABC");
int a = str1.compareTo(str2);	//a > 0
int b = str1.compareTo(str2);	//b = 0
boolean c = str1.equals(str2);	//c = false
boolean d = str1.equalsIgnoreCase(str2);	//d = true
```

#####5、字符串连接
public String concat(String string)	//将参数中的字符串string连接到当前字符串的后面，效果等价于"+"

```
String string = "aa".cancat("bb").cancat("cc");
相当于String string = "aa" + "bb" + "cc";
```

#####6、字符串中单个字符查找
public int indexOf(int ch/String string)	//用于查找当前字符串中字符或子串，返回字符或子串在当前字符春中从左边起首次出现的位置，若没有出现则返回-1

public int indexOf(int ch/String stirng, int fromIndex)	//该方法与第一种类似，区别在于该方法从fromIndex位置后查找

public int lastIndexOf(int ch/String string)	//该方法与第一种类似，区别在于该方法从字符串的末尾位置向前查找

public int lastIndexOf(int ch/String string, int fromIndex)	//该方法与第二种方法类似，区别在于该方法从fromIndex位置向前查找

```
String string = "I am a good student";
int a = string.indexOf('a');		//a = 2;
int b = string.indexOf("good");	//b = 7;
int c = string.index("w", 2);	//c = -1;
int d = string.lastIndexOf("a");	//d = 5;
int e = string.lastIndexOf("a", 3);	//e = 2;
```
#####7、字符串中大小写转换
public String toLowerCase()	//返回将当前字符串中所有字符串转换成小写后的新串
public String toUpperCase()	//返回将当前字符串中所有字符串转换成大写后的新串

```
String string = new String("ABCDabcd");
String str1 = string.toLowerCase();	str1 = "abcdabcd";
String str2 = string.toUpperCase();	str3 = "ABCDABCD";
```

#####7、字符串中字符的替换
public String replace(char oldChar, char newChar)	//用字符newChar替换当前字符串中所有的oldChar字符，并返回一个新的字符串

public String replaceFirst(String regex, String replacement)	//该方法用字符replacement的内容替换当前字符串中遇到的第一个和字符串regex相匹配的子串，应将新的字符串返回

public String replaceAll(String regex, String replacement)	//该方法用字符replacement的内容替换当前字符串中遇到的所有和字符串regex相匹配的子串，应将新的字符串返回

```
String string = "abcdefgabc";
String str1 = string.replace('a', 'g');		//str1 = "gbcdefggbc";
String str2 = string.replace("abc", "def");	//str2 = "defdefgdef";
String str3 = string.replacrFirst("abc", "bbc");	//str3 = "bbcdefgbbc";
String str4 = string.replacrAll("ab", "bb");		//str4 = "bbcdefgbbc";
```

#####8、字符串中字符的替换
public String replace(char oldChar, char newChar)//用字符newChar替换当前字符串中所有的oldChar字符，并返回一个新的字符串。

public String replaceFirst(String regex, String replacement)//该方法用字符replacement的内容替换当前字符串中遇到的第一个和字符串regex相匹配的子串，应将新的字符串返回。

public String replaceAll(String regex, String replacement)//该方法用字符replacement的内容替换当前字符串中遇到的所有和字符串regex相匹配的子串，应将新的字符串返回。

```
String str = "asdzxcasd";
String str1 = str.replace('a','g');	//str1 = "gsdzxcgsd"
String str2 = str.replace("asd","fgh");	//str2 = "fghzxcfgh"
String str3 = str.replaceFirst("asd","fgh");	//str3 = "fghzxcasd"
String str4 = str.replaceAll("asd","fgh");	//str4 = "fghzxcfgh"
```

#####9、其他类方法
String trim()	//截去字符串两端的空格，但对于中间的空格不处理。

String str = " a sd ";
String str1 = str.trim();
int a = str.length();	//a = 6
int b = str1.length();	//b = 4

boolean statWith(String prefix)或boolean endWith(String suffix)//用来比较当前字符串的起始字符或子字符串prefix和终止字符或子字符串suffix是否和当前字符串相同，重载方法中同时还可以指定比较的开始位置offset。

```
String str = "asdfgh";
boolean a = str.statWith("as");	//a = true
boolean b = str.endWith("gh");	//b = true
```

regionMatches(boolean b, int firstStart, String other, int otherStart, int length)//从当前字符串的firstStart位置开始比较，取长度为length的一个子字符串，other字符串从otherStart位置开始，指定另外一个长度为length的字符串，两字符串比较，当b为true时字符串不区分大小写。

contains(String str)//判断参数s是否被包含在字符串中，并返回一个布尔类型的值。

```
String str = "student";
str.contains("stu");	//true
str.contains("ok");		//false
```

String[] split(String str)//将str作为分隔符进行字符串分解，分解后的字字符串在字符串数组中返回。

```
String str = "asd!qwe|zxc#";
String[] str1 = str.split("!|#");
//str1[0] = "asd";str1[1] = "qwe";str1[2] = "zxc";
```
***
####五、字符串与基本类型的转换
#####1、字符串基本类型转换
java.lang包中有Byte、Short、Integer、Float、Double类的调用方法：
public static byte parseByte(String s)
public static short parseShort(String s)
public static short parseInt(String s)
public static long parseLong(String s)
public static float parseFloat(String s)
public static double parseDouble(String s)
例如：

```
String s1 = String.valueOf(12);
String s1 = String.valueOf(12.34);
```

#####3、进制转换
使用Long类中的方法得到整数之间的各种进制转换的方法：

```
Long.toBinaryString(long l)
Long.toOctalString(long l)
Long.toHexString(long l)
Long.toString(long l, int p)	  //p作为任意进制
```



1、length() 字符串的长度
　　例：char chars[]={'a','b'.'c'};
　　　　String s=new String(chars);
　　　　int len=s.length();

2、charAt() 截取一个字符
　　例：char ch;
　　　　ch="abc".charAt(1); 返回'b'

3、 getChars() 截取多个字符

```
void getChars(int sourceStart,int sourceEnd,char target[],int targetStart)
```

　　sourceStart指定了子串开始字符的下标，sourceEnd指定了子串结束后的下一个字符的下标。因此， 子串包含从sourceStart到sourceEnd-1的字符。接收字符的数组由target指定，target中开始复制子串的下标值是targetStart。

　　例：String s="this is a demo of the getChars method.";
　　　　char buf[]=new char[20];
　　　　s.getChars(10,14,buf,0);

4、getBytes()
　　替代getChars()的一种方法是将字符存储在字节数组中，该方法即getBytes()。

5、toCharArray()

6、equals()和equalsIgnoreCase() 比较两个字符串

7、regionMatches() 用于比较一个字符串中特定区域与另一特定区域，它有一个重载的形式允许在比较中忽略大小写。
　　boolean regionMatches(int startIndex,String str2,int str2StartIndex,int numChars)
　　boolean regionMatches(boolean ignoreCase,int startIndex,String str2,int str2StartIndex,int numChars)

8、startsWith()和endsWith()　　startsWith()方法决定是否以特定字符串开始，endWith()方法决定是否以特定字符串结束

9、equals()和==
　　equals()方法比较字符串对象中的字符，==运算符比较两个对象是否引用同一实例。
　　例：String s1="Hello";
　　　　String s2=new String(s1);
　　　　s1.eauals(s2); //true
　　　　s1==s2;//false

10、compareTo()和compareToIgnoreCase() 比较字符串

11、indexOf()和lastIndexOf()
　　indexOf() 查找字符或者子串第一次出现的地方。
　　lastIndexOf() 查找字符或者子串是后一次出现的地方。

12、substring()　　它有两种形式，第一种是：String substring(int startIndex)
　　　　　　　　 第二种是：String substring(int startIndex,int endIndex)

13、concat() 连接两个字符串

14 、replace() 替换
　　它有两种形式，第一种形式用一个字符在调用字符串中所有出现某个字符的地方进行替换，形式如下：
　　String replace(char original,char replacement)
　　例如：String s="Hello".replace('l','w');
　　第二种形式是用一个字符序列替换另一个字符序列，形式如下：
　　String replace(CharSequence original,CharSequence replacement)

15、trim() 去掉起始和结尾的空格

16、valueOf() 转换为字符串

17、toLowerCase() 转换为小写

18、toUpperCase() 转换为大写

19、StringBuffer构造函数
　　StringBuffer定义了三个构造函数：
　　StringBuffer()
　　StringBuffer(int size)
　　StringBuffer(String str)
　　StringBuffer(CharSequence chars)
　　
　　(1)、length()和capacity()　　　　一个StringBuffer当前长度可通过length()方法得到,而整个可分配空间通过capacity()方法得到。
　　
　　(2)、ensureCapacity() 设置缓冲区的大小
　　　　void ensureCapacity(int capacity)

　　(3)、setLength() 设置缓冲区的长度
　　　　void setLength(int len)

　　(4)、charAt()和setCharAt()
　　　　char charAt(int where)
　　　　void setCharAt(int where,char ch)

　　(5)、getChars()
　　　　void getChars(int sourceStart,int sourceEnd,char target[],int targetStart)

　　(6)、append() 可把任何类型数据的字符串表示连接到调用的StringBuffer对象的末尾。
　　　　例：int a=42;
　　　　　　StringBuffer sb=new StringBuffer(40);
　　　　　　String s=sb.append("a=").append(a).append("!").toString();

　　(7)、insert() 插入字符串
　　　　StringBuffer insert(int index,String str)
　　　　StringBuffer insert(int index,char ch)
　　　　StringBuffer insert(int index,Object obj)
　　　　index指定将字符串插入到StringBuffer对象中的位置的下标。

　　(8)、reverse() 颠倒StringBuffer对象中的字符
　　　　StringBuffer reverse()

　　(9)、delete()和deleteCharAt() 删除字符
　　　　StringBuffer delete(int startIndex,int endIndex)
　　　　StringBuffer deleteCharAt(int loc)

　　(10)、replace() 替换
　　　　StringBuffer replace(int startIndex,int endIndex,String str)

　　(11)、substring() 截取子串
　　　　String substring(int startIndex)
　　　　String substring(int startIndex,int endIndex)

例子：String所给出的方法均可以直接调用

```
public class Test{
	public static void main(String[] args){
		String s = "Welcome to Java World!";
		String s1 = " sun java ";
		System.out.println(s.startsWith("Welcome"));//字符串以Welcome开头
		System.out.println(s.endsWith("World"));//字符串以World结尾
		String sL = s.toLowerCase();//全部转换成小写
		String sU = s.toUpperCase();//全部转换成大写
		System.out.println(sL);
		System.out.println(sU);
		String b = s.substring(11);//从第十一位开始
		System.out.println(b);
		String c = s.substring(8,11);//从第八位开始在第十一位结束
		System.out.println(c);
		String d = s1.trim();//去掉首尾的空格
		System.out.println(d);
		String s2 = "我是程序员，我在学java";
		String e = s2.replace("我","你");
		System.out.println(e);
		int f = 5;
		String s3 = String.valueOf(f);
		System.out.println(s3);
		String s4 = "我是,这的,大王";
		String[] g = s4.split(",");
		System.out.println(g[0]);
	}
}
```

当把字符串转换成基本类型时，例如，int，integer.praseInt(String s)
当把基本类型转换成字符串时，例如，static String valueOf(int i)

