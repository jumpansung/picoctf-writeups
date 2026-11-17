# **GDB baby steps 1**

## **info**
- platform - picoctf
- category - reverse engineering
- difficulty - easy
- tools used - pwndbg, gdb

## **description**
<img width="639" height="197" alt="image" src="https://github.com/user-attachments/assets/ce2467b3-fc6f-49cb-a827-65c211bb6d09" />
the description says that the flag is the value of whatever is in the eax register in the main function, a register is like a holder of small amounts of data, and they are fast when handling them
we need to find what is inside of eax and put what is inside as a decimal number base with the format academy{n}

## **approach** 
after downloading the file i start using pwndbg on it, after running it i can see that it immediately exits, so i decide to do a breakpoint on the main function  

```
pwndbg> break main
Breakpoint 1 at 0x55555555513
pwndbg> run
Starting program: /mnt/c/Users/USER/desktop/ctfs/RE/GDB baby step 1/debugger0_a
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/usr/lib/libthread_db.so.1".

Breakpoint 1, 0x0000555555555131 in main ()
LEGEND: STACK | HEAP | CODE | DATA | WX | RODATA
─────────────────────────────────[ REGISTERS / show-flags off / show-compact-regs off ]─────────────────────────────────
 RAX  0x7ffff7e20de8 (environ) —▸ 0x7fffffffdca8 —▸ 0x7fffffffdf61 ◂— 'HOSTTYPE=x86_64'
 RBX  0
 RCX  0x555555557df8 (__do_global_dtors_aux_fini_array_entry) —▸ 0x5555555550e0 (__do_global_dtors_aux) ◂— endbr64
 RDX  0x7fffffffdca8 —▸ 0x7fffffffdf61 ◂— 'HOSTTYPE=x86_64'
 RDI  1
 RSI  0x7fffffffdc98 —▸ 0x7fffffffdf23 ◂— '/mnt/c/Users/USER/desktop/ctfs/RE/GDB baby step 1/debugger0_a'
 R8   0x7ffff7e19680 (__exit_funcs) —▸ 0x7ffff7e1afa0 (initial) ◂— 0
 R9   0x7ffff7e1afa0 (initial) ◂— 0
 R10  0x7fffffffd8c0 ◂— 0
 R11  0x7ffff7ffca90 (__pointer_chk_guard) ◂— 0x9e991b6f9513145b
 R12  0x7fffffffdc98 —▸ 0x7fffffffdf23 ◂— '/mnt/c/Users/USER/desktop/ctfs/RE/GDB baby step 1/debugger0_a'
 R13  1
 R14  0x7ffff7ffd000 (_rtld_global) —▸ 0x7ffff7ffe3c0 —▸ 0x555555554000 ◂— 0x10102464c457f
 R15  0x555555557df8 (__do_global_dtors_aux_fini_array_entry) —▸ 0x5555555550e0 (__do_global_dtors_aux) ◂— endbr64
 RBP  0x7fffffffdb60 —▸ 0x7fffffffdc10 —▸ 0x7fffffffdc70 ◂— 0
 RSP  0x7fffffffdb60 —▸ 0x7fffffffdc10 —▸ 0x7fffffffdc70 ◂— 0
 RIP  0x555555555131 (main+8) ◂— mov dword ptr [rbp - 4], edi
──────────────────────────────────────────[ DISASM / x86-64 / set emulate on ]──────────────────────────────────────────
   0x555555555129 <main>                          endbr64
   0x55555555512d <main+4>                        push   rbp
   0x55555555512e <main+5>                        mov    rbp, rsp
b► 0x555555555131 <main+8>                        mov    dword ptr [rbp - 4], edi        [0x7fffffffdb5c] <= 1
   0x555555555134 <main+11>                       mov    qword ptr [rbp - 0x10], rsi     [0x7fffffffdb50] <= 0x7fffffffdc98 —▸ 0x7fffffffdf23 ◂— '/mnt/c/Users/USER/desktop/ctfs/RE/GDB baby step 1/...'
   0x555555555138 <main+15>                       mov    eax, 0x86342                    EAX => 0x86342
   0x55555555513d <main+20>                       pop    rbp                             RBP => 0x7fffffffdc10
   0x55555555513e <main+21>                       ret                                <__libc_start_call_main+129>
    ↓
   0x7ffff7c27781 <__libc_start_call_main+129>    mov    edi, eax                        EDI => 0x86342
   0x7ffff7c27783 <__libc_start_call_main+131>    call   exit                        <exit>

   0x7ffff7c27788 <__libc_start_call_main+136>    call   __nptl_deallocate_tsd       <__nptl_deallocate_tsd>
───────────────────────────────────────────────────────[ STACK ]────────────────────────────────────────────────────────
00:0000│ rbp rsp 0x7fffffffdb60 —▸ 0x7fffffffdc10 —▸ 0x7fffffffdc70 ◂— 0
01:0008│+008     0x7fffffffdb68 —▸ 0x7ffff7c27781 (__libc_start_call_main+129) ◂— mov edi, eax
02:0010│+010     0x7fffffffdb70 —▸ 0x7ffff7fe0ca0 (dl_main) ◂— endbr64
03:0018│+018     0x7fffffffdb78 —▸ 0x7fffffffdc98 —▸ 0x7fffffffdf23 ◂— '/mnt/c/Users/USER/desktop/ctfs/RE/GDB baby step 1/debugger0_a'
04:0020│+020     0x7fffffffdb80 ◂— 0x1f7fbe000
05:0028│+028     0x7fffffffdb88 —▸ 0x555555555129 (main) ◂— endbr64
06:0030│+030     0x7fffffffdb90 —▸ 0x7fffffffdbd0 ◂— 0xc920d5d99e573d32
07:0038│+038     0x7fffffffdb98 —▸ 0x7ffff7fdf2a0 (_dl_sysdep_start+144) ◂— mov rax, qword ptr [rbp - 0x20]
─────────────────────────────────────────────────────[ BACKTRACE ]──────────────────────────────────────────────────────
 ► 0 0x555555555131 main+8
   1 0x7ffff7c27781 __libc_start_call_main+129
   2 0x7ffff7c278b9 __libc_start_main+137
   3 0x555555555065 _start+37
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```
i used a breakpoint so that pwndbg can stop at the main function and show me what happened inside of it
after reading the disassembly of the function i can see this 

```
0x555555555138 <main+15>                       mov    eax, 0x86342                    EAX => 0x86342
```
it is the eax register that we need, but we need it in decimal form
so i exit pwndbg and run a python command to print it in decimal format 
```
➜  GDB baby step 1 python3 -c "print(0x86342)"
549698
```
so now our flag should be academy{549698}
after putting it into the challenge it should be completed

## ** what did i learn**

this was a very easy RE challenge that is still fun and educational

takeaways
- gdb or pwndbg are both good debuggers that can be used on any program
- using a breakpoint on a function can make the program stop early and show what happened in the function
- you can read the disassembly to see what is inside of the registers 
