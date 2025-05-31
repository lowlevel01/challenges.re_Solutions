# Challenge 4 : https://challenges.re/4/

````assembly
<f>:
   0:          mov    edx,edi
   2:          shr    edx,1
   4:          and    edx,0x55555555
   a:          sub    edi,edx ; Check https://bits.stephan-brumme.com/countBits.html for a proof of this trick
   c:          mov    eax,edi ; After the substraction, each pair of bits now represent the number of set bits in the same pair in original number (e.g 11 become 10 since 2 bits are set and 2 is 10 in binary)
   e:          shr    edi,0x2 ; Next we need to add these numbers to get the total number of set bits
  11:          and    eax,0x33333333 ; Add every adjacent two pair of bits to get the count in every 4 bits (take 2 pair of numbers 0110, at this stage this means that in the original location of the pair 01 only one bit was set and in the case of 10, 2 bits were set. So, in the total 3 bits are set in these 4 bits which is what is computed here).
  16:          and    edi,0x33333333 
  1c:          add    edi,eax  
  1e:          mov    eax,edi
  20:          shr    eax,0x4
  23:          add    eax,edi ; Next add every adjacent 4 bits (that's why we shift by 4) to get the count of set bits in every byte. 
  25:          and    eax,0xf0f0f0f
  2a:          imul   eax,eax,0x1010101 ; Now, we're interested in the sum of the bytes to get the total of set bits in the original number
  30:          shr    eax,0x18  ; 0x1010101 has the property that if multiplied by an 4 byte number the most significant byte in the result is the sum of the bytes.
  33:          ret

````

This is a direct implementation of https://graphics.stanford.edu/~seander/bithacks.html#CountBitsFromMSBToPos explained in detail in https://bits.stephan-brumme.com/countBits.html which counts the number of set bits in an integer except in this assembly the count is about the whole integer.
