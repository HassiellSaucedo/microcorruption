# microcorruption
Here's my progress to the CTF https://microcorruption.com/
---
<p>
<h3>New Orleans</h3>
Found Flag by Static Analysis.

First, we can see that there was a password that was created before, when we get to the check_password function:
```
447e <create_password>
44bc:  0e43           clr	r14
44be:  0d4f           mov	r15, r13
44c0:  0d5e           add	r14, r13
44c2:  ee9d 0024      cmp.b	@r13, 0x2400(r14)
44c6:  0520           jnz	$+0xc <check_password+0x16>
44c8:  1e53           inc	r14
44ca:  3e92           cmp	#0x8, r14
44cc:  f823           jnz	$-0xe <check_password+0x2>
44ce:  1f43           mov	#0x1, r15
44d0:  3041           ret
44d2:  0f43           clr	r15
44d4:  3041           ret
```
We can see that there's the instruction to compare the value we entered that got stored in the address of r13 with the 0x2400 Memory Address.
If we glance at the Live memory dump there will be lying the password right in front of us, as it's being compared with our input.
```
2400: 4726 6b3b 6c48 5200 0000 0000 0000 0000   G&k;lHR.........
2410: 0000 0000 0000 0000 0000 0000 0000 0000   ................
```
Password
ASCII/Hex: ```G&k;lHR / 47266b3b6c485200```
</p>

---
<p>
<h3>Sydney</h3>
Found Flag by Static Analysis.

Here, the lock again comes with a password, if we check the check_password function we can see:
```
448a <check_password>
448a:  bf90 3644 0000 cmp	#0x4436, 0x0(r15)
4490:  0d20           jnz	$+0x1c <check_password+0x22>
4492:  bf90 4f7a 0200 cmp	#0x7a4f, 0x2(r15)
4498:  0920           jnz	$+0x14 <check_password+0x22>
449a:  bf90 514f 0400 cmp	#0x4f51, 0x4(r15)
44a0:  0520           jnz	$+0xc <check_password+0x22>
44a2:  1e43           mov	#0x1, r14
44a4:  bf90 3d32 0600 cmp	#0x323d, 0x6(r15)
44aa:  0124           jz	$+0x4 <check_password+0x24>
44ac:  0e43           clr	r14
44ae:  0f4e           mov	r14, r15
44b0:  3041           ret
```
We get the hint at the manual that the microcontroller is a MSP430, a 16-bit architecture, here we get into endianness concepts, sepecifically with little-endian
way of storage. at this block we can note that there's multiple cmp instructions, that compares our input with the bytes stated at the functions. reordering
the structure of the bits we get:

```36444f7a514f3d32```

And Bingo.

Password:

Hex : ```36444f7a514f3d32```

</p>
