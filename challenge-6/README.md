# Challenge 6 : https://challenges.re/6/

````assembly
<f>:
   0:             push   rbp
   1:             mov    rbp,rsp
   4:             mov    QWORD PTR [rbp-0x8],rdi ; [rbp-0x8] = param1
   8:             mov    QWORD PTR [rbp-0x10],rsi ; [rbp-0x10] = param2
   c:             mov    rax,QWORD PTR [rbp-0x8]  ; rax = param1
  10:             movzx  eax,BYTE PTR [rax] ; read and zero extend a byte from source parameter into eax
  13:             movsx  dx,al  ; sign extend the byte at the current param1 pointer to 2 bytes.
  17:             mov    rax,QWORD PTR [rbp-0x10]
  1b:             mov    WORD PTR [rax],dx ; write the extended byte to address currently pointed to by param2
  1e:             mov    rax,QWORD PTR [rbp-0x10]
  22:             movzx  eax,WORD PTR [rax]
  25:             test   ax,ax ; if we reach "\x00\x00", the function returns
  28:             jne    2c 
  2a:             jmp    38 
  2c:             add    QWORD PTR [rbp-0x8],0x1 ; otherwise inrement source pointer by 1 and destination pointer by 2.
  31:             add    QWORD PTR [rbp-0x10],0x2
  36:             jmp    c 
  38:             pop    rbp
  39:             ret
````

The function transform an array of signed 1 byte integers pointer to by the first parameter to an array of signed 2 byte integers. 
