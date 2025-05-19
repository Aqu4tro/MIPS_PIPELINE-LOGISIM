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
                                                                                                                                    



## Detecção de Hazards
Como mencionado anteriormente, o processador pode lidar com hazards do tipo Forwarding e Stall. Para poder garantir a execução correta das instruções no estágios de pipeline, é, antes de tudo,
detectar instruções que dependem uma da outra, o que é feito pelo circuito HazardDetector.


![image](https://github.com/user-attachments/assets/6343034b-3021-4244-a2b1-14bbf5b261ed)


Esse circuito possui 4 entradas: um registrador fonte(rs e rt), o registrador destino(rd), e dois sinais de controle, que indicam se o registrador fonte será lido, outro que indica se o registrador destino será escrito.
Caso o registrador destino seja diferente de zero, e igual ao registrador fonte, e os dois sinais de controle(leitura e escrita) forem 1, a saída do circuito será 1, o que indica que há hazard.


## Unidade de Hazard
### Forwarding
Para evitar atrasos desnecessários causados por dependências entre instruções consecutivas, quatro circuitos de detecção de hazards comparam os registradores de origem (rs e rt) da instrução no estágio EX com o registrador de destino (rd) das instruções 
nos estágios MEM e WB. Se houver coincidência e o registrador de destino for diferente de zero, significa que o dado necessário ainda não foi escrito no banco de registradores. Nesse caso, os sinais de detecção são usados para controlar multiplexadores 
que selecionam os valores diretamente dos estágios MEM ou WB, redirecionando os dados para o estágio EX sem a necessidade de esperar pela escrita no banco de registradores.


![image](https://github.com/user-attachments/assets/a8ee2fe7-d6f7-4d7c-b9bf-05568233ea83)


### Stall
Em situações específicas, como instruções lw, o dado lido da memória só estará disponível no fim do estágio MEM. Para evitar que a próxima instrução use dados ainda indisponíveis, dois detectores de hazard
comparam os registradores de origem (rs e rt) da instrução no estágio ID com o registrador de destino (rd) da instrução lw no estágio EX. Se houver dependência e a instrução no EX for um lw, um sinal de stall é ativado. Esse sinal interrompe a progressão 
da próxima instrução no pipeline, inserindo uma bolha (NOP) no estágio EX e atrasando a execução até que os dados estejam disponíveis.


![image](https://github.com/user-attachments/assets/ce88f6b7-4de1-438a-835a-5218fd820cd5)


