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

ascii字母表

![image-20240829162511552](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408291625605.png)

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

### t7.asm

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



# 第九章 转移指令原理

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

jmp short 标号

jmp near 标号

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

### xs8.asm

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

## 实验9

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

# 第十章call和ret指令

## ret和ref

ret指令使用栈中的数据，修改IP的内容

retf指令使用栈中的数据，修改cs和ip的内容

cpu执行**ret**指令时

（IP）=（（SS）*16 + （SP））
（SP）= （SP）+ 2

相当于 

- pop ip

cpu执行**retf**指令时

（IP）=（（SS）*16 + （SP））
（SP）= （SP）+ 2

（CS）=（（SS）*16 + （SP））
（SP）= （SP）+ 2

相当于：

 pop IP

pop CS

## call指令

cpu执行call指令时

- 1.将IP或者CS和IP压入栈中

- 2.转移

call不能实现短转移，

cpu执行call指令时

- （SP）=（SP）- 2

（（SS）*16+（SP））= （IP）

- （IP）=（IP）+16位位于

相当于：

push IP

jmp near ptr 标号

## 转移的目的地址在指令中的call指令

`call far ptr 标号`实现的是段间转移

- 1.(sp)=(sp)-2
  ((ss)`*`16+(sp))=(CS)
  (sp)=(sp)-2
  ((ss)`*`16+(sp))=(IP)

- 2.(CS)=标号所在段的段地址
  （IP)=标号所在段中的偏移地址

相当于：

push CS
push IP
jump far ptr 标号

## 转移地址在寄存器中的call指令

`call 16位reg`

(sp)=(sp)-2
((ss)*16+(sp))=ip
(IP)=(16位reg)

相当于：

push IP

jmp 16位的reg

## 转移地址在内存中的call指令

- call word ptr 内存单元地址

相当于

push IP

jmp word ptr 内存单元地址

```assembly
mov sp,10h
mov ax,0123h
mov ds:[0],ax
call word ptr ds:[0]	;push IP -> ip=0123h,sp=sp-2 -> sp=0eh ,jmp near ip
```



- call dword ptr 内存单元地址

push CS

pus IP

jmp dword ptr 内存单元地址

```assembly
mov sp,10h
mov ax,0123h
mov ds:[0],ax
mov word ptr ds:[2],0
call dword ptr ds:[0]	; push CS ,push IP, jmp far cs:ip
cs=0,ip=0123h,sp=0ch

```

## call和ret配合使用

```
call 标号1
	mov ax,4c00h
	int 21h ;程序结束

标号1：
	指令
	call 标号2
	ret
	
标号2：
	指令
	ret
```

先执行标号1，再执行标号2，标号2ret，标号1ret

## mul指令

mul乘法指令

（1）两个相乘的数：两个相乘的数要么是8位，要么都是16位，如果是8位，默认放在al中，另一个放在8位reg或者内存字节单元中；如果是16位，一个默认在ax中，另一个放在16位reg或者内存字单元中

（2）结果：如果是8位乘法，结果默认放在ax中；如果是16位乘法，结果高位默认在dx中存放，低位在ax中

例如：

100*10；100小于255，可用8位乘法

```assembly
mov al,100
mov bl,10
mul bl
;结果存放在ax中 ax=1000（03E8H）
```

计算100*10000

需要用16位乘法

```assembly
mov ax,100
mov bx,10000
mul bx
;结果低位存放在ax中，高位存放在dx中 ax=4240H dx=000Fh			(f4240h = 1000000)
```



## 检测点10.1 retf

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
	push ax				;cs 
	mov ax,0000h
	push ax				;ip 
	retf			;先获取ip 000h，再获取 cs 1000h 再跳转
code end
end start
```

## x检查点10.2 call 

x下面程序执行后,ax中的数值为多少？ `6`

```
内存地址	 		机器码 			汇编指令
1000：0			b8 00 00 			mov ax,0	;3B
1000：3			e8 01 00			call s		;3B 	类似于push IP -> ip 3+3=6 ;jmp near s
1000：6			40				    inc ax	 	;1B
1000：7			58				    s: pop ax 	;
```



## 检测点10.3 call far ptr

下面程序执行后,ax中的数值为多少？ `1010h`

```assembly
内存地址            机器码             汇编指令
1000:0			b8 00 00		mov ax,0
1000:3			9a 09 00 00 10   call far ptr s	;push CS -> cs=1000h,push IP -> ip=3+5=8,jump far ptr s
1000:8			40 				inc ax			;未执行
1000:9			58				s:pop ax		;ax=8
								add ax,ax		;ax=16
								pop bx			;bx=1000h
								add ax,bx		;ax = 1000h + 10h

```

## 检测点10.4 call reg

下面程序执行后,ax中的数值为多少？	`11`

```assembly
内存地址            机器码             汇编指令
1000:0			b8 06 00			mov ax,6		;3B		ax=6
1000:3			ff d0				call ax			;2B 	push IP-> ip =3+2=5,jump near ax -> jmp 6
1000:5			40					inc ax			;1B		未执行
1000:6								mov bp,sp		;		bp=sp -> sp=sp+2
									add ax,[bp]		; ax+ss:sp	6+5
```



## 检测点10.5  call word

(1)下面程序执行后，ax中的数值是多少？ 3

```assembly
assume cs:code
stack segment
	dw 8 dup(0)
stack ends
code segment
start:
	mov ax,stack	;3B
	mov ss,ax		;2B
	mov sp,16		;3B
	mov ds,ax		;3B 
	mov ax,0		;3B ax 0
	call word ptr ds:[0eh] 	;push IP -> ip位于inc ax处的ip ,jmp ip
					;ss:0eh   inc处的ip
					;ds=ss:[0] ss:0eh
	inc ax			;执行 ax 1
	inc ax			;执行 ax 2
	inc ax			;执行 ax 3
	
	mov ax,4c00h
	int 21h
code ends
end start
```



(2)下面程序执行后，ax和bx中的数值是多少？

ax 1,bx 0

```assembly
assume cs:code
data segment
	dw 8 dup(0)
data ends
code segment
start:
	mov ax,data							 
	mov ss,ax							 
	mov sp,16							 
	mov word ptr ss:[0],offset s		   
	mov ss:[2],cs						 
	call dword ptr ss:[0] 				 
	nop								     
s:
	mov ax,offset s						 
	sub ax,ss:[0ch]		;s.ip - nop.ip = 1		
	mov bx,cs							
	sub bx,ss:[0eh]						
	mov ax,4c00h
	int 21h
code ends
end start
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408201654704.jpeg)









### q101.asm

```assembly
assume cs:code 
code segment
start:
	mov ax,1
	mov cx,3
	call s		;push ip=mov.bx.ax.ip , jmp s.ip
	mov bx,ax	;(bx) 8
	mov ax,4c00h
	int 21h
s:
	add ax,ax	;执行3次 2 4 8 ax=8
	loop s		;cx 为0后执行ret
	ret			;pop ip, jmp mov.bx.ax.ip ,跳转至mov bx,ax
code ends
end start
```







### q1011.asm

计算data段中的第一组数据的3次方，结果放在后面一组dword单元中

```assembly
assume cs:code 
data segment
	dw 1,2,3,4,5,6,7,8
	dd 8 dup(0)
data ends
code segment
start:
	mov ax,data
	mov ds,ax
	mov di,16
	mov si,0
	
	mov cx,8
s:
	mov bx,ds:[si]
	call cube
	mov ds:[di],ax
	mov ds:[di+2],dx
	add di,2
	add si,2
	loop s
	
    mov ax,4c00h
	int 21h
	
cube:
	mov ax,bx
	mul bx
	mul bx
	ret
code ends
end start
```



## 实验10

编写子程序

名称：show_str

功能：在指定的位置，用指定的颜色，显示一个用0结束的字符串

参数：(dh) =行号（取值范围0~24），(dl)=列号（取值范围0~79） (cl)=颜色，ds:si指向字符串的首地址

返回：无

### s101.asm    x

```assembly
assume cs:code
data segment 
	db 'Welcome to masm!',0
	db 'Welcome to masm!',0
data ends


code segment
start:
	mov dh,8
	mov dl,3
	mov cl,2
	mov ax,data
	mov ds,ax
	mov si,0
	call show_str

	mov dh,10
	mov dl,3
	mov cl,5
	mov ax,data
	mov ds,ax
	mov si,17
	call show_str
	
	mov ax,4c00h
	int 21h
;功能 在屏幕 指定位置 用指定颜色 显示一个以0结尾的字符串
;参数 dh行号 dl列号 cl颜色 ds：si指向字符串的首地址
show_str:
	push ax 
	push bx
	push es
	push si
	
	mov ax,0b800h	
	mov es,ax	
 
		
	mov ax,2  	;获取偏移量 保存到bx中  bx = 2*dl + 160*dh 
	mul dl
	mov bx,ax	;bx保存 dl*2的值
	mov ax,160
	mul dh
	add bx,ax	;bx保存160*dh+ax的值

	mov al,cl
	mov cl,0

	change:
	mov ch,ds:[si]
	jcxz ok

 	mov es:[bx],ch
 	mov es:[bx+1],al
 	add bx,2
 	inc si
	jmp short change
	
	ok:
	pop si
	pop es
	pop bx 
	pop ax
	ret

code ends
end start
```

### s102.asm x

名称：divdw

功能：进行不会产生溢出的除法运算，被除数为dword型，除数为word型，结果为dword型

参数：(ax)=dword型数据的低16位  (dx)=dword型数据的高16位  (cx)=除数

返回(dx)=结果的高16位  (ax)-结果的低16位  (cx)=余数

公式：

X：被除数，范围：[0，FFFFFFFF]

N：除数，范围，[0，FFFF]

H：X高16位，范围[0，FFFF]

L：X低16位，范围[0，FFFF]

int（）：描述性运算符，取商，eg，int（38/10）= 3

rem（）：描述性运算符，取余数，eg，rem（38/10）= 8

X/N = int(H/N)`*`65536 + [rem(H/N)`*`65536+L]/N

eg:计算1000000/10 （f4240h/0ah）=186A0h

