---
title: Escopos, permissões e consentimento do Azure Active Directory v2.0 | Microsoft Docs
description: Uma descrição da autorização no ponto de extremidade v2.0 do Azure AD, incluindo escopos, permissões e consentimento.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin, jesakowi, justhu
ms.custom: aaddev
ms.openlocfilehash: 5283782188eaebe3997b6de31b087da74cf10486
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620125"
---
# <a name="permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Permissões e consentimento no ponto de extremidade v2.0 do Azure Active Directory

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Os aplicativos que se integram à plataforma de identidade da Microsoft seguem um modelo de autorização que dá aos usuários e administradores controle sobre como os dados podem ser acessados. A implementação do modelo de autorização foi atualizada no ponto de extremidade v2.0 e ele muda a forma como um aplicativo deve interagir com a plataforma de identidade da Microsoft. Este artigo aborda os conceitos básicos deste modelo de autorização, incluindo escopos, permissões e consentimento.

> [!NOTE]
> O ponto de extremidade v 2.0 não dá suporte a todos os cenários e recursos. Para determinar se você deve usar o ponto de extremidade v2.0, leia sobre as [limitações da v2.0](active-directory-v2-limitations.md).

## <a name="scopes-and-permissions"></a>Permissões e escopos

A plataforma de identidade da Microsoft implementa o protocolo de autorização [OAuth 2.0](active-directory-v2-protocols.md). O OAuth 2.0 é um método pelo qual um aplicativo de terceiros pode acessar recursos hospedados na Web em nome do usuário. Qualquer recurso hospedado na Web que se integre à plataforma de identidade da Microsoft tem um identificador de recurso ou *URI de ID de Aplicativo*. Por exemplo, alguns dos recursos hospedados na Web da Microsoft incluem:

* Microsoft Graph: `https://graph.microsoft.com`
* API de Email do Office 365: `https://outlook.office.com`
* Azure AD Graph: `https://graph.windows.net`

> [!NOTE]
> Recomendamos que você use o Microsoft Graph em vez do Azure AD Graph, API de Email do Office 365 e outros recursos.

