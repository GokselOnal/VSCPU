# Simple CPU

<b>32 bit Instruction Word (IW) of SimpleCPU</b>

```
                                          IW
               |-----------------------------------------------------------|
  bit position |31    29| 28 |27                  14|13                   0|
               |-----------------------------------------------------------|
    field name | opcode | i  |           A          |           B          |
               |-----------------------------------------------------------|
     bit width |   3b   | 1b |          14b         |          14b         |
               |-----------------------------------------------------------|

i=1 implies B is Immediate (meaning a number) as opposed to a Pointer (meaning Address)

```

### Instruction Set of SimpleCPU</b>

*<b>ADD   -></b>* unsigned Add
<ul style="margin-left:40px">
         {opcode, i} = {0, 0}<br>
         *A <- (*A) + (*B)<br>
         write ( readFromAddress(A) + readAddress(B) ) to address ( A )<br>
         *A = value (content of) address A = mem[A] (mem means memory)<br>
         *B = value (content of) address B = mem[B]<br>
         <- means write (assign)<br>
         mem[A] = mem[A] + mem[B]<br>
         mem[IW[27:14]] = mem[IW[27:14]] + mem[IW[13:0]]<br>
</ul>


-----

*<b>ADDi  -></b>* unsigned Add immediate
<ul style="margin-left:40px">
         {opcode, i} = {0, 1}<br>
         *A <- (*A) + B<br>
         B = read section B of the Instruction<br>
         mem[A] = mem[A] + B<br>
         mem[IW[27:14]] = mem[IW[27:14]] + IW[13:0<br>
</ul>



-----

*<b>NAND  -></b>* bitwise NAND
<ul style="margin-left:40px">
         {opcode, i} = {1, 0}<br>
         *A <- ~((*A) & (*B))<br>
         {opcode, i} = {0, 1}<br>
</ul>

-----

*<b>NANDi -></b>* bitwise NAND immediate
<ul style="margin-left:40px">
         {opcode, i} = {1, 1}<br>
         *A <- ~((*A) & B)<br>
</ul>               



-----

*<b>SRL   -></b>* Shift Right if the shift amount (*B) is less than 32, otherwise Shift Left
<ul style="margin-left:40px">
         {opcode, i} = {2, 0}<br>
         *A <- ((*B) < 32) ? ((*A) >> (*B)) : ((*A) << ((*B) - 32))<br>
</ul>    



-----

*<b>SRLi  -></b>* Shift Right if the shift amount (B) is less than 32, otherwise Shift Left
<ul style="margin-left:40px">
         {opcode, i} = {2, 0}<br>
         *A <- (B < 32) ? ((*A) >> B) : ((*A) << (B - 32))<br>
</ul>    



-----

*<b>LT    -></b>* if *A is Less Than *B then *A is set to 1, otherwise to 0.
<ul style="margin-left:40px">
         {opcode, i} = {3, 0}<br>
         *A <- ((*A) < (*B))<br>
</ul> 

-----

*<b>LTi   -></b>* if *A is Less Than B then *A is set to 1, otherwise to 0.
<ul style="margin-left:40px">
         {opcode, i} = {3, 1}<br>
         *A <- ((*A) < B)<br>
</ul>    



-----

*<b>CP    -></b>* Copy *B to *A
<ul style="margin-left:40px">
         {opcode, i} = {4, 0}<br>
         *A <- *B<br>
</ul>   


-----

*<b>CPi   -></b>* Copy B to *A
<ul style="margin-left:40px">
         {opcode, i} = {4, 1}<br>
         *A <- B<br>
</ul>    



-----

*<b>CPI   -></b>* (regular) Copy Indirect: Copy \**B to *A
<ul style="margin-left:40px">
         (go to address B and fetch the number then treat it as an address and go to that address and get that data and write to address A)<br>
         {opcode, i} = {5, 0}<br>
         *A <- *(*B)<br>
         write ( readFromAddress(readFromAddress(B)) ) to address ( A )<br>
</ul>    



-----

*<b>CPIi  -></b>* (immediate) Copy Indirect: Copy *B to \**A
  <ul style="margin-left:40px">
         (go to address B and fetch the number (*B) then go to address A and fetch the number there and treat it as an address and write there *B)<br>
         {opcode, i} = {5, 1}<br>
         *(*A) <- *B<br>
         write ( readFromAddress(B) ) to address ( readFromAddress(A) )<br>
</ul>    



-----


*<b>BZJ   -></b>* Branch on Zero
    <ul style="margin-left:40px">
         (Branch to *A if *B is Zero, otherwise increment Program Counter (PC))<br>
         {opcode, i} = {6, 0}<br>
         PC <- (*B == 0) ? (*A) : (PC+1)<br>
         if(*B == 0) goTo(*A), else goTo(nextInstruction)<br>
</ul>    


-----


*<b>BZJi  -></b>* Jump (unconditional branch)
<ul style="margin-left:40px">
         {opcode, i} = {6, 1}<br>
         PC <- (*A) + B<br>
</ul>    



-----

*<b>MUL   -></b>* unsigned Multiply
<ul style="margin-left:40px">
         {opcode, i} = {7, 0}<br>
         *A <- (*A) * (*B)<br>
</ul>          



-----

*<b>MULi  -></b>* unsigned Multiply
<ul style="margin-left:40px">
         {opcode, i} = {7, 1}<br>
         *A <- (*A) * B<br>
</ul>    
