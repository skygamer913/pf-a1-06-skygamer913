# Lista de Comandos do Git

Uma lista de 20 comandos do *Git*.

## Configuração Inicial e Criação de Repositórios

- `git init`
    Inicializa um novo repositório Git local na pasta atual.

    - **Exemplo 1**: Iniciando um projeto do zero absoluto
    - **Exemplo 2**: Colocando um projeto antigo/existente no Git

- `git clone <url-do-repositorio>`
    Baixa uma cópia completa de um repositório remoto para a sua máquina.

    - **Exemplo 1**: Clonando um projeto público para estudar ou contribuir
    - **Exemplo 2**: Clonando um projeto em uma pasta específica (e com outro nome)

- `git config --global user.name "Seu Nome"`
    Configura o nome que será associado aos seus commits.
    
    - **Exemplo 1**: Configurando o Git pela primeira vez no computador
    - **Exemplo 2**: Verificando o nome atual e corrigindo um erro de digitação

- `git config --global user.email "seu-email@email.com"`
    Configura o e-mail que será associado aos seus commits.

    - **Exemplo 1**:Configuração inicial (Vinculando ao seu GitHub)
    - **Exemplo 2**: Verificando o e-mail atual e alterando para um e-mail institucional

## Ciclo de Vida dos Arquivos (Trabalho Local)

- `git status`
    Exibe o estado atual do seu diretório de trabalho (quais arquivos foram modificados, adicionados ou estão prontos para o commit).

    - **Exemplo 1**: Identificando arquivos modificados e não rastreados
    - **Exemplo 2**: Verificando arquivos na Staging Area (Prontos para o Commit)

- `git add <nome-do-arquivo>`
    Adiciona um arquivo específico à Staging Area (prepara o arquivo para o próximo commit). Use git add . para adicionar todas as modificações de uma vez.

    - **Exemplo 1**: Adicionando todos os arquivos modificados de uma vez (`git add .`)
    - **Exemplo 2**: Adicionando arquivos específicos de forma isolada

- `git commit -m "Mensagem descritiva"`
    Grava as alterações que estavam na Staging Area no histórico do repositório local.

    - **Exemplo 1**: Realizar um Commit padrão com mensagem curta (`-m`)
    - **Exemplo 2**: Corrigindo o último commit feito por engano (`--amend`)

- `git diff`
    Mostra as alterações exatas feitas nos arquivos que ainda não foram adicionadas à Staging Area.

    - **Exemplo 1**: Verificar alterações que ainda não foram salvas
    - **Exemplo 2**: Comparar duas ramificações (*Branches*) diferentes

## Histórico e Logs

- `git log`
    Exibe o histórico de commits do repositório, mostrando o autor, a data e a mensagem de cada um.

    - **Exemplo 1**: Visualização do histórico completo e detalhado
    - **Exemplo 2**: Visualização do histórico simplificado em uma linha (`--oneline`)

- `git log --oneline`
    Uma versão simplificada do histórico, exibindo cada commit em apenas uma linha (ideal para uma visualização rápida).

- `git show <hash-do-commit>`
    Mostra os detalhes e as alterações específicas introduzidas por um commit específico.

## Ramificações (Branches) e Fusões

- `git branch`
    Lista todas as branches (ramificações) locais. A branch atual terá um asterisco (*) ao lado.

- `git branch <nome-da-branch>`
    Cria uma nova branch com o nome especificado, sem sair da branch atual.

- `git checkout <nome-da-branch>` (ou `git switch <nome-da-branch>`)
    Muda para a branch especificada. Para criar e mudar de branch ao mesmo tempo, você pode usar `git checkout -b <nome-da-branch>`.

    - **Exemplo 1**: Quando que criar novas funcionalidades em um projeto sem afetar o *branch* principal(`git checkout -b <nome-da-branch>`).
    - **Exemplo 2**: Desfazer alterações em arquivos específicos.

- `git merge <nome-da-branch>`
    Combina o histórico da branch especificada com a branch onde você está atualmente.

## Repositórios Remotos e Sincronização

- `git remote add origin <url-do-repositorio>`
    Vincula o seu repositório local a um repositório remoto (como GitHub ou GitLab) sob o codinome "origin".

- `git push origin <nome-da-branch>`
    Envia os seus commits locais da branch especificada para o repositório remoto.

    - **Exemplo 1**: Quando finaliza o trabalho e precisa enviar para o Git.
    - **Exemplo 2**: Para atualizar uma *branch* existente.
 
- `git fetch`
    Baixa as atualizações do repositório remoto para o seu histórico local, mas não altera os seus arquivos de trabalho atuais.

- `git pull`
    Baixa as novidades do repositório remoto e já faz o merge automático na sua branch atual (é essencialmente um git fetch seguido de um git merge).

## Desfazendo Alterações

- `git reset --soft HEAD~1`
    Desfaz o último commit mantendo as alterações dos arquivos intactas na Staging Area (prontas para serem corrigidas e commitadas de novo).
