Character 类用于对单个字符进行操作。
Character 类在对象中包装一个基本类型 **char** 的值
```java
char ch = 'a';
 
// Unicode 字符表示形式
char uniChar = '\u039A'; 
 
// 字符数组
char[] charArray ={ 'a', 'b', 'c', 'd', 'e' };
```
使用Character的构造方法创建一个Character类对象，例如：
`Character ch = new Character('a');`

---

## 转义序列
前面有反斜杠（\）的字符代表转义字符，它对编译器来说是有特殊含义的。
下面列表展示了Java的转义序列：

| 转义序列 | 描述 |
| :--- | :--- |
| \\t | 在文中该处插入一个tab键 |
| \\b | 在文中该处插入一个后退键 |
| \\n | 在文中该处换行 |
| \\r | 在文中该处插入回车 |
| \\f | 在文中该处插入换页符 |
| \\' | 在文中该处插入单引号 |
| \\" | 在文中该处插入双引号 |
| \\\\ | 在文中该处插入反斜杠 |


---

## Character 方法

| 序号 | 方法与描述 |
| :--- | :--- |
| 1 | [isLetter()](https://www.runoob.com/java/character-isletter.html)
是否是一个字母 |
| 2 | [isDigit()](https://www.runoob.com/java/character-isdigit.html)
是否是一个数字字符 |
| 3 | [isWhitespace()](https://www.runoob.com/java/character-iswhitespace.html)
是否是一个空白字符 |
| 4 | [isUpperCase()](https://www.runoob.com/java/character-isuppercase.html)
是否是大写字母 |
| 5 | [isLowerCase()](https://www.runoob.com/java/character-islowercase.html)
是否是小写字母 |
| 6 | [toUpperCase()](https://www.runoob.com/java/character-touppercase.html)
指定字母的大写形式 |
| 7 | [toLowerCase](https://www.runoob.com/java/character-tolowercase.html)()
指定字母的小写形式 |
| 8 | [toString](https://www.runoob.com/java/character-tostring.html)()
返回字符的字符串形式，字符串的长度仅为1 |

