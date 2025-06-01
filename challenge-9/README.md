# Challenge 9 : https://challenges.re/9/

````assembly
.LC0:
	.string	"error!"
f:
	sub	rsp, 8
	movzx	eax, BYTE PTR [rdi]
	cmp	al, 89 ; 'Y'
	je	.L3
	jle	.L21
	cmp	al, 110 ; 'n'
	je	.L6
	cmp	al, 121 ; 'y'
	jne	.L2
.L3:
	mov	eax, 1
	add	rsp, 8
	ret
.L21:
	cmp	al, 78 ; 'N'
	je	.L6
.L2:
	mov	edi, OFFSET FLAT:.LC0
	call	puts
	xor	edi, edi
	call	exit
.L6:
	xor	eax, eax
	add	rsp, 8
	ret

````

=> Simple code that returns 1 if parameter is 'Y' or 'y' and 0 if it is 'N' or 'n' and error out otherwise.