```assembly
assume cs:code

code segment
start:
	mov ax,4240h	;L
	mov dx,000fh	;H
	mov cx,0ah		;N
	call divdw
	
	mov cx,4c00h
	int 21h
;使用div时 做被除数 ax 低位 dx 高位,做结果时,ax 商 dx 余数 
;int(H/N)*65536 + [rem(H/N)*65536+L]/N
;参数：(ax)=dword型数据的低16位 (dx)=dword型数据的高16位 (cx)=除数
;返回：(dx)=结果的高16位 (ax)=结果的低16位 (cx)=余数
divdw:
	push ax		;保存被除数低位 L
	mov ax,dx 	;把被除数高位H赋给ax  相当于先计算H/N
	mov dx,0	;dx 清零
	div cx		;H/N,结果 ax 商 ,dx 余数 
	mov bx,ax 	;保存商 bx = ax int(H/N)
	pop ax		;取出L 余数做高位rem(H/N)*65536存放在dx中，而上一步中的dx就是余数 低位ax=L
	div cx		;[rem(H/N)*65536+L]/N
	mov cx,dx	;保存余数结果
	mov dx,bx 	;结果高位商 ，ax低位商
	ret

code ends
end start
```

div eg:

```
100001 = 186A1H

mov ax,86A1H	;低位数据
mov dx,1	;高位数据
mov bx,100
div bx
结果：商 ax 03e8 余数 dx 1 
```

### s103.asm

名称：dtoc

功能：将word型数据转变为表示十进制的字符串，字符串以0为结尾符

参数：(ax)=word 型数据  ds:si 指向字符串的首地址

应用举例：编程，将数据1266以十进制的形式在屏幕的8行3列，用绿色显示出来，显示时我们调用本次实验中的第一个子程序show_str

```assembly
assume cs:code
data segment
	db 256 dup(0)
data ends
code segment 
start:
    mov ax,data
    mov ds,ax

	mov ax,1
	mov si,10
	call dotc_word
	
	mov dh,9
	mov dl,3
	mov cl,2
	call show_str

	mov ax,12636
	mov si,25
	call dotc_word
	
	mov dh,10
	mov dl,3
	mov cl,2
	call show_str
	
	mov ax,4c00h
	int 21h
;功能：将word型数据转变为表示十进制的字符串，字符串以0为结尾符。
;参数：(ax)=word型数据 ds:si指向字符串的首地址
dotc_word:
	push dx
	push cx
	push bx
	push si
	push di

	mov di,si 	;保存字符串首地址，后面字符串逆序需要
	dotc_word_div_loops:
	mov dx,0 		;作被除数时 高位dx 为0
	mov bx,10 		;10 作除数
	div bx			; ax/10 结果：ax 商 dx 余数
	add dx,30h		;数字转字符
	mov ds:[si],dx
	inc si
	mov cx,ax	
	jcxz dotc_word_ok	;判断商为0时结束 即 cx == 0 
	jmp short dotc_word_div_loops
	
	dotc_word_ok:
	mov cl,0
	mov ds:[si],cl	;字符串结尾为0    需要逆序

	mov dx,0
	push si 		;保存si 字符串最高位下标
	sub si,di       ;字符串长度=最高位下标-初始首地址

	mov cx,si
	sub cx,1 		;只有一个数字字符 直接退出不需要逆序   	
	jcxz dotc_word_str_ok

	mov ax,si
	mov bx,2
	div bx 			;循环次数=字符串长度/2  
	mov cx,ax
	pop si 			;取出si 字符串最高位0下标
	dec si			;字符串最高位下标

	dotc_word_str_reverse:

	mov al,ds:[di]		;交换
	mov ah,ds:[si]
	mov ds:[si],al
	mov ds:[di],ah
	inc di
	dec si
	dec cx
	jcxz dotc_word_str_reverse_ok 
	jmp short dotc_word_str_reverse

	dotc_word_str_ok:
	pop si 	
	dotc_word_str_reverse_ok:
	pop di
	pop si
	pop bx
	pop cx
	pop dx
	ret

;功能 在屏幕 指定位置 用指定颜色 显示一个以0结尾的字符串
;参数 dh行号 dl列号 cl颜色 ds ： si 指向字符串的首地址
show_str:
	push ax 
	push bx
	push es
	push si

	mov ax,0b800h	
	mov es,ax	
 	
	mov ax,2  	;获取偏移量 保存到bx中  bx = 2*dl + 160*dh 
	mul dl
	mov bx,ax	;bx保存 dl*2的值
	mov ax,160
	mul dh
	add bx,ax	;bx保存160*dh+ax的值

	mov al,cl
	mov cl,0

	show_str_loops:
	mov ch,ds:[si]
	jcxz show_str_ok

 	mov es:[bx],ch
 	mov es:[bx+1],al
 	add bx,2
 	inc si

	jmp short show_str_loops
	
	show_str_ok:
	pop si
	pop es
	pop bx 
	pop ax
	ret

code ends
end start
```





## 课程设计1

将dword型数转变为十进制的数据有些已经大于65535，一个编写一个新的数据到字符串转化的子程序，完成dword型数据到字符串的转化

名称：dtoc

功能：将dword型数转变为表示十进制数的字符串，字符串以0为结尾符合

参数：（ax）= dword 型数据的低16位 （dx）= dword型数据的高16位，ds：si指向字符串的首地址

返回：无

### dotc2.asm

```assembly
assume cs:code
data segment
	db 24 dup(0)
data ends
code segment 
start:
	mov ax,0ffffh
	mov dx,0ffffh
	mov bx,data
	mov ds,bx
	mov si,0
	call dotc_dword
	
	mov dh,8
	mov dl,3
	mov cl,2
	call show_str

	mov ax,0ffffh
	mov dx,0ff1fh
	mov si,10
	call dotc_dword
	
	mov dh,9
	mov dl,3
	mov cl,8
	call show_str

	mov ax,4c00h
	int 21h

dotc_dword:
	push dx
	push cx
	push bx
	push si
	push di

	mov di,si 	;保存字符串首地址，后面字符串逆序需要
	dotc_dword_div_loops:

	mov cx,10 	;除数 10
	call divdw 	;dx ax / 10 结果：商dx ax 余数 cx
	add cx,30h	;余数cx数字转字符
	mov ds:[si],cx
	inc si
	mov cx,ax	 	;低16位ax
	jcxz dotc_dword_ok	;判断商为0时结束 即 cx == 0 
	jmp short dotc_dword_div_loops
	
	dotc_dword_ok:
	mov cl,0
	mov ds:[si],cl	;字符串结尾为0    data中的数据 需要逆序


	mov dx,0
	push si 		;保存si 字符串最高位0下标
	sub si,di       ;字符串长度=最高位下标-初始首地址

	mov cx,si
	sub cx,1 		;只有一个数字字符 直接退出不需要逆序   	
	jcxz dotc_dword_str_ok

	mov ax,si
	mov bx,2
	div bx 			;循环次数=字符串长度/2  
	mov cx,ax 		;循环次数
	pop si 			;取出si 字符串最高位0下标
	dec si			;字符串最高位下标

	dotc_dword_str_reverse:

	mov al,ds:[di]		;交换
	mov ah,ds:[si]
	mov ds:[si],al
	mov ds:[di],ah
	inc di
	dec si
	dec cx
	jcxz dotc_dword_str_reverse_ok 
	jmp short dotc_dword_str_reverse

	dotc_dword_str_ok:
	pop si 
	dotc_dword_str_reverse_ok:
	pop di
	pop si
	pop bx
	pop cx
	pop dx
	ret


;功能 在屏幕 指定位置 用指定颜色 显示一个以0结尾的字符串
;参数 dh行号 dl列号 cl颜色 ds：si指向字符串的首地址
show_str:
	push ax 
	push bx
	push es
	push si
	
	mov ax,0b800h	
	mov es,ax	
 
	mov ax,2  	;获取偏移量 保存到bx中  bx = 2*dl + 160*dh 
	mul dl
	mov bx,ax	;bx保存 dl*2的值
	mov ax,160
	mul dh
	add bx,ax	;bx保存160*dh+ax的值

	mov al,cl
	mov cl,0

	change:
	mov ch,ds:[si]
	jcxz show_str_ok

 	mov es:[bx],ch
 	mov es:[bx+1],al
 	add bx,2
 	inc si
	jmp short change
	
	show_str_ok:
	pop si
	pop es
	pop bx 
	pop ax
	ret
	
;使用div时 做被除数 ax 低位 dx 高位,做结果时,ax 商 dx 余数 
;int(H/N)*65536 + [rem(H/N)*65536+L]/N
;参数：(ax)=dword型数据的低16位 (dx)=dword型数据的高16位 (cx)=除数
;返回：(dx)=结果的高16位 (ax)=结果的低16位 (cx)=余数
divdw:
	push bx
	push ax		;保存被除数低位 L
	mov ax,dx 	;把被除数高位H赋给ax  相当于先计算H/N
	mov dx,0	;dx 清零
	div cx		;H/N,结果 ax 商 ,dx 余数 
	mov bx,ax 	;保存商 bx = ax int(H/N)
	pop ax		;取出L 余数做高位rem(H/N)*65536存放在dx中，而上一步中的dx就是余数 低位ax=L
	div cx		;[rem(H/N)*65536+L]/N
	mov cx,dx	;保存余数结果
	mov dx,bx 	;结果高位商 ，ax低位商
	pop bx
	ret
	
code ends
end start
```



![image-20240821180837440](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408211808712.png)

### kcsj1.asm 

