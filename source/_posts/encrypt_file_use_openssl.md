title: "利用python加密的文件在openssl解密时遇到的问题"
date: 2014-11-6 10:50:57
tags:
  - python
  - openssl
  - 文件加解密
categories:
  - 原创
  - 解决问题
thumbnailImagePosition: left
thumbnailImage: http://7xrt06.com1.z0.glb.clouddn.com/16-3-15/76111365.jpg
---

在文件压缩加密过程中需要注意padding的问题。
<!-- excerpt -->

之前的项目中用python的加密解密模块 `Crypto` 对一个tar文件加密，加密后的tar文件发送到客户端，客户端是运行openWRT的路由器，在路由器端用openssl进行解密，先附上代码。
用来加密文件的python code：

```python
# Encrypte file
def aes_encrypt_file(in_filename, out_filename, key, iv):
    block_size = AES.block_size
    pad = lambda s: s + (block_size - len(s) % block_size) \
                        * chr(block_size - len(s) % block_size)
    cipher = aes_build_cipher(key, iv)
    with open(in_filename, 'rb') as infile:
        with open(out_filename, 'wb') as outfile:
            while True:
                buf = infile.read(1024)
                if len(buf) == 0:
                    break
                elif len(buf) % block_size != 0:
                    buf = pad(buf)
                outfile.write(cipher.encrypt(buf))
```

用来解密的openssl 命令

```shell
openssl aes-256-cbc -d -nosalt -K $sum256 -iv $iv -in ${PACKAGE} | tar xzf -
```

需要说明的是每隔一小时就要执行一遍程序，将tar文件加密后发送到客户端解密解压。而且每次tar 文件都会发生变化，但基本上大小都在200KB左右。
最初的几次测试，程序运行的都很愉快、很正常。于是写了个脚本让程序每隔2分钟运行一次，跑了一晚上，第二天打开log立马傻眼，居然出现错误，平均每10次会出现一次，错误信息如下：

```shell
bad decrypt
2011837512:error:06065064:lib(6):func(101):reason(100):NA:0:
tar: short read
```

很明显解密出现问题，开始解决问题。首先排除了 `key` 和 `iv` 出错的情况，将问题定位到了在网上找的padding的这段代码，于是查找资料。
在google了一通错误码之后毫无帮助，于是直接查找openssl是如何处理padding的。
google之后找到[这里](http://https://www.openssl.org/docs/apps/enc.html), 在这里面讲到了openssl关于padding的参数nopad，但也只有这一个参数。同时还讲到了这样一句话：

>All the block ciphers normally use PKCS#5 padding also known as standard block padding: this allows a rudimentary integrity or password check to be performed. However since the chance of random data passing the test is better than 1 in 256 it isn’t a very good test.

意思就是说他在处理padding时通常是使用 `PKCS#5` 的方式，这个就很有用了。于是再去查看 `PKCS#5` 的标准，找到了[这个网站](http://tweetyf.org/2012/04/the_difference_between_pkcs7_pkcs5.html), 里面介绍了 `PKCS#5` 和PKCS#7的区别，也讲了原理。并且提到对于block_size是16的情况无论如何都要做填充，即使文件大小正好是16的倍数那也要在最后填充上16个16。看到这里就可以发现代码中是有问题的，因为在代码中如果文件大小正好是16的倍数就不会去padding，所以修改之后如下：

```python
# Encrypte file
def aes_encrypt_file(in_filename, out_filename, key, iv):
    block_size = AES.block_size
    pad = lambda s: s + (block_size - len(s) % block_size) \
                        * chr(block_size - len(s) % block_size)
    cipher = aes_build_cipher(key, iv)
    with open(in_filename, 'rb') as infile:
        with open(out_filename, 'wb') as outfile:
            buf = infile.read()
            buf = pad(buf)
            outfile.write(cipher.encrypt(buf))
```

这样就可以保证无论文件多大最后都要padding了，这样openssl就可以成功解密了。

在解决之前将问题发到了stackoverflow上，可以看一下大牛们的讨论：[链接](http://stackoverflow.com/questions/24531307/how-does-openssl-command-handle-pkcs7-padding-added-with-python)
***
(EOF)
