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

### 注意  dos命令 debug 命令：

dos命令

dir type cd

使用q命令退出Debug

注意p命令执行 int 21

debug中 

t，一条指令一条指令的执行

u，将cs段的地址翻译为汇编指令

p，跳出loop，或者跳到中断

g + IP；执行CS：IP处的指令，并打印出来

r，查看寄存器的值，或者修改

d，查看内存值

e，修改内存值

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

在dos下，有一段安全的空间 0：200~0：2ff（00200h~02ffh）(0020:0000~0020:00ff)256字节，该空间中没有系统或者其他程序的 数据 或者 代码



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

s4.2.asm

```assembly
assume cs:code 
code segment
	mov ax,0020h
	mov ds,ax
	mov bx,0
	mov cx,3Fh
	
s:	mov ds:[bx],bx
	inc bx
	loop s
	mov ax,4c00h
	int 12h
code ends
end
```

s4.3.asm

该段asm执行的地址是076A：0000

![image-20240814162858339](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408141628412.png)

```
assume cs:code 
code segment

	mov ax,076Ah		;3
	mov ds,ax			;2
	mov ax,0020h		;3
	mov es,ax			;2
	mov bx,0			;3
	mov cx,0018h		;3
	
s:	mov al,[bx]			;2
	mov es:[bx],al		;3
	inc bx				;1
	loop s 				;2
	mov ax,4c00h
	inc 12h
code ends
end
```

# 第六章

- 程序获取所需空间的方法

1.加载程序时分配的

2.程序执行过程中向系统申请的

- dw

dw命令，define word，使用dw定义2个字型数据（数据之间使用逗号分隔）

dw 0123h,0456h

```assembly
assume cs:code 
code segment
	dw 0123h,0456h,0789h,0abcdh,0defh,ofedh,ocbah,0987h
	start:	mov bx,0		;start程序入口标致
			mov ax,0
			
			mov cx,8
		s:	add ax,cs:[bx]
			add bx,2
			loop s
			
			mov ax,4c00h
			int 21h
code ends
end start	;	通知编译器程序结束，通知编译器程序的入口
			
```

## 检测点6.1

t61.asm

（1）依次用0:0 ~ 0:15单元中的内容改写程序中的数据

```assembly
assume cs:code 
code segment
	dw 0123h,0456h,0789h,0abcdh,0defh,ofedh,ocbah,0987h
	
start:
	mov ax,0
	mov ds,ax
	mov bx,0
	mov cx,8
s:	mov ax,[bx]
	mov cs:[bx],ax
	add bx,2
	loop s
	
	mov ax,4c00h
	int 21h
code ends
end start

```

（2）依次用内存0：0 ~ 0：15单元中的内容该写程序中的数据，数据传输用栈来进行

```
assume cs:code 
code segment
	dw 0123h,0456h,0789h,0abcdh,0defh,0fedh,0cbah,0987h
	dw 0,0,0,0,0,0,0,0,0,0
start:
	mov ax,cs
	mov ss,ax
	mov sp,32
	
	mov ax,0
	mov ds,ax
	mov bx,0
	mov cx,8
s:	push [bx]
	pop cs:[bx] 
	add bx,2
	loop s
	
	mov ax,4c00h
	int 21h
code ends
end start

```

- 6.3

数据段data、代码段code、栈stack

## 实验5

### s51.asm

```assembly
assume cs:code,ds:data,ss:stack
data segment
	dw 0123h,0456h,0789h,0abcdh,0defh,ofedh,ocbah,0987h
data ends
stack segment
	dw 0,0,0,0,0,0,0,0
stack ends

code segment
start:
	mov ax,stack
	mov ss,ax
	mov sp,16
	
	mov ax,data
	mov ds,ax
	
	push ds:[0]
	push ds:[2]
	pop ds:[2]
	pop ds:[0]
	
	mov ax,4c00h
	int 21h
code ends
end start
```

![image-20240815101037348](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408151010429.png)

cpu执行程序，程序返回前，data段中的数据是

23 01 56 04 89 07 CD AB EF 0D ED 0F BA 0C 87 09

  ![image-20240815101605031](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408151016068.png)

DS=075A 、CS=075D、SS=0769

程序加载后，data段地址为 076A ，stack段地址为 076B



