---
title: ASCII,GB2312,Unicode,UTF8,UTF16
date: 2020-11-10 15:03:56
tags: Web
---

## ASCII (American Standard Code Information Interchange)

`
可见字符 95个
控制字符 33个
`
![](/images/character_coding/ascii.png)

## 中文

### 设计字符集GB2312
分区管理，共计94个区，每个区含94个位，共8836个码位
01-09区收录除汉字外的682个字符t
10-15区为空白区，没有使用
16-55区收录3755个一级汉字，按拼音排序
56-87区收录3008个二级汉字，按部首/笔画排序
99-94区位空白区，没有使用

### 存储GB2312

eg 侃 5709

0x57+0xA0（大于127，避开ascii码）=0xD9
0x09+0xA0=0xA9

得到侃的GB2312码: `0xD90XA9`

### 扩展GB2312：GBK

新增20000个汉字和符号，不再规定地位大于127

### 扩展GBK：GB18030

新增几千少数民族字符


## Unicode

不同国家有不同的编码，Unicode统一所有的字符编码

一开始unicode使用UCS-2字符集(Universal Character Set)，16位可表示65536个字符
但还是无法表示世界上所有的字符
后来又有了UCS-4字符集，用32位表示一个字符，可表示近43亿个字符
但是空间过大，不能接受

`
Unicode是用0至65535之间的数字来表示所有字符.其中0至127这128个数字表示的字符仍然跟ASCII完全一样.65536是2的16次方.这是第一步.第二步就是怎么把0至65535这些数字转化成01串保存到计算机中.这肯定就有不同的保存方式了.于是出现了UTF(unicode transformation format),有UTF-8,UTF-16.
`

Unicode字符平面映射
https://zh.wikipedia.org/wiki/Unicode%E5%AD%97%E7%AC%A6%E5%B9%B3%E9%9D%A2%E6%98%A0%E5%B0%84

## UTF-8编码（unicode transformation format）

将UCS-4字符集的码位划分为4个区间
![](/images/character_coding/utf8.png)

e.g.
编 （unicode 32534) :
->十进制 32534
->二进制 0111 11 1100 01 0110
(1110xx 10xx 10xx)
->(1110) 0111 (10)11 1100 (10)01 0110
-> e     7    b      c    9      6
-> UTF-8: e7 bc 96
-> URL编码: %e7%bc%96

## UTF-16编码

在Unicode基本多文种平面(0號平面)定义的字符（无论是拉丁字母、汉字或其他文字或符号），一律使用2字节储存。而在辅助平面定义的字符，会以代理对（surrogate pair）的形式，以两个2字节的值来储存。

UTF-16比起UTF-8，好处在于大部分字符都以固定长度的字节（2字节）储存，但UTF-16却无法兼容于ASCII编码。

UTF-16 顾名思义，就是用两个字节表示一个字符。那么用两个字节表示必然存在字节序的问题，即大端小端的问题。下面就来讲讲 UTF-16BE、UTF-16LE、UTF-16 三者之间的区别吧。

UTF-16BE，其后缀是 BE 即 big-endian，大端的意思。大端就是将高位的字节放在低地址表示。
UTF-16LE，其后缀是 LE 即 little-endian，小端的意思。小端就是将高位的字节放在高地址表示。
UTF-16，没有指定后缀，即不知道其是大小端，所以其开始的两个字节表示该字节数组是大端还是小端。即FE FF表示大端，FF FE表示小端。

Python test

"王" unicode = 0x0000 738b = int(29579)
```
>>> bytes("王", "utf-8")
b'\xe7\x8e\x8b'
>>> bytes("王", "utf-16")
b'\xff\xfe\x8bs'
>>> hex(ord('s'))
'0x73'
```


Java char类型采用UTF-16编码表示一个代码单元, 辅助平面定义的字符需要用两个char表示

```
String s = "hi\uD83E\uDF60";
System.out.println(s);
System.out.println(s.length());
int cpCount = s.codePointCount(0, s.length());
System.out.println(cpCount);
char ch = s.charAt(2);
System.out.println(ch);

hi🭠
4
3
?
```
