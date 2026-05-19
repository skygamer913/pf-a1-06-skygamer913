# Diretrizes de GitFlow - Módulo do Totem de Check-in (PrettyFlights v1.0.0)

Este documento descreve a aplicação prática do fluxo de trabalho GitFlow no desenvolvimento do **Módulo do Totem de Check-in do PrettyFlights**. O objetivo é garantir um histórico de commits limpo, isolamento de funcionalidades e estabilidade em produção.

---

## Cenário do Projeto

Estamos iniciando o desenvolvimento da versão **1.0.0 do software do Totem de Check-in**. Este totem permitirá que os passageiros realizem o *Check-in* no aeroporto via interface interativa, **leiam códigos de barra de passagens** e **imprimam etiquetas de bagagem**.

---

## Passo a Passo dos Comandos Git

### a. Inicialização do Novo Repositório

Para iniciar o projeto, criamos uma pasta local, inicializamos o *Git* e configuramos o repositório remoto público.

- *Bash*
    ```
    # Cria e acessa a pasta do projeto
    mkdir totem-checkin-prettyflights
    cd totem-checkin-prettyflights

    # Inicializa o repositório Git com a branch principal 'main'
    git init -b main

    # Cria a primeira versão do arquivo de diretrizes
    echo "# Totem de Check-in - PrettyFlights" > README.md
    git add README.md
    git commit -m "chore: initial commit com README"

    # Vincula ao repositório público criado no GitHub/GitLab
    git remote add origin [https://github.com/usuario/totem-checkin-prettyflights.git](https://github.com/usuario/totem-checkin-prettyflights.git)
    git push -u origin main
    ```

### b. Criação da Estrutura de *Branches* do GitFlow

O *GitFlow* se baseia em duas *branches* permanentes: `main` (produção) e `develop` (integração de novas features).

- **Branches Permanentes**: `main` (código estável em produção) e `develop` (ambiente de integração).
- **Branches Temporárias**: `feature/*` (novas funcionalidades), `release/*` (preparação de versão) e `hotfix/*` (correções urgentes em produção).
- *Bash*
    ```
    # Cria a branch de desenvolvimento a partir da main
    git checkout -b develop

    # Envia a branch develop para o repositório remoto
    git push -u origin develop
    ```

### c. Desenvolvimento de uma *Feature*

Vamos simular o desenvolvimento da funcionalidade de **Leitura do Código de Barras do Bilhete**. As *features* sempre partem da `develop`.

- *Bash*
    ```
    # Garante que está na develop e atualizado
    git checkout develop
    git pull origin develop

    # Cria e entra na branch da feature
    git checkout -b feature/leitura-codigo-barras

    # [Simulação] Criação do arquivo da funcionalidade
    echo "function lerCodigoBarras() { console.log('Código escaneado com sucesso.'); }" > scanner.js

    # Registra o progresso no documento de diretrizes
    echo "- Feature: Leitura de código de barras adicionada." >> gitflow-diretrizes.md

    # Commita as alterações
    git add scanner.js gitflow-diretrizes.md
    git commit -m "feat: implementa scanner de código de barras para o totem"

    # Publica a feature no remoto para code review
    git push -u origin feature/leitura-codigo-barras
    ```

### d. Integração da Feature na Develop

Após a aprovação da *feature*, ela é integrada de volta à *branch* `develop`.

- *Bash*
    ```
    # Retorna para a develop
    git checkout develop

    # Atualiza a develop local em relação ao remoto
    git pull origin develop

    # Realiza o merge da feature aplicando a flag --no-ff (no-fast-forward) 
    # para preservar o histórico de que as alterações vieram de uma branch isolada
    git merge --no-ff feature/leitura-codigo-barras -m "merge: integra feature/leitura-codigo-barras na develop"

    # Envia os dados atualizados para o remoto
    git push origin develop

    # Remove a branch local e remota que já foi integrada
    git branch -d feature/leitura-codigo-barras
    git push origin --delete feature/leitura-codigo-barras
    ```

### e. Criação da *Release* 1.0.0 a partir da *Develop*

Com o escopo da versão finalizado na `develop`, abrimos uma *branch* de `release` para testes finais, ajustes de numeração de versão e documentação.

- *Bash*
    ```
    # Cria a branch de release a partir da develop
    git checkout -b release/1.0.0 develop

    # [Simulação] Atualiza arquivos de configuração/versão e este documento
    echo "- Release 1.0.0 preparada para homologação." >> gitflow-diretrizes.md
    git add gitflow-diretrizes.md
    git commit -m "chore: bump version para 1.0.0 e atualiza diretrizes"

    # ---- FIM DOS TESTES: CONCLUINDO A RELEASE ----

    # 1. Integra na main (Produção)
    git checkout main
    git merge --no-ff release/1.0.0 -m "merge: lança versão 1.0.0 na main"

    # Gera a tag correspondente à versão estável
    git tag -a v1.0.0 -m "Versão oficial 1.0.0 do Totem de Check-in"

    # 2. Integra de volta na develop (para manter correções de homologação alinhadas)
    git checkout develop
    git merge --no-ff release/1.0.0 -m "merge: sincroniza correções da release 1.0.0 com a develop"

    # Envia tudo para o repositório remoto
    git push origin main develop --tags

    # Remove a branch de release que já cumpriu seu papel
    git branch -d release/1.0.0
    ```

### f. Correção de um Bug Emergencial (*Hotfix*) a partir da *Main*

Cenário: O totem travava na tela de impressão de etiquetas em produção. Precisamos corrigir imediatamente sem puxar códigos inacabados da `develop`. O `hotfix` nasce da `main`.

- *Bash*
    ```
    # Vai para a main e puxa o estado mais recente (v1.0.0)
    git checkout main
    git pull origin main

    # Cria a branch de hotfix
    git checkout -b hotfix/1.0.1

    # [Simulação] Correção do bug crítico
    echo "function imprimirEtiqueta() { console.log('Etiqueta impressa sem travamentos.'); }" > impressora.js
    echo "- Hotfix 1.0.1: Correção do travamento na impressão." >> gitflow-diretrizes.md

    # Salva as alterações de correção
    git add impressora.js gitflow-diretrizes.md
    git commit -m "fix: corrige travamento crítico na fila de impressão"
    ```

### g. Integração do *Hotfix* na *Main* e na *Develop*

Para finalizar, o `hotfix` precisa salvar o dia em produção (`main`) e garantir que o *bug* não volte no futuro (`develop`).

- *Bash*
    ```
    # 1. Integra na Main (Produção)
    git checkout main
    git merge --no-ff hotfix/1.0.1 -m "merge: aplica hotfix 1.0.1 na main"
    git tag -a v1.0.1 -m "Versão de correção 1.0.1"

    # 2. Integra na Develop (Desenvolvimento)
    git checkout develop
    git merge --no-ff hotfix/1.0.1 -m "merge: sincroniza hotfix 1.0.1 com a develop"

    # Envia as atualizações e tags para o remoto
    git push origin main develop --tags

    # Exclui a branch de hotfix
    git branch -d hotfix/1.0.1
    ```