### s52.asm

```assembly
assume cs:code,ds:data,ss:stack
data segment
	dw 0123h,0456h
data ends
stack segment
	dw 0,0
stack ends

code segment
start:
	mov ax,stack
	mov ss,ax
	mov sp,16
	
	mov ax,data
	mov ds,ax
	
	push ds:[0]
	push ds:[2]
	pop ds:[2]
	pop ds:[0]
	
	mov ax,4c00h
	int 21h
code ends
end start
```

cpu执行程序，程序返回前，data段中的数据是

23 01 56 04 

DS=075A 、CS=076C、SS=0769

程序加载后，data段地址为 076A ，stack段地址为 076B

DS=076A 、CS=076C、SS=076B

### s53.asm

```
assume cs:code,ds:data,ss:stack


code segment
start:
	mov ax,stack
	mov ss,ax
	mov sp,16
	
	mov ax,data
	mov ds,ax
	
	push ds:[0]
	push ds:[2]
	pop ds:[2]
	pop ds:[0]
	
	mov ax,4c00h
	int 21h
code ends
data segment
	dw 0123h,0456h
data ends
stack segment
	dw 0,0
stack ends
code ends
end start
```

cpu执行程序，程序返回前，data段中的数据是



DS=075A 、CS=076C、SS=0769

程序加载后，data段地址为 076A ，stack段地址为 076B

DS=076D 、CS=076A、SS=076E

s51.asm和s52.asm将最后一条伪指令“end start”改为 “end” 后不能正确执行，原因是无法找到程序入口

### s55.asm

编写code段中的代码，将a b段中的数据依次相加，结果存到c段中

```
assume cs:code 
a segment
	db 1,2,3,4,5,6,7,8
a ends
b segment
	db 1,2,3,4,5,6,7,8
b ends
c segment
	db 0,0,0,0,0,0,0,0
c ends
code segment
start:
	mov ax,a
	mov ds,ax
	mov ax,b
	mov es,ax
	mov ax,c
	mov ss,ax
	mov bx,0
	
	mov cx,8
s:	mov ax,0
	mov al,ds:[bx]
	add al,es:[bx]
	mov ss:[bx],al
	inc bx
	loop s
	
	mov ax,4c00h
	int 21h
code ends
end start
```

### s56.asm

用push指令将a段中的前8个字型数据，逆序存储到b段中

```assembly
assume cs:code 
a segment
	dw 1,2,3,4,5,6,7,8,9,0ah,0bh,0ch,0dh,0eh,0fh,0ffh
a ends
b segment
	dw 0,0,0,0,0,0,0,0
b ends
code segment
start:
	mov ax,a
	mov ds,ax
	mov ax,b
	mov ss,ax
	mov sp,16
	mov bx,0
	
	mov cx,8
s:	push ds:[bx]
	inc bx
	inc bx
	loop s
	
	mov ax,4c00h
	int 21h
code ends
end start
```



# 第七章

![image-20240816164850682](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161648751.png)

```assembly

mov ax,101b
and ax,111b
ax=101b
mov ax,110b
or ax,111b
ax=111b

字符大小写变化
and al,11011111B 第5位置为0，则为大写字符
or al,00100000B 第5位置为1，则为小写字符

si和di和bx功能相似的寄存器，si和di不能分成两个8为寄存器
可以通过[bx(si或者di) + idata]来指明一个内存单元，还可以通过[bx+di]和[bx+si]
形式如下：
mov ax,[bx+si]
或者
mov ax,[bx][si]
```

bx、si、idata访问内存单元格式如下

![image-20240816154932450](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161549496.png)

![image-20240816154629820](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161546932.png)



问题7.2

### q72.asm

```assembly
assume cs:codesg,ds:datasg
datasg segment
	db 'welcome to masm!'
	db '................'
datasg ends
codesg segment
start:
	mov ax,datasg
	mov ds,ax
	mov si,0
	
	mov cx,8
s:	mov ax,ds:[si]		;mov ax,0[si]
	mov ds:[si+16],ax	;mov 16[si],ax
	inc si
	inc si
	loop s
	
	mov ax,4c00h
	int 21h
codesg ends
end start

```



### q76.asm

将datasg段中每个单词的头一个字母变为大写字母

