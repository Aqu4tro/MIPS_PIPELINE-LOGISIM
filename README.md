# MIPS Pipeline - Logisim

Esta é uma implementação básica de um CPU baseado na arquitetura MIPS de 32 bits no simulador Logisim. O circuito conta com um Pipeline de 5 estágios e uma unidade de Hazard que lida com os hazards do tipo Forwarding e Stall.

![image](https://github.com/user-attachments/assets/deb39936-ca54-410b-994a-264a005f78c4)


Esta implementação suporta o seguinte subconjunto de instruções do MIPS:

**Instrução**          | **Formato**               | **Instrução**             | **Formato**              
:--------------------: | :-----------------------: | :-----------------------: | :----------------------:
Add                    | add $rd, $rs, $rt         | Store Word                | sw $rt, offset($rs)     
Add Immediate          | addi $rt, $rs, imediato   | Branch on Equal           | beq $rs, $rt, label     
And                    | and $rd, $rs, $rt         | Branch on Not Equal       | bne $rs, $rt, label     
Shift Left Logical     | sll $rd, $rt, shamt       | Set Less Than             | slt $rd, $rs, $rt       
Shift Right Logical    | srl $rd, $rt, shamt       | Set Less Than Immediate   | slti $rt, $rs, imediato
Sub                    | sub $rd, $rs, $rt         | Jump                      | j label              
Or                     | or $rd, $rs, $rt          | Jump and Link             | jal label             
Load Word              | lw $rt, offset($rs)       | Jump Register             | jr $rs    
Mul                    | mul $rd, $rs, $rt         | Div                       | div $rd, $rs, $rt         


Pseudo-instruções:

**Instrução**          | **Formato**               | **OPCode**                         
:--------------------: | :-----------------------: | :-----------------------: 
Subi                   | subi $rt, $rs, imediato   | 011100      
Muli                   | muli $rt, $rs, imediato   | 011101   
Divi                   | divi $rt, $rs, imediato   | 011110   
                                                                                                                                    
