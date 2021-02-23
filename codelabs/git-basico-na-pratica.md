summary: Git básico na prática
id: git-basico-na-pratica
categories: Git
tags: treinamento, codelab
status: Published
authors: Leandro Leite
Feedback Link: http://google.com

## O que é Git?

O Git é um sistema distribuído de controle de versão utilizado pela grande maioria dos desenvolvedores atualmente. Com ele, podemos salvar nossas alterações sem de fato sobrescrever as versões anteriores, tornando possível ter um histórico de alterações de um projeto e também voltar e acessar qualquer momento e alteração específica.

Ser um sistema distribuído significa que todo desenvolvedor trabalhando em um repositório Git possui uma cópia inteira do projeto. Isso torna muito mais fácil controlar o fluxo de novas adições de código ao projeto, mesmo que elas sejam feitas por vários desenvolvedores ao mesmo tempo.

## Como funciona o Git

### Repositório

Um repositório do Git é um diretório com um conjunto de arquivos e suas diferentes versões devido às modificações que ocorreram ao longo do tempo. Cada colaborador de um projeto tem acesso ao seu repositório para baixá-lo em sua máquina e realizar suas modificações localmente para, então, submeter essas alterações.

### Pontos de verificação

Quase tudo no Git é feito através dos pontos de verificação do projeto, chamados de commits. Esses pontos são formados por um conjunto de alterações em um ou mais arquivos e uma mensagem que tem como objetivo descrever as modificações nesse determinado ponto.

https://developer.ibm.com/technologies/web-development/tutorials/d-learn-workings-git/

### Ramificações

Uma ramificação, chamada de branch, representa uma linha de desenvolvimento independente. Podemos pensar nas branches como uma forma de criar um novo e limpo diretório de trabalho para adicionarmos nossas alterações e em determinado momento, como é comum acontecer, juntarmos em outra branch.

### Rótulos

Um rótulo, também chamado de tag, é usado para marcar determinados pontos importantes no histórico de modificações de um repositório. É muito comum marcarmos os commits de mudanças de versão de um projeto.

### Fluxo comum de trabalho no Git

![](assets/git-basico-na-pratica/git-como-funciona.png)

Vamos interpretar a imagem acima juntos!

Existem 5 branches na imagem de exemplo: `master`, `develop`, `feature-A`, `bugfix` e `feature-B`. Também existem 3 tags `v1`, `v2` e `v3` na imagem, além de diversos commits representados pelos círculos em cada branch.

No dia 1, o ponto inicial do projeto, a branch `master` está vazia, pois não existe nenhum release em produção, a branch `develop` tem a versão inicial com a estrutura do projeto e a branch `feature-A` teve seu início a partir do código presente na branch `develop`. A feature A é desenvolvida e, ao seu término, seu código é agregado ao código da branch `develop`, que também envia seu código para a branch `master`, gerando o release com a tag `v1`.

Em um segundo momento, é iniciado o desenvolvimento da feature B na branch `feature-B` a partir da branch `develop`. Durante o desenvolvimento dessa feature, em um terceiro momento, um bug é encontrado na `develop` e sua correção é iniciada na branch `bugfix`. O bug é resolvido e seu código de correção é enviado para a `develop` e em seguida para a `master`, gerando o release com a tag `v2`. Repare que o bug foi resolvido antes do término da implementação da feature B e o código base da branch `develop` foi atualizado, fazendo com que a branch `feature-B` não esteja mais sincronizada com a `develop`, sendo então necessário fazer esse *sync* antes do término e envio do código para gerar o release `v3`.

### Ciclo de vida e estados dos arquivos no Git

![](assets/git-basico-na-pratica/git-lifecycle.png)

Cada arquivo no seu diretório de trabalho, também chamado de *working directory*, pode estar em um dos dois estados: rastreado e não rastreado (untracked). Os arquivos rastreados são os que foram incluídos em um determinado ponto do repositório e possuem três estados: não modificado (unmodified), modificado (modified) e preparado (staged). Os arquivos não rastreados são aqueles que ainda não foram incluídos em nenhum ponto do repositório e também não estão na área de stage (staging area).

Quando um arquivo untracked é adicionado, ele se torna um arquivo preparado, ficando na staging area aguardando o commit para se tornar um arquivo rastreado unmodified. Isso ocorre porque uma vez que o arquivo staged é commitado, o estado atual da branch passa a ser o do último commit e contém todas as últimas alterações desse arquivo, fazendo com que o mesmo se torne não modificado.

Uma vez que um arquivo unmodified seja alterado, o Git o identificará como modified. Os arquivos modificados podem ser preparados, adicionados ao pacote de um commit, e se tornarem arquivos staged. Quando os arquivos que estão classificados como staged são commitados, eles se tornam unmodified pelo mesmo motivo visto no caso dos untrackeds anteriormente.

Caso um arquivo unmodified seja removido, esse arquivo deixará de existir no diretório de trabalho, se tornando também do tipo não monitorado.

## Configuração inicial

https://git-scm.com/book/pt-br/v2/Come%C3%A7ando-Configura%C3%A7%C3%A3o-Inicial-do-Git

## Operações fundamentais

init, add, rm, status, checkout, clean -fd, e commit

https://www.git-scm.com/book/pt-br/v2/Fundamentos-de-Git-Gravando-Altera%C3%A7%C3%B5es-em-Seu-Reposit%C3%B3rio

## Desfazendo alterações

remover último commit, apendar um commit, tudo aquilo que a gente quer desfazer ou refazer e não sabe

## Ramificações do código: trabalhando com branches

branch, stash e checkout

## Trabalhando com repositórios remotos

clone, pull, fetch, push

## Lidando com conflitos

diff, merge

## Rebase (definir um nome bom)

https://git-scm.com/book/en/v2/Git-Internals-Git-Objects

pull com rebase?

## Convenções de uso do Git (ver nome)

git flow e trunk based development

## Extra: Usando apelidos para ganhar produtividade

https://git-scm.com/book/pt-br/v2/Fundamentos-de-Git-Apelidos-Git