```assembly
assume cs:codesg
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
	db 256 dup (0)	
stack ends
screen_table_data segment
    db 512 dup (0) ;4个字节年份+ 0 +8个字节总收入+ 0 +4个字节雇员+ 0 +4个字节人均收入+ 0 = 24
    						; 0-3,0,5-13,0,15-18,0,20-23,0
screen_table_data ends
codesg segment
start:
	mov ax,data
	mov ds,ax
	mov ax,table
	mov es,ax
	mov ax,stack
	mov ss,ax
	mov sp,256

	mov dx,0
	mov bx,0
	mov si,0
	mov di,0
	mov bp,0
	mov cx,21
s:	
	;复制年份
	mov ax,ds:[di]
	mov es:[bx],ax
	mov ax,ds:[di+2]
	mov es:[bx+2],ax
	
	;复制雇员数
	mov ax,ds:[si+168]
	mov es:[bx+10],ax

	;复制总收入
	mov ax,ds:[di+84]
	mov es:[bx+5],ax
	mov dx,ds:[di+86] ;存储高字节数据
	mov es:[bx+7],dx

	;计算平均收入
	push cx
	mov cx,es:[bx+10] 
	call divdw 		 ;收入/人数
	pop cx
	mov es:[bx+13],ax

	add si,2 	;控制雇员数
	add di,4 	;控制收入、年份
	add bx,16 	;控制结构体数组成员
	loop s

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
	mov dx,0
	mov bx,0
	mov si,0
	mov bp,0
	mov di,2 	;从第二行开始 
    mov ax,screen_table_data
    mov ds,ax 	;将 screen_table_data 段地址赋值给ds

    mov cx,21
show_loops:
	push cx
    ;从table中复制年份
    mov si,bp
	mov ax,es:[bx]
	mov ds:[bp],ax
	mov ax,es:[bx+2]
	mov ds:[bp+2],ax
	;显示年份
	mov ax,di ;控制行数
	mov dh,al
	mov dl,1

	mov cl,2
	call show_str

    ;从table中获取总收入 dword 型数据然后转字符串
    mov ax,es:[bx+5]
    mov dx,es:[bx+7]
    add si,5 		;si向后偏移5
  	call dotc_dword ;转字符串
  	;显示总收入
  	mov ax,di ;控制行数
  	mov dh,al
	mov dl,11
	mov cl,2
	call show_str

	;将雇员 word 型数据转字符串
    mov ax,es:[bx+10] ;取出雇员数据
    add si,9 		;si向后偏移9
    call dotc_word 	;转字符串
    ;显示将雇员数
	mov ax,di ;控制行数
  	mov dh,al
	mov dl,21
	mov cl,2
	call show_str

  	;将平均收入 word 型数据转字符串  
    mov ax,es:[bx+13]
    add si,5 	   	;si向后偏移5
    call dotc_word 	;转字符串
    ;显示平均收入
    mov ax,di ;控制行数
  	mov dh,al
	mov dl,31
	mov cl,2
	call show_str

	inc di 		;控制行数
	add bx,16 	;控制table中的偏移
	add bp,24 	;控制screen_table_data中的偏移量
	pop cx
	loop show_loops

	mov ax,4c00h
	int 21h	



;将dword数据变为表示十进制的字符串，字符串以0为结尾符合
;参数：（ax）= dword型数据的低16位 （dx）=dword型数据的高16位 di：si指向字符串的首地址
dotc_dword:
	push ax
	push bx
	push cx
	push dx
	push si
	push di

	mov di,si 	;保存字符串首地址，后面字符串逆序需要
	dotc_dword_div_loops:

	mov cx,10 	;除数 10
	call divdw 	;dx ax / 10 结果：商dx ax 余数 cx
	add cx,30h	;余数cx数字转字符
	mov ds:[si],cx
	inc si
	mov cx,ax	 	;低16位ax
	jcxz dotc_dword_ok	;判断商为0时结束 即 cx == 0 
	jmp short dotc_dword_div_loops
	
	dotc_dword_ok:
	mov cl,0
	mov ds:[si],cl	;字符串结尾为0    data中的数据为66621 需要逆序

	mov dx,0
	push si 		;保存si 字符串最高位0下标
	sub si,di       ;字符串长度=最高位下标-初始首地址
	mov cx,si
	sub cx,1 		;只有一个数字字符 直接退出不需要逆序   	
	jcxz dotc_dword_str_ok
	mov ax,si
	mov bx,2
	div bx 			;循环次数=字符串长度/2  
	mov cx,ax 		;循环次数
	pop si 			;取出si 字符串最高位0下标
	dec si			;字符串最高位下标

	dotc_dword_str_reverse:

	mov al,ds:[di]		;交换
	mov ah,ds:[si]
	mov ds:[si],al
	mov ds:[di],ah
	inc di
	dec si
	dec cx
	jcxz dotc_dword_str_reverse_ok 
	jmp short dotc_dword_str_reverse
	dotc_dword_str_ok:
 	pop si 
	dotc_dword_str_reverse_ok:
	pop di
	pop si
	pop dx
	pop cx
	pop bx
	pop ax
	ret


;功能：将word型数据转变为表示十进制的字符串，字符串以0为结尾符。
;参数：(ax)=word型数据 ds:si指向字符串的首地址
dotc_word:
	push ax
	push bx
	push cx
	push dx
	push si
	push di

	mov di,si 	;保存字符串首地址，后面字符串逆序需要
dotc_word_div_loops:
	mov dx,0 		;作被除数时 高位dx 为0
	mov bx,10d 		;10 作除数  d表示十进制
	div bx			; ax/10 结果：ax 商 dx 余数
	add dx,30h		;数字转字符
	mov ds:[si],dx
	inc si
	mov cx,ax	   		;商赋值给cx
	jcxz dotc_word_ok	;判断商为0时结束 即 cx == 0 
	jmp short dotc_word_div_loops
	
dotc_word_ok:
	mov cl,0
	mov ds:[si],cl	;字符串结尾为0    data中的数据为66621 需要逆序

	mov dx,0
	push si 		;保存si 字符串最高位下标
	sub si,di       ;字符串长度=最高位下标-初始首地址

	mov cx,si
	sub cx,1     	;只有一个数字字符 直接跳出循环
	jcxz dotc_word_str_ok

	mov ax,si
	mov bx,2
	div bx 			;循环次数=字符串长度/2  
	mov cx,ax
	pop si 			;取出si 字符串最高位0下标
	dec si			;字符串最高位下标

dotc_word_str_reverse:
	mov al,ds:[di]		;交换
	mov ah,ds:[si]
	mov ds:[si],al
	mov ds:[di],ah
	inc di
	dec si
	dec cx
	jcxz dotc_word_str_reverse_ok 
	jmp short dotc_word_str_reverse
dotc_word_str_ok:
	pop si 	
dotc_word_str_reverse_ok:
	pop di
	pop si
	pop ax
	pop cx
	pop bx
	pop dx
	ret


;功能 在屏幕 指定位置 用指定颜色 显示一个以0结尾的字符串
;参数 dh行号 dl列号 cl颜色 ds：si指向字符串的首地址
show_str:
	push ax 
	push bx
	push cx
	push es
	push si

	mov ax,0b800h	
	mov es,ax	
 
	mov ax,2  	;获取偏移量 保存到bx中  bx = 2*dl + 160*dh 
	mul dl
	mov bx,ax	;bx保存 dl*2的值
	mov ax,160
	mul dh
	add bx,ax	;bx保存160*dh+ax的值

	mov al,cl
	mov cl,0

	change:
	mov ch,ds:[si]
	jcxz show_str_ok

 	mov es:[bx],ch
 	mov es:[bx+1],al
 	add bx,2
 	inc si
	jmp short change
	
	show_str_ok:
	pop si
	pop es
	pop cx
	pop bx 
	pop ax
	ret
	
;使用div时 做被除数 ax 低位 dx 高位,做结果时,ax 商 dx 余数 
;int(H/N)*65536 + [rem(H/N)*65536+L]/N
;参数：(ax)=dword型数据的低16位 (dx)=dword型数据的高16位 (cx)=除数
;返回：(dx)=结果的高16位 (ax)=结果的低16位 (cx)=余数
divdw:
	push bx
	push ax		;保存被除数低位 L
	mov ax,dx 	;把被除数高位H赋给ax  相当于先计算H/N
	mov dx,0	;dx 清零
	div cx		;H/N,结果 ax 商 ,dx 余数 
	mov bx,ax 	;保存商 bx = ax int(H/N)
	pop ax		;取出L 余数做高位rem(H/N)*65536存放在dx中，而上一步中的dx就是余数 低位ax=L
	div cx		;[rem(H/N)*65536+L]/N
	mov cx,dx	;保存余数结果
	mov dx,bx 	;结果高位商 ，ax低位商
	pop bx
	ret


codesg ends
end start
```



![image-20240826111750601](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408261117739.png)

# 第十一章标志寄存器

8086cpu内部的寄存器中，有一个16位`flag`标志寄存器（`存储的信息`通常被称为程序状态字`PSW`）

![image-20240826112301820](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408261123933.png)

## 1.ZF标志——零标志位

ZF是flag的第6位，零标志位，记录相关指令执行后，其结果是否为0

eg：

```assembly
mov ax,1
sub ax,1
;执行后结果为0 zf=1
mov ax,2
sub ax,1
;执行后结果为1 zf=0
```



## 2.PF标志——奇偶标志位

PF是flag的第2位，奇偶标志位，记录相关指令执行后，其结果的所有bit位中1的个数是否为偶数。如果1的个数为偶数，pf=1；如果为奇数，pf=0

eg：

```assembly
mov al,1
add al,10
;执行后 结果00001011B 其中有3（奇数）个1，则pf=0
mov al,1
or al,2
;执行后 结果00000011B其中有2 （偶数）个1，则pf=1
```

## 3.SF标志——符号标志位

SF是flag的低7位，符号标志位，记录相关指令执行后，其结果是否为负。如果结果为负，sf=1；如果非负，sf=0

eg：

```assembly
00000001B 	;可以看作无符号数 1 ，或者有符号数+1
10000001B	;可以看作无符号数129，也可以看作有符合-127
```

### 注意：

​	指令的执行会影响标志寄存器的指令有 **add、sub、mul、div、inc、or、and**

​	指令执行对标志寄存器没有影响的指令有 **mov、push、pop**

## 检测点11.1

写出下面每条指令执行后，ZF、PF、SF等标志位的值

sub al,al		;ZF=1 PF=1 SF=0	;结果0b 1 1 0 0为0个1

mov al,1		;ZF=1 PF=1 SF=0 	;不影响

push ax		;ZF=1 PF=1 SF=0 	;不影响

pop bx			;ZF=1PF=1 SF=0	;不影响

add al,bl		;ZF=0 PF=0 SF=0  	;1+1 2 10b

add al,10		;ZF=0 PF=1 SF=0 	;12  1100b  	010

mul al			 ;ZF=0 PF=1 SF=0 	;12*12=144  1001 0000    010



## 4.CF标志——进位标志

flag的第0位是CF，进位标志，在进行`无符号数`运算时，它记录了运算结果的最高有效位向更高的进位值，或从更高的借位值

eg：

```assembly
mov al,98H
add al,al		;执行后(al)=30h cf=1 cf记录了从最高有效位向更高的进位值 98h+98h = 0001 0011 0000 超过8位需要进位, (al)=00110000=30h cf=1
add al,al		;执行后(al)=60h cf=0 cf记录了从最高有效位向更高的进位值

mov al,97h
sub al,98h		;执行后; al=ffh cf=1	
sub al,al		;执行后 al=0 cf=0
```

## 5.OF标志——溢出标志位

flag的第11位是OF，溢出标志位，OF记录了有`符号数运算的结果`是否发生了溢出。如果发生了溢出，OF=1，如果没有,OF=0

8位有符号数据所能表示的范围：`-128~127`

1000 0000 ~ 0111 1111

16位的有符号数据所能表示的范围：`-32768~32767`

## 检查点11.2 

写出下面每条指令执行后，ZF、PF、SF、CF、OF等标志位的值

