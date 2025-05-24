## Section 1 - init.asm Outputs
| PC | Opcode | src1_addr | src1_out   | src2_addr | src_out    |dst_addr | dst_data   |
|---:|-------:|----------:|-----------:|----------:|-----------:|--------:|-----------:|
|   0| 0x23   | 0         | 0x00000000 | 2         | 0x00000000 | 0       | 0x00000000 |
|   4| 0x23   | 0         | 0x00000000 | 2         | 0x00000000 | 0       | 0x00000000 |
|   8| 0x00   | 2         | 0x00000000 | 2         | 0x00000000 | 0       | XXXXXXXXXX |
|  12| 0x2B   | 0         | 0x00000000 | 3         | 0x00000000 | 2       | 0x00000056 |
|  16| 0x00   | 3         | 0x00000000 | 2         | 0x00000056 | 2       | 0x00000056 |
|  20| 0x08   | 3         | 0x00000000 | 5         | 0x00000000 | 3       | 0x00000000 |
|  24| 0x00   | 5         | 0x00000000 | 3         | 0x00000000 | 3       | 0x00000084 |
|  28| 0x00   | 6         | 0x00000000 | 2         | 0x00000056 | 4       | 0xFFFFFFAA |
|  32| 0x00   | 6         | 0x00000000 | 2         | 0x00000056 | 5       | 0x0000000C |
|  36| 0x00   | 5         | 0x0000000C | 4         | 0xFFFFFFAA | 6      | 0x00000000 |
|  40| 0x04   | 5         | 0x0000000C | 0         | 0x00000000 | 7       | 0x00000056 |
|  44| 0x23   | 0         | 0x00000000 | 8         | 0x00000000 | 8       | 0xFFFFFFA9 |
|  48| 0x00   | 0         | 0x00000000 | 0         | 0x00000000 | 6       | 0x00000000 |
|  52| 0x00   | 0         | 0x00000000 | 0         | 0x00000000 | 31       | 0x0000000C |
|  56| 0x00   | 0         | 0x00000000 | 0         | 0x00000000 | 8       | 0x00000000 |
## Section 2 - Pipelined Datapath vs. Non-Pipelined Datapath
```asm
1. lw $v0 31($zero)
2. add $v1 $v0 $v0
3. sw $v1 132($zero)
4. sub $a0 $v1 $v0
5. addi $a1 $v1 12
6. and $a2 $a1 $v1
7. or $a3 $a2 $v0
8. nor $t0 $a2 $v0
9. slt $a2 $a1 $a0
10. beq $a2 $zero -8
11. lw $t0 132($zero)
```

Data Hazard: The second instruction add $v1 $v0 $v0 is dependent on the first instruction lw $v0 31($zero). Instruction 2 must have the updated value from instruction 1 before executing, therefore it is a data hazard. 
<br>
Control Hazard: The processor needs to wait for the correct value of $a2 in instruction 10 beq $a2 $zero -8 before comparing with $zero and checking whether to branch or not. Therefore, this is a control hazard.
## Section 3 - MIPS assembly instruction
add  $zero, $zero, $zero <br>
When placing this instruction between the first and second instruction, it avoids a stall and hazard. Since this instruction does not affect $v0 and is essentially doing nothing (no registers are being updated, NOP), it avoids a stall. It also avoids a hazard by giving the processor another cycle to load the value at address 31 into the $v0 register.