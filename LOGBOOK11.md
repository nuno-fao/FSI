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
