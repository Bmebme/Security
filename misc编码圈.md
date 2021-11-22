#   进制
##  进制转换
### 16进制和ascii字符互相转换代码（可以转换很长的字符串）
```py
class Converter(object):
    @staticmethod
    def to_ascii(h):
        list_s = []
        for i in range(0, len(h), 2):
            list_s.append(chr(int(h[i:i+2], 16)))
        return ''.join(list_s)

    @staticmethod
    def to_hex(s):
        list_h = []
        for c in s:
            list_h.append(str(hex(ord(c))[2:]))
        return ''.join(list_h)


print(Converter.to_hex("FLAG"))
print(Converter.to_ascii("80f3d0d71f86b0059ac1171496054aae6e31513cdaaad9281cdeacec7d0dc53a90c8324d7080cdf7d6b65a00a1b8130052b07d26f06ed0e9775823bf7f1b5aa2"))


```
### 2 8 10 16之间的进制转换代码

十进制转二进制 ```bin(18)```--> ```'0b10010'```     去掉0b就是10010    即为十进制18转二进制是10010

十进制转八进制```oct(18)``` -->```'022'```  去掉0 就是22  即为十进制18转八进制是22

十进制转十六进制```hex(18)``` -->```'0x12'```  去掉0x 就是12  即为十进制18转八进制是12

反过来的话：

二进制转十进制 ```int('0b10010',2)``` --> `18`

八进制转十进制 ```int('022',8)```--> `18`

十六进制转十进制 `int('0x12',16)`--> `18`
### ascii码与字符串转换
1.ascii码转字符串  `chr(97`)->`a`
2.字符串转ascii码   `ord(a)`->`97`
### x进制转换成文件
我用的办法一般是转换成16进制后复制到winhex中另存为。
### 好用的各种进制之间转换的在线网站
`https://tool.lu/hexconvert/`
## 二进制转二维码
由于二进制的0和1正好与二维码的黑和白相对应，所以可以利用此特性将二进制转换为二维码
###   例题1
从某处得到的2进制字符串如下
```1111111000100001101111111100000101110010110100000110111010100000000010111011011101001000000001011101101110101110110100101110110000010101011011010000011111111010101010101111111000000001011101110000000011010011000001010011101101111010101001000011100000000000101000000001001001101000100111001111011100111100001110111110001100101000110011100001010100011010001111010110000010100010110000011011101100100001110011100100001011111110100000000110101001000111101111111011100001101011011100000100001100110001111010111010001101001111100001011101011000111010011100101110100100111011011000110000010110001101000110001111111011010110111011011```

注意到字符串是625位（25^2），正好是二维码的正方形。

代码如下：
```py
from PIL import Image#注意安装PIL的时候是pip install pillow，为避免python编码问题使用时请把注释删除
MAX = 25
pic = Image.new("RGB",(MAX, MAX))
str = "得到的二进制数字"
i=0
for y in range (0,MAX):
    for x in range (0,MAX):
        if(str[i] == '1'):
            pic.putpixel([x,y],(0, 0, 0))
        else:
            pic.putpixel([x,y],(255,255,255))
        i = i+1
pic.show()
pic.save("flag.png")
```
# 二维码扫描神器---QRsearch
自行下载
### 例题2
得到一串字符串
`0x253464253534253435253335253433253661253435253737253464253531253666253738253464253434253637253462253466253534253662253462253464253534253435253738253433253661253435253737253466253531253666253738253464253434253435253462253464253534253435253332253433253661253435253738253464253531253666253738253464253534253535253462253464253534253431253330253433253661253435253737253465253531253666253738253464253661253435253462253466253534253633253462253464253534253435253737253433253661253662253334253433253661253662253333253433253661253435253738253465253431253364253364`

由于是0x开头，想到是16进制字符串，将其转换成字符串
`%4d%54%45%35%43%6a%45%77%4d%51%6f%78%4d%44%67%4b%4f%54%6b%4b%4d%54%45%78%43%6a%45%77%4f%51%6f%78%4d%44%45%4b%4d%54%45%32%43%6a%45%78%4d%51%6f%78%4d%54%55%4b%4d%54%41%30%43%6a%45%77%4e%51%6f%78%4d%6a%45%4b%4f%54%63%4b%4d%54%45%77%43%6a%6b%34%43%6a%6b%33%43%6a%45%78%4e%41%3d%3d`

显然这是url编码，将其的`%`去掉和16进制的ascii编码没有区别了。还可以继续转换，也可以在线转换URL编码`http://tool.chinaz.com/tools/urlencode.aspx`