```assembly
				CF		OF		SF		ZF		PF
sub al,al 		 0		 0		 0	 	 1 		 1
mov al,10h	 	 0 		 0		 0		 1		 1
add al,90h 		 0		 0 		 1 		 0 		 1  
mov al,80h 		 0 	  	 0 		 1        0        1
add al,80h        1       1       0        1        1
mov al,0FCh       1       1       0        1        1
add al,05h        1       0       0        0         0          fc+5=101 0001 0000 0001                                                   
mov al,7dh       1        0       0        0         0
add al,0bh       0        1       1        0         1 ;7d+b 有符号 125+11=136 溢出 无符号 =88h，无进位，结果al最高位为1表示负数
```

## 6.adc指令

adc是带进位的加法指令，利用cf位上记录的进位值

```assembly
adc ax,bx  ;实现的是(ax)=(ax)+(bx)+CF
类似于
add al,bl
adc ah,bh
```

eg

```assembly
sub ax,ax		; CF=0
mov ax,1
add ax,ax
adc ax,3 		;(ax)=(ax)+3+CF=2+3+0
```

## 7.sbb指令

sbb是带借位减法指令，它利用了CF位上记录的借位值

```assembly
sbb ax,bx ;实现的功能是： (ax)=(ax)-(bx)-CF	
```

eg:

计算003E1000H-00202000H结果放在ax bx中

```assembly
mov bx,1000H
mov ax,003EH
sub bx,2000H
sbb ax,0020H
```

## 8.cmp命令

cmp是比较命令，cmp的功能相当于减法指令，只是结果保存在标志寄存器中

从标志位的值来看比较结果

```assembly
cmp ax,bx
```

![image-20240829144702308](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408291447464.png)

![image-20240829144714211](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408291447264.png)

eg:

```
mov ax,8
mov bx,3
cmp ax,bx   ;(ax)=8, zf=0, pf=1, sf=0, cf=0, of=0
```

## 9.检测比较结果的条件转移指令

![image-20240829145133526](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408291451620.png)

## 检测点11.3 x

统计F000：0处32个字节中，大小在[32，128]的数据的个数

```assembly
    mov ax,0f000h
    mov ds,ax

    mov bx,0
    mov dx,0
    mov cx,32
s:
	mov al,[bx]
	cmp al,32
	jb s0 		;小于
	cmp al,128
	ja s0 		;大于
	inc dx  	;保存结果
s0:
	inc bx
	loop s
```

统计F000：0处32个字节中，大小在(32,128)的数据的个数

```assembly
    mov ax,0f000h
    mov ds,ax

    mov bx,0
    mov dx,0
    mov cx,32
s:
	mov al,[bx]
	cmp al,32
	jna s0 		;小于等于
	cmp al,128
	jnb s0 		;大于等于
	inc dx
s0:
	inc bx
	loop s
```

## 10.DF标志和串传送指令

flag 的第10位是DF，方向标志位。在串处理指令中，控制每次操作后si、di的增减。

df=0 每次si、di递增

df=1 每次si、di递减

```assembly
movsb 	;功能是将ds:si指向的内存单元中的字节送入es:di中,根据df位判断si和di递增1 或递减1
;((es)*16+(di))=((ds)*16+(si))
;如果df = 0, (si)=(si)+1, (di)=(di)+1
;如果df = 1, (si)=(si)-1, (di)=(di)-1

movsw ;功能是将ds:si指向的内存单元中的字送入es:di中,根据df位判断si和di递增2 或递减2


rep movsb 
;相当于
s: 	movsb
	loop s
;都需要根据cx来控制次数
```

`cld`指令；将标志位寄存器的`df位`置`0`

`std`指令；将标志位寄存器的`df位`置`1`

eg:

将data段中的第一个字符串复制到它后面的空间中

```assembly
data segment
	db 'Welcome to asm!'
	db 16 dup(0)
data ends

start:
	mov ax,data
	mov ds,ax
	mov si,0
	mov es,ax
	mov di,16
	mov cx,16    ;rep 循环16次
	cld 		;设置df=0 正向传输
	rep movsb
	
end start
```

## 11.pushf和popf

pushf的功能是将`标志寄存器`的值压栈，而popf是将标志寄存器的值弹出数据，`送入标志寄存器`

## 检测点11.4

下面程序执行后：(ax)=?

0000 of df if tf sf zf 0 af 0 pf 0 cf

```assembly
mov ax,0
push ax
popf
mov ax,0fff0h ;ax fff0h
add ax,0010h  ;fff0 + 0010 = 1 0000
pushf
pop ax ;of=1, df=0, if=0, sf=0, zf=1, af=0, pf=1, cf=1
	   ;0000100 01000101
and al,11000101B
and ah,00001000B
;ax=01000101B = 69 = 45h
```

## 12.标志寄存器在Debug中的表示

![image-20240829155135764](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408291551831.png)

## 实验11 编写子程序小写变大写

### t11.asm

编写一个子程序，包含任意字符，以0结尾的字符串中的小写字母变成大写字母，描述如下。

名称：letterc

功能：将以0结尾的字符串中的小写字母变成大写字母

参数：ds:si指向的字符串首地址

```assembly
assume cs:codesg
datasg segment
	db "Beginning's All-purpose Symbolic Instruction Code.",0
datasg ends
codesg segment
begin:
	mov ax,datasg
	mov ds,ax
	mov si,0
	call letterc
	
	mov ax,4c00h
	int 21h
;将以0结尾的字符串中的小写字母变成大写字母
;参数ds:si指向的字符串首地址
letterc:
	push ax
	push bx
	push cx
	push dx
	push si
	pushf
	
	mov ax,0
	push ax
	popf 
	mov si,0
	mov cl,0
letterc_loops:
	mov ch,ds:[si]
	jcxz letterc_loops_ok
	;如果ch是小写字符,[97,122]
	cmp ch,61h
	jb s 			;小于
	cmp ch,7ah
	ja s 			;大于
	and ch,11011111B ;将第5位置为0，则为大写字符
	mov ds:[si],ch
s:
	inc si
	jmp short letterc_loops
letterc_loops_ok:	
	popf
	pop si
	pop dx
	pop cx
	pop bx
	pop ax
	ret

codesg ends
end begin
```

