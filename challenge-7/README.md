# Challenge 7 : https://challenges.re/7/

````assembly
<f>:
   0:                movzx  edx,BYTE PTR [rdi] ; read byte from argument
   3:                mov    rax,rdi
   6:                mov    rcx,rdi
   9:                test   dl,dl ; see if it's 0x00
   b:                je     29 ; if so, return.
   d:                nop    DWORD PTR [rax] ; this one is just for padding.
  10:                lea    esi,[rdx-0x41] ; rdx - 'A' <=> typical check for alphabets (to find index or check other things)
  13:                cmp    sil,0x19 
  17:                ja     1e ; if the difference > 0x19 (=25) => byte is a lowercase letter, just go to the next byte
  19:                add    edx,0x20 ; otherwise, then we have an uppercase letter. Add 0x20 to it to make it lowercase.
  1c:                mov    BYTE PTR [rcx],dl
  1e:                add    rcx,0x1
  22:                movzx  edx,BYTE PTR [rcx]
  25:                test   dl,dl
  27:                jne    10 
  29:                repz ret
````

The function transforms a string of uppercase letters to lowercase letters by adding 0x20 to each byte.
