# GIT basic to hero 
## Controle de versão VCS
- Sistema para **registrar alterações** ao longo do tempo, histórico de mudanças e responsáveis
- **Experiência controlada** e segura sem grandes esforços
- Fim do arquivo_tcc_versão_final_(7).doc

### Sistemas Locais de Controle de Versão
<img src="./static/vcs_-_modelos_-_controle-versao-local.png" alt="controle de versão local" width="400"/>

### Sistemas Centralizados de Controle de Versão
<img src="./static/vcs_-_modelos_-_controle-versao-centralizado.png" alt="controle de versão centralizado" width="400"/>

- Ambiente **colaborativo**
- Ambiente controlado: todos sabem quais arquivos estão sendo utilizados
- Por muitos anos, este tem sido o padrão para controle de versão.
- Falha no servidor para todo o projeto

### Sistemas Distribuídos de Controle de Versão (DVCS)
<img src="./static/vcs_-_modelos_-_controle-versao-distribuido.png" alt="controle de versão distribuido" width="400"/>

- Cada clone é de fato um backup completo
- **Colaboração concorrente**


#### Git
- Lançado o git em 2005
- Nasceu para resolver conciliar problemas de código colaborativo
    - antes compartilhamento de arquivos, e-mails, etc
    - Passou a utilizar BitKeeper
        - passou a ser pago
- Maioria das operações são **locais**
    - novas **branches**
    - **histórico** do projeto
        - **comparações** entre quaisquer versões no histórico
- integridade (checksum sha-1)
- ~~sempre~~ adiciona dados
    - cuidado ao commitar senhas
----

## Git

<img src="./static/git-history.png" width="400px" alt="histórico de commits" />

### [Git Objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects)

#### Blobs

#### Tree
    - filename
    - directories
<img src="./static/git-object-tree.png" width="400px" />

#### Commit
- Árvore pai
- parent commit
- autor
- Mensagem

<img src="./static/git-object-commit.png" width="400px" />


### 3 estados

#### modified
- rastreados, modificados mas não irão pro commit

#### staged
- pronto para ser comitado

      $ git add
    - atualiza índice do conteúdo atual na working tree

#### committed
- seguramente armazenados

      $ git commit
    
    - atualiza a etiqueta do branch atual

*Cuidado com arquivos modified e staged simultaneamente

    $ echo '\nADMIN_EMAIL=admin@admin.com.br'>config
    $ git add config
    $ echo '\nERROR_EMAIL=error@admin.com.br'>config
    $ git status
    On branch topic
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
            modified:   config

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working directory)
            modified:   config
    $ git commit -m 'admin email'


### Interpretando saída `git diff`

    diff --git a/config b/config
    index d7addde..0ef3692 100644
    --- a/config
    +++ b/config
    @@ -1,4 +1,5 @@
    ALLOWED_HOSTS = ['www.example.com']
    DEBUG = False
    -DEFAULT_FROM_EMAIL = 'webmaster@example.com'
    +DEFAULT_FROM_EMAIL = 'webmaster@example.com.br'
    ADMIN_EMAIL='admin@admin.com.br'
    +ERROR_NOTIFICATIOn_EMAIL='a@b.com.br'

1. `diff --git a/config b/config`  
Formato de saída do diff

2. `index d7addde..9af84f6 100644`  
metadados: identificadores hash do git  

3.
```
--- a/config
+++ b/config
```

4. `@@ -1,4 +1,5 @@`  
Fragmentos da comparação  
`-1,4`: 4 linhas extraídas a partir da linha 1  
`+1,5`: 5 linhas adicionadas a partir da linha 1  

### Ignorando arquivos (.gitignore)

- .gitignore wildcards
- logs, builds, IDE
- https://www.toptal.com/developers/gitignore


### Desfazendo as coisas ¯\\_(ツ)_/¯

#### Commits incompletos
- typos
- faltou arquivos

<img src="./static/attachment-meme.jpg" width="400px" />

    $ git commit -m 'Corresão do bug'
    $ git commit --amend -m 'Correção do bug'

#### Movendo commits para uma nova branch

    $ # fez 3 commits na master
    $ git branch topic/wip
    $ git reset --hard HEAD~2
    $ git switch topic/wip

#### Desfazer commits

    $ # adicionou senha
    $ git add config
    $ git commit -m 'senha'

Os commits ainda estão lá. Para remover da cadeia de commits você pode utilizar git reset

    $ git log
    $ git reset <commit>

* O commit ainda pode ser recuperado. para eliminar completamente, utilize `git reflog` + `git gc`

#### Reverter commits

    $ fez o bug
    $ git commit -a -m 'go, horse. fix it.'
    $ git commit -a -m 'other good fix'

<img src="./static/dont-panic.jpg" />

    $ git revert <commit>

#### Desfazendo temporariamente

    $ git stash
    $ faz o que tem que fazer
    $ git commit -m 'Fix in an hurry'
    $ git stash pop

##### Mais sobre stash

- alternar branchs com modificações
- aplicar parâmetros debug, homologação, etc

#### Desfazendo um `git pull`

- git pull = `git fetch` + `git merge`

      $ git pull
      $ git reset --merge ORIG_HEAD

### Merges e rebase

- Integrar mudanças de outros branches

<img src="./static/git-divergencia.png" width="400px" />

#### Merge

- Fundir branches

<img src="./static/git-merge.png" width="400px" />

#### Rebase

- reaplicar commits no seu branch
<img src="./static/git-rebase.png" width="400px" />


