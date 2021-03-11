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


### Instalação

Caso ainda não tenha o Git instalado, basta usar o comando `sudo apt install git` para realizar a instalação no Ubuntu.

### Sua Identidade

A primeira coisa que você deve fazer ao instalar Git é configurar seu nome de usuário e endereço de e-mail. Esses dados são usados a cada commit para determinar a sua autoria. Podemos fazer isso através dos comandos abaixo:

```
$ git config --global user.name "Fulano de Tal"
$ git config --global user.email fulanodetal@exemplo.br
```

Importante: a opção `--global` garante que seu usuário e e-mail serão usados, por padrão, em todos os repositórios em que você commitar. Caso seja necessário alterar essas informações para um repositório específico, basta ir até o diretório desse repositório e executar os comandos acima sem a opção `--global`.

### Configurando o editor padrão

Podemos escolher o editor de texto padrão que será utilizado quando o Git precisar que editemos uma mensagem. Se não for configurado, o Git usará o editor padrão, que pode variar de acordo com o Sistema Operacional. Para configurar o editor de texto padrão, basta usar o comando abaixo:

```
$ git config --global core.editor [seu editor aqui]
```

## Operações fundamentais

### Inicializando um repositório local

Para inicializar um repositório vazio, basta utilizar o comando `git init` no diretório em que deseja criar o repositório. Um novo subdiretório oculto `.git` será criado, contendo todos os arquivos e a estrutura de diretórios internos necessários para o versionamento do seu repositório funcionar corretamente.

![](assets/git-basico-na-pratica/git-init.png)

### Estado atual do repositório

O comando `git status` nos informa como está o estado atual do nosso repositório, ou seja, em que `branch` estamos operando, se existe algum arquivo novo ou alterado, se existem arquivos preparados para serem commitados, se estamos sincronizados com o repositório, entre outras possibilidades.

![](assets/git-basico-na-pratica/git-status.png)

### Gravando alterações no repositório

O comando `git add` adiciona arquivos do diretório de trabalho, tanto rastreados e modificados quanto os não rastreados, à área de stage. Quando adicionamos arquivos, eles se tornam preparados para serem commitados.

![](assets/git-basico-na-pratica/git-add-pre-commit.png)

Tanto o comando `git add README.md hello_world.c` quanto o comando `git add .` podem ser utilizados para adicionar e preparar todos os arquivos que foram alterados ou não são rastreados. A diferença entre esses comandos é que o `git add .` é mais prático, pois recebe como argumento o ponto `.`, que significa **diretório atual**. Quando adicionamos um diretório, por padrão, todos os seus arquivos e pastas modificados também são adicionados recursivamente.

Importante: ao usar `git add .` devemos sempre ter a atenção de avaliar com o comando `git status` se realmente todos os arquivos que estão marcados como novos ou alterados devem ser de fato preparados para o commit.

Após prepararmos os arquivos, usamos o comando `git commit` para submeter as nossas alterações ao repositório. É aberto o editor padrão para que possamos escrever a mensagem de commit, que é obrigatória e sem ela o commit é cancelado. Podemos utilizar também o comando `git commit -m [mensagem]` para realizar um commit com a mensagem `[mensagem]` sem ter que abrir o editor padrão, de forma mais rápida e prática.

![](assets/git-basico-na-pratica/git-commit.png)

Em resumo, é da forma a seguir que ocorrem as mudanças de estado no Git.

![](assets/git-basico-na-pratica/git-estados.png)

### Acessando o histórico de commits

Para acessar o histórico de commits, podemos usar o comando `git log`.

![](assets/git-basico-na-pratica/git-log-normal.png)

Podemos adicionar algumas opções ao comando para filtrar ou facilitar a visualização do histórico de commits. Por exemplo, se adicionarmos a opção `--oneline`, mostramos o histórico de commits de forma simplificada.

`git log --oneline`

![](assets/git-basico-na-pratica/git-log-oneline.png)

Também podemos filtrar os commits por autor utilizando a opção `--author` como em `git log --author=Pedro`:

![](assets/git-basico-na-pratica/git-log-author.png)

Muitas vezes queremos saber o histórico de alterações de um arquivo do nosso repositório. Para isso, podemos usar o comando `git log --follow [arquivo]`. Por exemplo, abaixo temos uma possível saída para o comando `git log --follow LoginActivity.kt`:

![](assets/git-basico-na-pratica/git-log-follow.png)

