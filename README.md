# Generating Assembly from a High Level Language

## General Example
How might one implement the high-level language (HLL) code block shown below (python) in assembly? 

Generic code
```python
op1 = 5
op2 = 10

if op1 == op2:
    x = 1
else:
    x = 2
```

x86 MASM code
```assembly
.data
op1	DWORD	5
op2	DWORD	10

.code
  MOV   EAX, op1
  CMP   EAX, op2
  JNE   _NotEqual
  MOV   x, 1        ; Operands equal
  JMP   _Finished
_NotEqual:          ; Operands not equal
  MOV   x, 2  
_Finished:
```

## C Example in Ubuntu 20.04

### Compiling and Generating Assembly with GCC

To compile to executable
```console
gcc <fname>.c -o <fname>
```

To run the executable
```console
./<fname>
```

To generate the assembly file
```console
gcc -S <fname>.c
```

### Examples with C / GCC

#### Example 1 - Flow Control

Code in C
```C
int main(void)
{
  int op1 = 5;
  int op2 = 10;
  int x;
  if (op1 == op2) {
    x = 1;
  } else {
    x = 2;
  } 
}
```

GCC Generated Assembly File
```assembly
	.file	"demo_ifelse.c"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movl	$5, -12(%rbp)
	movl	$10, -8(%rbp)
	movl	-12(%rbp), %eax
	cmpl	-8(%rbp), %eax
	jne	.L2
	movl	$1, -4(%rbp)
	jmp	.L3
.L2:
	movl	$2, -4(%rbp)
.L3:
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	 1f - 0f
	.long	 4f - 1f
	.long	 5
0:
	.string	 "GNU"
1:
	.align 8
	.long	 0xc0000002
	.long	 3f - 2f
2:
	.long	 0x3
3:
	.align 8
4:
```

Focusing just on the important bits...
```assembly
...
.LFB0:
	...
	movl	$5, -12(%rbp)
	movl	$10, -8(%rbp)
	movl	-12(%rbp), %eax
	cmpl	-8(%rbp), %eax
	jne	.L2
	movl	$1, -4(%rbp)
	jmp	.L3
.L2:
	movl	$2, -4(%rbp)
.L3:
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
...
```


x : -4(%rbp)
prop1 : -12(%rbp)
prop2 : -8(%rbp)



#### Example 2 - Repetition Structures


Code in C
```C
int main(void)
{
  int sum = 0;
  for (int i = 1; i < 11; i++) {
    sum += i;
  }
}
```

GCC Generated Assembly File
```assembly
	.file	"demo_repetition.c"
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movl	$0, -8(%rbp)
	movl	$1, -4(%rbp)
	jmp	.L2
.L3:
	movl	-4(%rbp), %eax
	addl	%eax, -8(%rbp)
	addl	$1, -4(%rbp)
.L2:
	cmpl	$10, -4(%rbp)
	jle	.L3
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	 1f - 0f
	.long	 4f - 1f
	.long	 5
0:
	.string	 "GNU"
1:
	.align 8
	.long	 0xc0000002
	.long	 3f - 2f
2:
	.long	 0x3
3:
	.align 8
4:
```



Focusing just on the important bits...
```assembly
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movl	$0, -8(%rbp)
	movl	$1, -4(%rbp)
	jmp	.L2
.L3:
	movl	-4(%rbp), %eax
	addl	%eax, -8(%rbp)
	addl	$1, -4(%rbp)
.L2:
	cmpl	$10, -4(%rbp)
	jle	.L3
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
```

sum : 	-8(%rbp)
i   : 	-4(%rbp)


## C++ Example in Windows with Visual Studio
You can also access/view the assembly code in Visual Studio via the debugger. Just set a breakpoint and open the dissasembly window when execution of the program is paused. 




## References
* [How to compile and run a C/C++ program](https://www.cyberciti.biz/faq/howto-compile-and-run-c-cplusplus-code-in-linux/)
* [Assembler output from C/C++](https://stackoverflow.com/questions/137038/how-do-you-get-assembler-output-from-c-c-source-in-gcc)






