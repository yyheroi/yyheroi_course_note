[toc]

# 第一章

## 检测点1.1

(1) 1个CPU的寻址能力为8KB，那么它的地址总线的宽带为 13。
x(2)1KB的存储器有 `1024` 个存储单元。存储单元的编号从`0` 到 `1023`
(3)1KB的存储器可以存储 `2^13` 个bit，`1024`个Byte。
(4)1GB、1MB、1KB分别是 2^30 2^20 2^10 Byte。
(5) 8080、8088、80286、80386的地址总线宽度分别为16根、20根、24根、32根，则它们的寻址能力分别为: 64 (KB)、 1 (MB)、 16 (MB)、 4 (GB)。
(6) 8080、8088、8086、80286、80386的数据总线宽度分别为8根、8根、16根、16根、32根。
则它们一次可以传送的数据为: 512（B)、512 (B)、64K (B)、 (B)、 4G(B)。
(7)从内存中读取1024字节的数据，8086至少要读 64 次，80386至少要读 32 次。
(8)在存储器中，数据和程序以 `二进制` 形式存放。

### 注意

8086CPU有14个寄存器，AX、BX、CX、DX、SI、DI、SP、BP、IP、CS、SS、DS、ES、PSW





# 第二章

## 检查点2.1

（1）写出每条汇编指令执行后的相关寄存器中的值

|     指令     |    AX    |
| :----------: | :------: |
| mov ax,62627 |  F4A3H   |
|  mov ah,31H  |  31A3H   |
|  mov al,23H  |  3123H   |
|  add ax,ax   |  6246H   |
| mov bx,826CH | BX=826CH |
|  mov cx,ax   | CX=6246H |
|  mov ax,bx   |  826CH   |
|  add ax,bx   |  04D8H   |
|  mov al,bh   |  0482H   |
|  mov ah,bl   |  6C82H   |
|  add ah,ah   |  D882H   |
|   add al,6   |  D888H   |
|  add al,al   |  D810H   |
|  mov ax,cx   |  6246H   |

x(2)只能使用目前学过的汇编指令，最多使用4条指令，编程计算2的4次方

mov ax,2     2
add ax,ax    4
add ax,ax    8 
add ax,ax    16

## 检查点2.2

x（1）给定段地址为0001H，仅通过变化偏移地址寻址，cpu的寻址范围为 `00010H` 到 `1000FH`
（2）有一数据存放在内存 20000H 单元中，给定段地址为SA，若想用偏移地址寻到此单元。则SA应满足的条件是：最小为：`1001H`，最大值为：`2000H`



### 2.10 CS和IP注意知识点

**CS**，`代码段寄存器`；IP为`指令指针寄存器`

8086PC机中，任意时刻，设CS中的内容为M，IP中的内容为N，8086CPU将从内存`Mx16+N`单元开始，读取一条指令并执行

可以修改CS、IP的指令：jmp

jmp使用方法：
1.同时修改CS、IP的内容`jmp 段地址：偏移地址`
2.修改IP的内容 `jump ax`

8086CPU的工作过程：
1.从CS：IP指向的内存单元执行读取指令，读取的指令进入指令缓冲器；
2.IP指向下一条指令；
3.执行指令（转到步骤1，重复这个步骤）

## 检查点2.3

x下面3条指令执行后，CPU几次修改IP？都在说明时候？最后IP中的值是多少？
mov ax，bx			；ax=bx
sub ax，bx			 ；ax=0
jump ax				  ；IP=ax=0

修改4次，第一次是执行完mov ax，bx，第二次是sub ax，bx，第三次是jump ax，第四次是jump ax；最后IP的值是0

## 实验1 (8953)

使用DosBox 的debug 功能

相关命令有 

R（查看、修改CPU寄存器的内容）

D（查看内存中的内容）

E（修改内存中的内容）

U（将内存中的机器指令翻译成汇编指令）

T（执行一条机器指令）

A（以汇编指令的格式在内存中写入一条机器指令）

1.进入Debug 

