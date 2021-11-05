# Trabalho realizado na Semana #3
# CVE-2020-5260

## Identificação

### Descrição geral:
 - Em 2020 uma vulnerabilidade do Git foi descoberta onde este poderia ser enganado a enviar as credenciais erradas a um host. Esta vulnerabilidade era provocada fornecendo um URL malicioso, com um newline, ao comando “git clone” porque os programas de gestores de credenciais externos que o git usa não conseguem tratar valores com newlines.
### Sistemas:
 - Ubuntu (v16.04; v18.04; v19.10)
 - Debian (v8.0; v9.0; v10.0)
 - Fedora (v30; v31; v32)
 - Leap (v15.1)
### Aplicação:
 - Git

## Catalogação

### Quem: 
 - Felix Wilhelm from Google Project Zero
### Quando: 
 - 12/03/2020
### Como: 
 - O erro é cometido pela adição de um newline, hardcoded no URL que é enviado para o protocolo de gestão de credenciais dos utilizadores, utilizado pelo Github, que não é capaz de tratar de newlines. Foi resultado de uma escolha de design intencional de forma a manter a simplicidade do protocolo. Isto levou a equipa a assumir que nunca seria enviado qualquer newline para o gestor de credenciais. No entanto, esta assunção abre uma vulnerabilidade, dado que criando um URL personalizado e com um newline no seu segmento, levaria o protocolo a ter que tratar desta gestão.
### Bug-bounty:
 - None
### Nivel de gravidade: 
 - Confidencialidade: há leak de informação confidencial
 - Integridade: não afetada
 - Disponibilidade: não afetada
 - Não necessita de condições especiais para ser replicado nem habilidades específicas ou autenticação
### Fix:
 - Verificar a existência de um newline no URL antes de o enviar ao protocolo. Se existir parar a execução do processo de clone. Este fix foi feito por Jeff King, um membro da equipa do GitHub. https://github.com/git/git/commit/9a6bbee8006c24b46a85d29e7b38cfa79e9ab21b
### CVSS 2:
 - 5.0
### CVSS 3:
 - 7.5 (NVD) e 9.3 (Git)

## Exploit
 - Este exploit é causado pela manipulação de URL e injeção de newlines no mesmo.
 - Não há informação sobre automação relativa a este cve e no Metasploit também não.

## Ataques
 - Não há relatos de ataques que envolvam este cve.
 - No entanto apresenta um potencial algo elevado para causar dano evidentemente devido ao leak de informação privada para o host errado.

