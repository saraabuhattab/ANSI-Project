# ANSI-Project
Only takes an input of one number and one letter in the exact format as such "3A", then displays the number and letter given like this "Displaying shape of size 3 made of letters A".

%include "asm_io.inc"

SECTION .data

printp1: db "Displaying shape of size ",0
printp2: db " made of letters ",0
e1: db "only 1 command line argument allowed",10,0
e2: db "command line argument should have a length of 2",10,0
e3: db "First entry should be a digit, 2nd should be a capital letter",10,0
e4: db "The digit must be greater than 1 and be odd (3,5,7, or 9)",10,0
SECTION .bss

SECTION .text
   global  asm_main
asm_main:
   enter 0,0             ; setup routine
   pusha                 ; save all registers

   mov eax, dword [ebp+8]   ; argc
   cmp eax, dword 2         ; argc should be 2
   jne E1
   ; checking number of arguments

   mov ebx, dword [ebp+12]  ; address of argv[]
   mov eax, dword [ebx+4]   ; argv[1]
   mov ecx, eax
   mov bl, byte [eax]       ; 1st byte of argv[1]
   cmp bl, 0
   je E2
   mov bl, byte [eax + 1]
   cmp bl, 0
   je E2
   mov bl, byte [eax + 2]
   cmp bl, 0
   jne E2
   ;done checking length command line arguement

   mov bl, byte [eax]
   cmp bl, '0'
   jb  E3 
   cmp bl, '9'
   ja  E3 
   ;check if number is between 0 and 9

   mov bl, byte [eax + 1]       ; 2nd byte of argv[1]
   cmp bl, 'A'
   jb E3
   ; so we know its below A
   cmp bl, 'Z'
   ja E3
  ; done checking capital letter
  ;digit AND a capital letter both checked

   mov bl, byte [eax]
   sub bl, '0'
   cmp bl, 1
   jng E4
   ; check if number is odd below
   cmp bl, 3
   je PRINT
   cmp bl,5
   je PRINT
   cmp bl,7
   je PRINT
   cmp bl,9
   je PRINT
   jmp E4

 PRINT:
   mov eax, printp1
   call print_string
   ;mov ebx, dword [ebp+16] 
   mov al, byte [ecx]
   call print_char
   mov eax, printp2
   call print_string
   mov al, byte [ecx+1]
   call print_char
   call print_nl
   jmp asm_main_end

 E1:
   mov eax, e1
   call print_string
   jmp asm_main_end
 E2:
   mov eax, e2
   call print_string
   jmp asm_main_end
 E3:
   mov eax, e3
   call print_string
   jmp asm_main_end

 E4:
   mov eax, e4
   call print_string
   jmp asm_main_end


 asm_main_end:
   popa                  ; restore all registers
   leave                     
   ret
