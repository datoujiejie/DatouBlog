title: iOS_DES
date: 2015-09-06 20:30:10
tags: iOS
---

#iOS使用Openssl库实现DES加解密

随着iOS应用的发展，数据的安全也越来越重要，但是在以往的面试过程中，发现很多开发者只用过md5或者base64加密，对称加密和非对称加密都不知道概念，自己将相关知识总结一下。

##关于DES
DES是一种对称加密算法，所谓对称加密算法即：加密和解密使用相同密钥的算法。DES加密算法出自IBM的研究，后来被美国政府正式采用，之后开始广泛流传，但是近些年使用越来越少，因为DES使用56位密钥，以现代计算能力，24小时内即可被破解。所以DES有了进化版--3DES，即：使用3条56位的密钥对数据进行三次加密。
##加密流程
DES算法是把64位的明文变为64位的密文，它所使用的密钥也是64位。主要算法流程如下：
![DES 流程](http://h.hiphotos.baidu.com/baike/c0%3Dbaike80%2C5%2C5%2C80%2C26/sign=bf7cb45b367adab429dd1311eabdd879/adaf2edda3cc7cd9ca7533aa3901213fb80e916c.jpg)

3DES加密就是进行了3次加密，输入数据的长度和输出数据的长度都还是64位。

##分组加密模式
在平时使用中，输入的数据肯定不止64位，所以有了分组加密的概念。如果加解密选择的分组模式不同，是不能够正常解密的。分组加密有四种模式，分别为：ECB、CBC、CFB和OFB。
###ECB(电子密码本模式：Electronic codebook)
ECB是最简单的块密码加密模式，加密前根据加密块大小分成若干块，之后将每块使用相同的密钥单独加密，解密同理。
![ECB Encrypt](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d6/ECB_encryption.svg/601px-ECB_encryption.svg.png)

ECB模式由于每块数据的加密是独立的因此加密和解密都可以并行计算，缺点是相同的明文会被加密成相同的密文，这样对数据不能很好的保密。维基百科上提供了图片的例子来说明这种加密方式的缺点：

![ECB Encrypt](https://upload.wikimedia.org/wikipedia/commons/5/56/Tux.jpg "Title")
  ![ECB Encrypt](https://upload.wikimedia.org/wikipedia/commons/f/f0/Tux_ecb.jpg "Title") ![ECB Encrypt](https://upload.wikimedia.org/wikipedia/commons/a/a0/Tux_secure.jpg "Title")
  
 第二张图片就是ECB加密后的图片，可以看出原图片的轮廓还清晰可见，由此可见ECB模式的保密性不是很好。
 
###CBC(密码块链：Cipher Block Chaining)
CBC模式对于每个待加密的密码块在加密前会先与前一个密码块的密文异或然后再用加密器加密。第一个明文块与一个叫初始化向量的数据块异或。

![CBC Encrypt](https://upload.wikimedia.org/wikipedia/commons/thumb/8/80/CBC_encryption.svg/601px-CBC_encryption.svg.png)
![CBC Decrypt](https://upload.wikimedia.org/wikipedia/commons/thumb/2/2a/CBC_decryption.svg/601px-CBC_decryption.svg.png)

http://blog.poxiao.me/p/advanced-encryption-standard-and-block-cipher-mode/