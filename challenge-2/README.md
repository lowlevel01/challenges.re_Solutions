# Challenge 2 : https://challenges.re/2/

#### What does this code do?
#### Optimizing GCC 4.8.2 -m32:
````assembly
<f>:
   0:          mov    eax,DWORD PTR [esp+0x4]  <---- Sine this is 32bit, argument passed via the stack : Only one argument : integer : 4 bytes
   4:          bswap  eax  <----- bswap reverses the order of the bytes b4,b3,b2,b1 => b1,b2,b3,b4
   6:          mov    edx,eax  <---- create another copy of the number
   8:          and    eax,0xf0f0f0f  <---- Choose every other 4 bits
   d:          and    edx,0xf0f0f0f0  <---- choose the other 4 bits
  13:          shr    edx,0x4  
  16:          shl    eax,0x4  <---- These two shifts change swap every 4 bits every byte a1,a2 becomes a2,a1
  19:          or     eax,edx  <----- Since the two masks don't intersect, ORing the two numbers is just adding them back together
  1b:          mov    edx,eax
  1d:          and    eax,0x33333333  <--- 3 in binary is 0011
  22:          and    edx,0xcccccccc  <---- 0xC in binart is 1100
  28:          shr    edx,0x2
  2b:          shl    eax,0x2
  2e:          or     eax,edx  <---- Now we're swapping every two adjacent bits
  30:          mov    edx,eax
  32:          and    eax,0x55555555 <---- 5 in binary is 0101
  37:          and    edx,0xaaaaaaaa  <----- 0xA in binary is 1010
  3d:          add    eax,eax  <---- equivalent to shr eax, 1
  3f:          shr    edx,1
  41:          or     eax,edx   <---- again swapping every bits with the one next to it
  43:          ret
````

Take a byte for example with the digits : abcd efgh
After swapping every other 4 bits it becomes : efgh abcd
After swapping evert other 2 bits it becomes : ghef cdab
After swapping every other bit it becomes: hgfe dcba <====> abcd efgh reversed
Since the bytes were reverse in the beginning and the digits of every byte are reversed then this function reverses the bits in an integer

