# Tarabalho semanal #

## Tarefa 1
Como tarefa inicial tinhamos então que crashar o servidor usando com "format string". Para tal apenas precisamos de dar como input ao servidor um string com multiplos '%s' que faz com que o valor obtido seja interpretado como um endereço. Quando esta string chega ao printf, é provavel que este vá tentar imprimir algo de um endereço que não tem acesso e por isso crasha como se pode ver na seguinte imagem (não observamos o as linhas com 'target value after' nem de 'returned properly'):

![semana6_t1](images/semana6_t1.png)

## Tarefa 2
Nesta tarefa temos que usar o format string para imprimir o valor de variaveis tanto na stack como na heap.

### stack
Para isto apenas precisamos de fornecer uma string com uns caracteres cujos valores são faceis de identificar em hexadecimal '%.8x'. Fornecemos então '@@@@' (hexadecimal 40404040) seguidos de multiplos '%.x' para imprimir os vários endereços e valores.
Analisando o output do programa verificamos que a uma distancia de 64 posições encontramos o valor da de '@@@@' o que indica que é necessário 64 'format specifiers' para imprimir os primeiros 4 bytes do nosso input

### heap
