---
title: 'Tutorial: Integração do Azure Active Directory ao Canvas LMS | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Canvas LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: af2c997f0842da751eb93f0788a7402fc7d144ae
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433282"
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Tutorial: Integração do Azure Active Directory ao Canvas LMS

Neste tutorial, você aprende a integrar o Canvas ao Azure AD (Azure Active Directory).

A integração do Canvas ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Canvas
- É possível permitir que os usuários se conectem automaticamente ao Canvas (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Canvas, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Canvas

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Canvas por meio da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-canvas-from-the-gallery"></a>Adicionando o Canvas por meio da galeria
Para configurar a integração do Canvas ao Azure AD, você precisa adicionar o Canvas à lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Canvas por meio da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **Canvas**.

    ![Criação de um usuário de teste do AD do Azure](./media/canvas-lms-tutorial/tutorial_canvaslms_search.png)

1. No painel de resultados, selecione **Canvas** e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o Canvas, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Canvas é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Canvas.

No Canvas, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Canvas, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criando um usuário de teste do Canvas](#creating-a-canvas-test-user)** – para ter um equivalente de Brenda Fernandes no Canvas que esteja vinculado à representação de usuário do Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo Canvas.

**Para configurar o logon único do Azure AD com o Canvas, realize as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo do **Canvas**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

1. Na seção **Domínio e URLs do Canvas**, realize as seguintes etapas:

    ![Configurar o logon único](./media/canvas-lms-tutorial/tutorial_canvaslms_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<tenant-name>.instructure.com`

    b. Na caixa de texto **Identificador**, digite o valor usando o seguinte padrão: `https://<tenant-name>.instructure.com/saml2`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Contate a [equipe de suporte ao Cliente do Canvas](https://community.canvaslms.com/community/help) para obter esses valores. 
 
1. Na seção **Certificado de Autenticação SAML**, copie o valor da **IMPRESSÃO DIGITAL** do certificado.

    ![Configurar o logon único](./media/canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/canvas-lms-tutorial/tutorial_general_400.png)

1. Na seção **Configuração do Canvas**, clique em **Configurar o Canvas** para abrir a janela **Configurar logon**. Copie a **URL de Alteração de Senha, a URL de Saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção Referência Rápida.**

    ![Configurar o logon único](./media/canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
1. Em outra janela do navegador da Web, faça logon em seu site de empresa do Canvas como administrador.

1. Vá para **Cursos \> Contas Gerenciadas \> Microsoft**.
   
    ![Canvas](./media/canvas-lms-tutorial/IC775990.png "Canvas")

1. No painel de navegação à esquerda, selecione **Autenticação** e clique em **Adicionar Nova Config. do SAML**.
   
    ![Autenticação](./media/canvas-lms-tutorial/IC775991.png "Autenticação")

1. Na página de integração atual, execute as seguintes etapas:
   
    ![Integração Atual](./media/canvas-lms-tutorial/IC775992.png "integração Atual")

    a. Na caixa de texto **ID da Entidade do IdP**, cole o valor da **ID da Entidade SAML** copiado no portal do Azure.

    b. Na caixa de texto **URL de Logon**, cole o valor da **URL do Serviço de Logon Único SAML** copiado no portal do Azure.

    c. Na caixa de texto **URL de Logoff**, cole o valor da **URL de Saída** copiado no portal do Azure.

    d. Na caixa de texto **Link Alteração de Senha**, cole o valor da **URL de Alteração de Senha** copiado no portal do Azure. 

    e. Na caixa de texto **Impressão Digital do Certificado**, cole o valor de **Impressão Digital** do certificado copiado do Portal do Azure.      
        
    f. Na lista **Atributo de Logon**, selecione **NameID**.

    g. Na lista **Formato de Identificador**, selecione **emailAddress**.

    h. Clique em **Salvar Configurações de Autenticação**.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/canvas-lms-tutorial/create_aaduser_01.png) 

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/canvas-lms-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/canvas-lms-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/canvas-lms-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-canvas-test-user"></a>Criando um usuário de teste do Canvas

Para permitir que os usuários do Azure AD façam logon no Canvas, eles devem ser provisionados no Canvas.

No caso do Canvas, o provisionamento de usuário é uma tarefa manual.

**Para provisionar uma conta de usuário, execute as seguintes etapas:**

1. Faça logon em seu locatário do **Canvas** .

1. Vá para **Cursos \> Contas Gerenciadas \> Microsoft**.
   
   ![Canvas](./media/canvas-lms-tutorial/IC775990.png "Canvas")

1. Clique em **Usuários**.
   
   ![Usuários](./media/canvas-lms-tutorial/IC775995.png "Usuários")

1. Clique em **Adicionar Novo Usuário**.
   
   ![Usuários](./media/canvas-lms-tutorial/IC775996.png "Usuários")

1. Na página da caixa de diálogo Adicionar um Novo Usuário, execute as seguintes etapas:
   
   ![Adicionar Usuário](./media/canvas-lms-tutorial/IC775997.png "Adicionar Usuário")
   
   a. Na caixa de texto **Nome Completo**, insira o nome de usuário, como **BrendaFernandes**.

   b. Na caixa de texto **Email**, insira o email de usuário como **brittasimon@contoso.com**.

   c. Na caixa de texto **Logon**, insira o endereço de email do Azure AD do usuário como **brittasimon@contoso.com**.

   d. Selecione **Enviar email ao usuário sobre a criação desta conta**.

   e. Clique em **Adicionar Usuário**.

>[!NOTE]
>É possível usar qualquer outra ferramenta de criação da conta de usuário do Canvas ou as APIs fornecidas pelo Canvas para provisionar as contas de usuário do AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao Canvas.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Canvas, realize as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Canvas**.

    ![Configurar o logon único](./media/canvas-lms-tutorial/tutorial_canvaslms_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco Canvas no Painel de Acesso, deverá ser automaticamente conectado ao aplicativo Canvas.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/canvas-lms-tutorial/tutorial_general_203.png