```
assume cs:codesg,ds:datasg
datasg segment
	db '1. file         '
	db '2. edit         '
	db '3. search       '
	db '4. view         '
	db '5. options      '
	db '6. help         '
datasg ends
codesg segment
start:
	mov ax,datasg
	mov ds,ax
	mov si,3
	mov bx,0
	
	mov cx,6
s:  mov al,ds:[bx+si]
	and al,11011111B
	mov ds:[bx+si],al
	add bx,16
	loop s
	
	mov ax,4c00h
	int 21h
codesg ends
end start
```

### q77.asm

将datasg段中每个单词变为大写字母

```assembly
assume cs:codesg,ds:datasg
datasg segment
	db 'ibm             '
	db 'dec             '
	db 'dos             '
	db 'vax             '
	dw 0 				;定义一个字，暂存cx
datasg ends
codesg segment
start:
	mov ax,datasg
	mov ds,ax
	mov bx,0
	
	mov cx,4
s0:	mov ds:[40h],cx
	mov si,0
	mov cx,3
	
s1:	mov al,ds:[bx+si]
	and al,11011111B
	mov ds:[bx+si],al
	inc si
	
	loop s1

	add bx,16
	mov cx,ds:[40h]
	loop s0
	
	mov ax,4c00h
	int 21h
codesg ends
end start
```

or

xx一般来说使用栈保存临时数据

```
assume cs:codesg,ds:datasg,ss:stack
datasg segment
	db 'ibm             '
	db 'dec             '
	db 'dos             '
	db 'vax             ' 
datasg ends
stack segment
	dw 0,0,0,0,0,0,0,0		;定义一个段，用来做栈段，容量为16个字节
stack ends
codesg segment
start:
	mov ax,stack
	mov ss,ax
	mov sp,16
	mov ax,datasg
	mov ds,ax
	mov bx,0

	
	mov cx,4
s0:	push cx
	mov si,0
	mov cx,3
	
s1:	mov al,ds:[bx+si]
	and al,11011111B
	mov ds:[bx+si],al
	inc si
	
	loop s1

	add bx,16
	pop cx
	loop s0
	
	mov ax,4c00h
	int 21h
codesg ends
end start
```

### q78.asm

将datasg段中每个单词的前4个字母改为大写字母

```
assume cs:codesg,ds:datasg,ss:stack
datasg segment
	db '1. display      '
	db '2. brows        '
	db '3. replace      '
	db '4. modify       '
datasg ends
stack segment
	dw 0,0,0,0,0,0,0,0		;定义一个段，用来做栈段，容量为16个字节
stack ends
codesg segment
start:
	mov ax,stack
	mov ss,ax
	mov sp,16
	mov ax,datasg
	mov ds,ax
	mov bx,0

	
	mov cx,4
s0:	push cx
	mov si,3
	mov cx,4
	
s1:	mov al,ds:[bx+si]
	and al,11011111B
	mov ds:[bx+si],al
	inc si
	
	loop s1

	add bx,16
	pop cx
	loop s0
	
	mov ax,4c00h
	int 21h
codesg ends
end start
```

## 实验6

​	done!

# 第八章

```
bx si di bp
只有如下指令是正确的
mov ax,[bx]
mov ax,[si]
mov ax,[di]
mov ax,[bp]
mov ax,[bx+si]
mov ax,[bx+di]
mov ax,[bp+si]
mov ax,[bp+di]
mov ax,[bx+si+idata]
mov ax,[bx+di+idata]
mov ax,[bp+si+idata]
mov ax,[bp+di+idata]
下面是错误的
mov ax,[bx+bp]
mov ax,[si+di]
注意：
只要在[..]中使用bp,而指令中没有显示的给出段地址,段地址默认在ss中
```

![image-20240816170526618](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161705676.png)

处理指令分为三类：读取、写入、运算

机器指令 `关心指令执行前一刻`

指令执行前，所要处理的数据可以在3个地方：cpu内部、内存、端口

例如：

![image-20240816170831215](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161708261.png)

### 数据位置的表达：

1.立即数（idata），汇编指令中直接给出

2.寄存器，汇编指令中给出相应的寄存器名

3.段地址（SA）和偏移地址（EA）

### 寻址方式：

