```
```

| Format            | Bit             |                |            |                |              |              |          |            |
| ----------------- | --------------- | -------------- | ---------- | -------------- | ------------ | ------------ | -------- | ---------- |
| Register/register | funct7 (7)      | rs2 (5)        | rs1 (5)    | funct3 (3)     | rd (5)       | opcode (7)   |          |            |
| Immediate         | imm[11:0] (12)  | rs1 (5)        | funct3 (3) | rd (5)         | opcode (7)   |              |          |            |
| Store             | imm[11:5] (7)   | rs2 (5)        | rs1 (5)    | funct3 (3)     | imm[4:0] (5) | opcode (7)   |          |            |
| Branch            | [12] (1)        | imm[10:5] (6)  | rs2 (5)    | rs1 (5)        | funct3 (3)   | imm[4:1] (4) | [11] (1) | opcode (7) |
| Upper immediate   | imm[31:12] (20) | rd (5)         | opcode (7) |                |              |              |          |            |
| Jump              | [20] (1)        | imm[10:1] (10) | [11] (1)   | imm[19:12] (8) | rd (5)       | opcode (7)   |          |            |

| Field Name | Description                                                                                           |
| ---------- | ----------------------------------------------------------------------------------------------------- |
| opcode     | (7 bits) Partially specifies one of the 6 types of instruction formats.                               |
| funct7     | (7 bits) Extends the opcode field to specify the operation to be performed.                          |
| funct3     | (3 bits) Extends the opcode field to specify the operation to be performed.                          |
| rs1        | (5 bits) Specifies, by index, the first operand register (source register).                          |
| rs2        | (5 bits) Specifies, by index, the second operand register (source register).                         |
| rd         | (5 bits) Specifies, by index, the destination register to which the computation result is directed.  |
| imm        | Immediate values used in various formats.                                                             |


|     |     |
| --- | --- |
|     |     |
