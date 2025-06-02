# Challenge 5 : https://challenges.re/5/

We're given this code:

````assembly
f: ; RDI : param1 , RSI : param2 , RDX : param3 , RCX : param4
	cmp	rcx, rsi  ; 
	ja	.L10  ; if param4 > param2 return 0
	sub	rsi, rcx 
	add	rsi, 1
	mov	r11, rsi ; if param4 <= param2 : param2(=r11) = param2 - param4 + 1
	je	.L10 ; if param2 = param4 : return 0
	test	rcx, rcx 
	jne	.L16
	mov	rax, rdi  ; if rcx == 0 : return RDI
	ret
.L10:
	xor	eax, eax
	ret
.L16: ; if param2 > param2 this runs
	push	rbx
	xor	r10d, r10d ; r10 = 0
	mov	r9d, 1 ; r9 = 1
.L4:
	lea	rax, [rdi+r10] ; rax = rdi+r10 ; in the beginning rax = rdi = param1
	xor	esi, esi ; RSI = 0
	xor	r8d, r8d ; R8 = 0
.L8:
	movzx	ebx, BYTE PTR [rdx+rsi] ; RDX : param3
	cmp	BYTE PTR [rax+rsi], bl ; RSI seems to be a counter ; means we compare two strings starting at param1 and param3
	cmovne	r8d, r9d ; if bytes don't match => r8 = 1
	add	rsi, 1 ; counter incremented
	cmp	rsi, rcx ; see if counter reached param4
	jne	.L8 ; if not go again and compare the next byte
	test	r8d, r8d ; R8 was set to 1 in case a byte didn't match.
	je	.L12 ; if all bytes matched, we leave the function with RAX pointing to the start of the bytes of param1 that matched param3.
	add	r10, 1 ; increment R10 so RAX now points to the next byte in param1
	cmp	r10, r11 ; check if R10 reached param2 - param4 + 1 which means we reached the last window
	jne	.L4 ; if there's still a param4 window to compare, we re-do the loop.
	xor	eax, eax ; otherwise, we return 0
.L12:
	pop	rbx
	ret
````

If you read through the comments in the assembly code, you'll understand that the function looks for the first occurence of a string in another string.