## Fazendo bons commits

### Escrevendo mensagens de commit nos padrões da comunidade

Uma mensagem de commit é dividida em, pelo menos, duas partes importantes: assunto e corpo da mensagem. Um commit deve ter, pelo menos, o assunto da mensagem, que **não deve exceder 50 caracteres**. Um indicativo de que o commit necessita de um corpo de mensagem é que seu assunto está estourando os seu limite de 50 caracteres. Algumas comunidades aumentam esse limite de 50 caracteres para 72 caracteres, mas nunca excedem o limite de 72 caracteres.

O assunto do commit tem algumas particularidades importantes que devem ser respeitadas. A principal é o seu modo verbal, todo assunto de commit é escrito de forma imperativa. Por exemplo, não escrevemos "Corrigindo bug #234" ou "Corrigimos bug #234", e sim **"Corrige o bug #234!**. Além disso, existe o limite de 50 a 72 caracteres que falamos anteriormente, a convenção de pular uma linha entre o assunto e o corpo da mensagem e não usar ponto final no assunto do commit.

Sobre o corpo do commit, não existe uma convenção sobre o tamanho total do corpo. Ele pode ser tão detalhado quanto um determinado commit necessitar, porém é uma boa prática, e um pedido da comunidade, quebrar linhas a cada 80 caracteres para facilitar a leitura em um terminal clássico.

O motivo para todas essas regras é garantir a legibilidade e o entendimento quando determinada pessoa revisitar o seu commit. Ela irá ler a sua mensagem de commit para descobrir o que aconteceu com o código naquele exato momento. Por exemplo:

    Se aplicado, o que faz o commit d967462?

    commit d967462 (HEAD -> master)
    Author: Leandro Leite <leandro.leite@concrete.com.br>
    Date:   Tue Feb 23 18:30:00 2021 -0300

    Corrige um crash na tela de login

    Existia a possibilidade do campo email retornar null quebrando
    o contrato e causando um NullPointerException na LoginActivity.

A ideia é que o nosso assunto complete a frase "Se aplicado, este commit ...", que no nosso caso ficaria como **"Se aplicado, este commit corrige um crash na tela de login**. 

## Desfazendo alterações

Desfazer alterações é uma operação importante no Git. Nem sempre tudo sai como imaginamos, um arquivo que não deveria entrar no commit é submetido, queremos reverter o último commit, ir para um commit específico na linha do tempo, entre outras muitas situações que podem ocorrer. Aqui tentaremos mostrar as mais comuns.

### Deletando arquivos

O comando `git rm` serve para deletar arquivos do diretório de trabalho.  

![](assets/git-basico-na-pratica/git-rm.png)

Importante: caso o argumento do comando `git rm` seja um diretório, devemos utilizar a opção `-r`, semelhante ao comando `rm` que é bastante usado nos terminais.

### Removendo um arquivo da área de preparo

Supondo que um arquivo tenha sido alterado e preparado para o commit com o comando `git add`, podemos remover esse arquivo da área de preparo e fazer com que ele volte a ser apenas um arquivo modificado e candidato para commit. Para isso, usamos o comando `git reset`.

![](assets/git-basico-na-pratica/git-reset-basics.png)

### Retornando commits

Podemos acessar um commit específico na linha do tempo e fazer com que os seus arquivos voltem para o diretório de trabalho como arquivos modificados. Para isso, devemos usar também o comando `git reset`, porém de uma forma um pouco diferente, passando como argumento a hash de um commit obtida pelo comando `git log` ou de acordo com alguma operação usando a referência `HEAD`.

Exemplo de um commit retornado pelo comando `git log`:

    commit d96746659346a2a8083c119e331d87c25dbed538
    Author: Leandro Leite <l.carneiro.leite@accenture.com>
    Date:   Tue Feb 23 18:30:00 2021 -0300

        Adiciona arquivos README e hello_world.c

A hash do commit é a string `d96746659346a2a8083c119e331d87c25dbed538` e é ela que usaremos como argumento para o comando `git reset`.

![](assets/git-basico-na-pratica/git-reset-commit.png)

### Acessando commits

Podemos acessar um commit específico na linha do tempo e navegar para um estado antigo do projeto. Para isso, devemos usar o comando `git checkout` passando como argumento a hash de um commit obtida pelo comando `git log` ou de acordo com alguma operação usando a referência `HEAD`.

