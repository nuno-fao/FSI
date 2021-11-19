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

Esta tarefa toma como partida o reconhecimento da utilidade da variável ambiente "PATH". Esta variável terá, como consequência, a localização de onde são "levantadas" as libraries. A ordem de leitura é feita sequêncialmente pelas sucessivas variáveis, pelo que, dado o objetivo desta tarefa, para substituirmos o comando shell `ls`, teremos que colocar o diretório da nossa nova versão do `ls` (possívelmente maliciosa), no início da PATH.

Para tal criamos um ficheiro `ls.c`, guardado em `/home/seed/Documents` que foi posteriormente compilado com o comando `gcc ls.c -o ls`. Isto produz um ficheiro binário, compilado, cujo nome será `ls`. Este procedimento encontra-se assinalado na imagem abaixo:

![Task6](/images/1_T6_1.png)

Na mesma imagem acima, notamos que foi adicionada à variável PATH, no ínicio, a localização do ficheiro de execução `ls`. Tal ação, resulta em que o nosso ls seja selecionado, antes do "default" `ls`, dado que a leitura do PATH é feita sequêncialmente, começando pelos primeiros diretórios listados.

De seguida, realizamos um procedimento semelhante à Tarefa 5, compilando o código dado no guião, que executa o chamada: `system("ls");` e tornando o programa num SET-UID program. A imagem abaixo ilustra a execução:

![Task6](/images/1_T6_3.png)

Finalmente, executando o programa task6.c (compilado na imagem acima), visualizamos a execução do nosso novo ls, em vez do "ls" pré-definido. As conclusões obtidas é que o SET-UID e a variável PATH, levam a condições de insegurança que devem exigir grandes cuidados. Isto porque permite a um utilizador qualquer redefinir o seu próprio PATH e executar um programa SET-UID, com os comandos de root (caso o owner seja definido como tal). De um simples utilizador, passamos a ter um utilizador que tem poder absoluto sobre a máquina e que pode realizar o que bem entender.


**:: Tarefa 7 ::**

Começamos por realizar os passos iniciais do guião, criando um ficheiro mylib.c e compilando com os comandos indicados, sendo estes:

`gcc -fPIC -g -c mylib.c` - As flags têm os seguintes significados: -fpic (dynamic linking, código de posição independente) | -g (debugging) | -c (compilar, mas sem dar linking)

`gcc -shared -o libmylib.so.1.0.1 mylib.o -lc` - As flags têm os seguintes significados: -shared (pode ser partilhado por outros objetos para formar uma library) | -o (produz o output do ficheiro)

Realizamos também o pedido feito pelo guião de adicionar este executável à LD_PRELOAD:

`export LD_PRELOAD=./libmylib.so.1.0.1`

A imagem representa os passos realizados: 

![Task7](/images/T7_1.png)

De seguida, criamos um ficheiro `myprog.c` que faz uso da função sleep() definida no `mylib.c`. O processo também se encontra ilustrado abaixo:

![Task7](/images/T7_2.png)

No entanto, de uma forma mais costumizada, decidimos adicionar a linha: `system("env | grep LD")` ao myprog.c, que nos indicará as variáveis, no ambiente de execução. Isto facilita o trabalho. Foi então executado o programa de imediato, testando a primeira condição exigida no enunciado:

![Task7](/images/T7_3.png)

Pela imagem acima, constatamos que a nossa função `sleep()` foi executada com o utilizador em ambiente normal, e que a LD_PRELOAD environmental variable se encontrava disponível.

Realizando o segundo teste pedido, tornamos o programa SET-UID em modo root user, com os resultados abaixo onde se pode facilmente verificar que o printf() do nosso sleep não foi executado. No entanto, o sleep() da library standard foi executado no lugar do nosso. Note-se, ainda, que a variável ambiente LD_PRELOAD já não está presente! A imagem abaixo contasta estes resultados:

![Task7](/images/T7_4.png)

Partimos para o terceiro teste, onde mantivemos o programa em SET-UID, com o mesmo owner: o root. Apenas passamos a batch para root com o comando: `sudo bash`. Executando, verificamos que o nosso sleep() foi executado e que a variável ambiente LD_PRELOAD continuava lá.

![Task7](/images/T7_5.png)

Finalmente, mudamos o owner para um utilizador normal e executamos o programa nesse mesmo utilizador. Verificaram-se os mesmos resultados que na imagem acima, mas fica a seguinte imagem como comprovação:

![Task7](/images/T7_6.png)

Tendo todos estes resultados em mente, podemos concluír que a variável ambiente LD_PRELOAD permanece ativa se o utilizador que executa o programa SET-UID, for o mesmo que o owner. Caso contrário, por uma questão de segurança, esta variável é imediatamente eliminada. No terceiro e último caso, como os owners eram os utilizadores a executar o programa, a variável LD_PRELOAD permaneceu nos ambientes de execução. Já no segundo, esta condição não se verifica, pelo que a variável é imediatamente eliminada por questão de segurança, impedindo que um utilizador possa "abusar" dos previlégios adquiridos com o SET-UID. Finalmente, o primeiro caso é um subconjunto do terceiro e último caso, onde, por default, o owner é o próprio utilizador. Todavia, dada a inexistência da SET-UID flag, este mecanismo de segurança não é colocado em prática.


**:: Tarefa 8 ::**

Começamos por compilar o ficheiro `catall.c` fornecido no guião. De seguida atribuimos o owner do executável como sendo o `root` e depois tornamos este programa num SET-UID program. Para o efeito, corremos um simples teste para verificar que, de facto, um ficheiro por nós criado (`superfile`) era mostrado corretamente. A imagem abaixo ilustra os resultados obtidos nesta simples fase inicial:

![Task7](/images/T8_1.png)

De seguida, testamos o acesso através de um utilizador normal, usando `su noob`. Tal como anteriormente, o programa era executado corretamente, mostrando os conteúdos do ficheiro `superfile` passado como argumento. No entanto, sendo este argumento lido pelo `system()` call, passamos o seguinte conteúdo como argumento: "`superfile;/bin/sh`". Isto resulta na pesquisa do ficheiro, mas, dado o system() call, ele posteriormente executa o executável `/bin/sh` que abre a shell necessária. Dentro desta shell, podiamos ter acesso equivalente ao `root`, que nos permitiu eliminar o ficheiro. A imagem abaixo ilustra esses resultados:

![Task7](/images/T8_2.png)

De seguida, todos estes procedimentos foram realizados sob o mesmo ambiente que os dois últimos procedimentos, mas comentando o `system()` e utilizando antes `execve()`. Neste caso, a vulnerabilidade / ataque do passo anterior já não resultou, tendo a shell iniciado com permissões não pertencentes ao `root`. As duas imagens abaixo ilustram este evento:

![Task7](/images/T8_3.png)

![Task7](/images/T8_4.png)

As conclusões obtidas nestes processos, permitem concluír dois aspetos:

Primeiro, a leitura de uma string por parte do `system()` (por exemplo: "echo Hello;/bin/sh"), não é vista como um único argumento. O System, por si mesmo, quebra a cada marcação do character ";", realizando os comandos individualmente. Já o `execve()` trata a string como um argumento único, não quebrando em comandos individuais. Ou seja, esta última alternativa possuí procedimentos cuidadosos, ao contrário do `system()` que permite o tratamento de dados sem a sua verificação.

Segundo, o `execve()` substitui o processo, pelo indicado, eliminando assim a possibilidade do user normal ser visto como o `root` user quando executa um comando SET-UID que tem permite chamadas externas usando `execve()`.


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




