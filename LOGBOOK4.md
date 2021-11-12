## Tarefas - Semana 4

**::Tarefa 1::**

Tendo como objetivo a manipulação de variáveis de ambiente, começamos por executar o comando `printenv` na bash do OS na VM. Obtemos uma listagem relativamente grande de variáveis ambiente. Os resultados estão tal e qual como listados na seguinte imagem:

![PrintEnv](/images/printenv.png)

Adicionamos também uma variável ambiente de teste ('yo') através do comando `export` e com o comando `printenv` confirmamos os valores desta. Verificamos que a variável foi adicionada. Após a execução, removemos a mesma variável com o comando `unset` e voltamos a confirmar o resultado. A imagem abaixo ilustra os procedimentos executados, assim como as respetivas confirmações empíricas:

![PrintEnv](/images/export_unset.png)


**::Tarefa 2::**

Seguimos os procedimentos indicados no guião. Utilizando o ficheiro `myprintenv.c` fornecido, procedemos à sua compilação através do `gcc`. De seguida, executamos o código duas vezes, um com o print das variáveis a ser feito no processo pai, e outra execução do lado do processo filho. O output foi guardado em dois ficheiros, cuja comparação é a seguinte:

![ForkTest](/images/forkoutputcomparison.png)

Com estes dados concluímos que ambos os processos gerados por `fork()` mantêm as mesmas variáveis ambiente que as do processo que os criou. 

-----------
## CTF - Logbook