得到`MTE5CjEwMQoxMDgKOTkKMTExCjEwOQoxMDEKMTE2CjExMQoxMTUKMTA0CjEwNQoxMjEKOTcKMTEwCjk4Cjk3CjExNA==`

显然这是base64加密，解码
得到`119 101 108 99 111 109 101 116 111 115 104 105 121 97 110 98 97 114`

再进行一次10进制转ascii字符串就可以了

### 例题三
题目地址，攻防世界-misc-高阶-A-Weird-C-Program
```C
#include     			 	  <stdio.h>
#include	<string.h>
#include     		 	   <bits/stdc++.h>
#include	<codeme.h>
/*int     main(		int argc, char	**argv 	)
{*/	
#define     	EEr_Rs 					0x4b
#include	<stdlib.h>
#include     		  		 <sys/socket.h>
#include	<netinet/in.h>
#define     	LINE_new	 		  '\n'
#include	<assert.h>
#include     		    	<time.h>
#include	<sys/types.h>
#include     		  			<arpa/inet.h>
#include	<netdb.h>
#include     	 					<unistd.h>
	
int main(){ int run;  		 	  	
	run>>=5;run=0;
    run&=01; 		int	FELICITY[10000];  		
	run>>=5;
     using	namespace std;					
	
     	 	 			
	
char *res[6] = {"Nothing_"  			    ,
	
" and _no _one _is _perfect.	 	 	 	",
	
     "It_	just _takes_    a_good	_eye_",
	
     	  	  "to_find_"	,
	
     		"those_	hidden_" 	  ,
	
     			"imperfections. :)" 		};
	
     		int i = 0,j=0; 	
	
     		for( i=0;i <	6 ; i++)for(j=0;j<strlen(res[i]);j++)
	
     		{int t=(int)res[i][j];if(t	==	'_' )FELICITY[run++]=32;else	if((t==32||t==9)&&(j!=27))FELICITY[run++]=-1;
	
     		else FELICITY[run++]=	t   ;}
	
     		for( i=0; i< run ;i++	)
	
     		 	if(FELICITY[i]+1)printf("%c",(char)FELICITY[i]); i-=1		;
	
     	 printf("\n")	 ;
	
return  0;
 } 



/*END*/
```
题目是一段c语言代码，但是源码的格式非常诡异，就像题目描述的那样——“a_weird_c_program"，将这段代码使用一些文本编辑器打开，此处我使用的是sublime，如图所示。