r查看cpu寄存器的内容，注意CS和IP的值，073F：0100，此处存的机器码为0000

![image-20240812101847856](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408121018179.png)

2.使用汇编操作内存相关数据

a 1000：0

mov ax,1
add ax,ax

3.查看内存数据 

d 2000：0 f

4.修改内存数据

e 2000：0  0 1 2 3 4 

# 第三章

不能直接操作DS(段寄存器)

mov指令形式：

mov寄存器，数据	比如: mov ax,8
mov寄存器，寄存器	比如: mov ax,bx
mov寄存器，内存单元	比如: mov ax,[0]
mov内存单元，寄存器	比如: mov [0],ax
mov段寄存器，寄存器	比如: mov ds,ax

DS，`数据段寄存器`

## 检查点3.1

(1)

0000:0000 70 80 F0 30  EF 60  30  E2-00 80  80 12 66 20 22 60
0000:0010 62 26 E6 D6 CC 2E 3C 3B-AB BA 00 00 26 06 66 88

mov ax,1
mov ds,ax
mov ax,[0000]		ax=2662H
mov bx,[0001]		bx=E626H
mov ax,bx			  ax=E626H
mov ax,[0000] 	   ax=2662H
mov bx,[0002] 	   bx=D6E6H
add ax,bx			   ax=FD48H
add ax,[0004] 		ax=2C14H
mov ax,0				ax=0
mov al,[0002]		 ax=00E6H
mov bx,0				bx=0
mov bl,[000C]		bx=0026H
add al,bl				ax=000CH

(2)

CS=2000H，IP=1000H，AX=0，BX=0

1.写出cpu执行的指令序列
mov ax,6622H			CS=2000H,IP=3H     	  ax=6622H,bx=0,DS=1000H
jmp 0ff0:0100			 CS=0FF0H,IP=100H	   ax=6622H,bx=0,DS=1000H
mov ax,2000H			CS=0FF0H,IP=103H 	  ax=2000H,bx=0,DS=1000H
mov ds,ax				   CS=0FF0H,IP=105H       ax=2000H,bx=0,DS=2000H
mov ax,[0008]             CS=0FF0H,IP=108H 	  ax=C389H,bx=0,DS=2000H
mov ax,[0002]             CS=0FF0H,IP=10BH  	 ax=EA66H,bx=0,DS=2000H

数据和出现都是二进制形式存在内存中的，通过段地址来区分，数据就是DS、程序就CS

### 3.7 SS和SP注意知识点

SS:SP在任意时刻指向栈顶元素

SS，栈段地址寄存器，SP栈偏移地址寄存器

push和pop指令执行时，CPU从SS和SP中得到栈顶的地址

在操作入栈和出栈，注意越界的问题，因为cpu不会检查越界情况，越界会导致数据覆盖

cpu将内存中的某段内存当作代码是因为CS：IP指向了那里，CPU将内存当作栈，是因为SS：SP指向了那里



## 检查点3.2

(1)
mov ax,2000H
mov ss,ax
mov sp,0010H

(2)
mov ax,1000H
mov ss,ax
mov sp,0000H

## 实验2 (8954)

```
设置指令执行的内容起始段地址1000
r cs 
1000
r ip
0

查看1000段地址处的数据
d 1000:0
```

向1000:0地址写入相关汇编指令
![image-20240813175003356](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408131750726.png)

将2000:0内容清空并查看

```
e 2000:0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
d 2000:0 f
```

ffff:0出数据为：
![image-20240813175135726](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408131751747.png)



![image-20240813181249443](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408131812484.png)



![image-20240813181337008](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408131813041.png)

mov ax,[0]  ;ax = C0EAH
add ax,[2]  ;ax =  C0FCH
mov bx,[4] ;bx = 30F0H
add bx,[6]  ;bx = 6021H

push ax  ;sp = FE	修改的内存单元地址是22100内容为FC C0
push bx  ;sp = FC	修改的内存单元地址是22098内容为21 60
pop ax  ;sp = FE	;ax = 6021H
pop bx  ;sp = 100  ;bx = C0FCH

