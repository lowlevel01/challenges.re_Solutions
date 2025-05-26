# Challenge 3 : https://challenges.re/3/

We're given this array of integers
````c
int v[64]=
	{ -1,31, 8,30, -1, 7,-1,-1, 29,-1,26, 6, -1,-1, 2,-1,
	  -1,28,-1,-1, -1,19,25,-1, 5,-1,17,-1, 23,14, 1,-1,
	   9,-1,-1,-1, 27,-1, 3,-1, -1,-1,20,-1, 18,24,15,10,
	  -1,-1, 4,-1, 21,-1,16,11, -1,22,-1,12, 13,-1, 0,-1 };
````

And this assembly (argument passed via RDI)

````assembly
f:
	mov	edx, edi  <---- Argument is passed via RDI but since we're dealing with integers (4 bytes). The code is manipulating EDI.
	shr	edx  <----- Number is shifted once 
	or	edx, edi  <---- ORed with the original one
	mov	eax, edx
	shr	eax, 2  <--- Shifted twice
	or	eax, edx  <--- ORed with previous result
	mov	edx, eax
	shr	edx, 4
	or	edx, eax
	mov	eax, edx
	shr	eax, 8
	or	eax, edx
	mov	edx, eax
	shr	edx, 16
	or	edx, eax  <---- Until here same pattern goes shift by power of 2 and OR with previous result.
	imul	eax, edx, 79355661 ; 0x4badf0d
	shr	eax, 26
	mov	eax, DWORD PTR v[0+rax*4]
	ret
````

``
Let's start with the binary number 1000 0000
1st shift and or gives 1000 0000 | 01000 000 = 1100 0000
2nd one gives : 1100 0000 | 0011 0000 = 1111 0000
3rd one gives : 1111 0000 | 0000 1111 = 1111 1111
...

So basically, the Shifts and ORs take the Most Significant Bit and OR it with all the bits right to it. If MSB is at 2^k then the result if 2^(k+1) - 1
==> After this algorithm, numbers with same MSB set will have the same result.

The constant 0x4badf0d is carefully chosen since it's what's called a De Bruijn sequence (i.e. every substring appears only once in the bit sequence).
When this constant is multiplied by the result of the previous algorithm it yield a unique sequence in the upper 5 bits. 
In the end we have (2^(k+1) - 1) * 0x4badf0d >> 26 . 
The constant is chose to be a De Bruijn constant so that the result of the previous operation is unique for each k between 0 and 31. 
Which creates a unique index in the array. Which a pre computed table to store the MSBs. 

==> This assembly transforms numbers with same MSB to the same number by setting all the bits to the right. 
==> So, numbers with same MSB map to the same number. 
==> You can verify by running the code. This assembly determines the position of the MSB starting from the left.
``
