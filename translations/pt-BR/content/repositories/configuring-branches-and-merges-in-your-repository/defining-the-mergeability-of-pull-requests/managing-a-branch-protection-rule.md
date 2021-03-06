---
title: Gerenciar uma regra de proteção de branch
intro: 'Você pode criar uma regra de proteção de branch para aplicar certos fluxos de trabalho para um ou mais branches, como exigir uma revisão de aprovação ou verificações de status de aprovação para todos os pull requests mesclados no branch protegido.'
product: '{% data reusables.gated-features.protected-branches %}'
redirect_from:
  - /articles/configuring-protected-branches
  - /enterprise/admin/developer-workflow/configuring-protected-branches-and-required-status-checks
  - /articles/enabling-required-status-checks
  - /github/administering-a-repository/enabling-required-status-checks
  - /articles/enabling-branch-restrictions
  - /github/administering-a-repository/enabling-branch-restrictions
  - /articles/enabling-required-reviews-for-pull-requests
  - /github/administering-a-repository/enabling-required-reviews-for-pull-requests
  - /articles/enabling-required-commit-signing
  - /github/administering-a-repository/enabling-required-commit-signing
  - /github/administering-a-repository/requiring-a-linear-commit-history
  - /github/administering-a-repository/enabling-force-pushes-to-a-protected-branch
  - /github/administering-a-repository/enabling-deletion-of-a-protected-branch
  - /github/administering-a-repository/managing-a-branch-protection-rule
  - /github/administering-a-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
permissions: People with admin permissions to a repository can manage branch protection rules.
topics:
  - Repositories
shortTitle: Regra de proteção de branch
---

## Sobre as regras de proteção do branch

{% data reusables.repositories.branch-rules-example %}

É possível criar uma regra para todos os branches atuais e futuros no repositório com a sintaxe curinga `*`. Pelo fato de o {% data variables.product.company_short %} usar o sinalizador `File::FNM_PATHNAME` para a sintaxe `File.fnmatch`, o curinga não corresponde aos separadores de diretório (`/`). Por exemplo, `qa/*` pode fazer correspondência com todos os branches que começam com `qa/` e contêm uma única barra. Você pode incluir várias barras com `qa/**/*` e você pode estender a string `qa` com `qa**/**/*` para tornar a regra mais inclusiva. Para obter mais informações sobre opções de sintaxe para regras de branch, consulte a [documentação de fnmatch](https://ruby-doc.org/core-2.5.1/File.html#method-c-fnmatch).

Se um repositório tiver várias regras de branch protegido que afetem os mesmos branches, as regras que incluírem um nome de branch específico terão a prioridade mais alta. Se houver mais de uma regra de branch protegido que faça referência ao mesmo nome de branch específico, a regra de branch criada primeiro terá a prioridade mais alta.

As regras de branch protegido que mencionam um caractere especial, como `*`, `?` ou `]`, são aplicadas na ordem em que foram criadas, de modo que as regras mais antigas com esses caracteres têm uma prioridade mais alta.

Para criar uma exceção a uma regra de branch existente, você pode criar outra regra de proteção de branch que tenha prioridade superior, como uma regra para um nome de branch específico.

Para obter mais informações sobre cada uma das configurações de proteção de branch disponíveis, consulte "[Sobre branches protegidos](/github/administering-a-repository/about-protected-branches)".

## Criar uma regra de proteção de branch

Ao criar uma regra de branch, o branch que você especificar ainda não existe no repositório.

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.repositories.repository-branches %}
{% data reusables.repositories.add-branch-protection-rules %}
{% ifversion fpt or ghec %}
1. Optionally, enable required pull requests.
   - Under "Protect matching branches", select **Require a pull request before merging**. ![Caixa de seleção Pull request review restriction (Restrição de revisão de pull request)](/assets/images/help/repository/PR-reviews-required-updated.png)
   - Optionally, to require approvals before a pull request can be merged, select **Require approvals**, click the **Required number of approvals before merging** drop-down menu, then select the number of approving reviews you would like to require on the branch. ![Menu suspenso para selecionar o número de revisões de aprovação obrigatórias](/assets/images/help/repository/number-of-required-review-approvals-updated.png)
{% else %}
1. Opcionalmente, habilite as revisões obrigatórias de de pull request.
   - Em "Proteger os branches correspondentes", selecione **Exigir revisões de pull request antes do merge**. ![Caixa de seleção Pull request review restriction (Restrição de revisão de pull request)](/assets/images/help/repository/PR-reviews-required.png)
   - Click the **Required approving reviews** drop-down menu, then select the number of approving reviews you would like to require on the branch. ![Menu suspenso para selecionar o número de revisões de aprovação obrigatórias](/assets/images/help/repository/number-of-required-review-approvals.png)
{% endif %}
   - Opcionalmente, para ignorar uma revisão de aprovação de pull request quando um commit de modificação de código for enviado por push para o branch, selecione **Ignorar aprovações obsoletas de pull request quando novos commits forem enviados por push**. ![Caixa de seleção Dismiss stale pull request approvals when new commits are pushed (Ignorar aprovações de pull requests obsoletas ao fazer push de novos commits)](/assets/images/help/repository/PR-reviews-required-dismiss-stale.png)
   - Opcionalmente, para exigir a revisão de um proprietário do código quando o pull request afeta o código que tem um proprietário designado, selecione **Exigir revisão de Proprietários do Código**. Para obter mais informações, consulte "[Sobre proprietários do código](/github/creating-cloning-and-archiving-repositories/about-code-owners)". ![Require review from code owners (Exigir revisão de proprietários de código)](/assets/images/help/repository/PR-review-required-code-owner.png)
   - Opcionalmente, se o repositório fizer parte de uma organização, selecione **Restringir quem pode ignorar as revisões de pull request**. Em seguida, procure e selecione as pessoas ou equipes que têm permissão para ignorar as revisões de pull request. Para obter mais informações, consulte "[Ignorar uma revisão de pull request](/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/dismissing-a-pull-request-review)". ![Caixa de seleção Restrict who can dismiss pull request reviews (Restringir quem pode ignorar revisões de pull request)](/assets/images/help/repository/PR-review-required-dismissals.png)
