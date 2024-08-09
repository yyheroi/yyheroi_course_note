https://bbs.aw-ol.com/assets/uploads/files/1616985814689-risc-v-reader-chinese-v2p1.pdf



RV32I是RISC-V固定不变的基础整数指令集，是RISC-V的核心内容

[toc]

# 汇编基础：

## 概述

汇编语言就是低级语言，直接描述/控制 CPU 的运行，**汇编语言是二进制指令的文本形式**，与指令是一一对应的关系

每种 CPU 的机器指令都是不一样的，因此对应的汇编语言也不一样

![img](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408061753372.png)


## 寄存器

学习汇编语言，首先必须了解两个知识点：寄存器和内存模型

先来看寄存器。CPU 本身只负责运算，不负责储存数据。数据一般都储存在内存之中，CPU 要用的时候就去内存读写数据。但是，CPU 的运算速度远高于内存的读写速度，为了避免被拖慢，CPU 都自带一级缓存和二级缓存。基本上，CPU 缓存可以看作是读写速度较快的内存。

但是，CPU 缓存还是不够快，另外数据在缓存里面的地址是不固定的，CPU 每次读写都要寻址也会拖慢速度。因此，除了缓存之外，CPU 还自带了寄存器（register），用来储存最常用的数据。也就是说，那些最频繁读写的数据（比如循环变量），都会放在寄存器里面，CPU 优先读写寄存器，再由寄存器跟内存交换数据。

![img](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408061800805.png)

寄存器不依靠地址区分数据，而依靠名称。每一个寄存器都有自己的名称，我们告诉 CPU 去具体的哪一个寄存器拿数据，这样的速度是最快的。有人比喻寄存器是 CPU 的零级缓存

## 内存模型

寄存器只能存放很少量的数据，大多数时候，CPU 要指挥寄存器，直接跟内存交换数据。所以，除了寄存器，还必须了解内存怎么储存数据。

## 


https://www.ruanyifeng.com/blog/2018/01/assembly-language-primer.html



