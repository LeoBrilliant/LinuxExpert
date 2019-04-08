# AT&T Syntax
## Prefix
% register
$ immediate number
0x hexadecimal number

## Direction of Operands
instr   source, dest
movl    (%ecx), %eax

## Memory Operands
movl    (%ebx), %eax
movl    3(%ebx),    %eax

%segreg:disp(base, index, scale)

instr %segreg:disp(base, index, scale), foo
movl    0x20(%ebx), %eax
addl    (%ebx, %ecx, 0x2), %eax
leal    (%ebx, %ecx),   %eax
subl    -0x20(%ebx, %ecx, 0x4), %eax

## Suffixes
b   byte
w   word
l   long

## Syscalls
if less than 6 args, syscall number in eax, args in ebx, ecx, edx, esi, edi.
if greater then 5 args, syscall number in eax, the pointer to the first arg in ebx.

for socket syscall, SYS_socketcall in eax, subfunction number in ebx, a pointer to syscall args in ecx.

# GCC inline asm
__asm__("
        movl    $1, %eax    // SYS_exit
        xor     %ebx,   %ebx
        int     $0x80
        ");

__asm__("<asm routine>" : output : input : modify );

The output and input fields must consist of an operand constraint string followed by a C expression enclosed in parentheses. The output operand constraints must be preceded by an '=' which indicates that it is an output. There may be multiple outputs, inputs, and modified registers. Each "entry" should be separated by commas (',') and there should be no more than 10 entries total. The operand constraint string may either contain the full register name, or an abbreviation.

## Abbreviation
a   %eax/%ax/%al
b   %ebx/%bx/%bl
c   %ecx/%cx/%cl
d   %edx/%dx/%dl
S   %esi/%si
D   %edi/%di
m   memory

- http://asm.sourceforge.net/articles/linasm.html

## Comment
# this is a comment
/* this is a comment */
- https://stackoverflow.com/questions/17442444/commenting-syntax-for-x86-att-syntax-assembly

# Source code of X86 Assembly Language: From real mode to protect mode
- https://github.com/lichuang/x86-asm-book-source
- https://github.com/yifengyou/32to64

# Generate raw binaries like nasm -f bin
$ as -o test.o test.S
$ ld -Ttext 0x7C00 -o test.elf test.o
$ objcopy -O binary kernel.elf kernel.bin
$ ld -T linker.ld --oformat binary -o test.bin test.o # if we have a linker script
$ hexdump or hd to view the binary file.

- https://stackoverflow.com/questions/6828631/how-to-generate-plain-binaries-like-nasm-f-bin-with-the-gnu-gas-assembler
- https://stackoverflow.com/questions/8482059/how-to-compile-an-assembly-file-to-a-raw-binary-like-dos-com-format-with-gnu?rq=1
- https://stackoverflow.com/questions/1647359/is-there-a-way-to-get-gcc-to-output-raw-binary

## disassemble raw x86 code
$ dd if=/dev/sda of=mbr bs=512 count=1
$ objdump -D -b binary -mi386 -Maddr16,data16 mbr
- https://stackoverflow.com/questions/1737095/how-do-i-disassemble-raw-x86-code

# X86 Assembly Cheat
- https://github.com/cirosantilli/x86-assembly-cheat
