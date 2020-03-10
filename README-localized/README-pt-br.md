---
page_type: sample
products:
- office
- office-365
- ms-graph
languages:
- python
- javascript
description: "Como integrar-se aos dados das funções / lista de alunos da escola, bem como aos serviços do O365 disponíveis através da API do Graph"
urlFragment: edu-graph-api-python
extensions:
  contentType: samples
  technologies:
  - Microsoft Graph
  services:
  - Office 365
  - Eduation
  - Education
  createdDate: "6/6/2016 1:23:24 PM"
  scenarios:
  - Education
---

# EDUGraphAPI – Exemplo de Código Educacional do Office 365 (Phyton)

Neste exemplo, vamos mostrar como integrar com dados da escola/lista de funções, além de serviços do O365 através da API do Graph. 

Os dados da escola são mantidos em sincronia nos locatários do O365 Education pelo [Microsoft School Data Sync](http://sds.microsoft.com).  

**Sumário**

- [Metas de exemplo](#sample-goals)
- [Pré-requisitos](#prerequisites)
- [Registrar o aplicativo no Azure Active Directory](#register-the-application-in-azure-active-directory)
- [Executar o exemplo local](#run-the-sample-locally)
- [Implantar o exemplo no Azure](#deploy-the-sample-to-azure)
- [Compreender o código](#understand-the-code)
- [Perguntas e comentários](#questions-and-comments)
- [Colaboração](#contributing)

## Metas de exemplo

O exemplo demonstra:

- Chamando APIs do Graph, incluindo:

  - [Microsoft Azure Active Directory Graph](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)
  - [API do Microsoft Graph](https://www.nuget.org/packages/Microsoft.Graph/)

- Vincular contas de usuário gerenciadas localmente e contas de usuário do Office 365 (Azure Active Directory). 

  Após vincular as contas, os usuários podem usar as contas locais ou do Office 365 para entrar no site de exemplo e usá-lo.

- Obter escolas, seções, professores e alunos do Office 365 Education:

  - [Referência da API REST do Office 365 Schools](https://msdn.microsoft.com/office/office365/api/school-rest-operations)

Este exemplo é implementado com o idioma Python e a estrutura da web[Django](https://www.djangoproject.com/).

> [Django](https://www.djangoproject.com/) é uma estrutura da Web Python de alto nível que incentiva o desenvolvimento rápido e o design simples e pragmático. Criados por desenvolvedores experientes, isso cuida muito das complicações do desenvolvimento na Web, para que você possa se concentrar em escrever seu aplicativo sem precisar reinventar a roda. É gratuito e de fonte aberta.

## Pré-requisitos

**A implantação e execução desse exemplo requer**:

- Uma assinatura do Azure com permissões para registrar um novo aplicativo e implantação do aplicativo Web.

- Um locatário do O365 Education com o Microsoft School Data Sync habilitado

  - Um dos seguintes navegadores: Edge, Internet Explorer 9, Safari 5.0.6, Firefox 5, Chrome 13 ou uma versão mais recente de um desses navegadores.

  Além disso: O desenvolvimento/execução desse exemplo local requer o seguinte:  

  - [Python](https://www.python.org/downloads/) 3.5.2 ou superior
  - [Django](https://www.djangoproject.com/download/) 1.11 ou superior
  - [SQLite](https://www.sqlite.org/)



## Registrar o aplicativo no Azure Active Directory

1. Entre no novo portal do Azure: [https://portal.azure.com](https://portal.azure.com/).

2. Escolha seu locatário do Azure AD ao selecionar sua conta no canto superior direito da página:

   ![](Images/aad-select-directory.png)

3. Clique em **Azure Active Directory** -> **Registros de aplicativo** -> **+Adicionar**.

   ![](Images/aad-create-app-01.png)

4. Insira um **Nome** e selecione **Aplicativo Web/API** como **Tipo de Aplicativo**.

   Entada **URL de entrada**: http://127.0.0.1:8000/

   ![](Images/aad-create-app-02.png)

   Clique em **Criar**.

5. Depois de concluído, o aplicativo será exibido na lista.

   ![](/Images/aad-create-app-03.png)

6. Clique nele para exibir os detalhes. 

   ![](/Images/aad-create-app-04.png)

7. Clique em **Todas configurações**, se a janela de configuração não aparecer.

   - Clique em **Propiedades** e, em seguida, defina **Multilocatário** para **Sim**.

     ![](/Images/aad-create-app-05.png)

     Copie aparte o **ID do aplicativo** e, em seguida, clique em **Salvar**.

   - Clique em **Permissões necessárias**. Adicione as seguintes permissões:

     | API | Permissões de Aplicativo | Permissões Delegadas |
| ------------------------------ | ----------------------- | ---------------------------------------- |
| Microsoft Graph | Ler dados do diretório | Ler os perfis completos de todos os usuários<br>Ler dados do diretório<br>Ler dados do diretório<br>Acessar o diretório como o usuário conectado<br>Conectar usuários |
| Windows Azure Active Directory | Entrar e ler perfil do usuário<br>Ler e gravar dados de diretório |

     ![](/Images/aad-create-app-06.png)

   - Clique em **Chaves** e, em seguida, adicione uma nova:

     ![](Images/aad-create-app-07.png)

     Clique em **Salvar** e, em seguida, copie aparte o **VALOR** da chave. 

   Feche a janela de Configurações.

## Executar o exemplo local

Estes são os softwares necessários:

- [Python](https://www.python.org/downloads/) 3.5.2 ou superior
- [Django](https://www.djangoproject.com/download/) 1.11 ou superior
- [SQLite](https://www.sqlite.org/)

Execute o aplicativo:

1. Configure as seguintes **variáveis de ambiente **:

   - **clientId**: use a ID do cliente do registro do aplicativo criado anteriormente.
   - **clientSecret**: use o valor da chave do registro do aplicativo criado anteriormente.
   - **SourceCodeRepositoryURL**: Use a URL deste repositório.

2. Abra o terminal e navegue até a pasta código-fonte. Execute o comando abaixo:

   ```sh
   pip install -r requirements.txt
   python manage.py runserver
   ```

3. Abra http://127.0.0.1:8000/em um navegador.

## Implantar o exemplo no Azure

**Autorização do GitHub**

1. Gerar token

   - Abra https://github.com/settings/tokens no seu navegador da Web.
   - Entre na sua conta do GitHub na qual você bifurca esse repositório.
   - Clique em **Gerar Token**
   - Inserir um valor na caixa de texto da **descrição do Token**
   - Selecione os seguintes (as seleções devem corresponder à captura de tela abaixo):
     - repo (all) -> repo:status, repo\_deployment, public\_repo
     - admin:repo\_hook -> read:repo\_hook

   ![](Images/github-new-personal-access-token.png)

   - Clique em **Gerar token**
   - Copiar o token

2. Adicionar o token GitHub ao Azure no Gerenciador de Recursos do Azure

   - Abra https://resources.azure.com/providers/Microsoft.Web/sourcecontrols/GitHub no seu navegador da Web.
   - Entre com sua conta do Azure.
   - A assinatura do Azure está correta.
   - Marque modo**Leitura/Gravação**.
   - Clique em **Editar**.
   - Cole o token no **token de parâmetro**.

   ![](Images/update-github-token-in-azure-resource-explorer.png)

   - Clique em **COLOCAR**

**Implantar os componentes do Azure do GitHub**

1. Verifique se a compilação está passando pela criação do VSTS.

2. Bifurque esse repositório para a sua conta do GitHub.

3. Clique no botão implantar no Azure:

   [![Implantar no Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2FO365-EDU-Python-Samples%2Fmaster%2Fazuredeploy.json)

4. Preencha os valores na página de implantação e marque a caixa de seleção **Concordo com os termos e condições descritos acima**.

   ![](Images/azure-auto-deploy.png)

   - **Grupo de recursos**: sugerimos a criação de um novo grupo.

   - **Nome do site**: forneça um nome. Como EDUGraphAPICanviz ou EDUGraphAPI993.

     > Observação: Se o nome digitado já foi usado, ocorrerá alguns erros de validação:
     >
     > ![](Images/azure-auto-deploy-validation-errors-01.png)
     >
     > Clique nele para obter mais detalhes, por exemplo, se a conta de armazenamento já está em outro grupo de recursos/assinatura.
     >
     > Nesse caso, use outro nome.

   - **URL do Repositório de Código-fonte**: substitua <YOUR REPOSITORY> pelo nome do repositório da sua bifurcação.

   - **Integração Manual do Código-fonte**: escolha **falso**, uma vez que você está implantando da sua bifurcação.

   - **Id do Cliente**: use a ID do cliente do registro do aplicativo criado anteriormente.

   - **Segredo do Cliente**: use o valor da chave do registro do aplicativo criado anteriormente.

   - Marque **Concordo com os termos e condições declarados acima**.

5. Clique em **Comprar**.

**Adicionar URL DE RESPOSTA ao registro de aplicativo**

1. Após a implantação, abra o grupo de recursos:

   ![](Images/azure-resource-group.png)

2. Clique no aplicativo Web.

   ![](Images/azure-web-app.png)

   Copie a URL aparte e altere o esquema para **https**. Essa é a URL de reprodução e será usada na próxima etapa.

3. Navegue até o registro do aplicativo no novo portal do Azure e, em seguida, abra as configurações do Windows.

   Adicionar a URL de resposta:

   ![](Images/aad-add-reply-url.png)

   > Observação: para depurar o exemplo localmente, certifique-se de que https://127.0.0.1:8000/ esteja nas URLs de resposta.

4. Clique em **SALVAR**.

## Compreender o código

### Introdução

**Diagrama de componente de solução**

![](Images/solution-component-diagram.png)

### **EDUGraphAPI**

**Autenticação**

O Django vem com um sistema de autenticação de usuário. Ele manipula contas de usuários, grupos, permissões e sessões de usuário baseadas em cookies. Para obter mais detalhes, consulte [Sistema de autenticação Django](https://docs.djangoproject.com/en/1.10/topics/auth/).

A autenticação de usuários locais se baseia na API interna:

* [autenticação](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.authenticate)
* [logon](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.login)
* [logout](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.logout)

A autenticação de usuários do O365 é implementada com a ID aberta. 

**Acesso a dados**

Nesta amostra, o [ORM interno do Django](https://docs.djangoproject.com/en/1.11/topics/db/) é usado para acessar dados do banco de dados SQLite de back-end.

Veja a seguir as tabelas:

| Tabela | Descrição |
| ------------------------------ | ---------------------------------------- |
| auth\_user | tabela interna de usuário Django, que contém informações de autenticação de usuários: nome de usuário, e-mail, senha... |
| user\_roles | Contém as funções dos usuários. Três funções são usadas neste exemplo: administrador, professor e estudante. |
| perfis | Contém as informações adicionais dos usuários: *favoriteColor*, *organization\_id*, *o365UserId*e *o365Email*. Os dois posteriores são usados para conectar o usuário local com um usuário do O365\. |
| organizações | Uma linha nessa tabela representa um locatário no AAD.<br>A coluna *IsAdminConsented* registra se o locatário foi consentido por um administrador. |
| TokenCache | Contém os tokens de acesso/atualização dos usuários. |
| ClassroomSeatingArrangements | Contém as disposições dos assentos da sala de aula. |

Os modelos são definidos em **/models/db.py**.

**Exibições**

Veja a seguir os modos de exibição:

| Caminho | Descrição |
| -------------------- | ---------------------------------------- |
| /Account/views.py | Contém modos de exibição para que os usuários registrem, façam logon e efetuem logoff. |
| /admin/views.py | Inclui modos de exibição administrativos como locatário do consentimento, gerenciar contas vinculadas. |
| /link/views.py | Contém modos de exibição usados para vincular contas de usuários. |
| /Management/views.py | Contém modos de exibição da página sobre mim. |
| /Schools/View.py | Inclui modos de exibição de educação, como escolas, aulas e detalhes da classe. |

**Decoradores**

Os decoradores a seguir foram usados neste aplicativo:

| Decorador | Descrição |
| ----------------- | ---------------------------------------- |
| login\_required | Permite que somente usuários conectados acessem os recursos protegidos. |
| admin\_only | Permite que somente os administradores acessem os recursos protegidos. |
| linked\_users\_only | Permite que somente usuários vinculados acessem os recursos protegidos. |

**Serviços**

Os serviços usados pelo aplicativo do lado do servidor:

| Serviço | Descrição |
| ---------------- | ---------------------------------------- |
| UserService | Contém o método usado para manipular usuários no banco de dados. |
| LinkService | Contém o método usado para vincular contas de usuários. |
| AuthService | Contém métodos usados para autorização e autenticação. |
| AADGraphService | Contém métodos usados para acessar as APIs do AAD Graph. |
| MSGraphService | Contém métodos usados para acessar APIs do MS Graph. |
| EducationService | Contém dois métodos para acessar APIs REST do Education. |
| TokenService | Contém o método usado para obter e atualizar tokens do banco de dados |

Os serviços estão na pasta **/services**.

**Middleware**

**`HandleRefreshTokenExceptionMiddleware`** Catches `RefreshTokenException` lançados por `TokenService` quando o token de atualização não existir ou tiver expirado. Em seguida, ele redireciona o usuário para uma página para permitir que o usuário faça logon novamente.

**Aplicativos multilocatário**

Esse aplicativo da Web é um **aplicativo multilocatário**. No AAD, habilitamos a opção:

![](Images/app-is-multi-tenant.png)

Os usuários de qualquer locatário do Azure Active Directory podem acessar esse aplicativo. Algumas permissões usadas por este aplicativo exigem que um administrador do locatário consinta antes que os usuários possam usar o aplicativo. Caso contrário, os usuários verão o seguinte erro:

![](Images/app-requires-admin-to-consent.png)

Para saber mais, confira [Criar um aplicativo Web SaaS multilocatário usando o Azure AD e OpenID Connect](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapp-multitenant-openidconnect/).

### API do Office 365 Education

As [APIs do Office 365 Education](https://msdn.microsoft.com/office/office365/api/school-rest-operations) ajudam a retornar dados de qualquer locatário do Office 365 sincronizado com a nuvem pelo Microsoft School Data Sync. Essas APIs fornecem informações sobre escolas, seções, professores, estudantes e listamentos. A API REST de Escolas fornece acesso a entidades de ensino no Office 365 para locatários da Educação.

Nesse exemplo, o projeto Biblioteca de Classes do **Microsoft.Education** foi criado para encapsular a API do Office 365 Education. 

O **EducationServiceClient** é a principal classe da biblioteca. É usado para obter dados de educação facilmente.

**Obter escolas**

~~~typescript
def get_schools(self):
    url = self.api_base_uri + 'administrativeUnits'
    return self.rest_api_service.get_object_list(url, self.access_token, model=School)
~~~

~~~typescript
def get_school(self, object_id):
    url = self.api_base_uri + 'administrativeUnits/%s' % object_id
    return self.rest_api_service.get_object(url, self.access_token, model=School)
~~~

**Obter classes**

~~~typescript
def get_sections(self, school_id, top=12, nextlink=''):
    skiptoken = self._get_skip_token(nextlink)
    url = self.api_base_uri + "groups?$filter=extension_fe2174665583431c953114ff7268b7b3_Education_ObjectType eq 'Section' and extension_fe2174665583431c953114ff7268b7b3_Education_SyncSource_SchoolId eq '%s'&$top=%s%s" % (school_id, top, skiptoken)
    return self.rest_api_service.get_object_list(url, self.access_token, model=Section, next_key='odata.nextLink')
~~~

```typescript
def get_section(self, object_id):
    url = self.api_base_uri + 'groups/%s' % object_id
    return self.rest_api_service.get_object(url, self.access_token, model=Section)
```
**Get users**

```typescript
def get_members(self, object_id, top=12, nextlink=''):
    skiptoken = self._get_skip_token(nextlink)
    url = self.api_base_uri + 'administrativeUnits/%s/members?$top=%s%s' % (object_id, top, skiptoken)
    return self.rest_api_service.get_object_list(url, self.access_token, model=EduUser, next_key='odata.nextLink')
```
Below are some screenshots of the sample app that show the education data.

![](Images/edu-schools.png)

![](Images/edu-users.png)

![](Images/edu-classes.png)

![](Images/edu-class.png)

### Autenticação

Há quatro fluxos de autenticação nesse projeto.

Os 2 primeiros fluxos (Logon Local/O365) permitem que os usuários entrem com uma conta local ou com uma conta do Office 365 e se vinculam a outra conta de tipo. Esse procedimento é implementado no LinkController.

**Fluxo de Autenticação de Logon Local**

![](Images/auth-flow-local-login.png)

**Fluxo de Autenticação de Logon do O365**

![](Images/auth-flow-o365-login.png)

**Fluxo de Autenticação de Logon de Administrador**

Esse fluxo mostra como um administrador faz logon no sistema e realiza operações administrativas.

Após fazer logon no aplicativo com uma conta do Office 365, o administrador será solicitado a vincular a uma conta local. Esta etapa não é necessária e pode ser ignorada. 

Como mencionado anteriormente, o aplicativo Web é um aplicativo multilocatário que usa algumas permissões de aplicativo; portanto, o administrador do locatário deve consentir o aplicativo primeiro.  

Esse fluxo é implementado no AdminController. 

![](Images/auth-flow-admin-login.png)

### Dois Tipos de API do Graph

Há duas APIs do Graph diferentes usadas nesse exemplo:

|
| [API do Azure AD Graph](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api) | [API do Microsoft Graph](https://graph.microsoft.io/) |
| ------------ | ---------------------------------------- | ---------------------------------------- |
| Descrição | A API do Graph do Azure Active Directory fornece acesso programático para o Azure Active Directory por meio de pontos de extremidade da API REST. Os aplicativos podem usar a API do Azure AD Graph para executar operações de criação, leitura, atualização e exclusão (CRUD) em dados de diretório e objetos de diretório, como usuários, grupos e contatos organizacionais | Uma API unificada que inclui APIs de outros serviços da Microsoft, como o Outlook, o OneDrive, o OneNote, o Planner e o Office Graph, todas acessadas por meio de um único ponto de extremidade com um único token de acesso. |
| Cliente | Install-Package [Microsoft. Azure. ActiveDirectory. GraphClient](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/) | Install-Package [Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) |
| Ponto de extremidade | https://graph.windows.net | https://graph.microsoft.com |
| Gerenciador de API | https://graphexplorer.cloudapp.net/| https://graph.microsoft.io/graph-explorer |

> **OBSERVAÇÃO IMPORTANTE:** A Microsoft está investindo pesadamente na nova API do Microsoft Graph e não está mais investindo na API do Graph do Azure AD (exceto corrigindo problemas de segurança).

> Portanto, tente usar mais a nova API do Microsoft Graph ao invés da API do Graph do Azure AD.

A seguir, um pedaço de código mostra como obter documentos do grupo na API do Microsoft Graph.

```typescript
def get_documents(self, object_id):
    url = self.api_base_uri + 'groups/%s/drive/root/children' % object_id
    return self.rest_api_service.get_object_list(url, self.access_token, model=Document)
```

Observe que nas configurações do Aplicativo AAD, as permissões de cada API do Graph são configuradas separadamente:

![](Images/aad-create-app-06.png) 

## Perguntas e comentários

- Se você tiver problemas para executar este exemplo, [relate um problema](https://github.com/OfficeDev/O365-EDU-Python-Samples/issues).
- Perguntas sobre o desenvolvimento do GraphAPI em geral devem ser postadas no [Stack Overflow](http://stackoverflow.com/questions/tagged/office-addins). Não deixe de marcar as perguntas ou comentários com \[ms-graph-api]. 

## Colaboração

Recomendamos que você contribua para nossos exemplos. Para obter diretrizes sobre como proceder, confira [nosso guia de contribuição](/CONTRIBUTING.md).

Este projeto adotou o [Código de Conduta de Código Aberto da Microsoft](https://opensource.microsoft.com/codeofconduct/).  Para saber mais, confira as [Perguntas frequentes sobre o Código de Conduta](https://opensource.microsoft.com/codeofconduct/faq/) ou entre em contato pelo [opencode@microsoft.com](mailto:opencode@microsoft.com) se tiver outras dúvidas ou comentários.



**Copyright (c) 2017 Microsoft. Todos os direitos reservados.**