push [4] ;sp = FE	修改的内存单元地址是22100  内容为 F0 30
push [6] ;sp = FC    修改的内存单元地址是22102  内容为 31 2F

![image-20240813181534231](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408131815257.png)

![image-20240813181713809](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408131817844.png)

查看2200：E0处内存

![image-20240813181801492](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408131818525.png)

# 第四章

segment，一个段的开始，ends一个段的结尾

段名 segment

段名 ends

![image-20240814100439065](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408141014997.png)



![image-20240814101354453](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408141013485.png)

一段正常的汇编代码

![image-20240814101951992](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408141342773.png)

### 注意：

dos命令

dir type cd

使用q命令退出Debug

注意p命令执行 int 21

## 实验3 (8955)

```assembly
assume cs:codesg
codesg segment
	mov ax,2000h	;ax 2000h
	mov ss,ax		;ax 2000h ss 2000h
	mov sp,0		;ax 2000h SS：SP为2000：0000
	add sp,10		;ax 2000h SS：SP为2000：000A
	pop ax			;ax 076A  SS：SP为2000：000C
	pop bx			;ax 076A bx 7206		 SS：SP为2000：000E
	push ax			;ax 076A bx 7206		 SS：SP为2000：000C
	push bx			;ax 076A bx 7206		 SS：SP为2000：000A
	pop ax			;ax 7206 bx 7206	 	 SS：SP为2000：000C
	pop bx			;ax 7206 ax 076A		 SS：SP为2000：000E

	mov ax,4c00h
	int 21h
	
codesg ends
end
```

- 生成1.exe文件步骤

mount d d:\dos

d:

1.编译 masm.exe 1.asm 生成 1.obj

2.连接 link.exe 1.obj 生成 1.exe

3.debug 1.exe

- 开始debug

未执行pop ax前 SS：SP 为2000：000A

ax 2000 bx 0000

`2000：0处数据为`

![image-20240814134224998](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408141342040.png)

执行pop ax 后SS：SP为2000：000C

ax  076A bx 0000

执行pop bx 后SS：SP为2000：000E

ax  076A bx 7206

执行push ax 后 SS：SP为2000：000C

ax 076A bx 7206

执行push bx 后 SS：SP为2000：000A

ax 076A bx 7206

`2000：0处数据为`

![image-20240814135518123](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408141355151.png)

发生了交换

然后

执行pop ax 后 SS：SP为2000：000C

ax 7206 bx 7206

执行pop bx后 SS：SP为2000：000E

ax 7206 ax 076A

![image-20240814135901700](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408141359743.png)

### 注意：

`push 命令后 SP寄存器的地址会减少2`

`pop 命令后 SP寄存器地址会增加2`

# 第五章



![image-20240814150742642](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408141507690.png)



```
assume cs:code
code segment
	mov ax 0ffffh
	mov ds,ax
	mov bx,0

	mov dx,0

	mov cx,0ch
	
s:	mov al,ds:[bx]
	mov ah,0
	add dx,ax
	inc bx
	loop s

	mov ax,4c00h
	int 12h
code ends
end
```

段地址寄存器：ds cs ss es

mov al,ds:[bx]中 `ds称为段前缀`

在dos下，有一段安全的空间 0：200~0：2ff（00200h~00ffh）256字节，该空间中没有系统或者其他程序的 数据 或者 代码



t5.8.asm 将内存ffff:0~ffff:b中的数据复制到0:200 ~ 0:200b中

0：200 ~ 0：20b   等同于  0020：0 ~ 0020：b

```assembly
assume cs:code 
code segment
	mov ax,0ffffh
	mov ds,ax
	
	mov ax,0020h
	mov es,ax
	
	mov bx,0
	
	mov cx,0bh
	
s: 	mov dl,ds:[bx]
	mov es:[bx],dl
	inc bx
	loop s
	
	mov ax,4c00h
	int 12h
code ends
end
```

## 实验4

