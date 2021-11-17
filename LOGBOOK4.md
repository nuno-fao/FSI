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

**::Tarefa 3::**

Na primeira execução realizada sob o código no ficheiro `myenv.c` constatamos que não foi realizado qualquer output por parte do programa `"/usr/bin/env"`. 

Realizando a modificação pedida no guião, que substituí o `NULL` com `environ`, constatamos que o output das variáveis ambiente já foi realizado.

Assim, como conclusão, é possível passar variáveis ambiente através do `execve()` mas que é necessário passar através do último argumento, tal como indicado no último argumento do protótipo da função:

`int execve(const char *pathname, char *const argv[], char *const envp[]);`


**:: Tarefa 4 ::**

Tal como listado no guião, foi executado o código listado, utilizando a função `system()`. O programa executado (`"/usr/bin/env"`), tal como listado na Tarefa 3, é responsável por imprimir as variáveis ambiente neste mesmo. 

Ora, após a execução, constatamos que, de facto, chamando a função `system()` as variáveis foram impressas como esperado. A imagem seguinte, comprova este facto:


![SystemTest](/images/systemtest.png)


**:: Tarefa 5 ::**

Após execução dos passos indicados no guião, foram colocadas três variáveis ambiente, tendo dado append aos conteudos da `LB_LIBRARY_PATH` e o `PATH` por forma a preservar a estabilidade do OS. Quando foi executado o programa no final, constatamos que os conteúdos da variável ambiente `LB_LIBRARY_PATH` tinha sido eliminados. 

Concluímos que tal resultado se deve a um mecanismo de segurança do sistema operativo, por forma a prevenir que usuários normais, sem permissões, possam desempenhar o papel de root e executar libraries maliciosas. 

Segue-se a seguinte imagem que comprova os resultados indicados:
![Task5](/images/task6process.png)


**:: Tarefa 6 ::**


-----------
## CTF - Logbook

### Reconhecimento
Primeiro passo para encontrar uma vulnerabilidade neste site foi então pesquisar informação relevante como a versão do mesmo e alguns plugins usados. Após alguma navegação encontramos na página http://ctf-fsi.fe.up.pt:5001/product/wordpress-hosting/#comment-3 a informação mencionada.

![info](/images/info.png)

### Pesquisa e escolha da vulnerabilidade e exploit
Com estes dados temos tudo necessário para procurar por um cve. Pesquisando na [exploit database](https://www.exploit-db.com/), encontramos este exploit:
- [CVE-2021-34646](https://www.exploit-db.com/exploits/50299).

Felizmente podemos também encontrar neste site um exploit para explorar esta vulnerabilidade

### Explorar a vulnerabilidade
Sendo que o exploit era um simples script de python, depois de o baixar bastou correr e obtemos os seguintes resultados:

![Links Generated](/images/links_generated.png)

Segundo o autor do script, um destes links deve permitir aceder ao site como admin e assim foi:

![Admin Log](/images/authentication.png)

De seguida acedemos ao site indicado no moodle do ctf:

![Posts](/images/posts.png)

E encontramos a flag dentro duma das mensagens:

![Flag](/images/flag.png)