![image-20240829191505430](https://cdn.jsdelivr.net/gh/yyheroi/yyheroi_blog_img_resource@main/images/202408291915513.png)

# 第十二章内中断

中断向量表存放在：0000：0000到0000：03FF的1024个单元中

## 检测点12.1

（1）用debug查看内存，情况如下

`0000：0000`  `68 10 A7 00 8B 01 70 00-16 00 9D 03 8B 01 70 00`

则3号中断源对应的中断处理程序的入口地址为：`0070：018B`

（2）存储N号中断源对应的中断处理程序入口的偏移地址的内存单元的地址为：`N*4`

存储N号中断源对应的中断处理程序入口的段地址的内存单元的地址为：`4N+2`



## 中断过程

- 取出中断类型码N
- pushf
- TF=0，IF=0
- push CS
- push IP
- (IP)=(N*4)

中断处理程序的编写方法：

- 保存用到的寄存器
- 处理中断
- 恢复用到的寄存器
- 用iret指令返回

iret指令的功能用汇编语法描述为：

pop IP

pop CS

popf

iret通常和硬件自动完成的中断过程配合实验，寄存器入栈的顺序是标志寄存器、CS、IP，而iret的出栈顺序是IP、CS、标志寄存器，刚好和其相对应，实现了用执行中断处理程序前的cpu现场恢复标志寄存器和CS、IP的工作。iret指令执行后，cpu回到执行中断处理程序前的执行点继续执行程序

```
mov ax,1000h
mov bh,1
div bh

溢出 1/ax 产生0号中断，0号中断处理程序是在屏幕中间显示"Divide overflow"
```

## do0.asm

```assembly
assume cs:code 
code segment
start:
	mov ax,cs
	mov ds,ax
	mov si,offset do0
	mov ax,0
	mov es,ax
	mov di,200h
	;安装do0 至0000：0200
	mov cx,offset do0end-offset do0
	cld
	rep movsb

	;设置中断向量
	mov ax,0
	mov es,ax
	mov word ptr es:[0*4],200h	;ip
	mov word ptr es:[0*4+2],0 	;cs
	
	;测试被除数为0 
	mov ax,2
	mov dl,0
	div dl

	mov ax,4c00h
	int 21h
	
do0: 
	jmp short do0start
	db "overflow!"
do0start:
	mov ax,cs
	mov ds,ax
	mov si,202h
	
	mov ax,0b800h
	mov es,ax
	mov di,12*160+36*2
	
	mov cx,9
s:
	mov al,ds:[si]
	mov es:[di],al
	inc si
	add di,2
	loop s
	
	mov ax,4c00h
	int 21h
do0end:
	nop
code ends
end start
```



## 实验12 编写0号中断的处理程序

s12.asm

```assembly
assume cs:code 
code segment
start:
	mov ax,cs
	mov ds,ax
	mov si,offset do0
	mov ax,0
	mov es,ax
	mov di,200h
	;安装do0 至0000：0200
	mov cx,offset do0end-offset do0
	cld
	rep movsb

	;设置0号中断向量
	mov ax,0
	mov es,ax
	mov word ptr es:[0*4],200h	;ip
	mov word ptr es:[0*4+2],0 	;cs
	
	;测试结果溢出 跳到0000：0000 0号中断获取 cs：ip
	mov ax,100h
	mov bh,1
	div bh

	mov ax,4c00h
	int 21h
	
do0: 
	jmp short do0start
	db "divide error!"
do0start:
	mov ax,cs  	  ;获取 0000：0002中的cs值 也就是 0
	mov ds,ax
	mov si,202h
	
	mov ax,0b800h
	mov es,ax
	mov di,12*160+36*2
	
	mov cx,13 ;字符串长度
s:
	mov al,ds:[si]
	mov es:[di],al
	inc si
	add di,2
	loop s
	
	mov ax,4c00h
	int 21h
do0end:
	nop
code ends
end start
```

# 第十三章

## 检测点 13.1

t131.asm  x

```assembly
assume cs:code 
code segment
start:
	mov ax,cs
	mov ds,ax
	mov si,offset lp
	mov ax,0
	mov es,ax
	mov di,200h
	;安装7ch中断例程 至0000：0200
	mov cx,offset lpend-offset lp
	cld
	rep movsb
	;设置7ch中断向量
	mov ax,0
	mov es,ax
	mov word ptr es:[124*4],200h	;ip
	mov word ptr es:[124*4+2],0 	;cs
		
	mov ax,0b800h
	mov es,ax
	mov di,160*12
	
	mov bx,offset s-offset se
	mov cx,80
s:	
	mov byte ptr es:[di],'!'
	add di,2
	int 7ch
se: 
	nop
	
	mov ax,4c00h
	int 21h

;7ch中断例程
lp:	
	push bp
	mov bp,sp
	dec cx
	jcxz lpret
	add ss:[bp+2],bx
lpret:
	pop bp
	iret
lpend:
	nop
	
code ends
end start
```

(1)在上面的内容中，我们用 7ch 中断例程实现 loop 的功能，则上面的 7ch 中断例
程所能进行的最大转移位移是多少? `-32768-32767 `
(2)用 7ch 中断例程完成 jmp near ptrs指令的功能，用 bx 向中断例程传送转移位移。
应用举例:在屏幕的第 12 行，显示 data 段中以0结尾的字符串

t132.asm x

```assembly
assume cs:code
data segment
	db 'conversation',0
data ends
code segment
start:
	mov ax,cs
	mov ds,ax
	mov si,offset lp
	mov ax,0
	mov es,ax
	mov di,200h
	;安装7ch中断例程 至0000：0200
	mov cx,offset lpend-offset lp
	cld
	rep movsb
	;设置7ch中断向量
	mov ax,0
	mov es,ax
	mov word ptr es:[124*4],200h	;ip
	mov word ptr es:[124*4+2],0 	;cs
		
	mov ax,data
	mov ds,ax
	mov si,0
	mov ax,0b800h
	mov es,ax
	mov di,12*160
s:
	cmp byte ptr [si],0
	je ok
	mov al,[si]
	mov es:[di],al
	inc si
	add di,2
	mov bx,offset s-offset ok	;设置bx为转移位置=标号ok至标号s的转移位移
	int 7ch 					;功能相当于： jmp near ptr s
ok:	
	mov ax,4c00h
	int 21h

;7ch中断例程
lp:	
	push bp
	mov bp,sp
	add ss:[bp+2],bx
	pop bp
	iret
lpend:
	nop

code ends
end start	
```

## 检测点13.2

判断下面说法的正误:

1.我们可以编程改变FFFF：0 处的指令，使得CPU不去执行BIOS中的硬件系统检测和初始化程序

  `错误，该内存区域为BIOS的ROM，可读不可写`

2.int 19h中断例程，可以由DOS提供 

 `错误，int19h 不是由 DOS 提供的，因为在 DOS 装入中断例程前 int19h 已存在于内存中，int19h恰恰是为了引导操作系统的安装，在此之前操作系统还未建立不可用。`



d136.asm

编程：在屏幕的第五行12列显示3个红底高亮闪烁绿色的"a"

```assembly
assume cs:code
code segment 

	mov ah,2			;置光标
	mov bh,0			;第0页
	mov dh,5			;dh中放行号
	mov dl,12			;dl中放列号
	int 10h
	
	mov ah,9			;在光标位置中显示字符
	mov al,'a' 			;字符
	mov bl,11001010b	 ;颜色属性
	mov bh,0			;第0页
	mov cx,3			;字符重复个数
	int 10h
	
	mov ax,4c00h
	int 21h
code ends
end
```

## 实验13 编写、应用中断例程

(1)编写并安装 int 7ch 中断例程，功能为显示一个用0结束的字符串，中断例程安装在 0:200 处。
参数:(dh)=行号，(dl)=列号，(cl)=颜色，ds:si指向字符串首地址。
以上中断例程安装成功后，对下面的程序进行单步跟踪，尤其注意观察int、iret 指令执行前后 CS、IP 和栈中的状态。

#### s131.asm

```assembly
assume cs:code
data segment
	db "welcome to masm!",0
data ends
code segment
start:
	call install_lp7
	
	mov dh,10
	mov dl,10
	mov cl,2
	mov ax,data
	mov ds,ax
	mov si,0
	int 7ch

	mov ax,4c00h
	int 21h

lp7:
	push ax 
	push bx
	push cx
	push es
	push si

	mov ax,0b800h
	mov es,ax

	mov ax,2  	;获取偏移量 保存到bx中  bx = 2*dl + 160*dh 
	mul dl
	mov bx,ax	;bx保存 dl*2的值
	mov ax,160
	mul dh
	add bx,ax	;bx保存160*dh+ax的值

	mov al,cl
	mov cl,0
change:
	mov ch,ds:[si]
	jcxz show_str_ok
 	mov es:[bx],ch
 	mov es:[bx+1],al
 	add bx,2
 	inc si
	jmp short change

show_str_ok:
	pop si
	pop es
	pop cx
	pop bx 
	pop ax
	iret
lp7end:
	nop

install_lp7:
	push ax
	push si
	push es
	push cx

	mov ax,cs
	mov ds,ax
	mov si,offset lp7
	mov ax,0
	mov es,ax
	mov di,200h
	;安装7ch中断例程 至0000：0200
	mov cx,offset lp7end-offset lp7
	cld
	rep movsb
	;设置7ch中断向量
	mov ax,0
	mov es,ax
	mov word ptr es:[124*4],200h	;ip
	mov word ptr es:[124*4+2],0 	;cs

	pop cx
	pop es
	pop si
	pop ax
	ret

code ends
end start
```

(2)编写并安装 int 7ch 中断例程，功能为完成 loop 指令的功能。
参数:(cx)=循环次数，(bx)=位移。

以上中断例程安装成功后，对下面的程序进行单步跟踪，尤其注意观察int、iret 指令执行前后 CS、IP 和栈中的状态。
在屏幕中间显示 80个“!”

#### s132.asm

```assembly
assume cs:code
code segment
start:
	
	call install_lp7
	mov ax,0b800h
	mov es,ax
	mov di,160*12
	mov bx,offset s-offset se 	;设置从标号se到s的转移位移
	mov cx,80
s:
	mov byte ptr es:[di],'!'
	add di,2
	int 7ch 			;如果(cx) != 0 转移到标号s处
se:
	nop
	mov ax,4c00h
	int 21h

lp7:
	push bp
	mov bp,sp
	dec cx
	jcxz lpret
	add ss:[bp+2],bx
lpret:
	pop bp
	iret
lp7end:
	nop


install_lp7:
	push ax
	push si
	push es
	push cx

	mov ax,cs
	mov ds,ax
	mov si,offset lp7
	mov ax,0
	mov es,ax
	mov di,200h
	;安装7ch中断例程 至0000：0200
	mov cx,offset lp7end-offset lp7
	cld
	rep movsb
	;设置7ch中断向量
	mov ax,0
	mov es,ax
	mov word ptr es:[124*4],200h	;ip
	mov word ptr es:[124*4+2],0 	;cs

	pop cx
	pop es
	pop si
	pop ax
	ret
code ends
end start
```



(3)下面的程序，分别在屏幕的第 2、4、6、8 行显示4句英文诗，补全程序。

#### s133.asm

```assembly
assume cs:code
code segment
	s1:		db 'Good,better,best,','$'
	s2:		db 'Never let it reset,','$'
	s3:		db 'Till good is better,','$'
	s4:		db 'And better,best.','$'
	s:		dw offset s1,offset s2,offset s3,offset s4
    row:	db  2,4,6,8
    
start:
	mov ax,cs
	mov ds,ax
	mov bx,offset s
	mov si,offset row
	mov cx,4
ok:	
	mov bh,0 	;第0页
	mov dh,ds:[si] 	;dh中存放行号
	mov dl,0	;dl中存放列
	mov ah,2	;10h号中断的2号子程序，
	int 10h
	
	mov dx,ds:[bx] 	;指向需要显示的字符串，以$作为结束符
	mov ah,9		;功能号9，表示在光标位置显示字符串
	int 21h
	
	add bx,2
	inc si
	
	loop ok
	mov ax,4c00h
	int 21h
code ends
end start
	
```

# 第十四章

## 检测点14.1

![image-20240923114111839](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240923114111839.png)

（1）编程，读取CMOS RAM的2号单元的内容

```assembly
mov al,2
out 70h,al 	;往70端口写入数据2  70端口是地址位
in al,71h	;从71端口读入数据
```

（2）编程，向CMOS RAM的2号单元写入0

```assembly
mov al,2
out 70h,al 	;往70端口写入数据2  70端口是地址位
mov al,0  	;
out 71,al	;往71端口写入0  71端口是数据位
```

## 检测点14.2

编程，用加法和移位指令计算 （ax）=（ax）*10

（ax）`*`10=（ax）`*`2+（ax）`*`8

```assembly
mov ax,10
mov cl,2
shl ax,cl
mov bx,ax
mov ax,10
mov cl,8
shl ax,cl
add ax,bx
```



## 实验14 访问CMOS RAM

编程，以“年/月/日 时：分：秒”的格式，显示当前的日期、时间

s14.asm

```assembly
assume cs:code
data segment 
    db '2024/09/23 00:00:00','$'
data ends

code segment
start:
    mov ax,data
    mov es,ax

    call get_hms_func
    call get_ymd_func

	mov dh,12 	;dh中存放行号
	mov dl,24	;dl中存放列
    mov bh,0 	;第0页
	mov ah,2	;10h号中断的2号子程序
	int 10h
	
    mov ax,data
    mov ds,ax
	mov dx,0 	  ;指向需要显示的字符串，以$作为结束符
	mov ah,9	  ;功能号9，表示在光标位置显示字符串
	int 21h

    mov ax,4c00h
    int 21h


get_hms_func:    
    mov dl,0
    mov di,17
    mov cx,3
    push cx
    push bx
    push dx
    push di
    push ax
get_hms: 
    mov al,dl
    out 70h,al
    in al,71h

    mov ah,al
    push cx
    mov cl,4
    shr ah,cl
    pop cx
    and al,00001111b
    
    add ah,30h
    add al,30h
    mov byte ptr es:[di],ah
    mov byte ptr es:[di+1],al

    sub di,3
    add dl,2
    loop get_hms

get_hms_ret:
    pop ax
    pop di
    pop dx
    pop bx
    pop cx
    ret

get_ymd_func:
    mov di,8
    mov dl,7
    mov cx,3

    push cx
    push bx
    push dx
    push di
    push ax
get_ymd:
    mov al,dl
    out 70h,al
    in al,71h

    mov ah,al
    push cx
    mov cl,4
    shr ah,cl
    pop cx
    and al,00001111b
    add ah,30h
    add al,30h
    mov byte ptr es:[di],ah
    mov byte ptr es:[di+1],al
    
    sub di,3
    inc dl
    loop get_ymd
get_ymd_ret:
    pop ax
    pop di
    pop dx
    pop bx
    pop cx
    ret

   
code ends
end start

```



# 第十五章

## 15.2可屏蔽中断

中断触发过程

1.取出中断类型码n

2.标志寄存器入栈，IF=0，TF=0

3.CS、IP入栈

4.（IP)=(n*4),(CS)=(n`*`4+2)

在这个过程中IF置为0的原因是，禁止其他的可屏蔽中断

