---
title: 'Tutorial: Integração do Azure Active Directory com o SD Elements | Microsoft Docs'
description: Saiba como configurar o logon único entre o Active Directory do Azure e o SD Elements.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 4d5c830df47ff212d2f4d93eb48001ce3a3e2207
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446917"
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Tutorial: Integração do Active Directory do Azure com SD Elements

Neste tutorial, você aprende a integrar o SD Elements ao Azure AD (Azure Active Directory).

A integração do SD Elements ao Azure AD oferece os seguintes benefícios:

- No AD do Azure, você pode controlar quem tem acesso ao SD Elements
- Você pode habilitar seus usuários a fazerem logon automaticamente em SD Elements (logon único) com suas contas do AD do Azure
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do AD do Azure a SD Elements, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do SD Elements

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando elementos SD da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-sd-elements-from-the-gallery"></a>Adicionando elementos SD da galeria
Para configurar a integração de SD Elements ao Azure AD, você precisa adicionar SD Elements da galeria à sua lista de aplicativos de SaaS gerenciados.

**Para adicionar SD Elements da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **SD Elements**.

    ![Criação de um usuário de teste do AD do Azure](./media/sd-elements-tutorial/tutorial_sdelements_search.png)

1. No painel de resultados, selecione **SD Elements** e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o SD Elements, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do SD Elements é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do SD Elements.

No SD Elements, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o SD Elements, é preciso concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criando um usuário de teste do SD Elements](#creating-a-sd-elements-test-user)** – para ter um equivalente de Brenda Fernandes no SD Elements que esteja vinculado à representação de usuário do Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo SD Elements.

**Para configurar o logon único do Azure AD com o SD Elements, execute as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo **SD Elements**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_sdelements_samlbase.png)

1. Na seção **Domínio e URLs do SD Elements**, realize as seguintes etapas:

    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_sdelements_url.png)

    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com o Identificador e a URL de Resposta reais. Contate a [equipe de suporte do SD Elements](mailto:support@sdelements.com) para obter esses valores.

1. O aplicativo SD Elements espera que as declarações SAML estejam em um formato específico. Configure as declarações a seguir para este aplicativo. Gerencie os valores desses atributos na guia “**Atributo de Usuário**” do aplicativo. A captura de tela a seguir mostra um exemplo disso.

    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_sdelements_attribute.png)

1. Na seção **Atributos do Usuário**, na caixa de diálogo **Logon único**, configure o atributo do token SAML como mostra a imagem e execute as etapas a seguir: 

    | Nome do atributo | Valor do atributo |
    | --- | --- |
    | email |user.mail |
    | nome |user.givenname |
    | sobrenome |user.surname |

    a. Clique em **Adicionar atributo** para abrir o diálogo **Adicionar Atributo**.

    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_officespace_04.png)

    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_officespace_05.png)

    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    c. Na lista **Valor**, digite o valor do atributo mostrado para essa linha.

    d. Clique em **OK**.
 
1. Na seção **Certificado de Autenticação SAML**, clique em **Certificado (Base64)** e, em seguida, salve o arquivo do certificado em seu computador.

    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_sdelements_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_general_400.png)

1. Na seção **Configuração do SD Elements**, clique em **Configurar o SD Elements** para abrir a janela **Configurar logon**. Copie a **ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção Referência Rápida.**

    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_sdelements_configure.png)

1. Para habilitar logon único, entre em contato com a [equipe de suporte do SD Elements](mailto:support@sdelements.com) e forneça a ela o arquivo de certificado baixado. 

1. Em outra janela do navegador, faça logon no locatário do SD Elements como administrador.

1. No menu na parte superior, clique em **Sistema** e, depois, em **Logon Único**. 
   
    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_sd-elements_09.png) 

1. Na caixa de diálogo **Configurações de Logon Único** , execute as seguintes etapas:
   
    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Como **Tipo de SSO**, selecione **SAML**.
   
    b. Na caixa de texto **ID da Entidade do Provedor de Identidade**, cole o valor da **ID da Entidade SAML** copiado do portal do Azure. 
   
    c. Na caixa de texto **Serviço de Logon Único do Provedor de Identidade**, cole o valor da **URL do Serviço de Logon Único SAML** copiado do portal do Azure. 
   
    d. Clique em **Salvar**.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/sd-elements-tutorial/create_aaduser_01.png) 

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/sd-elements-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/sd-elements-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/sd-elements-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-sd-elements-test-user"></a>Criar um usuário de teste de elementos de SD

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no SD Elements. No caso de SD Elements, criar usuários do SD Elements é uma tarefa manual.

**Para criar Brenda Fernandes no SD Elements, execute as seguintes etapas:**

1. Em uma janela de navegador da web, faça logon no site SD Elements da sua empresa como administrador.

1. No menu na parte superior, clique em **Gerenciamento de Usuários** e, depois, em **Usuários**.
   
    ![Criar um usuário de teste de elementos de SD](./media/sd-elements-tutorial/tutorial_sd-elements_11.png) 

1. Clique em **Adicionar Novo Usuário**.
   
    ![Criar um usuário de teste de elementos de SD](./media/sd-elements-tutorial/tutorial_sd-elements_12.png)
 
1. Na caixa de diálogo **Adicionar Novo Usuário**, realize as seguintes etapas:
   
    ![Criar um usuário de teste de elementos de SD](./media/sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. Na caixa de texto **Email**, insira o email do usuário, como **brittasimon@contoso.com**.
   
    b. Na caixa de texto **Nome**, digite o nome do usuário, como **Brenda**.
   
    c. Na caixa de texto **Sobrenome**, digite o sobrenome do usuário como **Fernandes**.
   
    d. Como **Função**, selecione **Usuário**. 
   
    e. Clique em **Criar Usuário**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao SD Elements.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao SD Elements, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **SD Elements**.

    ![Configurar o logon único](./media/sd-elements-tutorial/tutorial_sdelements_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.
  
Quando você clica no bloco SD Elements no Painel de Acesso, deve fazer logon automaticamente no seu aplicativo SD Elements.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/sd-elements-tutorial/tutorial_general_203.png