Usaremos a mesma hash do commit anterior, a string `d96746659346a2a8083c119e331d87c25dbed538`, como argumento para o comando `git checkout`.

![](assets/git-basico-na-pratica/git-checkout-commit.png)

Ao acessar um commit específico, recebemos a mensagem que estamos em no estado *detached HEAD*. Isso ocorre quando apontamos especificamente para um commit, ou seja, esse estado pode ser definido como um estado onde a referência HEAD não está apontando para algo com referência, como uma branch ou uma tag. É importante saber que se commitarmos alguma alteração em cima de uma *detached HEAD*, o Git continuará sem posicionar a HEAD em alguma referência e podemos perder essas alterações caso uma branch não seja criada.

![](assets/git-basico-na-pratica/git-checkout-commit-detached.png)

### A referência HEAD

HEAD é um ponteiro que referencia um objeto de commit. Por padrão, quando está em um estado normal, essa referência é o último commit feito em uma branch. Quando a HEAD aponta para um commit sem referência, como por exemplo um commit histórico em uma branch, ela entra em estado *detached*.

Como, geralmente, a HEAD aponta para o último commit de uma branch, é muito comum utilizarmos o modificador `~` quando utilizamos essa referência. Esse modificador significa, basicamente, parente de um determinado grau. Por exemplo, a referência `HEAD~1` significa parente de primeiro grau do último commit de uma branch, ou seja, seu penúltimo commit. Da mesma forma que `HEAD~N` é o parante do n-ésimo grau do último commit.

```
$ git log --oneline

84fde99 (HEAD -> master) Adiciona function.c
d967466 Adiciona arquivos README e hello_world.c
aa1cf8d Adiciona README

$ git checkout HEAD~2

Note: checking out 'HEAD~2'.
You are in 'detached HEAD' state. (...)

HEAD is now at aa1cf8d Adiciona README
```

### Revertendo commits

É possível reverter um commit no Git, como se fosse uma operação de desfazer. Ao reverter um commit, o Git identifica as alterações do commit a ser revertido e cria um novo commit, que será o último da branch, desfazendo essas alterações. O comando para reverter um commit é o `git revert`.

```
$ git log --oneline

1c43838 (HEAD -> master) Adiciona graph.c
84fde99 Adiciona function.c
d967466 Adiciona arquivos README e hello_world.c
aa1cf8d Adiciona README


$ git revert 84fde99

[master a06f865] Revert "Adiciona function.c"
 2 files changed, 2 insertions(+), 2 deletions(-)

$ git log --oneline

a06f865 (HEAD -> master) Revert "Adiciona function.c"
1c43838 Adiciona graph.c
84fde99 Adiciona function.c
d967466 Adiciona arquivos README e hello_world.c
aa1cf8d Adiciona README
```

### Diferença entre reset e revert

A operação de reverter tem uma diferença principal quando comparada a operação de resetar. O ato de reverter não altera o histórico do projeto, sendo uma opção mais segura para os projetos que são compartilhados em um repositório remoto. Isso porque se revertemos um commit, um novo commit é criado revertendo as alterações desse commit, sem alterar o histórico do projeto. Se usamos o comando `reset`, o comportamento é bem diferente. Quando resetamos um commit, voltamos para esse commit na linha do tempo e removemos todos os commits que foram feitos depois desse commit alvo, sendo necessário commitar novamente qualquer alteração que tenha sido feita após esse determinado momento no histórico da branch. Em diversos casos, é uma operação muito custosa e inviável, por esse motivo o comando `revert` é mais utilizado no cenário citado.

### Desfazendo as modificações

Se executarmos o comando `git checkout` em um arquivo, caso ele esteja marcado como modificado, esse arquivo voltará ao seu estado original, ou seja, não alterado.

![](assets/git-basico-na-pratica/git-checkout-commit.png)

Obs: também podemos usar como argumento o diretório atual através do `.` com o comando `git checkout .` e desfazer todas as modificações de uma só vez.

![](assets/git-basico-na-pratica/git-checkout-ponto.png)

## Ramificações do código: trabalhando com branches

Os repositórios do Git possuem a estrutura de uma árvore. Quando inicializamos um repositório, ele possui apenas um ramo (branch) padrão que geralmente possui o nome de master. 

![](assets/git-basico-na-pratica/git-branch-master.png)

Podemos pensar nas branches como caminhos diferentes que a base de código segue a partir de um ponto inicial. Esses caminhos podem ou não se encontrar em um ponto futuro.