sti ，设置IF=1

cli，设置IF=0

## 15.4编写int 9 中断例程

### 中断过程

> 取得中断类型码N
> pushf
> TF=0 IF=0
> push CS
> push IP
> （IP）=（N`*`4） （CS）=（N`*`4+2）

### iret指令

>pop IP
>pop CS
>popf



键盘输入的处理过程

1.键盘产生扫描码
2.扫描码送人60h端口
3.引发9号中断
4.cpu指向int 9中断例程处理键盘输入(1，2，3步都是由硬件完成)

`sbb ax,bx ;实现的功能是： (ax)=(ax)-(bx)-CF; CF是借位`

循环100000h次

```assembly
	mov dx,10h
	mov ax,0
s:	
	sub ax,1
	sbb dx,0 ;dx=dx-0-CF dx减去10次,ax产生的借位CF; 即循环100000
	cmp ax,0 ;低位ax不等于0继续循环
	jne s
	cmp dx,0 ;高位dx不等于0继续循环
	jne s
```



循环显示a-z字符的程序如下：

#### s1504.asm

```assembly
assume cs:code
stack segment
	db 128 dup(0)
stack segment

start: 
	mov ax,stack
	mov ss,ax
	mov sp,128
	
	mov ax,0b800h
	mov es,ax
	mov ah,'a'
s:
	mov es:[160*12+40*2],ah
	call delay
	inc ah
	cmp ah,'z'
	jna s 	;比z小则循环
	
	mov ax,4c00h
	int 21h
	
delay:
	push ax
	push dx
	mov dx,10h  ;循环100000h次
	mov ax,0
s1:
	sub ax,1
	sbb dx,0
	cmp ax,0
	jne s1
	cmp dx,0
	jne s1
	pop dx
	pop ax
	ret
code ends
end start
	
```





### int9中断例程 

#### s15041.asm

显示a - z，按下Esc键后，改变显示颜色

键盘输入达到69h后就会引发9号中断，cpu则去执行int 9中断例程

编写int 9中断例程，功能如下

1.从60h端口中读出键盘的输入
2.调用BIOS的int 9 中断例程，处理其他硬件细节
3.判断是否为Esc的扫描码，如果是，改变显示的颜色后返回；如果不是则直接返回

```assembly
assume cs:code

stack segment 
	db 128 dup(0)
stack ends

data segment
	dw 0,0
data ends

code segment
start:
	mov ax,stack
	mov ss,ax
	mov sp,128
	
	mov ax,data
	mov ds,ax
	
	mov ax,0
	mov es,ax
	
	push es:[9*4]
	pop ds:[0]
	push es:[9*4+2]
	pop ds:[2] ;将原来的中断例程的入口地址保存到ds:0 ds:2单元中
	
	mov word ptr es:[9*4],offset int9
	mov es:[9*4+2],cs	;在中断向量表中设置新的int 9中断例程的入栈地址
	
	mov ax,0b800h
	mov es,ax
	mov ah,'a'
s:
	mov es:[160*12+40*2],ah
	call delay
	inc ah
	cmp ah,'z'
	jna s
	
	mov ax,0
	mov es,ax
	
	push ds:[0]
	pop es:[9*4]	;将中断向量表中int 9中断例程的入口恢复为原来的入口地址
	push ds:[2]
	pop es:[9*4+2]
	
	mov ax,4c00h
	int 21h

delay:
	push ax
	push dx
	mov dx,5h
	mov ax,0
s1:
	sub ax,1
	sbb dx,0
	cmp ax,0
	jne s1
	cmp dx,0
	jne s1
	pop dx
	pop ax
	ret

int9: 
	push ax
	push bx
	push es
	
	in al,60h

	pushf
	pushf
	pop bx
	and bh,11111100b 	
	push bx
	popf
	call dword ptr ds:[0] 	;对int指令进行模拟，调用原来的int 9中断例程
	
	cmp al,1 	;Esc扫描码
	jne int9ret
	
	mov ax,0b800h
	mov es,ax
	inc byte ptr es:[160*12+40*2+1] ;将属性值加1，改变颜色

int9ret:
	pop es 
	pop bx
	pop ax
	iret
	
code ends
end start
```

## 检测点15.1

1.进入中断例程后IF和TF都已经置0，没有必要进行设置了，对于以下程序：

```assembly
	pushf
	pushf
	pop bx
	and bh,11111100b 	
	push bx
	popf
	call dword ptr ds:[0] 
```

可以精简为：

```assembly
pushf
call dword ptr ds:[0] 
```

2.s15041.asm中主程序有什么潜在的问题？

在主程序中，如果执行设置int 9中断例程的段地址的指令之间发生了键盘中断，则CPU将转去一个错误的地址执行，将发生错误，修改如下

添加cli 和sti暂时屏蔽中断

```assembly
	cli
	mov word ptr es:[9*4],offset int9
	mov es:[9*4+2],cs	;在中断向量表中设置新的int 9中断例程的入栈地址
	sti
```

## 15.5安装新的int 9中断例程

任务：安装一个新的int 9中断例程

功能：在dos下，按F1键后改变当前屏幕的显示颜色，其他键照常处理

1.改变屏幕的显示颜色

```assembly
    mov ax,0b800h
    mov es,ax
    mov bx,1
    mov cx,2000
s:	
	inc byte ptr es:[bx]
	add bx,2
	loop s
```

2.其他键照常处理

可以调用原int 9中断处理程序，来处理其他键盘的输入

3.原int 9中断例程入口地址保存

因为在新编写的int 9中断例程中要调用原int 9中断例程，所以，要保存原int 9中断，将地址保存在0：200单元处

4.新int 9中断例程的安装

新的int 9中断例程安装在0：204处

#### s1505.asm

```assembly
assume cs:code

stack segment
	db 128 dup(0)
stack ends

code segment
start:
	mov ax,stack
	mov ss,ax
	mov sp,128
	
	push cs
	pop ds
	
	mov ax,0
	mov es,ax
	
	mov si,offset int9		;设置ds:si执向源地址
	mov di,204h				;设置es:di指向目的地址
	mov cx,offset int9end-offset int9 ;设置cx为传输长度
	cld						;设置传输方向为正
	rep movsb				
	
	push es:[9*4]
	pop es:[200h]
	push es:[9*4+2]
	pop es:[202h]
	
	cli
	mov word ptr es:[9*4],204h
	mov word ptr es:[9*4+2],0h
	sti
	
	mov ax,4c00h
	int 21h

int9:
	push ax
	push bx
	push cx
	push es
	
	in al,60h		;读取端口60h
	
	pushf
	call dword ptr cs:[200h]	;当此中断例程执行时（CS）= 0
	
	cmp al,3bh
	jne int9ret
	
	mov ax,0b800h
	mov es,ax
	mov bx,1
	mov cx,2000
s:
	inc byte ptr es:[bx]
	add bx,2
	loop s
int9ret:
	pop es
	pop cx
	pop bx
	pop ax
	iret
int9end:
	nop
	
code ends
end start
```





## 实验15 安装新的int 9中断例程

功能：在dos下，按下 A键后，除非不再松开，如果松开，就显示满屏幕的"A"，其他键照常处理

按下一个键时产生的扫描码称为通码，松开一个键产生的扫描码称为断码。断码 = 通码 + 80h

键盘扫描码，键盘码，15.3

![image-20241022111334692](.\imgs\image-20241022111334692.png)

A键的通码：1E
A键的断码：9E

