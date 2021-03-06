---
title: Configurando seu projeto Node.js para Codespaces
shortTitle: Configurando seu projeto Node.js
intro: 'Comece com seu projeto JavaScript, Node.js ou TypeScript em {% data variables.product.prodname_codespaces %} criando um contêiner de desenvolvimento personalizado.'
product: '{% data reusables.gated-features.codespaces %}'
versions:
  fpt: '*'
  ghec: '*'
redirect_from:
  - /codespaces/getting-started-with-codespaces/getting-started-with-your-nodejs-project-in-codespaces
type: tutorial
topics:
  - Codespaces
  - Developer
  - Node
  - JavaScript
---



## Introdução

Este guia mostra como configurar seu projeto JavaScript, Node.js ou TypeScript em {% data variables.product.prodname_codespaces %}. Ele irá apresentar a você um exemplo de abertura de seu projeto em um codespace e adicionar e modificar uma configuração de contêiner de desenvolvimento a partir de um modelo.

### Pré-requisitos

- Você deve ter um projeto existente de JavaScript, Node.js ou TypeScript em um repositório em {% data variables.product.prodname_dotcom_the_website %}. Se você não tiver um projeto, você poderá tentar este tutorial com o seguinte exemplo: https://github.com/microsoft/vscode-remote-try-node
- Você precisa ter {% data variables.product.prodname_codespaces %} habilitado para a sua organização.

## Etapa 1: Abra o seu projeto em um codespace