![](assets/git-basico-na-pratica/git-branches-fluxo.png)

### Criando branches

Durante o desenvolvimento em projetos mais robustos e com equipes maiores, é comum ter a necessidade de seguir mais de um caminho a partir da branch padrão. Por exemplo, na maioria dos projetos, a branch que possui a base de código em desenvolvimento se chama `develop`. Vamos supor que dois times necessitem, cada um, criar uma funcionalidade nova nesse projeto. Os dois times irão partir inicialmente da branch develop, cada um criando sua própria branch com seu próprio código.

![](assets/git-basico-na-pratica/git-checkout-b-branch.png)

Importante: o comando `git checkout -b [branch]` é equivalente a sequência de comandos `git branch [branch]` e `git checkout [branch]`, sendo uma maneira mais prática de criar e já apontar para a branch nova.

### Listando as branches

Podemos utilizar o comando `git branch -l` para listar as branches de um repositório.

```
$ git branch -l

* develop
  feature/login
  feature/sign-up
```

A branch com uma marcação de destaque `*`  é a branch na qual estamos trabalhando no momento em que o comando foi rodado.

### Removendo uma branch

Para remover uma branch, basta utilizar o comando `git branch -d [nome]`.

$ git branch -d feature/login

Deleted branch feature/login (was 56bdd48).

$ git branch -l

* develop
  feature/sign-up

### Mesclando branches com merge

O merge é a forma que o Git tem de unir novamente duas branches que divergiram em certo ponto na linha do tempo do repositório. O comando `git merge [branch]` tem como alvo a **branch atual** que receberá a branch passada como argumento do comando `merge`.

![](assets/git-basico-na-pratica/git-merge-default.png)

No exemplo acima, a branch `feature/sign-up` foi mergeada na branch `develop`, ou seja, a `develop` foi atualizada com o código divergente presente na branch `feature/sign-up`.

O merge funciona combinando uma sequência de commits em uma única linha do tempo, representada por uma única branch. Existem duas operações comuns ao mergear duas branches, o *fast forward* e o *3-way merge*.

O fast forward é o caso mais simples e acontece quando uma branch é complementar a outra, ou seja, existe um caminho simples, uma diferença de adição ou subtração na linha do tempo entre uma branch e outra.

![](assets/git-basico-na-pratica/git-merge-ff-fluxo.png)

No exemplo acima, a branch `develop` não foi alterada desde o ponto de partida da branch `feature-1`. Quando o merge da feature-1 é realizado na develop, ocorre um *fast forward* e a HEAD da develop passa a apontar para o commit da HEAD da branch feature-1.

Já o 3-way merge ocorre quando uma branch não é complementar a outra, ou seja, existe mais código do que apenas o complemento presente na branch alvo. Nesse caso, o merge é realizado gerando um commit de merge, responsável por unir os códigos das duas branches. Essa união é feita através dos últimos commits das duas branches, gerando um terceiro commit. É por conta dessa operação que o merge é chamado de 3-way.

![](assets/git-basico-na-pratica/git-merge-3-way.png)

No segundo exemplo acima, a branch `develop` dá origem às branches `feature-1` e `feature-2`. A `feature-1` é mergeada primeiro, fazendo com que a `develop` se torne divergente em relação à branch `feature-2`. Por isso, quando o merge da branch feature-2 na develop é realizado, é gerado um commit de merge, caracterizando um merge 3-way. 

## Alterações temporárias e stash

Muitas vezes necessitamos trocar de branch, ou até mesmo atualizar a branch que estamos trabalhando, mas temos alterações pendentes que não estão concluídas a ponto de serem commitadas e também não são desprezíveis para serem descartadas. Podemos resolver essas situações com o comando `git stash`. O stash possibilita guardar essas alterações para serem utilizadas novamente em outro momento.

### Guardando alterações no stash

Usamos o comando `git stash` para guardar as alterações de uma branch sem realizar um commit.

![](assets/git-basico-na-pratica/git-stash.png)

Por padrão, o stash não guarda arquivos não rastreados. Para forçar e guardar também esses arquivos no stash, usamos o comando adicionamos a opção `-u` de *untracked* ao comando `git stash -u`.

![](assets/git-basico-na-pratica/git-stash-u.png)

Os stashes são criados e armazenados em uma pilha, de forma que quando são acessados, o último stash salvo é o primeiro a ser acessado.

