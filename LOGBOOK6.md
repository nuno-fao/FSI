# Tarabalho semanal #

## Tarefa 1
Como tarefa inicial tinhamos então que crashar o servidor usando com "format string". Para tal apenas precisamos de dar como input ao servidor um string com multiplos '%s' que faz com que o valor obtido seja interpretado como um endereço. Quando esta string chega ao printf, é provavel que este vá tentar imprimir algo de um endereço que não tem acesso e por isso crasha como se pode ver na seguinte imagem (não observamos o as linhas com 'target value after' nem de 'returned properly'):

![semana6_t1](images/semana6_t1.png)

## Tarefa 2
