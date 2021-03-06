---
title: 'Tutorial: Integração do Azure Active Directory com o Elium | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Elium.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: jeedes
ms.openlocfilehash: dfa90474632b2cf18055e0ba95994f120cb293ef
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447777"
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Tutorial: Integração do Microsoft Azure Active Directory com o Elium

Neste tutorial, você aprende a integrar o Elium ao Azure AD (Azure Active Directory).

A integração do Elium com o Microsoft Azure Active Directory oferece os seguintes benefícios:

- Você pode controlar no Microsoft Azure Active Directory quem tem acesso ao Elium.
- Você pode permitir que seus usuários entrem automaticamente no Elium (Logon Único) com suas contas do Microsoft Azure Active Directory.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Microsoft Azure Active Directory com o Elium, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Elium

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do Elium a partir da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-elium-from-the-gallery"></a>Adição do Elium a partir da galeria
Para configurar a integração do Elium com o Microsoft Azure Active Directory, você precisa adicionar o Elium da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Elium a partir da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

1. Na caixa de pesquisa, digite **Elium**, selecione **Elium** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Elium na lista de resultados](./media/elium-tutorial/tutorial_elium_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Microsoft Azure Active Directory com o Elium, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Microsoft Azure Active Directory precisa saber qual usuário do Elium é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Microsoft Azure Active Directory e o usuário relacionado no Elium.

Para configurar e testar o logon único do Microsoft Azure Active Directory com o Elium, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
1. **[Criar um usuário de teste do Elium](#create-an-elium-test-user)** – para ter um equivalente de Brenda Fernandes no Elium que esteja vinculado à representação de usuário do Azure Active Directory.
1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
1. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Microsoft Azure Active Directory no portal do Azure e configurará o logon único no aplicativo Elium.

**Para configurar o logon único do Microsoft Azure Active Directory com o Elium, execute as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **Elium**, clique em **Logon único**.

    ![Link Configurar logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/elium-tutorial/tutorial_elium_samlbase.png)

1. Na seção **Domínio e URLs do Elium**, realize as seguintes etapas se desejar configurar o aplicativo no modo iniciado pelo **IDP**:

    ![Informações de logon único de Domínio e URLs do Elium](./media/elium-tutorial/tutorial_elium_url.png)

    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<platform-domain>.elium.com/login/saml2/acs`

1. Marque **Mostrar configurações avançadas de URL** e realize a seguinte etapa se quiser configurar o aplicativo no modo iniciado pelo **SP**:

    ![Informações de logon único de Domínio e URLs do Elium](./media/elium-tutorial/tutorial_elium_url1.png)

    Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: ` https://<platform-domain>.elium.com/login/saml2/login`
     
    > [!NOTE] 
    > Esses valores não são reais. Você obterá esses valores do **arquivo de metadados de SP** disponível para download em `https://<platform-domain>.elium.com/login/saml2/metadata`, o que é explicado mais adiante neste tutorial.

1. O aplicativo Elium espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados de acordo com a sua configuração de atributos do token SAML. Configure as declarações a seguir para este aplicativo. Você pode gerenciar os valores desses atributos da seção "**Atributos de Usuário**" na página de integração do aplicativo.

    ![Configurar o logon único](./media/elium-tutorial/tutorial_attribute.png)

1. Na seção **Atributos de Usuário** da caixa de diálogo **Logon único**, configure o atributo do token SAML, conforme mostrado na imagem anterior e realize as seguintes etapas:
           
    | Nome do atributo | Valor do atributo |   
    | ---------------| ----------------|
    | email   |user.mail |
    | first_name| user.givenname |
    | last_name| user.surname|
    | job_title| user.jobtitle|
    | company| user.companyname|
    
    > [!NOTE]
    > Essas são as declarações padrão. **Somente a declaração de email é necessária**. Para provisionamento de JIT, apenas a declaração de email será obrigatória também. Outras declarações personalizadas podem variar de uma plataforma de cliente para outra.

    a. Clique em **Adicionar atributo** para abrir o diálogo **Adicionar Atributo**.

    ![Configurar o logon único](./media/elium-tutorial/tutorial_attribute_04.png)

    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    ![Configurar o logon único](./media/elium-tutorial/tutorial_attribute_05.png)

    c. Na lista **Valor**, digite o valor do atributo mostrado para essa linha.

    d. Deixe o namespace em branco.
    
    e. Clique em **OK**. 

1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![O link de download do Certificado](./media/elium-tutorial/tutorial_elium_certificate.png) 

1. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/elium-tutorial/tutorial_general_400.png)
    
1. Em uma janela diferente do navegador da Web, faça logon no site da sua empresa Elium como administrador.

1. Clique no **Perfil de usuário** no canto superior direito e selecione **Administração**.

    ![Configurar o logon único](./media/elium-tutorial/user1.png)

1. Selecione a guia **Segurança**.

    ![Configurar o logon único](./media/elium-tutorial/user2.png)

1. Role até a seção **Logon único (SSO)** e execute as seguintes etapas:

    ![Configurar o logon único](./media/elium-tutorial/user3.png)

    a. Copie o valor de **Verificar se a autenticação SAML2 funciona para sua conta** e cole-o na caixa de texto **URL de logon** na seção **Domínio e URLs do Elium** no Portal do Azure.

    > [!NOTE]
    > Depois de configurar o SSO, você poderá sempre acessar a página de logon remoto padrão na seguinte URL: `https://<platform_domain>/login/regular/login` 

    b. Marque a caixa de seleção **Ativar federação de SAML2**.

    c. Marque a caixa de seleção **Provisionamento de JIT**.

    d. Abra o **SP Metadata** clicando no botão **Fazer o download**.

    e. Procure **entityID** no arquivo **SP Metadata**, copie o valor **entityID** e cole-o na caixa de texto **Identificador** na seção **Domínio e URLs do Elium** no Portal do Azure. 

    ![Configurar o logon único](./media/elium-tutorial/user4.png)

    f. Procure **AssertionConsumerService** no arquivo **SP Metadata**, copie o valor **Local** e cole-o na caixa de texto **URL de resposta** na seção **Domínio e URLs do Elium** no Portal do Azure.

    ![Configurar o logon único](./media/elium-tutorial/user5.png)

    g. Abra o arquivo de metadados baixado do Portal do Azure no bloco de notas, copie o conteúdo e cole-o na caixa de texto **Metadados do IdP**.

    h. Clique em **Salvar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/elium-tutorial/create_aaduser_01.png)

1. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/elium-tutorial/create_aaduser_02.png)

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/elium-tutorial/create_aaduser_03.png)

1. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/elium-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.
 
### <a name="create-an-elium-test-user"></a>Criar um usuário de teste do Elium

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Elium. O Elium dá suporte ao provisionamento just-in-time, que está habilitado por padrão. Não há itens de ação para você nesta seção. Um novo usuário é criado durante uma tentativa de acessar o Elium, caso ele ainda não exista.
>[!Note]
>Se você precisar criar um usuário manualmente, contate a [equipe de suporte do Elium](mailto:support@elium.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Elium.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao Elium, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, escolha **Elium**.

    ![O link do Elium na lista de Aplicativos](./media/elium-tutorial/tutorial_elium_app.png)  

1. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Elium no Painel de Acesso, você deverá ser conectado automaticamente ao aplicativo Elium.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/elium-tutorial/tutorial_general_01.png
[2]: ./media/elium-tutorial/tutorial_general_02.png
[3]: ./media/elium-tutorial/tutorial_general_03.png
[4]: ./media/elium-tutorial/tutorial_general_04.png

[100]: ./media/elium-tutorial/tutorial_general_100.png

[200]: ./media/elium-tutorial/tutorial_general_200.png
[201]: ./media/elium-tutorial/tutorial_general_201.png
[202]: ./media/elium-tutorial/tutorial_general_202.png
[203]: ./media/elium-tutorial/tutorial_general_203.png