Isso se aplica a todos os recursos de terceiros que se integraram à plataforma de identidade da Microsoft. Qualquer um desses recursos também pode definir um conjunto de permissões que pode ser usado para dividir a funcionalidade desse recurso em partes menores. Por exemplo, o [Microsoft Graph](https://graph.microsoft.com) definiu permissões para realizar as seguintes tarefas, entre outras:

* Ler o calendário de um usuário
* Escrever no calendário de um usuário
* Enviar email como um usuário

Ao definir esses tipos de permissões, o recurso tem controle refinado sobre seus dados e como a funcionalidade de API é exposta. Um aplicativo de terceiros pode solicitar essas permissões de usuários e administradores que devem aprovar a solicitação antes que o aplicativo possa acessar dados ou agir em nome do usuário. Ao dividir a funcionalidade do recurso em conjuntos menores de permissão, os aplicativos de terceiros podem ser criados para solicitar apenas as permissões específicas que eles precisam para realizar suas funções. Os usuários e administradores conseguem saber exatamente a que dados o aplicativo tem acesso e podem ter mais confiança de que não estão se comportando de forma mal-intencionada. Os desenvolvedores devem sempre obedecer o conceito de privilégios mínimos, solicitando apenas as permissões necessárias para que seus aplicativos funcionem.

No OAuth, esses tipos de permissões são chamados de *escopos*. Eles também costumam ser chamados de *permissões*. Uma permissão é representada na plataforma de identidade da Microsoft como um valor de cadeia de caracteres. Continuando com o exemplo do Microsoft Graph,o valor da cadeia de caracteres para cada permissão é:

* Ler o calendário de um usuário usando o `Calendars.Read`
* Escrever no calendário de um usuário usando o `Calendars.ReadWrite`
* Enviar email como um usuário usando `Mail.Send`

Um aplicativo solicita essas permissões com mais frequência especificando os escopos nas solicitações para o ponto de extremidade v2.0 de autorização. No entanto, determinadas permissões com privilégios elevados só podem ser concedidas pelo administrador e geralmente solicitadas/concedidas usando o [ponto de extremidade de consentimento de administrador](v2-permissions-and-consent.md#admin-restricted-scopes). Continue lendo para saber mais.

## <a name="permission-types"></a>Tipos de permissão

A plataforma de identidade da Microsoft dá suporte a dois tipos de permissões: **permissões delegadas** e **permissões de aplicativo**.

* **Permissões delegadas** são usadas por aplicativos que têm um usuário conectado presente. Para esses aplicativos, o usuário ou um administrador consente as permissões que o aplicativo solicita e o aplicativo é delegado para agir como o usuário conectado ao fazer chamadas para o recurso de destino. Algumas permissões delegadas podem ser concedidas por usuários não administrativos, mas algumas permissões de privilégios mais altos exigem o [consentimento do administrador](v2-permissions-and-consent.md#admin-restricted-scopes). Para saber quais funções de administrador podem consentir as permissões delegadas, consulte [Permissões da função de administrador no Microsoft Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

* **Permissões de aplicativo** são usadas por aplicativos executados sem um usuário conectado presente, por exemplo, aplicativos executados como serviços em segundo plano ou daemons.  As permissões de aplicativo só podem ser [concedidas pelo administrador](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

_Permissões efetivas_ são as permissões que seu aplicativo terá ao fazer solicitações para o recurso de destino. É importante compreender a diferença entre as permissões delegadas e de aplicativo que seu aplicativo recebeu e suas permissões reais na hora de fazer chamadas para o recurso de destino.

- Para obter permissões delegadas, as _permissões efetivas_ do aplicativo serão a interseção menos privilegiada das permissões delegadas que tiverem recebido o aplicativo (por meio de consentimento) e os privilégios do usuário conectado no momento. Seu aplicativo não pode ter mais privilégios que o usuário conectado. Dentro das organizações, os privilégios do usuário conectado podem ser determinados pela política ou por associação em uma ou mais funções de administrador. Para saber quais funções de administrador podem consentir as permissões delegadas, consulte [Permissões da função de administrador no Microsoft Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).
  Por exemplo, suponha que seu aplicativo tenha recebido a permissão delegada _User.ReadWrite.All_. Essa permissão concede uma permissão ao seu aplicativo para ler e atualizar o perfil de todos os usuários em uma organização. Se o usuário conectado for um administrador global, seu aplicativo poderá atualizar o perfil de todos os usuários na organização. No entanto, se o usuário conectado não estiver em uma função de administrador, seu aplicativo poderá atualizar apenas o perfil do usuário conectado. Não poderá atualizar os perfis de outros usuários na organização porque o que ele tem permissão para agir em nome de usuário não tem os privilégios.
  
- Para permissões de aplicativo, as _permissões efetivas_ do aplicativo serão o nível completo de privilégios indicado pela permissão. Por exemplo, um aplicativo que tenha a permissão de aplicativo _User.ReadWrite.All_ poderá atualizar o perfil de todos os usuários na organização. 

## <a name="openid-connect-scopes"></a>Escopos do OpenID Connect

A implementação v2.0 do OpenID Connect tem alguns escopos bem definidos que não se aplicam a nenhum recurso específico: `openid`, `email`, `profile` e `offline_access`.

### <a name="openid"></a>openid

Se um aplicativo fizer conexão usando o [OpenID Connect](active-directory-v2-protocols.md), ele deverá solicitar o escopo `openid`. O escopo `openid` aparecerá na página de consentimento da conta corporativa como a permissão "Conectar você" e na página de consentimento da conta pessoal da Microsoft como a permissão "Exibir seu perfil e se conectar a aplicativos e serviços usando sua conta da Microsoft". Com essa permissão, um aplicativo pode receber um identificador exclusivo para o usuário na forma da declaração `sub`. Ele também fornece ao aplicativo acesso ao ponto de extremidade UserInfo. O escopo `openid` também pode ser usado no ponto de extremidade v2.0 para adquirir tokens de ID, que podem ser usados para proteger chamadas HTTP entre diferentes componentes de um aplicativo.

### <a name="email"></a>email

O escopo do `email` pode ser usado com o escopo do `openid` e com muitos outros. Ele concede ao aplicativo acesso ao endereço de email principal do usuário na forma da declaração `email` . A declaração `email` só será incluída nos tokens se um endereço de email estiver associado à conta de usuário, o que nem sempre é o caso. Se estiver usando o escopo de `email`, seu aplicativo deverá estar preparado para lidar com casos em que a declaração `email` não existe no token.

### <a name="profile"></a>Perfil

O escopo do `profile` pode ser usado com o escopo do `openid` e com muitos outros. Ele fornece o acesso de aplicativo a uma quantidade substancial de informações sobre o usuário. As informações que ele pode acessar incluem, mas sem limitação, o nome, sobrenome, nome de usuário preferido e ID de objeto. Para obter uma lista completa de declarações de perfil disponíveis no parâmetro id_tokens para um usuário específico, confira a [`id_tokens`referência](id-tokens.md).

### <a name="offlineaccess"></a>offline_access

O [escopo do `offline_access`](https://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) concede ao seu aplicativo acesso a recursos em nome do usuário por um longo período. Na página de consentimento de conta corporativa, esse escopo aparecerá como a permissão "Acessar dados a qualquer momento". Na página de consentimento de conta pessoal da Microsoft, ele aparecerá como a permissão "Acessar dados a qualquer momento". Quando um usuário aprovar o escopo `offline_access`, seu aplicativo poderá receber tokens de atualização do ponto de extremidade do token v2.0. Os tokens de atualização têm uma vida longa. Seu aplicativo pode obter novos tokens de acesso quando os mais antigos expirarem.

Se o aplicativo não solicitar o escopo `offline_access`, ele não receberá tokens de atualização. Isso significa que, ao resgatar um código de autorização no [fluxo de código de autorização do OAuth 2.0](active-directory-v2-protocols.md), você só receberá de volta um token de acesso do ponto de extremidade `/token`. O token de acesso é válido por um curto período. Geralmente, o token de acesso expira em uma hora. Nesse ponto, seu aplicativo precisa redirecionar o usuário novamente para o ponto de extremidade `/authorize` para obter um novo código de autorização. Durante esse redirecionamento, dependendo do tipo de aplicativo, o usuário poderá ou não precisar inserir suas credenciais novamente ou consentir de novo as permissões.

Para saber mais sobre como obter e usar tokens de atualização, veja a [referência do protocolo v2.0](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Solicitando consentimento de usuário individual

Em uma solicitação de autorização do [OpenID Connect ou OAuth 2.0](active-directory-v2-protocols.md), um aplicativo pode solicitar as permissões de que precisa usando o parâmetro de consulta `scope`. Por exemplo, quando um usuário entra em um aplicativo, o aplicativo envia uma solicitação como o exemplo a seguir (com quebras de linha adicionadas para legibilidade):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

O parâmetro `scope` é uma lista de permissões delegadas separadas por espaço que o aplicativo está solicitando. Cada permissão é indicada acrescentando o valor da permissão ao identificador do recurso (URI da ID de Aplicativo). No exemplo de solicitação, o aplicativo precisa de permissão para ler o calendário e enviar emails como o usuário.

Depois que o usuário insere suas credenciais, o ponto de extremidade v2.0 verifica se há um registro correspondente do *consentimento de usuário*. Se o usuário não tiver autorizado nenhuma das permissões solicitadas anteriormente e se nenhum administrador tiver concedido essas permissões em nome de toda a organização, o ponto de extremidade v 2.0 pedirá ao usuário para conceder as permissões solicitadas.

![Consentimento de conta de trabalho](./media/v2-permissions-and-consent/work_account_consent.png)

Quando o usuário aprovar a solicitação de permissão, o consentimento será registrado para que o usuário não tenha que consentir novamente a cada entrada no aplicativo.

## <a name="requesting-consent-for-an-entire-tenant"></a>Solicitando consentimento para um locatário inteiro

Muitas vezes, quando uma organização adquire uma licença ou assinatura para um aplicativo, a organização deseja configurar proativamente o aplicativo para que ele seja usado por todos os membros. Como parte desse processo, um administrador pode autorizar que o aplicativo atue em nome de todos os usuários no locatário. Se o administrador conceder permissão para todo o locatário, os funcionários da empresa não virão uma página de consentimento para o aplicativo.

Para solicitar consentimento de permissões delegadas para todos os usuários em um locatário, o aplicativo pode usar o ponto de extremidade de consentimento de administrador.

Além disso, os aplicativos devem usar o ponto de extremidade de consentimento do administrador para solicitar permissões de aplicativo.

## <a name="admin-restricted-permissions"></a>Permissões restringidas pelo administrador

Algumas permissões de alto privilégio no ecossistema da Microsoft podem ser definidas como *restritas ao administrador*. Exemplos desses tipos de permissão incluem as seguintes:

* Ler os perfis completos de todos os usuários usando `User.Read.All`
* Gravar dados no diretório da organização usando o `Directory.ReadWrite.All`
* Ler todos os grupos no diretório da organização usando `Groups.Read.All`

Embora um usuário consumidor possa conceder acesso de aplicativo para esse tipo de dados, os usuários organizacionais são impedidos de conceder acesso ao mesmo conjunto de dados confidenciais da empresa. Se seu aplicativo solicita acesso a uma dessas permissões de um usuário organizacional, o usuário receberá uma mensagem de erro que diz que não está autorizado a consentir com as permissões de seu aplicativo.

Se seu aplicativo requer acesso a escopos restritos a administradores para as organizações, você deve solicitá-los diretamente de um administrador da empresa também usando o ponto de extremidade de consentimento do administrador descrito a seguir.

Se o aplicativo está solicitando permissões delegadas de privilégio alto e um administrador concede essas permissões pelo ponto de extremidade de consentimento do administrador, a permissão é concedida para todos os usuários no locatário.

Se o aplicativo está solicitando permissões de aplicativo e um administrador concede essas permissões por meio de um ponto de extremidade de consentimento do administrador, essa concessão não é feita em nome de um usuário específico. Na verdade, o aplicativo cliente recebe as permissões *diretamente*. Geralmente, esses tipos de permissões só são usados por serviços de daemon e por outros aplicativos não interativos que são executados em segundo plano.

## <a name="using-the-admin-consent-endpoint"></a>Usando o ponto de extremidade de consentimento do administrador

Quando um administrador da empresa usa seu aplicativo e é direcionado para o ponto de extremidade autorizado, a plataforma de identidade da Microsoft detecta a função do usuário e pergunta se ele gostaria de conceder as permissões solicitadas por você em nome de todo o locatário. No entanto, há também um ponto de extremidade de consentimento de administrador dedicado que você pode usar se quiser solicitar proativamente que um administrador conceda permissão em nome do locatário inteiro. O uso desse ponto de extremidade também é necessário para solicitar Permissões de aplicativo (que não podem ser solicitadas usando o ponto de extremidade de autorização).

Se você seguir estas etapas, seu aplicativo poderá solicitar permissões para todos os usuários em um locatário, incluindo escopos restringidos pelo administrador. Isso é uma operação com privilégios elevados e só deverá ser feita se for necessária para seu cenário.

Para obter um exemplo de código que implementa as etapas, veja o [exemplo de escopos restritos ao administrador](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Solicitar as permissões no portal de registro do aplicativo

O consentimento do administrador não aceita um parâmetro de escopo e, portanto, todas as permissões solicitadas devem ser definidas estaticamente no registro do aplicativo. Em geral, é uma prática recomendada para garantir que as permissões definidas estaticamente para determinado aplicativo sejam um subconjunto das permissões que ele está solicitando dinamicamente/incrementalmente.

Para configurar a lista de permissões solicitadas estaticamente para um aplicativo: 
1. Vá para seu aplicativo no [Portal de Registro de Aplicativo](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ou [crie um aplicativo](quickstart-v2-register-an-app.md) se ainda não tiver feito isso.
2. Localize a seção **Permissões do Microsoft Graph** e adicione as permissões que seu aplicativo requer.
3. **Salve** o registro do aplicativo.

### <a name="recommended-sign-the-user-in-to-your-app"></a>Recomendado: conectar o usuário ao aplicativo

Normalmente, quando você cria um aplicativo que usa o ponto de extremidade de consentimento do administrador, o aplicativo precisa de uma página ou de um modo de exibição em que o administrador possa aprovar as permissões do aplicativo. Essa página pode ser parte do fluxo de inscrição no aplicativo, parte das configurações do aplicativo ou um fluxo dedicado de "conexão". Em muitos casos, faz sentido que o aplicativo somente mostre o modo de exibição "conectar" depois que o usuário entra com uma conta corporativa ou de estudante da Microsoft.

Quando o usuário entra em seu aplicativo, você pode identificar a organização à qual o administrador pertence antes de pedir a ele que aprove as permissões necessárias. Embora não seja estritamente necessário, isso pode ajudá-lo a criar uma experiência mais intuitiva para os usuários empresariais. Para conectar o usuário, siga nossos [tutoriais de protocolo v2.0](active-directory-v2-protocols.md).

### <a name="request-the-permissions-from-a-directory-admin"></a>Solicitar permissões de um administrador de diretório

Quando você estiver pronto para solicitar permissões de administrador da empresa, redirecione o usuário para o *ponto de extremidade de consentimento do administrador* v2.0.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parâmetro | Condição | DESCRIÇÃO |
| --- | --- | --- |
| `tenant` | Obrigatório | O locatário do diretório para o qual você deseja solicitar permissão. Pode ser fornecido em GUID ou formato de nome amigável OU referenciado genericamente com "comum", como visto no exemplo. |
| `client_id` | Obrigatório | A ID de aplicativo do [Portal de Registro de Aplicativo](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) atribuída ao seu aplicativo. |
| `redirect_uri` | Obrigatório |O URI de redirecionamento onde você deseja que a resposta seja enviada para ser tratada pelo aplicativo. Ela deve corresponder exatamente a um redirecionamento de URIs que você registrou no portal de registro de aplicativo. |
| `state` | Recomendadas | Um valor incluído na solicitação também será retornado na resposta do token. Pode ser uma cadeia de caracteres de qualquer conteúdo desejado. Use o estado para codificar as informações sobre o estado do usuário no aplicativo antes da solicitação de autenticação ocorrida, como a página ou exibição em que ele estava. |

Neste ponto, o Azure AD requer um administrador de locatários para entrar e concluir a solicitação. O administrador deverá aprovar todas as permissões que você solicitou para o aplicativo no portal de registro de aplicativos.

#### <a name="successful-response"></a>Resposta bem-sucedida

Se o administrador aprovar as permissões para o seu aplicativo, a resposta bem-sucedida será:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parâmetro | DESCRIÇÃO |
| --- | --- | --- |
| `tenant` | O locatário do diretório que concedeu as permissões solicitadas, no formato de GUID. |
| `state` | Um valor incluído na solicitação também será retornado na resposta do token. Pode ser uma cadeia de caracteres de qualquer conteúdo desejado. O estado é usado para codificar as informações sobre o estado do usuário no aplicativo antes da solicitação de autenticação ocorrida, como a página ou exibição em que ele estava. |
| `admin_consent` | Será definido como **true**. |

#### <a name="error-response"></a>Resposta de erro

Se o administrador não aprovar as permissões para o seu aplicativo, a resposta de falha será:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parâmetro | DESCRIÇÃO |
| --- | --- | --- |
| `error` |Uma cadeia de caracteres de códigos de erro que pode ser usada para classificar tipos de erro que ocorrem e pode ser usada para responder aos erros. |
| `error_description` |Uma mensagem de erro específica que pode ajudar um desenvolvedor a identificar a causa raiz de um erro. |

Depois de receber uma resposta bem-sucedida do ponto de extremidade de consentimento do administrador, o aplicativo terá as permissões solicitadas por ele. Em seguida, você pode solicitar um token para o recurso desejado.

## <a name="using-permissions"></a>Usando permissões

Depois que o usuário consente permissões para o aplicativo, este pode adquirir tokens de acesso que representam a permissão do seu aplicativo para acessar um recurso em alguma capacidade. Um token de acesso só pode ser usado para um único recurso, mas codificada dentro do token de acesso estará cada permissão que o aplicativo recebeu para esse recurso. Para adquirir um token de acesso, o aplicativo poderá fazer uma solicitação ao ponto de extremidade do token v2.0 como esta:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Você pode usar o token de acesso resultante em solicitações HTTP para o recurso. Ele confiável indica ao recurso que seu aplicativo tem a permissão apropriada para executar uma tarefa específica. 

Para saber mais sobre o protocolo OAuth 2.0 e como obter tokens de acesso, consulte a [referência do protocolo do ponto de extremidade v2.0](active-directory-v2-protocols.md).

## <a name="troubleshooting"></a>solução de problemas

Se você ou os usuários do aplicativo estiverem vendo erros inesperados durante o processo de consentimento, confira este artigo para ver etapas de solução de problemas: [Erro inesperado ao executar o consentimento para um aplicativo](../manage-apps/application-sign-in-unexpected-user-consent-error.md).