注意：调用BIOS的中断例程时应该用“[dword](https://so.csdn.net/so/search?q=dword&spm=1001.2101.3001.7020) ptr”而不是“word ptr”。

### s15.asm

```assembly
assume cs:code

stack segment
	db 128 dup(0)
stack ends

code segment
start:
	mov ax,stack
	mov ss,ax
	mov sp,128

	push cs
	pop ds

	mov ax,0
	mov es,ax

	mov si,offset int9
	mov di,204h
	mov cx,offset int9end-offset int9
	cld
	rep movsb 	;将ds:si 拷贝到 es:di 长度cx

	push es:[9*4]  		;保存原int9地址
	pop es:[200h]
	push es:[9*4+2]
	pop es:[202h]
	cli
	mov word ptr es:[9*4],204h  ;新的int9中断例程地址修改到中断向量表中
	mov word ptr es:[9*4+2],0h
	sti

	mov ax,4c00h
	int 21h

int9:
	push ax
	push bx
	push cx
	push es

	in al,60h ;从60H端口接收键盘输入

	pushf ;将标志寄存器压入栈,下面模拟调用原int9例程
	call dword ptr cs:[200h]  ;中断执行时cs为0 0:200H处存放BIOS的9号中断例程的入口地址

	cmp al,9Eh 			;A的通码为1EH，断码为9EH
	jne int9ret 		;若按下的是A键，则显示满屏的A

	mov ax,0b800h
	mov es,ax
	mov bx,0
	mov cx,2000   		;一屏能显示2000个字符
s:
	mov byte ptr es:[bx],'A'
	add bx,2
	loop s

int9ret: 				;程序返回
	pop es
	pop cx
	pop bx
	pop ax
	iret
int9end:
	nop

code ends
end start

```



# 第十六章 直接定址表

### 检测点16.1

下面的程序将code段中a处的8个数据累加，结果存储到b处的双字节中，补全程序

```assembly
assume cs:code
code segment
	a dw 1,2,3,4,5,6,7,8
	b dd 0
start:
	mov si,0
	mov cx,8
s:	
	mov ax,a[si]
	add word ptr b[0],ax
	adc word ptr b[2],0
	add si,2
	loop s
	
	mov ax,4c00h
	int 21h

code ends
end start
```



## 16.2在其他段中使用数据标号

### assume

通常我们不在`代码段`中定义数据。
通常在`数据段`定义数据，为了在`代码段`中直接使用`数据标号`访问数据，
我们需要为`编译器`使用伪指令`assume`将`标号所在段`与一个`段寄存器`关联起来。
（这个关联是给`编译器`看的。`DS`还是需要我们自己设置）

### 通过标号取地址

可以将标号当作数据用，此时，编译器视其地址为值。
是取 偏移地址 还是 偏移地址和段地址 取决于数据的类型：

- 偏移地址
  C的类型为 dw 字，就只取偏移地址

```assembly
data segment
	a db 1,2,3,4,5,6,7,8
	b dw 0
	c dw a,b	; 相当于：c dw offset a offset b
data ends
```

- 偏移地址 + 段地址
  C的类型为 dd 双字，就要取偏移地址和段地址

```assembly
data segment
	a db 1,2,3,4,5,6,7,8
	b dw 0
	c dd a,b	; 相当于：c dw offset a,seg a,offset b,seg b
data ends
```



### 检测点16.2

下面的程序将data段中a处的8个数据累加，结果存储到b处的字中

分析

只需要设置一下`数据段`地址即可。注意 `assum es:data` 关联的是 `es`

```assembly
assume cs:code,es:data
data segment
	a db 1,2,3,4,5,6,7,8
	b dw 0
data ends
code segment
start:
	mov ax,data
	mov es,ax
	mov si,0
	mov cx,8
s:
	mov al,a[si]
	mov ah,0
	add b,ax
	inc si
	loop s
	
	mov ax,4c00h
	int 21h
code ends
end start
```



## 16.3 直接定址表（Direct Addressing Table）

在8086汇编语言中，直接定址表通常用于存储一系列数据，并允许程序通过索引直接访问表中的特定元素。
是常用的空间换时间算法，适用于键的数量相对固定且不会频繁变化的情况。

直接定址表的好处包括：

> 快速访问：由于键直接映射到地址，因此访问速度非常快，几乎没有延迟。
> 简单性：实现直接定址表的算法相对简单，容易理解和维护。
> 预分配内存：在创建直接定址表时，可以根据预计的键数量预先分配足够的内存空间，避免了动态分配内存的开销



### 例1 se1631.asm

以十六进制的形式在屏幕中间显示给定的字节型数据。

**分析**
每个`字节`可分为`高低`两个`4位`。每`4位`对应一个`十六进制数。 如果每4位取出来 `+30h`转`ascii` 就挺麻烦， 不如直接用`数值`当`索引`去一块内存中直接取对应字符。

- **用`直接定址表`算法更`清晰和简洁`**

```assembly
assume cs:code
code segment
 start:	mov al,0F1h
		call showbyte
		
		mov ax,4c00h
		int 21h

showbyte:
		jmp short show
		table db '0123456789ABCDEF'	;字符表

	show:
		push bx
		push es
		
		mov bx,0b800h		; 设置显存段
		mov es,bx
		
		mov ah,al
		shr ah,1
		shr ah,1
		shr ah,1
		shr ah,1			;右移4位，ah 保留高4位的值
		and al,00001111b	;高4位置0，a1 保留低4位的值

		mov bl,ah
		mov bh,0
		mov ah,table[bx]	;高4位的值当索引，取得对应的字符
		mov es:[160*12+40*2],ah
		
		mov bl,al
		mov bh,0
		mov al,table[bx]	;低4位的值当索引，取得对应的字符
		mov es:[160*12+40*2+2],al
		
		pop es
		pop bx
		ret
		
code ends
end start

```



### 例2  se1632.asm

编写一个子程序，计算 `sim(x)`，`x` ∈ `{ 0°，30°，60°，90°，120°，150°，180}`，并在屏幕中间显示计算结果。比如 `sin(30)` 的结果显示为`“0.5”`

#### 分析

- **用`直接定址表`算法提高算法性能。**

`table` 类型是 `dw

| -              | ag0   | ag30  | ag60  | ag90  | ag120 | ag150 | ag180 |
| -------------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| 值             | 0     | 0.5   | 0.866 | 1     | 0.866 | 0.5   | 0     |
| 索引 `角度/30` | 0     | 1     | 2     | 3     | 4     | 5     | 6     |
| 位置           | 1B~1C | 1D~20 | 21~26 | 27~28 | 29~2E | 2F~32 | 33~34 |
| 长度 `字节`    | 2     | 4     | 6     | 2     | 6     | 4     | 2     |



```assembly
assume cs:code
code segment
 start:	mov ax,120
		call showsin
		
		mov ax,4c00h
		int 21h

showsin:				; 位 置	  机器码	反汇编
		jmp short show	; cs:0B	  EB28		jmp 0035
		; table 
		table	dw	ag0,ag30,ag60,ag90,ag120,ag150,ag180 ;字符串偏移地址表 cs:0d~1a
		ag0		db	'0',0		;cs:1B~1C	sin(0)  对应的字符串“0”
		ag30	db	'0.5',0		;cs:1D~20	sin(30) 对应的字符串“0.5”
		ag60	db	'0.866',0	;cs:21~26	sin(60) 对应的字符串“0.866”
		ag90	db	'1',0		;cs:27~28	sin(90) 对应的字符串“1”;
		ag120	db	'0.866',0	;cs:29~2E	sin(120)对应的字符串“0.866”
		ag150	db	'0.5',0		;cs:2F~32	sin(150)对应的字符串“0.5”
		ag180	db	'0',0		;cs:33~34	sin(180)对应的字符串“0”	

	show:
		push bx
		push es
		push si
		
		mov bx,0b800h		; 设置显存段
		mov es,bx
		
		;以下用角度值/30 作为相对于 table 的偏移，取得对应的字符串的偏移地址，放在 bx 中
		mov ah,0
		mov bl,30
		div bl
		mov bl,al
		mov bh,0
		add bx,bx
		mov bx,table[bx]		

		; 以下显示 sin(x) 对应的字符串
		mov si,160*12+40*2
 shows: mov ah,cs:[bx]
		cmp ah,0
		je showret
		mov  es:[si],ah
		inc bx
		add si,2
		jmp short shows		
showret:
		pop si
		pop es
		pop bx
		ret
		
code ends
end start

```



## 16.4 程序入口地址的直接定址表

在`直接定址表`中存储`子程序`的`地址`。

| 功能      | 实现一个子程序 `setscreen`，为显示输出提供如下功能。 1. 清屏; 2. 设置前景色; 3. 设置背景色; 4. 向上滚动一行。 |
| --------- | ------------------------------------------------------------ |
| 参数 `ah` | 用 `ah` 寄存器传递`功能号`： 0 表示清屏， 1 表示设置前景色， 2 表示设置背景色, 3 表示向上滚动一行; |
| 参数 `al` | 用 `al` 传送`颜色值`，(al)∈ {0,1,2,3,4,5,6,7}。<br/>用于`1`、`2`号功能 |



| 功能             | 实现方案                                                     |
| ---------------- | ------------------------------------------------------------ |
| 1.清屏           | 将显存中当前屏幕中的字符设为空格符;                          |
| 2.设置前景色     | 设置显存中当前屏幕中处于奇地址的属性字节的第`0`、`1`、`2`位; |
| 3.设置背景色     | 设置显存中当前屏幕中处于奇地址的属性字节的第`4`、`5`、`6`位; |
| 4.又向上滚动一行 | 依次将第 `n+1` 行的内容复制到第`n`行处;最后一行为空。        |



### 例1 se1641.asm

```assembly
assume cs:code
code segment
 start:	mov ah,3
		mov al,2
		call setscreen
		
		mov ax,4c00h
		int 21h


; =======================================================	
; ------------------- 子程序 setscreen  -----------------
; 设置显示效果
; -------------------------------------------------------
; 参数: ah	功能号：0 表示清屏，1 表示设置前景色，2 表示设置背景色, 3 表示向上滚动一行;
; 参数: al	颜色值。用于1、2号功能
; 返回: 无
; -------------------------------------------------------
setscreen:
		jmp short set
		
		table dw sub1,sub2,sub3,sub4
		
	set:
		push bx			; 备份寄存器
		
		cmp ah,3		; 判断功能号是否大于3
		ja sret
		mov bl,ah
		mov bh,0
		add bx,bx		; 根据 ah 中的功能号计算对应子程序在 table 表中的偏移
		
		call word ptr table[bx]
		
  sret:	pop bx			; 还原寄存器
		ret				; 返回
; -------------------- 子程序 setscreen -----------------
; =======================================================

; =======================================================	
; ---------------------- 子程序 sub1 --------------------
; 清屏：; 将显存中当前屏幕中的字符设为空格符
; -------------------------------------------------------
; 参数: 无
; 返回: 无
; -------------------------------------------------------
  sub1:
		push bx			; 备份寄存器
		push cx
		push es
		
		mov bx,0b800h
		mov es,bx
		mov bx,0
		mov cx,2000
 sub1s:	mov byte ptr es:[bx],' '	; 当前屏全设为空格
		add bx,2
		loop sub1s
		
		pop es			; 备份寄存器
		pop cx
		pop	bx
		ret				; 返回
; ---------------------- 子程序 sub1 --------------------
; =======================================================

; =======================================================	
; ---------------------- 子程序 sub2 --------------------
; 设置前景色：设置当前屏幕中所有奇列的第0、1、2位（前景色）
; -------------------------------------------------------
; 参数: 无
; 返回: 无
; -------------------------------------------------------
  sub2:
		push bx			; 备份寄存器
		push cx
		push es
		
		mov bx,0b800h
		mov es,bx
		mov bx,1						; 设置字符属性从 1 开始
		mov cx,2000
 sub2s:	and byte ptr es:[bx],11111000b	; 清空前景色 0、1、2
		or es:[bx],al					; 应用 al 传来的颜色值
		add bx,2
		loop sub2s
		
		pop es			; 备份寄存器
		pop cx
		pop	bx
		ret				; 返回
; ---------------------- 子程序 sub2 --------------------
; =======================================================

; =======================================================	
; ---------------------- 子程序 sub3 --------------------
; 设置背景色：设置当前屏幕中所有奇列的第4、5、6位（背景色）
; -------------------------------------------------------
; 参数: 无
; 返回: 无
; -------------------------------------------------------
  sub3:
		push bx			; 备份寄存器
		push cx
		push es
		
		mov cl,4
		shl al,cl		
		mov bx,0b800h
		mov es,bx
		mov bx,1						; 设置字符属性从 1 开始
		mov cx,2000
 sub3s:	and byte ptr es:[bx],10001111b	; 清空 4 ~ 6
		or es:[bx],al					; 应用 al 传来的颜色值
		add bx,2
		loop sub3s
		
		pop es			; 备份寄存器
		pop cx
		pop	bx
		ret				; 返回
; ---------------------- 子程序 sub3 --------------------
; =======================================================

; =======================================================	
; ---------------------- 子程序 sub4 --------------------
; 向上滚动一行：依次将第 n+1 行的内容复制到第n行处;最后一行为空。
; -------------------------------------------------------
; 参数: 无
; 返回: 无
; -------------------------------------------------------
  sub4:
		push cx			; 备份寄存器
		push si
		push di
		push es
		push ds
				
		mov si,0b800h
		mov es,si
		mov ds,si
		mov si,160		; ds:si 指向第 n+1 行
		mov di,0		; es:di 指向第 n 行
		cld
		mov cx,24		; 共复制 24 行

 sub4s:	push cx
		mov cx,160
		rep movsb		; 复制
		pop cx
		loop sub4s
		
		mov cx,80
		mov si,0
 sub4s1:mov	byte ptr [160*24+si],' '
		add si,2
		loop sub4s1
 
		pop ds			; 备份寄存器
		pop es
		pop	di
		pop	si
		pop	cx
		
		ret				; 返回
; ---------------------- 子程序 sub4 --------------------
; =======================================================
code ends
end start

```





## 实验16 编写包含多个功能子程序的中断例程

## 需求

安装一个新的 `int 7ch` 中断例程，为显示输出提供如下功能子程序。

| 功能      | 1. 清屏; <br />2. 设置前景色; <br />3. 设置背景色;<br /> 4. 向上滚动一行。 |
| --------- | ------------------------------------------------------------ |
| 参数 `ah` | 用 `ah` 寄存器传递`功能号`： `0` 表示清屏，`1` 表示设置前景色，`2` 表示设置背景色, `3` 表示向上滚动一行; |
| 参数 `al` | 用 `al` 传送`颜色值`，`(al)` ∈ `{ 0,1,2,3,4,5,6,7 }`。<br/>用于`1`、`2`号功能 |

### s16.asm

```assembly
assume cs:code
code segment
 start:	
		call install_int7ch
 		mov ah,0
		mov al,3
		int 7ch		

		mov ax,4c00h
		int 21h

; =======================================================	
; ------------------- 子程序 install_7ch  -----------------
; 设置显示效果
; -------------------------------------------------------
; 参数: 	无
; 返回: 无
; -------------------------------------------------------
install_int7ch:
		push ax
		push si
		push di
		push es
		push cx
		; ---------------- 安装(复制数据) ----------------
		mov ax,cs
		mov ds,ax

		mov ax,0
		mov es,ax

		mov si,offset int7ch ;设置 ds:si 指向源地址
		mov di,200h 		;设置 es:di 指向目的地址
		mov cx,offset int7ch_end-offset int7ch
		cld			;设置传输方向为正。movsb中si,di递增
		rep movsb 	;将ds:si 拷贝到 es:di 长度cx
		; ---------------- 安装(复制数据) ----------------

		; ----------------- 设置中断向量 -----------------
		mov ax,0
		mov es,ax
		cli								; 临时屏蔽中断
		mov word ptr es:[7ch*4],200h  	; 设置【中断处理程序】的：偏移地址
		mov word ptr es:[7ch*4+2],0h 	; 设置【中断处理程序】的：段地址
		sti 							; 恢复中断响应
		; ----------------- 设置中断向量 -----------------
		pop cx
		pop es
		pop di
		pop si
		pop ax
		ret
; -------------------- 子程序 install_7ch -----------------
; =======================================================

	org 200h ; ORG 指定下面代码从一个特定地址开始编译
	int7ch:
		; =======================================================	
		; ------------------- 子程序 setscreen  -----------------
		; 设置显示效果
		; -------------------------------------------------------
		; 参数: ah	功能号：0 表示清屏，1 表示设置前景色，2 表示设置背景色, 3 表示向上滚动一行;
		; 参数: al	颜色值。用于1、2号功能
		; 返回: 无
		; -------------------------------------------------------
		setscreen:
				jmp short set
				
				table dw sub1,sub2,sub3,sub4
				
			set:
				push bx			; 备份寄存器
				
				cmp ah,3		; 判断功能号是否大于3
				ja sret
				mov bl,ah
				mov bh,0
				add bx,bx		; 根据 ah 中的功能号计算对应子程序在 table 表中的偏移
				
				call word ptr table[bx]
				
		  sret:	pop bx				; 还原寄存器
				iret				; 返回
		; -------------------- 子程序 setscreen -----------------
		; =======================================================

		; =======================================================	
		; ---------------------- 子程序 sub1 --------------------
		; 清屏：; 将显存中当前屏幕中的字符设为空格符
		; -------------------------------------------------------
		; 参数: 无
		; 返回: 无
		; -------------------------------------------------------
		  sub1:
				push bx			; 备份寄存器
				push cx
				push es
				
				mov bx,0b800h
				mov es,bx
				mov bx,0
				mov cx,2000
		 sub1s:	mov byte ptr es:[bx],' '	; 当前屏全设为空格
				add bx,2
				loop sub1s
				
				pop es			; 备份寄存器
				pop cx
				pop	bx
				ret				; 返回
		; ---------------------- 子程序 sub1 --------------------
		; =======================================================

		; =======================================================	
		; ---------------------- 子程序 sub2 --------------------
		; 设置前景色：设置当前屏幕中所有奇列的第0、1、2位（前景色）
		; -------------------------------------------------------
		; 参数: 无
		; 返回: 无
		; -------------------------------------------------------
		  sub2:
				push bx			; 备份寄存器
				push cx
				push es
				
				mov bx,0b800h
				mov es,bx
				mov bx,1						; 设置字符属性从 1 开始
				mov cx,2000
		 sub2s:	and byte ptr es:[bx],11111000b	; 清空前景色 0、1、2
				or es:[bx],al					; 应用 al 传来的颜色值
				add bx,2
				loop sub2s
				
				pop es			; 备份寄存器
				pop cx
				pop	bx
				ret				; 返回
		; ---------------------- 子程序 sub2 --------------------
		; =======================================================

		; =======================================================	
		; ---------------------- 子程序 sub3 --------------------
		; 设置背景色：设置当前屏幕中所有奇列的第4、5、6位（背景色）
		; -------------------------------------------------------
		; 参数: 无
		; 返回: 无
		; -------------------------------------------------------
		  sub3:
				push bx			; 备份寄存器
				push cx
				push es
				
				mov cl,4
				shl al,cl		
				mov bx,0b800h
				mov es,bx
				mov bx,1						; 设置字符属性从 1 开始
				mov cx,2000
		 sub3s:	and byte ptr es:[bx],10001111b	; 清空 4 ~ 6
				or es:[bx],al					; 应用 al 传来的颜色值
				add bx,2
				loop sub3s
				
				pop es			; 备份寄存器
				pop cx
				pop	bx
				ret				; 返回
		; ---------------------- 子程序 sub3 --------------------
		; =======================================================

		; =======================================================	
		; ---------------------- 子程序 sub4 --------------------
		; 向上滚动一行：依次将第 n+1 行的内容复制到第n行处;最后一行为空。
		; -------------------------------------------------------
		; 参数: 无
		; 返回: 无
		; -------------------------------------------------------
		  sub4:
				push cx			; 备份寄存器
				push si
				push di
				push es
				push ds
						
				mov si,0b800h
				mov es,si
				mov ds,si
				mov si,160		; ds:si 指向第 n+1 行
				mov di,0		; es:di 指向第 n 行
				cld
				mov cx,24		; 共复制 24 行

		 sub4s:	push cx
				mov cx,160
				rep movsb		; 复制
				pop cx
				loop sub4s
				
				mov cx,80
				mov si,0
		 sub4s1:mov	byte ptr [160*24+si],' '
				add si,2
				loop sub4s1
		 
				pop ds			; 备份寄存器
				pop es
				pop	di
				pop	si
				pop	cx
				
				ret				; 返回
		; ---------------------- 子程序 sub4 --------------------
		; =======================================================
   int7ch_end:	nop
; --------------------- 中断处理程序 --------------------
; =======================================================
code ends
end start

```





### ORG (Origin)

在x86汇编语言中，`ORG`指令的全称是`Origin`，它指示[汇编程序](https://so.csdn.net/so/search?q=汇编程序&spm=1001.2101.3001.7020)从指定地址开始放置汇编指令生成的机器码。在Intel 8086及后续的x86架构中，`ORG`作为伪指令使用，用于设置汇编源代码中下一条指令的地址标记。

> 数据标号 `table` 的地址错误问题
> 当 `7ch` 中断例程安装时 `table` 记录的`安装程序`中它所处的`偏移位置`。
> 而 `中断例程` 直正调用时 `call word ptr table[bx]` 这 `table[bx]` 取值肯定是错的。
> 所以这里可以用 `org` 指定程序的起始地址。

`ORG`在以下场景中需要用到`ORG`指令：

1. **设定代码段起始地址：** 指定汇编代码在内存中的起始逻辑地址。
   1.1. 如:设置引导扇区地址。
   1.2. 如:插入或修改代码时，精确调整新代码在内存中的位置。
2. **保持与物理地址一致性：** 确保代码按预期物理地址布局，适用于直接操作硬件或特定地址通信。
3. **辅助重定位：** 为链接器提供初始逻辑地址参考，便于多目标文件链接时地址调整

##### 1. 设定代码段起始地址

假设有一个简单的 8086 程序，包含一个主程序和一个子程序，我们希望主程序从内存地址 `0x1000` 开始，子程序从 `0x2000` 开始。源代码可以这样组织：

```assembly
; 主程序段
ORG 0x1000

main:
    ; 主程序代码...
    call sub_program ; 调用子程序
    ...

; 子程序段
ORG 0x2000

sub_program:
    ; 子程序代码...
    ret

```

这里，`ORG 0x1000` 和 `ORG 0x2000` 分别指定了主程序和子程序的起始地址。汇编器在编译时，会根据这些 `ORG `指令将对应的[汇编指令](https://so.csdn.net/so/search?q=汇编指令&spm=1001.2101.3001.7020)分别定位到指定的内存地址。

##### 2. 保持与物理地址一致性

在某些嵌入式系统中，设备控制器可能要求特定的中断服务例程（Interrupt Service Routine, ISR）位于特定的物理地址。比如，假设一个外设的中断向量表指定了中断服务例程应在 `0x0000:0x8000`（物理地址 `0x8000`，段地址 `0x0000`）处。源代码可以这样编写：

```assembly
; 中断服务例程
ORG 0x8000

isr_handler:
    ; 中断处理代码...
    iret ; 结束中断服务例程

```

这里的 `ORG 0x8000` 确保了编译出的中断服务例程会被放置在物理地址 `0x8000`，满足设备控制器的要求。

##### 3. 辅助重定位

假设有一个大型项目，由多个独立编译的模块（.obj 或 .o 文件）组成。每个模块内部使用 ORG 设置了自身的逻辑地址，如模块 A 使用 ORG 0x1000，模块 B 使用 ORG 0x2000。在链接阶段，链接器需要将这些模块合并成一个可执行文件，并调整它们在最终内存布局中的地址。

尽管实际链接时可能涉及更复杂的重定位过程，但模块内部的 ORG 指令提供了初始的逻辑地址参考。例如，模块 A 的第一条指令在链接前被认为位于 0x1000，链接器可以根据这个信息和其他模块的位置以及链接脚本的指示，决定模块 A 在最终可执行文件中的实际位置，并进行相应的地址修正。

总的来说，ORG用于在汇编过程中控制代码在内存中的布局和定位。

