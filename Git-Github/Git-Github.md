## Git - Github - Sistema de versionamento de código distribuído

​							   Software é colaborativo, não é trabalho de um homem apenas..

_**windows**_                **_linux_**
seta pra cima	   seta pra cima - memória de comandos digitados
 CD						CD
dir						  ls  listar arquivos e subdiretórios (flag -a lista arquivos e pastas ocultas
mkdir					mkdir criar diretórios
del						 rm  deletar diretórios, no windows é diferente deletar arquivos e diretórios 

​							  (repositórios). parâmetros (flag) /S subdi ou tudo que estiver dentro /Q quiet - 							  não pede confirmação. No Linux falg -rf (recursivo, subdiret e force não pede 							  confirmação.
del/rmdir			 rm -rf  renomear diretórios/arquivos
cls						 Clear (Ctrl L) limpar a tela
Tab					   Tab (autompletar)
echo					 echo (printa a mesangem na tela ou impressora) parâmetro > redireciona para 							  um arquivo. ex.: echo > hello > hello.txt

cat						para visualizar conteúdo de arquivos (cat nome do arquivo.ext

pwd					 mostra caminho completo de onde você está (pasta)

move				   mv - move arquivos entre pastas (mv strogonoff.md ./receitas/ - quando num 							 terminal linux, dentro do Windows, precisa pedir premissão para criar pasta

​							 (sudo su - superusuário )

​							 cada comando possui flags (parâmetros)

#### Git por baixo dos panos

​	SHA1 (secure hash algoritm - NSA) - criptografia de 40 dígitos

Se voce gerar um sha1 para um arquivo, ele gera um conjunto único de cacteres criptografados. se altera uma vígula, ele já gera outro, mas se voltar e voltar o conteúdo original, ele gera o primeiro conjunto, ou seja, a criptografia sempre será a mesma para arquivos inalterados, isso gera identidade.

​	O Git gera sha1 não só para arquivos mas para cada objeto dele:



### Blobs (bolha)
​	echo 'conteudo' | git hash-object --stdin essa função espera um arquivo e a flag --stdin faz o comando entender que é apenas um texto e não um arquivo, mas está dentro de um objeto (blob).

​	Se rodar o próximo comando, o sha1 é diferente.
echo -e 'conteudo" | openssl sha1

​	Os arquivos ficam dentro do objeto chamado blob que contém metadados, então o blob contém o tipo do objeto, tamanho dele (arquivo ou string), \0 e o conteúdo do arquivo, se é texto, binário, etc...ex.:

- echo -e 'blob 9\0conteudo' | openssl sha1

​	**Ou seja , rodando o sha1 ele vai gerar o código mas ele guarda metadados, o Git gera blobs. blob contém metadados do Git**

​	O blob não armazena o nome do arquivo, apenas o sha1 dele e o conteúdo.

### Trees - as trees armazenam blobs

- Tree \0 e aponta para um blob, que tem um sha1, e também armazena o nome do arquivo.
- As trees organizam a estrutura de organização, localização dos arquivos
- Podem apontar para arquivos ou para outras trees (subdiretórios)
- As blobs tem o sha1 do arquivo, as trees pussem um sha1 com os metadados delas mesmas. se mudar uma vírgula no arquivo, alteram-se o sha1 das blobs e das trees (cascata)

### Commits 

- Objeto mais importante de todos** . o sha1 do commit é o hash de toda uma informação.
- O commit junta tudo, dá sentido a operação
- Commit aponta para uma árvore, para um parent (commit anterior a ele), aponta pra um autor e para uma mensagem também. 
  Escrever uma mensagem no commit explicando o que está acontecendo ( o que são os arquivos dentro das pastas), levam o nome do autor também.
  contém timestamp, data e hora de quando foi criado.
- Quando você tem um commit, você está garantindo que ninguém alterou aquele commit. Como os commits apontam para parents também, então fica traçada uma linha do tempo de alterações.
- O Commit é único para cada autor***

​	O Git é um sistema distribuído seguro porque mantém confiável que os arquivos no servidor são os mesmos em cada máquina mantenedora, não importando a quantidade.

​	Github não aceita mais apenas autenticação por username e senha.

### Chaves SSH

- Forma segura de conectar duas máquinas de forma segura.
  Chave Pública no Github (identidade)
  chave Privada é local, guardada em local seguro.
- Gerar chave ssh - ssh-keygen -t ed25519 -c + mail .. antes do nome indica que é arquivo oculto.
- Colocar senha
- Publica e Privada tem o mesmo nome, mas a pública tem a extensão pub

​	***Usar apenas a chave pública no Github***

​	Chave SSH e alias usado na chave (identificação do PC)

inicializar a chave ssh:

- eval $(ssh-agent -s) - mostra o pid do processo
- ssh-add id_25519 (sem extensão, a chave privada)**

​	Clonar repositório - não copiar o link http e sim o caminho ssh!

​		git clone (colar link ssh)



### Tokens (acesso pessoal)

- Pra clonar um repositório com chave token, copiar o url da página.
- Primeiros comandos com o Git
  - Iniciar o Git
    Iniciar o Versionamento
    Criar um Commit

​	**Em terminal, sempre o programa na frente**

- git init - Iniciar repositório
- git add - empurrar arquivos e iniciar o versionamento
- git commit -m ""

### Criar Repositório no Desktop

​	Antes de criar qualquer arquivo, rodar configuração inicial. Quando o objeto commit é criado ele leva o nome do autor.

​	configuração inicial **username e email

​	pode ser global ou apenas pro repositório

- git config --global user.email "email do usuário"  - String
- git config --global user.name "user name do usuário"  - String
- git add * (todos os arquivos e subdiretórios da pasta atual) move para staged.
- **git add strogonoff.md receitas/ (comando para adicionar arquivos que estão em subdiretórios. outra opção é entrar no subdiretório e fazer o git add)**
- git add nome_do_arquivo.
- git add . adiciona todas as modificações do nosso repositório local.
- git commit -m "commit inicial" (flag -m para passar mensagem, informação do commit) passa de staged para unmodified no repositório local.
- git status - ajuda a monitorar os status dos arquivos (untracked, unmodified, modified, staged)
- git restore --staged nome_do_arquivo (para unstage).
- git config --list (listar as configurações do Github) "q" para sair.
- git config --global --unset + propriedade a ser alterada (--list)
- git config --global + propriedade a ser setada, gravada (--list)
- git remote add origin + https (url do repositório remoto) (origin é um álias, um apelido, para não ficar digitando a url o tempo inteiro. Poderia ser qualquer nome, mas, por convenção, melhor usar origin.
- git remote -v (listas de repositório remoto que tem cadastrado)

- **Sempre usar git status no final, antes de empurrar, pra certificar que não existe erro ou falta de arquivos no commit, tudo está staged.**

- git push origin master (origin = álias) (master = branch que está enviando o código).
- git pull origin master - traz pro repositório local o arquivo com alteração de outro usuário, ou que você commitou na plataforma com uma alteração que não tem na versão local (conflito).
- git clone + url do código (clonar repositório - códigos)

​	Os commits são objetos do Git que dão significado as operações e que esses objetos em si carregam uma mensagem de texto juntamente com outros metadados como autor, a data em que foi criado

**Usar Markdown**


​	Textos de tamanho diferentes - Tag h1...h6 ---> em Markdown #...######

​	**Markdown é uma forma humanizada de se escrever html sem saber html

​	*.md - estruturas simples para criar textos html

​	no Markdown dá pra usar emoticon - colocar sinal de " : " e nome do emoticon
negrito - **texto**
itálico - _texto_
​	Lista não ordenada (iniciando com ponto grande em negrito) - sinal de " - " e espaço.

***

### Ciclo de vida dos arquivos dentro do Git

- git init - além de criar a pasta .git/, inicializa o repositório. Quando usamos esse comando estamos criando um repositório dentro da pasta.
- Untracked - são arquivos que ainda não temos ciência deles.
  git add - arquivo esta untracked, então, empurrou em staged.
- Tracked - arquivos que temos ciência deles, arquivos rastreáveis pelo Git. **Ciclo Unmodified - modified - git add - staged - commit - unmodified**

- Unmodified - arquivo dentro do repositório do git que ainda não sofreu alteração. Abrir o arquivo e alterar algo muda o status para modified. Git compara o sha1 dos arquivos para certificar alterações.
  Se remover o arquivo unmodified ele será movido para Untracked novamente (não temos ciência dele)

- Modified = unmodified que sofreu alteração (alteração de texto, código, etc). rodando Git add em arquivo modified move ele para staged.

- Staged (conceito chave para entendimento) - são arquivos que estão se preparando para fazer parte de outro tipo de agrupamento, estão se preparando para um commit (envelopamento das modificações dos arquivos, com significância, escreve uma mensagem, que carrega autor, data, metadados) que move ele para Unmodified, até que sofra alguma alteração. Acabou-se as alterações dos arquivos - cria-se Snapshot (foto)

### Ambiente de Desenvolvimento - máquina local

​	Working Directory  -  Staging Area  -  Local Repository

​	Alterações locais não repercutem imediatamente no repositório remoto.

​	Quando fazemos um commit ele pega os as alterações e empurra para unmodified no repositório Local (unmodified). Lembrar do ciclo. Só conseguimos enviar para o repositório remoto os arquivos "commitados", que estão em unmodified.

​	Criar Arquivo de Index (com informações sobre as receitas que estarão no repositório) pasta do repositório principal.

​	README.md - Capa do Repositório Remoto "Livro de Receitas"

​	Servidor - Remote Repository - git push origin master

### RESOLVENDO CONFLITOS (conflito de merge)

​	Arquivos no repositório local = arquivos no repositório remoto.

​	Ocorrem problemas de conflitos quando duas ou mais pessoas editam a mesma linha de código.

​	Alguém editou antes de você e empurrou, quando você for fazer o commit o Github vai dizer que já existe uma versão mais atual, e que é diferente da sua que está editando, pedindo pra você baixar a versão remota, editar suas linhas e depois commitar novamente.. Github não faz nada, ele deixa a gente editar manualmente e depois empurrar novamente.

### Como baixar (clonar) repositórios de outros

- Clica em code e copia a url

- Entra no git bash

- retorna a pasta de trabalho principal (workspace)

- git clone