当数据放在内存中的时候，可以通过多种方式给定内存单元的偏移地址，来定位内存单元的方法称为寻址方式

![image-20240816171216987](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161712047.png)

### 指令要处理数据的长度：

1.通过寄存器指明

```
mov ax,1	;ax指明了指令指向的内存单元是1字节
mov al,1	;al指明了指令指向的内存单元是一个字单元
```



2.通过ptr指明

例如

```
mov byte ptr ds:[0],1		;byte指明了指令指向的内存单元是1字节
mov word ptr ds:[1],0001h	;word指明了指令指向的内存单元是一个字单元
```

3.注意push 指令只进行`字`操作

### div指令：

div做除法，注意以下问题：

1.除数：有8位和16位，在一个reg或内存单元中

2.被除数：默认放在 ax 或者 ax 和 dx 中，如果除数为8位，被除数则为16位，默认放在ax中存放，
如果除数位16位，被除数则位32位，在dx和ax中存放，`dx存放高八位`，`ax存放低八位`

3.结果：如果除数为8位，则al存储除法操作的商，ah存储除法操作的余数；
如果除数为16为，则ax存储除法操作的商，dx存储除法操作的余数

![image-20240816172831661](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161728709.png)

![image-20240816172840314](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161728378.png)

eg:

利用除法指令计算100001/100

```assembly
100001 = 186A1H

mov dx,1	;高位数据
mov ax,86A1H	;低位数据
mov bx,100
div bx
结果：商 ax 03e8 余数 dx 1 
```

debug中的操作

```
rcs 
1000

rip
0

a 1000:0
mov dx,1
mov ax,86A1H
mov bx,100
div bx

t
t
t
t
```

![image-20240816174200435](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408161742493.png)



### db、dw、dd：

db 字节型数据

dw 字型数据

dd （double word）双字型数据

### dup：

dup是一个操作符，用于进行数据的重复

例如：

```assembly
db 3 dup('abc','ABC') ;定义了18个字节,它们是'abcABCabcABCabcABC'

stack segment
	db 200 dup(0) 	;定义了容量位200字节的栈段
stack ends
```





### q81.asm

用div 计算data段中第一个数据除以第二个数据的结果，商存在第三个数据的存储单元中

00 00 00 00 00 00 00 00

```assembly
assume cs:codesg,ds:datasg
datasg segment
	 dd 100001
	 dw 100
	 dw 0
datasg ends

codesg segment
start:
	mov ax,datasg
	mov ds,ax
	mov ax,ds:[0]	;低16位存在ax中
	mov dx,ds:[2]	;高16位存在dx中
	div word ptr ds:[4] ;用dx:ax中的32位数据除以ds:4字单元中的数据
	mov ds:[6],ax	;商将存在ds:6字单元中
	
	mov ax,4c00h
	int 21h
codesg ends
end start
```

## 实验7

t7.asm

```assembly
assume cs:codesg,es:table,ss:stack
data segment 
	db '1975','1976','1977','1978','1979','1980','1981','1983','1983'
	db '1984','1985','1986','1987','1988','1989','1990','1991','1992'
	db '1993','1994','1995'
	dd 16,22,382,1356,2390,8000,16000,24486,50064,97479,140417,197514
	dd 345980,590827,803530,1183000,1843000,2759000,3753000,4649000,5937000
	dw 3,7,9,13,28,38,130,220,476,778,1001,1442,2258,2793,4037,5635,8226
	dw 11542,14430,15257,17800
data ends
table segment 
	db 21 dup ('year summ ne ?? ')
table ends
stack segment
	db 8 dup (0)	
stack ends
codesg segment
start:
	mov ax,data
	mov ds,ax
	mov ax,table
	mov es,ax
	mov ax,stack
	mov ss,ax
	mov sp,16

	mov dx,0
	mov bx,0
	mov si,0
	mov di,0

	mov cx,21
s:	
	;复制年份
	mov ax,ds:[di]
	mov es:[bx],ax
	mov ax,ds:[di+2]
	mov es:[bx+2],ax
	
	;复制收入
	mov ax,ds:[di+84]
	mov es:[bx+5],ax
	mov dx,ds:[di+86] ;存储高字节数据
	mov es:[bx+7],dx
	;计算平均收入
	div word ptr es:[bx+10]
	mov es:[bx+13],ax

	;复制雇员数
	mov ax,ds:[si+168]
	mov es:[bx+10],ax

	add si,2 	;控制雇员数
	add di,4 	;控制收入、年份
	add bx,16 	;控制结构体数组成员
	loop s

	mov ax,4c00h
	int 21h

codesg ends
end start
```