1. Opcionalmente, habilite as verificações de status obrigatórias.
   - Selecione **Require status checks to pass before merging** (Exigir verificações de status para aprovação antes de fazer merge). ![Opção Required status checks (Verificações de status obrigatórias)](/assets/images/help/repository/required-status-checks.png)
   - Opcionalmente, para garantir que os pull requests sejam testados com o código mais recente no branch protegido, selecione **Exigir que os branches estejam atualizados antes do merge**. ![Caixa de seleção Status obrigatório rígido ou flexível](/assets/images/help/repository/protecting-branch-loose-status.png)
   - Na lista de verificações de status disponíveis, selecione as verificações que você deseja tornar obrigatórias.![Lista de verificações de status disponíveis](/assets/images/help/repository/required-statuses-list.png)
{%- ifversion fpt or ghes > 3.1 or ghae-issue-4382 %}
1. Opcionalmente, selecione **Exige resolução de conversas antes de fazer merge**. ![Exigir resolução de conversas antes de fazer o merge](/assets/images/help/repository/require-conversation-resolution.png)
{%- endif %}
1. Opcionalmente, selecione **Exigir commits assinados**. ![Opção Require signed commits (Exigir commits assinados)](/assets/images/help/repository/require-signed-commits.png)
1. Opcionalmente, selecione **Exigir histórico linear**. ![Opção de histórico linear necessária](/assets/images/help/repository/required-linear-history.png)
{%- ifversion fpt or ghec %}
1. Optionally, to merge pull requests using a merge queue, select **Require merge queue**. {% data reusables.pull_requests.merge-queue-references %} ![Require merge queue option](/assets/images/help/repository/require-merge-queue.png)
  {% tip %}

  **Tip:** The pull request merge queue feature is currently in limited public beta and subject to change. Organizations owners can request early access to the beta by joining the [waitlist](https://github.com/features/merge-queue/signup).

  {% endtip %}
{%- endif %}
1. Outra opção é selecionar **Include administrators** (Incluir administradores). ![Caixa de seleção Include administrators (Incluir administradores)](/assets/images/help/repository/include-admins-protected-branches.png)
1. Opcionalmente, {% ifversion fpt or ghec %} se o repositório pertencer a uma organização que usa {% data variables.product.prodname_team %} ou {% data variables.product.prodname_ghe_cloud %},{% endif %} habilitar as restrições de branches.
   - Selecione **Restringir quem pode fazer push para os branches correspondentes**. ![Caixa de seleção Branch restriction (Restrição de branch)](/assets/images/help/repository/restrict-branch.png)
   - Procurar e selecionar pessoas, equipes ou aplicativos que tenham permissão para fazer push para o branch protegido. ![Pesquisa de restrição de branch](/assets/images/help/repository/restrict-branch-search.png)
1. Opcionalmente, em "Regras aplicadas a todos incluindo administradores", selecione **Permitir pushes forçados**. ![Permitir opção push forçado](/assets/images/help/repository/allow-force-pushes.png)
1. Opcionalmente, selecione **Permitir exclusões**. ![Permitir a opção de exclusão de branch](/assets/images/help/repository/allow-branch-deletions.png)
1. Clique em **Criar**.

## Editar uma regra de proteção de branch

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.repositories.repository-branches %}
1. À direita da regra de proteção de branch que você deseja editar, clique em **Editar**. ![Botão editar](/assets/images/help/repository/edit-branch-protection-rule.png)
1. Faça as alterações desejadas na regra de proteção do branch.
1. Clique em **Save changes** (Salvar alterações). ![Botão Edit message (Editar mensagem)](/assets/images/help/repository/save-branch-protection-rule.png)

## Excluir as regras de proteção do branch

{% data reusables.repositories.navigate-to-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.repositories.repository-branches %}
1. À direita da regra de proteção do branch que você deseja excluir, clique em **Excluir**. ![Botão excluir](/assets/images/help/repository/delete-branch-protection-rule.png)