![123](https://note.youdao.com/yws/api/personal/file/WEBcb568656f9370cf6268f60b4a9adcce4?method=download&shareKey=16a0e884b42fc84c7a891c8229ab40b6)

可以看到，代码格式是被空格和tab分割而成的，这种二元的组合方式让我们可以想到二进制的01，我们这里将空格视为0，tab视为1，这样我们就得到了一些二进制组合。
```
000001110100 000001101000 000001100101 000001011111 000001100110 000001101100 000001100001 000001100111 000001011111 000001101001 000001110011 000001011111 000001010111 000001110000 000001010101 000001000001 000001001001 000001110100 000001110011 000001100001 000001100100 000001101101 000001101000 000001100001 000001101011 000001010
```
写一个简单的脚本将二进制转换成ascii字符
```py
x = "000001110100 000001101000 000001100101 000001011111 000001100110 000001101100 000001100001 000001100111 000001011111 000001101001 000001110011 000001011111 000001010111 000001110000 000001010101 000001000001 000001001001 000001110100 000001110011 000001100001 000001100100 000001101101 000001101000 000001100001 000001101011 000001010 "
flag= ''
for i in x.split() :
    flag +=str( chr(int(i,2)))
print flag
```
运行脚本后得到字符串“the_flag_is_WpUAItsadmhak”


#   BASE系列
##  BASE64（最全，啥都有）
64个可打印字符，A~Z、a~z、0~9、+、/，64个可打印字符，“=”符号用作后缀填充
##  BASE58（少了0OIl+/）
相比Base64，Base58不使用数字”0″，字母大写”O”，字母大写”I”，和字母小写”l”，以及”+”和”/”符号。
##  base32(全大写)
32个可打印字符，A~Z、2~7、32个可打印字符，“=”符号用作后缀填充
##  base16（像16进制）
16个可打印字符，A~F、0-9，16个可打印字符

##  base64编码转图片
### python代码形式
```py
import os,base64 
 
with open("C:\\Users\\wonai\\Desktop\\1.txt","r") as f:
#str = "base64编码
    imgdata = base64.b64decode(f.read())
    file = open('1.jpg','wb')
    file.write(imgdata)
    file.close()
```
### 浏览器模式
可以在浏览器直接输入
`data:image/png;base64,[base64字符串]`
将base64字符串直接在浏览器中显示出来
#### 例子
`data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAANwAAAAoCAIAAAAaOwPZAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAQuSURBVHhe7ZptmoMgDIR7rh6o5+lpvEwP01XUGshAokgX+8z+7PKRTF6SoN7e/KMCnSlw68wemkMF3oSSEHSnAKHsLiQ0iFCSge4UIJTdhYQGEUoy0J0ChLK7kNAgQkkGulOAUHYXEhpEKMlAdwpcG8rhcRv/HkN3stIgW4F88DYoX89nObjmANuOc0eMXpHHcyX9+mowhgHKmdlChM0BZzvzet6DSSW7xjEWk8Hu+/O1x7zF1237/Uu4t/O46V6sZuARoZb9KqbO7On4rJlykqcYYnNAjSbx3Gmrj6WTzxirVlA+90F82G+nm4fX3zOxgqyKqRaUU7b8FpRDOeyjJa7k5oByT1yWse4mxfDC3NrrprnQtQeUMuUXoURmCGHdKfl/oTS8MElxu2mudO0BXUCZL8efVGU0EmsQjkGpM2H8y/CwGtW1C3el8ywxhHKWxgOlaPNj0VcRRW+OoiKvCXF0o6YeXWLQDaNQyMf1Clhsi22D9HUNXOBCVZamaBmiO5BxRdRQOt3M3oFUAD4/HDolSChx7AvXzRIJQtgsUfMu6HB+HglNLc5d5KiwpcAqTH7Idk/lvLD9Z0rUx4vYWL2UJ4WY6XbdL91ML57+EjsRNEMnw/LCrKklN9NNkbuLvKsdabjM/ZMByh+PDWuuw6kDEYXPzeSfzGARlNG1M1ENRCfGLlUuJ5MVTg+UyxGzC+1+KN/DkDyuTSVbqo7vNnagfKPTrH9b8pQtgQ/PRCifDTaUJaIWw8adUycklLrcppkyCZfkJ5cYlSZnQTkmsYf58OYAlMpg6JnlhYlC9uxhIdWvbr1NS8Ahc9pgQlkkai3fOorVUK4JGeYTJIgVTm+mnCqrmSfOgDJ0mOlOlhcmClk3M0KmPzeF0mnDGVB6LjqbmKB8p5GRQ34DStRCdpEpp5MRNWRNocwsjk9i7nyqugzPYTWUSZuqe0qVucAT5tgH9ITmxEdCdihjpcCVAgfI8uJ4pgx3K3UhgBeRQ9dtbJmjp1TnYmsKoSH1UGqKE23mxlrsri4yKsuAFnZ5BrAugypw0/IdSvHmxHJbEI6lREzj0asuOc7TR8BONdd9pNKCo4LRNY9CdgCEXjqObDhQvsFpy7z7DsqHP9khxp9DzNeKbSR+Iy3/n31tqVFYe17xFUZkTu507+4px4USFwBRm32lbzFyXphgRMtn3cwqqaef8a0UrMHlaJYM8RC1Iq2DeOXvKUdVjALmzromST8+4N+Egm9rrwzl/DpAVlddnE9su36Jyx6ECtkUxufaUMJOzfwQsxldUbnTLyO/ckCcNsS112yDmkkGF/4xKL8rHndrowChbKMrV61QgFBWiMepbRQglG105aoVChDKCvE4tY0ChLKNrly1QgFCWSEep7ZRgFC20ZWrVihAKCvE49Q2ChDKNrpy1QoF/gDXIhmWmc+CSAAAAABJRU5ErkJggg==`

直接在浏览器输入这段代码，或者将在python代码中输入后面的base64编码都可以显示出图片

### 图片转base64
```py
import base64
 
with open("C:\\Users\\Administrator\\Desktop\\ww\\1.jpg", 'rb') as f:
    base64_data = base64.b64encode(f.read())
    s = base64_data.decode()
    print('data:image/jpeg;base64,%s'%s)
```
##  条形码
条形码(barcode)是将宽度不等的多个黑条和空白，按照一定的编码规则排列，用以表达一组信息的图形标识符。常见的条形码是由反射率相差很大的黑条（简称条）和白条（简称空）排成的平行线图案。
### 例子
![Image text](https://image.3001.net/images/20200303/1583202337_5e5dc021f292e.png)
这里大部分writeup中是说把条形码用ps修改成相同长度后在扫描，这里推荐一个网站，`https://online-barcode-reader.inliteresearch.com/`,这个网站可以在线扫描一些码，比如二维码，条形码，例子中的这个斑马是不需要处理直接就可以在这个网站扫出来的。
##  二维码
### 二维码反色
####    判断二维码是否颜色反了的方法
![image](https://image.3001.net/images/20200303/1583202518_5e5dc0d6f1e8e.png!small)

比如这个二维码，看二维码的三个小正方形，小正方形内部是黑色，代表是正常的二维码，否则就是反色的二维码了，这里用ps将图片反色，就可以得到正确的二维码了。（注：qq和微信`有时候`可以扫出来反色二维码）
### 二维码修补
![123](https://image.3001.net/images/20200303/1583202542_5e5dc0eec94ec.png!small)

二维码的左上左下右上三个角落的小正方形，是没有数据的，在二维码中起到的是确定二维码位置的作用。~~众所周知三点确定一个正方形~~
将三个角落都填到正常大小的正方形块，就可以扫描了
####    例题
攻防世界-misc-新手-give_u_flag

下载附件后，发现是一个gif图，使用stegsolve，可以逐帧分析gif，也可以使用gifsniffer将gif分解，分析后可以发现小人有一帧是带有残缺的二维码。如图所示

![123](https://adworld.xctf.org.cn/media/task/writeup/cn/give_you_flag/1.png)
这里的二维码就少了三个定位的部分，用各种工具都可以将二维码修复，修复后的二维码可以如图所示。

![2](https://s1.ax1x.com/2020/03/18/8wa14s.png)

### PDF417
PDF417条码是一种高密度、高信息含量的便携式数据文件，是实现**及卡片等大容量、高可靠性信息自动存储、携带并可用机器自动识读的理想手段。
上面提到的`https://online-barcode-reader.inliteresearch.com/`,可以对pdf417进行解码
#   其他编码
##  URl编码
URL编码按照我的理解就是加了`%`的16进制ascii码，<label style="color:red">注意url可能会连续两次使用，比如%2868就是（%28）（68）就是%68就是h</label>
##  Quoted-printable
可打印字符引用编码”、“使用可打印字符的编码”，我们收邮件，查看信件原始信息，经常会看到这种类型的编码。

一个等号”=”后跟随两个十六进制数字(0–9或A–F)表示该字节的数值。

测试链接：`http://web.chacuo.net/charsetquotedprintable`

```=3Cmeta=20name=3D=22description=22=20flag=3D=22tidesec=22=20=2F=3E=0A```
##  UUENCODE
uuencode是将二进制文件转换为文本文件的过程，转换后的文件可以通过纯文本e-mail进行传输，在接收方对该文件进行uudecode，即将其转换为初始的二进制文件。

测试链接：`http://web.chacuo.net/charsetuuencode`

```
6OMFX]L#]U],*9FQA9WMT:61E<V5C?0``
```
##  Unicode 
Unicode是计算机科学领域里的一项业界标准，包括字符集、编码方案等。它为每种语言中的每个字符设定了统一并且唯一的二进制编码，以满足跨语言、跨平台进行文本转换、处理的要求。

测试链接：`http://tool.chinaz.com/Tools/Unicode.aspx`

`&#36825;&#26159;&#19968;&#20010;&#20363;&#23376;&#10;&#102;&#108;&#97;&#103;&#123;&#116;&#105;&#100;&#101;&#115;&#101;&#99;&#125;`
##  编码特征简单总结
![123](https://image.3001.net/images/20200303/1583202822_5e5dc206599ee.png!small)

#   混淆和加密
##  php混淆加密 
测试链接：`https://www.toolfk.com/tool-convert-php`
##  css/js混淆加密 
测试链接：`http://tool.chinaz.com/js.aspx`
##  VBScript.Encode混淆加密
VBScript.Encode解码器 

测试链接：`http://www.cftea.com/tools/online/vbscriptDecode/`
##  PPENCODE（这个我只找到了加密，没找到解密）
Perl -> 英文字母（无特殊符号）

把Perl代码转换成只有英文字母的字符串

`http://namazu.org/~takesako/ppencode/demo.html`

##  rrencode(大家给出的demo网址都一样而且都被404了)
Ruby -> 特殊符号

把ruby代码全部转换成符号
##  jjencode/aaencode
Javascript -> 颜文字

`http://utf-8.jp/public/aaencode.html`
只有加密和执行，未有解密
可以在浏览器中按F12->console执行代码
##  JSfuck
用6 个字符 ( ) [ ] !+ 来对JavaScript进行编码

测试链接：`https://www.bugku.com/tools/jsfuck/`
同上
##  jother
密文为8个字符! + ( ) [ ] { }
`http://tmxk.org/jother/`
同上
##  brainfuck
密文由+.<>[]’ && ‘!.?或者’+-.<>[]’等构成
`http://ctf.ssleye.com/brain.html`
### 例子
攻防世界-misc-高阶-can_has_stdio?
打开附件后可以看到如下代码
```

                                                                              
                                                                              
                                      +                                       
                                     ++                                       
                                     +++                                      
                                    ++[>                                      
                                    +>++>                                     
                                   +++>++                                     
                                   ++>++++                                    
                                  +>++++++                                    
                                  >+++++++>                                   
                                 ++++++++>+                                   
                                 ++++++++>++                                  
                                ++++++++>+++                                  
                                ++++++++>++++                                 
          ++++++++>+++++++++++++>++++++++++++++>+++++++++++++++>++            
            ++++++++++++++<<<<<<<<<<<<<<<<-]>>>>>>>>>>>>>--.++<<              
              <<<<<<<<<<<>>>>>>>>>>>>>>----.++++<<<<<<<<<<<<<<                
                >>>>>>>>>>>>+.-<<<<<<<<<<<<>>>>>>>>>>>>>-.+<                  
                  <<<<<<<<<<<<>>>>>>>>>>>>>>>+++.---<<<<<<                    
                    <<<<<<<<<>>>>>>>>>>>>>---.+++<<<<<<<                      
                      <<<<<<>>>>>>>>>>>>>>+++.---<<<<<                        
                        <<<<<<<<<>>>>>>>>>>>>>>-.+<<                          
                          <<<<<<<<<<<<>>>>>>>>>>>>                            
                          >>----.++++<<<<<<<<<<<<<                            
                          <>>>>>>>>>>>>+.-<<<<<<<<                            
                         <<<<>>>>>>>>>>>>>>--.++<<<                           
                         <<<<<<<<<<<>>>>>>>>>>>>>-.                           
                        +<<<<<<<<<<<<<>>>>>>>>>>>>>>                          
                        +++.---<<<<<<   <<<<<<<<>>>>                          
                       >>>>>>>>-.+<       <<<<<<<<<<<                         
                       >>>>>>>>>>           >>>--.++<                         
                      <<<<<<<<<               <<<>>>>>                        
                      >>>>>>                    >>>-.+                        
                     <<<<<                        <<<<<                       
                     <<<                            <>>                       
                    >>                                >>                      
                                                                              
                                                                              
                                                                              
>>>>>>>>++.--<<<<<<<<<<<<<<>>>>>>>>>>>>-.+<<<<<<<<<<<<>>>>>>>>>>>>>--.++<<<<<<<<<<<<<>>>>>>>>>>>>>>>---.+++<<<<<<<<<<<<<<<>>>>>>>>>>>>>>--.++<<<<<<<<<<<<<<>>>>>>>>>>>>-.+<<<<<<<<<<<<>>>>>>>>>>>>+.-<<<<<<<<<<<<>>>>>>>>>>>>>>--.++<<<<<<<<<<<<<<>>>>>>>>>>>>>----.++++<<<<<<<<<<<<<>>>>>>>>>>>>-.+<<<<<<<<<<<<>>>>>>>>>>>>>>.<<<<<<<<<<<<<<>>>>>>>>>>>>>>++.--<<<<<<<<<<<<<<>>>>>>>>>>>>>>-.+<<<<<<<<<<<<<<>>>>>>>>>>>>>--.++<<<<<<<<<<<<<>>>>>>>>>>>>>+.-<<<<<<<<<<<<<>>>>>>>>>>>>>>>----.++++<<<<<<<<<<<<<<<>>>>>>>>>>>>>>>>---.+++<<<<<<<<<<<<<<<<.
```
##  Ook
密文由(Ook、Ook?、Ook!)等构成
`https://www.splitbrain.org/services/ook`
##  Npiet
特点文档是像素点的图片

测试链接：`https://www.bertnase.de/npiet/npiet-execute.php`