# 第九章

### offset操作符

```
start:
	mov ax,offset start 	;相当于mov zx,0
	s: mov ax,offset s		;相当于mov ax,3 
	nop					   ;机器码占1字节
```



### jmp指令

jmp short 标号

这种格式的jmp指令实现的是段内段转移，对ip的修改范围位-128 ~ 127

cpu在执行jmp指令的时候不需要转移的目标地址

![image-20240819164945827](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408191649939.png)

jmp word ptr 内存单元地址，段内转移，存放的是转移的目的偏移量

jmp dword ptr 内存单元地址，段间转移，高地址是专业的目的段地址，低地址是转移的目标偏移量

## 检测点9.1

### t91.asm 

x

```
assume cs:code 
data segment
	dw 2 dup(0)
data ends
code segment
start:
	mov ax,data
	mov bx,ax
	mov bx,0
	mov [bx],0
	mov [bx+2],cs
	jump dword ptr ds:[0]
code ends
end start

```

## 检测点9.2

### t92.asm

x

```
assume cs:code 

code segment
start:
	mov ax,2000H
	mov ds,ax
	mov bx,0
s:  
	mov cl,[bx]
	mov ch,0
	mov cx,ds:[bx]
	jcxz ok
	inc bx
	jmp short s
ok:	
	mov dx,bx
	mov ax,4c00h
	int 21h
code ends
end start

```

## 检测点9.3

### t93

```
inc cx
```

### jcxz指令

**jcxz(**jmp if CX is zero**)有条件跳转指令，类似于段内短跳转jmp short，所能变化的ip范围同样为(-128~127)。**格式为 jcxz [标号]。**唯一的不同在于，只有当满足条件寄存器cx=0时，才会进行跳转，否则就和正常情况一样IP自增，按顺序执行下一条指令，这也是jcxz被称为有条件跳转指令的原因(只有满足条件才进行跳转)。**

**ax/bx/cx/dx并不是英文字母abcd的简称，而分别是accumulate-register累加寄存器、based-register基地址寄存器、count-register计数寄存器、data-register 数据寄存器。**　

**ax accumulate-register累加寄存器：**ax一般用于存放算术、逻辑运算中的操作数或结果。同时I/O指令也都需要使用ax与外设接口传递数据。

　　**bx based-register基地址寄存器 ：**bx一般用于存放访问内存时的地址。在8086内存寻址时，指定偏移地址时有提到过。

　　**cx count-register计数寄存器：**cx一般用于有条件跳转、循环、串操作指令。jcxz和loop循环等指令都依赖于cx寄存器。

　　**dx data-register 数据寄存器：**dx一般用于寄存器间接寻址中的I/O指令中存放I/O端口的地址。

### loop指令　　

循环指令同样依赖寄存器`cx`。**格式为loop [标号]**。loop指令的语义是，首先将cx自减1，如果cx不为0，则跳转至标号处。否则什么也不做，离开循环，顺序执行下移。循环指令的跳转范围和有条件跳转指令一样，ip的变化范围为(-128~127)。

### t93.asm

```
assume cs:code 

code segment
start:
	mov ax,2000H
	mov ds,ax
	mov bx,0
s:  
	mov cl,[bx]
	mov ch,0
	jcxz ok
	inc bx
	loop s
ok:	
	dec bx
	mov dx,bx
	
	mov ax,4c00h
	int 21h
code ends
end start

```

## 实验8

### s8.asm

```assembly
assume cs:codesg
codesg segment
	mov ax,4c00h			;3B
	int 21h					;2B

start:mov ax,0				;3B

s:	nop						;1B
	nop						;1B
	mov di,offset s			;3B		mov di,0008h offset s=3+2+3=8
	mov si,offset s2		;3B		mov si,0020h 0ffset s2=3+2+3+1+1+3+3+3+3+2+3+2+3=32
	mov ax,cs:[si]			;3B
	mov cs:[di],ax			;3B		将s2的标记处指令 即内存中的机器码 复制到s处 -> s2：处的值为 F6EB ， s：F6EB -> jmp -10 

s0:	jmp short s				;2B		跳到s处 执行 jmp -10 

s1:	mov ax,0				;3B
	int 21h					;2B
	mov ax,0				;3B
	
s2: jmp short s1			; F6EB -> jmp -10
	nop						
codesg ends
end start
	
```

