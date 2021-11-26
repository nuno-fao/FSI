# Trabalho Semanal #

## Tarefa 1

Executamos o código indicado, resultando na abertura de uma shell no código sem qualquer problema. A compilação foi também realizada com as devidas desativações dos mecanismos de segurança do Ubuntu.

O código resume-se a fazer um simples teste, onde, alocamos um buffer e colocamos uma shell code no seu interior. De seguida, declaramos uma função cujo endereço de "execução" é igual ao buffer que criamos. O que resulta, é que quando chamamos a função, é executado o código da stack, onde haviamos guardado o buffer. Sendo a CPU responsável por ler toda e cada instrução no shellcode. 

## Tarefa 2

Em seguimento do que foi pedido pelo guião, procedemos à compilação do código em C fornecido, aplicando o respetivo programa a um SET-UID e mudando a ownership do programa para o root antes de o tornar num SET-UID. 

Não se registaram quaisquer informações dignas de registo.

## Tarefa 3

Começamos por fazer o estudo do programa correndo o debugger associado ao gcc. Tal como sugerido no guião, colocamos um breakpoint no ponto de entrada da função bof() e corremos o programa. Seguido de um comando "next" procuramos obter o endereço no registo `$ebp` e do `&buffer` associado ao nosso código. Procuramos, ainda, obter o resultado inteiro da subtração entre os dois endereços. O resultado apresenta-se em inteiro, sem qualquer formato hexadecimal.

Assim, relativamente à nossa máquina, os nossos resultados foram:

![BufferOverflow](/images/BufferOverflow_1.png)

Relativamente à criação de um "badfile" que causaria um bufferoverflow attack, utilizamos o python script indicado, com a seguinte shellcode:

![BufferOverflow](/images/BufferOverflow_3.png)

A decisão dos valores no script python seguem a seguinte lógica:

Dado que o frame pointer se encontra no endereço 0xFFFFEB38 então o endereço de retorno encontra-se no endereço 0xFFFFEB38 + 4. Assim, o endereço a que podemos saltar será o 0xFFFFEB38 + 8. Temos, ainda, de descobrir onde colocar o nosso endereço de retorno, no input do badfile, de forma a ficar colocado no endereço de retorno da stack. Para tal fazemos a diferença entre o ebp e o buffer, obtendo `108`. Ora o resultado está na imagem, e sabemos que pela arquitetura 32-bits (dado que é um registo `$ebp`), o endereço de retorno será de 4 bytes acima de onde o registo `$ebp` termina. Ora, assim, a distância entre o oendereço de retorno e o início do buffer será realmente 108 + 4 = 112. 

Ora, assim, no python file, theremos: 

Offset = 112
Ret = 0XFFFFCBB0 (Dado que o código foi executado com gdb, a stack poderá estar bem acima, logo, teremos: 0xFFFFEB38 + 120 (~))
Start = 517 - len(shellcode) (Queremos que a shellcode esteja no final - O resto serão NOPs)

Assim, os dados no python file serão:

![BufferOverflow](/images/BufferOverflow_4.png)

Cujo resultado leva a:

![BufferOverflow](/images/BufferOverflow_2.png)

## CTF - Logbook

### Desafio 1

Tal como pedia no enunciado o primeiro passo foi verificar as proteção do programa. Usando checksec verificamos então que não existe um cannary a proteger o return address (Stack), a stack tem permissão de execução (NX), e as posições do binário não estão randomizadas (PIE), por fim existem regiões de memória com permissões de leitura, escrita e execução (RWX):

![checksec](/images/checksec.png)

De seguida analisamos o código fonte à procura de um buffer overflow que nos permita alterar o valor de variaveis na stack. Encontramos então essa mesma vulnerabilidade no código quando neste se tenta fazer scanf de uma string com 28 chars para um array de de apenas 20 chars. Felizmente esses 8 caracteres são exatamente o que precisamos para mudar o nome do ficheiro que é lido no programa. Para tal apenas temos que mandar um input ao programa com 28 caracteres onde os ultimos 8 são o nome do ficheiro alvo:

![input_buffer_overflow](/images/newcode.png)

Com isto testamos o exploit localmente com sucesso:

![run_local](/images/run_local.png)

E depois no servidor, também com sucesso:

![run_remote](/images/flag_desafio1.png)


### Desafio 2

Sendo este desafio semelhante ao primeiro fizemos começamos novamente por usar checksec para verificar as proteções do programa obtendo um resultado identico ao primeiro:

![checksec 2](/images/checksec2.png)

De seguida analisamos novamente o código para identificar a vulnerabilidade do programa. Encontramos assim um problema identico ao do primeiro desafio mas desta vez teriamos que alterar o valor de 2 variaveis em vez de uma com um overflow de 12 caracteres em vez de 8 (os primeiros 4 para a variavel val e os ultimos 8 para o nome do ficheiro). 
Assim, analisando o código percebemos que a variavel "val" deveria ter o valor 0xfefc2122. Isto é facilmente feito convertendo a string hexadecimal para um string normal usando uma ferramenta online. Com isso obtemos a string ""!üþ" no entanto, é importante escrever os valor por ordem inversa dado estarmos a alterar diretamente na stack.

Com isto tudo o input que mandamos ao programa será neste formato:
![newcode](/images/print_buffer_overflow.png)

Corremos primeiro localmente para testar, com sucesso:
![local sucess 2](/images/local_sucess_2.png)

E por fim remotamente para obter a flag:
![remote sucess](/images/remote_success_2.png)
