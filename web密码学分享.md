# WEB密码学相关

## ECB加密模式（电子密码本分组模式）

![ecb](https://s1.ax1x.com/2020/06/01/t8ApJ1.jpg)

ecb加密模式是先将明文分组，然后将分组后的明文一一加密，最终得到密文分组，再将密文分组组合成真正的密文。解密时，将密文分组，再进行解密最后得到明文分组，最终进行组合。
针对此，我们可以想到，如果我只改变明文的其中一个分组，并不会对其他分组造成影响。另外，如果我们想知道一个明文对应的密文应该怎么办呢？可以这样做：    
首先知道明文是如何分组的，我如果可以控制其中的一个分组，那么我只需要将这部分的明文修改成我所需的明文，然后查看密文与原密文不同的位置，就为所需的明文。另外，如果我们不能控制已经分组好的明文，我们可以通过将明文扩展，这里举例用每16bit进行一次加密，总共分组4次，那么总加密位数就是48位，如果我能控制第49位即以后，那么我就可以得到后面的加密密文，也就达到了不需要密钥就可以加密的目的。
另外，如果我们可以知道ecb的填充方式，并且可以在明文前面添加自己想要的字节，这样的话我们就可以将明文全部爆破出来。方法如下：

假设明文是这样的aaaaa,aaaaa,aaaaa,
使用x代表填充字节
如果我将第一个分组添加1位b，那么加密时的明文就为baaaa,aaaaa,axxxx
然后我再设定一个分组为cxxxx，通过爆破c的值得到密文，然后与axxxx的密文进行比较，就可以爆破出a的值，重复此方法，就可以得到全部密文值。

## CBC密码分组链接模式

![cbc](https://img-blog.csdn.net/20160405180943506?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![cbc](https://img-blog.csdn.net/20160405180951038?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

cbc的主要特点是链式加密，首先将明文分组，定义一个初始化向量iv，先将明文的第一个分组与iv异或，得到的结果再加密，得到密文的第一个分组，然后将密文的第一个分组视为新的iv，重复此过程得到密文2，最终得到完整的密文。

这样就会出现以下两个情况：

1.初始向量iv，可以影响c1

2.分组密文cn，可以影响cn+1

举个例子，明文：a:2:{s:4:"name";s:6:"sdsdsd";s:8:"greeting";s:20:"echo 'Hello sdsdsd!'";}
分组后是如下

Block 1: a:2:{s:4:"name";

Block 2: s:6:"sdsdsd";s:8

Block 3: :"greeting";s:20

Block 4: :"echo 'Hello sd

Block 5:sdsd!'";}

假设，我想利用控制block1密文把block2中的6修改为7，因为block2中的6是第三位，而cbc模式是按位异或，所以我只需修改block1密文的第三位就可以了。

\$v = "a:2:{s:4:"name";s:6:"sdsdsd";s:8:"greeting";s:20:"echo 'Hello sdsdsd!'";}";

\$enc = @encrypt($v);

\$enc[2] = chr(ord($enc[2]) ^ ord("6") ^ ord ("7"));

\$b = @decrypt($enc);

### padding oracle attack（应用在cbc模式上的攻击方法）

条件：已知密文的分组方式，题目会给你真正用来加密的iv，我们可以控制iv的值，服务器会以各种方式应答你以下三种情况，1.密文不能正常解密2.密文可以正常解密但是解密结果不正确。3.密文可以正常解密且解密结果正确。

先讲一下以3des为例的填充方式，假设一组中有8个字节，如果加密的明文字节数不够8个字节，缺n个字节就补n个0x0n，比如0x0a 0x0a 0x0a 0x0a 0x0a 在填充后就是0x0a 0x0a 0x0a 0x0a 0x0a 0x03 0x03 0x03 

回头看一下解密过程，密文首先要经过3des解密，得到一个数据流，我们记其为中间值(intermediary value)，这个中间值再与iv异或，就得到了明文。以第一组分组为例，我们先将iv全部设定为0x00，这样在异或的时候，解密出来的明文就是前面提到的中间值，如图。
![cbcpadding1](https://upload-images.jianshu.io/upload_images/2087924-65ec7d2db092f3d5.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

如果它成功返回了这个中间值，我们就可以直接将其与真正的iv进行解密了。

如果其不返回这个中间值，我们又讲怎么做呢？

这时我们先认定该字节流已经进行了一次0x01填充（尽管事实并不是这样），这样我们就认为明文的最后一位就是0x01了，我们将padding的最后一个字节由0x00一直递增到0xff，这样的话，当且仅当解密出来的最后一位是0x01时，服务器才会返回确认可以正常解密，但是其解密结果肯定不正确（这没关系）。刚才提到中间值与我们定义的iv最后一位异或得到0x01,这样我们就可以确定一个唯一的最后一个字节的中间值。0x01与我们的iv最后一位异或。

接下来我们认定字节流进行了一次0x02填充，由于最后一位我们已知了，可以定义一个新的iv最后一位，让二者异或得到0x02，这时再爆破使得解密结果为0x02的iv第7位，然后再得到中间值的第七位，将中间值第7位与真正的iv异或，反复执行这个循环，就可以得到这一组的明文。如果有第二组密文，将确定的iv变成第一组的密文就可以了。

## CFB加密模式

在CFB(Cipher FeedBack,密文反馈模式)模式中，前一个密文分组会被送回到密码算法的输入端。所谓反馈，这里指的就是返回输入端的意思。加密方式如图：
![cfb](https://img-blog.csdn.net/20180903220102338?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW5ncWl1bWluZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
注意：其加密和解密的时候，都不会用到算法的解密方式，只会用到加密和异或。
对CFB模式可以实施重放攻击。 

![cfbattack](https://img-blog.csdn.net/20180903220450915?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoZW5ncWl1bWluZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Alice向Bob发送一条消息，这条消息由4个密文分组组成。主动攻击者将该消息中的后3个密文分组保存了下来。第二天，Alice又向Bob发送了内容不同的4个密文分组（假设Alice使用了相同的密钥）。攻击者用昨天保存下来的3个密文分别将今天发送的后3个密文分组进行了替换。

于是，Bob解密时，4个分组中只有第1个可以解密成正确的明文分组，第2个会出错，而第3个和第4个则变成了被攻击者替换的内容（也就是昨天发送的明文内容）。攻击者没有破解密码，就成功地将以前的电文混入了新电文中。而第2个分组出错到底是通信错误呢，还是被人攻击所造成的呢？Bob是无法做出判断的。