![image-20240820101946129](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408201019204.png)



di 0008
si 0020

mov ax,cs:[si]	; ax = cs:0020该地址处内存单元的内容为F6EB

mov cs:[di],ax	;将F6EB复制到 CS：0008处也就是  将s2处的jmp short s1的机器码复制到 s处

```
即：
s:
	nop
	nop
修改为如下：
s: jmp short s1		该处机器码为：F6EB   F6为偏移量
实际上 是 jmp的偏移量  jmp F6  -> jmp 1111 0110
F6的原码为 1000 1010 即 -10
偏移量-10个字节

```

![image-20240820102713664](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408201027723.png)

然后跳转到程序的开通，即jmp 0000，可以正常结束程序



### 补码与原码

```
在计算机系统中，数值一律用补码来表示（存储）。

已知一个数值，求补码的操作分两种情况：
（1）正数的补码：与原码相同。
      例如，+9的补码是00001001。
（2）负数的补码：符号位为1，其余位为该数绝对值的原码按位取反；然后整个数加1。
      例如，-7的补码：因为是负数，则符号位为“1”,整个为10000111；其余7位为-7的绝对值+7的原码
     0000111按位取反为1111000；再加1，所以-7的补码是11111001。
     
已知一个数的补码，求原码的操作分两种情况：
（1）如果补码的符号位为“0”，表示是一个正数，所以补码就是该数的原码。
（2）如果补码的符号位为“1”，表示是一个负数，求原码的操作可以是：符号位为1，其余各位取反，然后再整个数加1。
     例如，已知一个补码为11111001，则原码是10000111（-7）：因为符号位为“1”，表示是一个负数，所以该位不变，仍为   “1”；其余7位1111001取反后为0000110；再加1，所以是10000111。
```

## 实验九

要求 `在屏幕中间分别显示绿色、绿底红色、白底蓝色的字符串 'welcome to masm!'`
7  6 5 4 3 2 1 0
BL R G B I R G B
闪烁 背景 高亮 前景
红底绿字, 01000010B

绿色：
00100010B
绿底红色：
00100100B
白底蓝色：
00000001B

80x25彩色字符模式显示缓冲区： 80列25行
1.内存地址 B8000H~BFFFFH 32K空间
2.共8页 第0页B8000H~B8F9FH 4000个字节
3.一行中，一个字符占两个字节的存储空间，`低位字节存储ascii码，高位字节存储字符的属性`,共80个字符，占160字节
4.一页中的显示缓冲区 第1行偏移000~09F 80个字符共160个字节 第2行偏移0A0~13F 。。。 第25行偏移F00~F9F
5.一行中，00~01对应第一列，9E~9F对应第80列

### s9.asm

```
assume cs:codesg,ds:data

data segment
	db 'welcome to masm!'
	db 02h,24h,71h
data ends
codesg segment
start:			 

	mov ax,data
	mov ds,ax

	mov ax,0b800h	
	mov es,ax		

	mov bx,0
	mov si,07c0h	; 80*2*12 + 32*2 从第十二行 32个字符开始
	mov di,12h

	mov cx,16
 s:
 	mov al,ds:[bx]		;低位字符
 	mov es:[si],al
 	inc si
 	mov al,ds:[di]	 	;高位字符颜色属性
 	mov es:[si],al
 	inc bx
 	inc si
 	loop s
	 
	mov ax,4c00h			;3B
	int 21h					;2B
codesg ends
end start
	
```

![image-20240820140415877](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408201404952.png)

### 检测点10.1

实现从1000：0000处开始执行指令

```
assume cs:code,ss:stack
stack segment
	db 16 dup (0)
stack ends
code segment
start:	
	mov ax,stack
	mov ss,ax
	mov sp,16
	mov ax,1000h
	push ax	;cs
	mov ax,0000h
	push ax	;ip
	retf
code end
end start
```