1. No nome do repositório, use o menu suspenso **Código de {% octicon "code" aria-label="The code icon" %}** e na aba **Codespaces** de código, clique em {% octicon "plus" aria-label="The plus icon" %} **Novo codespace**.

  ![Botão de codespace novo](/assets/images/help/codespaces/new-codespace-button.png)

  Se você não vir esta opção, significa que {% data variables.product.prodname_codespaces %} não está disponível para o seu projeto. Consulte [Acesso a {% data variables.product.prodname_codespaces %}](/codespaces/developing-in-codespaces/creating-a-codespace#access-to-codespaces) para mais informações.


Ao criar um código, seu projeto será criado em uma VM remota dedicada a você. Por padrão, o contêiner para o seu código possui muitas linguagens e tempos de execução, incluindo Node.js, JavaScript, Typescript, nvm, npm e yarn. Ele também inclui um conjunto comum de ferramentas, como git, wget, rsync, openssh e nano.

Você pode personalizar o seu codespace ajustando a quantidade de vCPUs e RAM, [adicionando dotfiles para personalizar seu ambiente](/codespaces/setting-up-your-codespace/personalizing-codespaces-for-your-account)ou modificando as ferramentas e scripts instalados.

{% data variables.product.prodname_codespaces %} usa um arquivo denominado `devcontainer.json` para armazenar configurações. Ao iniciar, {% data variables.product.prodname_codespaces %} usa o arquivo para instalar quaisquer ferramentas, dependências ou outro conjunto que possa ser necessário para o projeto. Para obter mais informações, consulte "[Configurar codespaces para o seu projeto](/codespaces/setting-up-your-codespace/configuring-codespaces-for-your-project)".

## Etapa 2: Adicione um contêiner de desenvolvimento ao seu codespace a partir de um modelo

O contêiner de codespaces padrão será compatível com a execução de projetos de Node.js como [vscode-remote-try-node](https://github.com/microsoft/vscode-remote-try-node) fora da caixa. Ao configurar um contêiner personalizado, você poderá personalizar as ferramentas e scripts que são executados como parte da criação de código e garantir um ambiente reprodutível para todos os usuários de {% data variables.product.prodname_codespaces %} no seu repositório.

Para configurar seu projeto com um contêiner personalizado, você deverá usar um arquivo `devcontainer.json` para definir o ambiente. Em {% data variables.product.prodname_codespaces %}, você pode adicionar isto a partir de um modelo ou você pode criar o seu próprio. Para obter mais informações sobre contêineres de desenvolvimento, consulte "[Configurar codespaces para o seu projeto](/codespaces/setting-up-your-codespace/configuring-codespaces-for-your-project)".

{% data reusables.codespaces.command-palette-container %}
3. Para este exemplo, clique em **Node.js**.  Se você precisar de funcionalidades adicionais, você poderá selecionar qualquer contêiner específico para o Node ou uma combinação de ferramentas como Node e MongoDB. ![Selecione a opção Node na lista](/assets/images/help/codespaces/add-node-prebuilt-container.png)
4. Clique na versão recomendada do Node.js. ![Seleção de versão do Node.js](/assets/images/help/codespaces/add-node-version.png)
{% data reusables.codespaces.rebuild-command %}

### Anatomia do seu contêiner de desenvolvimento

A adição do modelo de contêiner de desenvolvimento do Node.js adiciona uma pasta `.devcontainer` à raiz do repositório do seu projeto com os seguintes arquivos:

- `devcontainer.json`
- arquivo Docker

O arquivo recém-adicionado `devcontainer.json` define algumas propriedades que são descritas após a amostra.

#### devcontainer.json

```json
// For format details, see https://aka.ms/devcontainer.json. For config options, see the README at:
// https://github.com/microsoft/vscode-dev-containers/tree/v0.162.0/containers/javascript-node
{
    "name": "Node.js",
    "build": {
        "dockerfile": "Dockerfile",
        // Update 'VARIANT' to pick a Node version: 10, 12, 14
        "args": { "VARIANT": "14" }
    },

    // Set *default* container specific settings.json values on container create.
    "settings": {
        "terminal.integrated.shell.linux": "/bin/bash"
    },

    // Add the IDs of extensions you want installed when the container is created.
    "extensions": [
        "dbaeumer.vscode-eslint"
    ],

    // Use 'forwardPorts' to make a list of ports inside the container available locally.
    // "forwardPorts": [],

    // Use 'postCreateCommand' to run commands after the container is created.
    // "postCreateCommand": "yarn install",

    // Comment out connect as root instead. Mais informações: https://aka.ms/vscode-remote/containers/non-root.
    "remoteUser": "node"
}
```

- **Nome** - Você pode dar qualquer nome ao seu contêiner de desenvolvimento. Este é apenas o padrão.
- **Build** - As propriedades de compilação.
  - **Arquivo Docker** - No objeto de compilação, o arquivo Docker é uma referência ao arquivo Docker que também foi adicionado a partir do modelo.
  - **Args**
    - **Variante**: Este arquivo contém apenas um argumento de compilação, que é a variante de nó que queremos usar e que é passada para o arquivo Docker.
- **Configurações** - Estas são configurações de {% data variables.product.prodname_vscode %} que você pode definir.
  - **Terminal.integrated.shell.linux** - Embora o bash seja o padrão, você pode usar outros shells do terminal, fazendo a modificação.
- **Extensões** - Estas são extensões incluídas por padrão.
  - <**Dbaeumer.vscode-eslint** - ES lint é uma ótima extensão para linting, mas para o JavaScript, há uma série de ótimas extensões do Marketplace que você também pode incluir.
- **forwardPorts** - Todas as portas listadas aqui serão encaminhadas automaticamente. Para obter mais informações, consulte "[Encaminhando portas no seu codespace](/codespaces/developing-in-codespaces/forwarding-ports-in-your-codespace)".
- **postCreateCommand** - Se você quiser executar qualquer coisa depois de chegar ao seu codespace que não está definido no arquivo Docker, você poderá fazer isso aqui.
- **remoteUser** - Por padrão, você está executando como usuário do vscode, mas, opcionalmente, você pode definir isso como root.

#### arquivo Docker

```bash
# [Choice] Node.js version: 14, 12, 10
ARG VARIANT="14-buster"
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-${VARIANT}

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Uncomment if you want to install an additional version of node using nvm
# ARG EXTRA_NODE_VERSION=10
# RUN su node -c "source /usr/local/share/nvm/nvm.sh && nvm install ${EXTRA_NODE_VERSION}"

# [Optional] Uncomment if you want to install more global node modules
# RUN su node -c "npm install -g <your-package-list-here>"
```

Você pode usar o arquivo Docker para adicionar camadas adicionais de contêiner para especificar os pacotes do sistema operacional, versões de nó ou pacotes globais que queremos que sejam incluídos no nosso arquivo Docker.

## Etapa 3: Modifique seu arquivo devcontainer.json

Com o seu contêiner de desenvolvimento adicionado e um entendimento básico do que tudo faz, agora você pode fazer alterações para configurá-lo para o seu ambiente. Neste exemplo, você irá adicionar propriedades para instalar o npm quando seu codespace for lançado e para fazer uma lista de portas dentro do contêiner disponível localmente.

1. No Explorer, selecione o arquivo `devcontainer.json` a partir da árvore para abri-lo. Você pode ter que expandir a pasta `.devcontainer` para vê-la.

  ![Arquivo devcontainer.json no Explorador](/assets/images/help/codespaces/devcontainers-options.png)

2. Adicione as seguintes linhas ao seu arquivo `devcontainer.json` após as `extensões`:

  ```json{:copy}
  "postCreateCommand": "npm install",
  "forwardPorts": [4000],
  ```

  Para obter mais informações sobre as propriedades `devcontainer.json`, consulte a referência de [devcontainer.json](https://code.visualstudio.com/docs/remote/devcontainerjson-reference) na documentação de {% data variables.product.prodname_vscode %}.

{% data reusables.codespaces.rebuild-command %}

  A reconstrução dentro do seu codespace garante que as suas alterações funcionem conforme o esperado antes de realizar o commit das alterações no repositório. Se algo falhar, você será colocado em um codespace com um contêiner de recuperação que você pode reconstruir para continuar ajustando o seu contêiner.


## Etapa 4: Execute o seu aplicativo

Na seção anterior, você usou o `postCreateCommand` para instalar um conjunto de pacotes via npm. Agora você pode usar isso para executar nosso aplicativo com npm.

1. Execute seu comando inicial no terminal com`npm start`.

  ![início do npm no terminal](/assets/images/help/codespaces/codespaces-npmstart.png)

2. Quando o seu projeto for iniciado, você deverá ver um alerta no canto inferior direito com uma instrução para conectar-se à porta que seu projeto usa.

  ![Notificação de encaminhamento de porta](/assets/images/help/codespaces/codespaces-port-toast.png)

## Etapa 5: Faça commit das suas alterações

{% data reusables.codespaces.committing-link-to-procedure %}

## Próximas etapas

Agora você deve estar pronto para começar a desenvolver seu projeto JavaScript em {% data variables.product.prodname_codespaces %}. Aqui estão alguns recursos adicionais para cenários mais avançados.

- [Gerenciar segredos criptografados para seus codespaces](/codespaces/managing-your-codespaces/managing-encrypted-secrets-for-your-codespaces)
- [Gerenciar a verificação de GPG para {% data variables.product.prodname_codespaces %}](/codespaces/managing-your-codespaces/managing-gpg-verification-for-codespaces)
- [Encaminhar portas no seu código](/codespaces/developing-in-codespaces/forwarding-ports-in-your-codespace)
