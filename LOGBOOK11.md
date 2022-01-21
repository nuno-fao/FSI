# Tarefas

## Task 1

De acordo com o indicado no guião, criaram-se os diretórios e copiou-se o cfg file do ssl para o novo diretório. Editou-se, ainda o conteudo do ficheiro cfg para refletir a estrutura do diretório, assim como do parâmetro comentado.

![T1](images/L/T1/Screenshot%202022-01-21%20at%2014.56.12.png)

Prodecedemos, depois, à criação do CA:

![T1](images/L/T1/Screenshot%202022-01-21%20at%2015.00.17.png)

Em resposta às questões temos:

- What part of the certificate indicates this is a CA’s certificate?

![T1](images/L/T1/Screenshot%202022-01-21%20at%2015.05.49.png)

- What part of the certificate indicates this is a self-signed certificate?

![T1](images/L/T1/Screenshot%202022-01-21%20at%2015.06.49.png)

- In the RSA algorithm, we have a public exponent e, a private exponent d, a modulus n, and two secret numbers p and q, such that n = pq. Please identify the values for these elements in your certificate and key files.

O resultado encontra-se presente neste [ficheiro de texto](https://git.fe.up.pt/fsi/fsi2122/m03g09/-/blob/main/images/L/rsa_data.txt). Nomeadamente as partes: modulus, privateExponent, publicExponent, prime1 e prime 2 para p e q.

## Task 2

Criou-se um pedido de key request através do seguinte comando tal como indicado no guião, cuja password seria: dees.

![T1](images/L/Screenshot%202022-01-21%20at%2015.22.41.png)

## Task 3

De seguida, com o request gerou-se o certificado para o servidor utilizando o seguinte comando (isto tendo em conta os resultados de T1):

![T1](images/L/Screenshot%202022-01-21%20at%2015.33.35.png)

## Task 4

Seguindo os passos do guião, foi utilizada a pasta "volumes" no diretório do github utilizado para este trabalho, por forma a guardar os dados dos nossos certificados. Isto permitiu a que o servidor apache conseguisse aceder a estes certificados. Para tal, acedemos ao terminal do component do docker e iniciamos o apache através deste (isto já com o cfg file atualizado para considerar os dados no "volume"). Com isto, o resultado que obtivemos utilizando um HTTPS, foi: 

![T1](images/L/Screenshot%202022-01-21%20at%2016.17.19.png)

Permitiu-nos, assim, aceder ao nosso website.

## Task 5

Utilizou-se o repositório default, ou seja, o bank32 que já vinha incorporado no docker. Através da mudança do ficheiro /etc/host, notamos no seguinte resultado quando tentavamos aceder ao www.example.com. Ou seja, acediamos ao nosso index e não index estipulado pelo www.example.com. Notamos que, todavia, este resultado só era produzido no servidor com porta 80 (non-http).

![T1](images/L/Screenshot%202022-01-21%20at%2016.37.28.png)

## Task 6

Utilizou-se o resultado utilizado na Tarefa 1, mas desta vez utilizando a chave deste, isto permitiu-nos que, desta vez, ao aceder ao https do example.com, o seguinte fosse apresentado, mostrando o funcionamento pretendido:

![T1](images/L/working.png.png)