### Listando stashes

Para listar os stashes criados, basta usar o comando `git stash list`

```
$ git stash list

stash@{0}: WIP on develop: fa2d54d Altera layout da tela de login
stash@{1}: WIP on feature/sign-up: f1e7b5b Adiciona request de cadastro
```

Obs.: caso queira personalizar o seu stash para ajudar na hora de identificá-lo na listagem, basta usar o comando `git stash push -m [Mensagem]`

```
$ git stash push -m "Meu stash"
Saved working directory and index state On develop: Meu stash

$ git stash list

stash@{0}: On develop: Meu stash
```

### Aplicando um stash

Tanto os comandos `git stash apply` quando `git stash pop` servem para aplicar o último stash que foi criado no espaço de trabalho atual. A diferença entre os dois comandos é que o `apply` aplica e mantém o stash, enquanto o `pop` aplica e deleta o stash da pilha. Se queremos recuperar um stash específico, devemos passar o identificador dele como argumento tanto no `apply` quanto no `pop`

![](assets/git-basico-na-pratica/git-stash-apply.png)

### Removendo stashes

Para deletar um stash, devemos seguir a mesma lógica de aplicar um stash. Se usamos apenas o comando `git stash drop`, o último stash criado é deletado, mas também podemos deletar um stash específico utilizando `git stash drop [id]`.

```
$ git stash list

stash@{0}: WIP on develop: fa2d54d Altera layout da tela de login
stash@{1}: WIP on feature/sign-up: f1e7b5b Adiciona request de cadastro

$ git stash drop

Dropped refs/stash@{0} (5514000d8aceda7e576feaebdf41143604794e6c)

$ git stash list

stash@{0}: WIP on feature/sign-up: f1e7b5b Adiciona request de cadastro
```

Para limpar todos os stashes de uma só vez, basta usar o comando `git stash clear`.

## Lidando com conflitos

Quando realizamos um merge ou aplicamos um stash, existe a possibilidade de aparecerem conflitos entre o código que está no seu diretório de trabalho e o código que será aplicado ou mergeado.

![](assets/git-basico-na-pratica/git-merge-conflict.png)

No exemplo acima, o merge da branch `feature/sign-up` na branch `develop` falhou por um ou mais conflitos no arquivo `LoginActivity.kt`. Existem algumas abordagens para resolver esses conflitos. Quando os conflitos são resolvidos, podemos seguir normalmente adicionando e commitando as nossas alterações.

### Aceitando alterações completas

Se desejamos aceitar as alterações da branch `feature/sign-up` por completo, ou da branch `develop` por completo, sem mesclar essas alterações, podemos simplesmente utilizar o comando `git checkout` com os arquivos alvos.

Usamos o comando `git checkout --theirs` para aceitar as alterações da branch que estamos trazendo, ou seja, a branch `feature/sign-up`. Usamos o comando `git checkout --ours` para mantermos as nossas alterações da branch que estamos atualmente, ou seja, a branch `develop`.

![](assets/git-basico-na-pratica/git-merge-ours.png)

### Resolvendo conflitos manualmente

Quando um arquivo está em conflito, nós podemos identificar facilmente onde ocorrem esses conflitos, pois o Git destaca bastante e mantém as duas versões conflitantes no mesmo arquivo.

![](assets/git-basico-na-pratica/git-merge-conflict-file.png)

A estrutura de um conflito é, basicamente, a seguinte: 

```
<<<<<<< HEAD da branch atual

/* bloco */

=======

/* bloco */

>>>>>>> nome da branch que está sendo mergeada
```

Como estamos na branch `develop`, o bloco relacionado a `HEAD` é a parte do arquivo `LoginActivity.kt` que está na branch `develop`, e o outro bloco, a parte do arquivo que está na branch `feature/sign-up`.

Para resolver um conflito de forma manual, basta remover o bloco que não será utilizado, removendo também os identificadores de branch e qualquer outros caractéres que fazem divisão entre os blocos.

## Trabalhando com repositórios remotos

clone, pull, fetch, push, comentário sobre perigo do push force

## Rebase (definir um nome bom)

https://git-scm.com/book/en/v2/Git-Internals-Git-Objects

pull com rebase?

https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase

## Extra: Usando apelidos para ganhar produtividade

https://git-scm.com/book/pt-br/v2/Fundamentos-de-Git-Apelidos-Git
