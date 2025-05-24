# Challenge 1 : https://challenges.re/1/

#### The function has 4 arguments and it is compiled by GCC for Linux x64 ABI

````assembly

<f>:
   0:                   mov    r8,rdi   ; <---- RDI : 1st parameter => R8 becomes 1st parameter
   3:                   push   rbx             
   4:                   mov    rdi,rsi  ; <------- RSI : 2nd parameter => RDI becomes 2nd parameter
   7:                   mov    rbx,rdx  ; <-------- RDX : 3rd parameter  => RBX becomes 3rd parameter
   a:                   mov    rsi,r8   ; <-------- R8 : 1st parameter => RSI becomes first parameter
   d:                   xor    rdx,rdx  ; <------RDX Zero'ed before first division

begin:
  10:                   lods   rax,QWORD PTR ds:[rsi]  ; <---- RSI is source ===> 1st paramter = Source
  12:                   div    rbx   ; <------ RBX is divisor => 3rd parameter divisor  <====> RDX:RAX is divided by RBX , Quotient is put in RAX and Remainder in RDX
  15:                   stos   QWORD PTR es:[rdi],rax ; <---------- RDI : Destination => 2nd parameter = Destination
  17:                   loop   begin        ; <------ RCX : 4th parameter ===> Loop repeated RCX times ===> 4th parameter is size of Source
  19:                   pop    rbx
  1a:                   mov    rax,rdx   ; <---- returns the last remainder
  1d:                   ret

````

 The code basically reads 8 bytes a time from the source (1st Parameter) and divides it by the Divisor (3rd parameter). The DIV instruction divides RDX:RAX (128bits) by RBX and places the quotient in RAX and Remainder in RDX. RDX is not zero'ed after each division which created a cascading effect. This is a big integer (Source) divided by an 8 byte integer, the quotient is written to the destination and the last remainder is returned which is the remainder of the whole operation. 

 This can be intuitively seen if you write the big number in base 2^64, each time you divide, the quotient is put in the correspoing 8 bytes. The remainder doesn't contribute to it. So, it must contribute to the next 8 bytes and so on and so forth.

Check out http://x86asm.net/articles/working-with-big-numbers-using-x86-instructions/ 
