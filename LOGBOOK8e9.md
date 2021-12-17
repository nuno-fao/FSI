# CTF #

## Desafio 1

Observando o código php é evidente que podemos fazer com que a query retorne algo que nos permita fazer login através duma SQL injection

Para tal, através do seguinte input forçamos a que a query retorne os vários utilizadores, que para o 'IF' implementado no php é suficiente para logar:

![d1 input](images/ctf7d1_injection.png)

E assim encontramos a primeira flag:

![d2 input](images/ctf7d1_flag.png)

## Desafio 2

Para este desafio, é primeiro necessário encontrar um ponto de contact com o utilitário unix. Encontramos então uma funcionalidade que nos permite dar ping a um determinado host que provavelmente é feita por esse mesmo utilitário.

Só nos resta enviar um comando que nos permita dar o ping sem erro e, ao mesmo tempo, ler o conteudo do ficheiro que contem a flag.

Para tal um input deste género chega:

![d2 input](images/ctf7d2_injection.png)

E assim obtemos a segunda flag:

![d2 flag](images/ctf7d2_flag.png)
