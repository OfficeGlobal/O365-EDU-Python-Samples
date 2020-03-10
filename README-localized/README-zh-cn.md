---
page_type: sample
products:
- office
- office-365
- ms-graph
languages:
- python
- javascript
description: "如何通过 Graph API 集成学校角色/名册数据与可用 O365 服务。"
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

# EDUGraphAPI - Office 365 教育版代码示例 (Python)

在此示例中，我们将向你展示如何通过 Graph API 与学校角色/名册数据以及可用的 O365 服务集成。 

O365 教育版租户中的学校数据通过 [Microsoft School Data Sync](http://sds.microsoft.com)保持同步。  

**目录**

- [示例目标](#sample-goals)
- [先决条件](#prerequisites)
- [在 Azure Active Directory 中注册应用程序](#register-the-application-in-azure-active-directory)
- [本地运行示例](#run-the-sample-locally)
- [将示例部署到 Azure](#deploy-the-sample-to-azure)
- [了解代码](#understand-the-code)
- [问题和意见](#questions-and-comments)
- [参与](#contributing)

## 示例目标

本示例演示：

- 调用 Graph API，包括：

  - [Microsoft Azure Active Directory Graph API](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)
  - [Microsoft Graph API](https://www.nuget.org/packages/Microsoft.Graph/)

- 关联本地托管用户账户和 Office 365 (Azure Active Directory) 用户账户。 

  关联账户后，用户可使用本地或 Office 365 账户登录示例网站并使用。

- 从 Office 365 教育版中获取学校、章节、教师和学生：

  - [Office 365 Schools REST API 参考](https://msdn.microsoft.com/office/office365/api/school-rest-operations)

此示例采用 Python 语言和 [Django](https://www.djangoproject.com/) Web 框架实现。

> [Django](https://www.djangoproject.com/) 是一种高级 Python Web 框架，有助于快速开发、设计简洁实用。由经验丰富的开发人员构建，可解决 Web 开发方面的许多麻烦，能跟专注于编写，避免重复工作。该框架免费，采用开源代码。

## 先决条件

**部署和运行此示例需要**：

- 具有注册新应用、部署 Web 应用权限的 Azure 订阅。

- 启用了 Microsoft School Data Sync 的 O365 教育版租户

  - 下列一款浏览器：Microsoft Edge、Internet Explorer 9、Safari 5.0.6、Firefox 5、Chrome 13 或这些浏览器的更高版本。

  另外：本地部署/运行此示例需要下列内容：  

  - [Python](https://www.python.org/downloads/) 3.5.2 或更高版本
  - [Django](https://www.djangoproject.com/download/) 1.11 或更高版本
  - [SQLite](https://www.sqlite.org/)



## 在 Azure Active Directory 中注册应用程序

1. 登录新的 Azure 门户：[https://portal.azure.com/](https://portal.azure.com/).

2. 在页面右上角选择帐户，选择 Azure AD 租户：

   ![](Images/aad-select-directory.png)

3. 单击“**Azure Active Directory**” -> “**应用程序注册** -> “**+添加**”。

   ![](Images/aad-create-app-01.png)

4. 输入“**名称**”，并选择“**Web 应用 / API**”作为“**应用类型**”。

   输入**登录 URL**：http://127.0.0.1:8000/

   ![](Images/aad-create-app-02.png)

   单击“**创建**”。

5. 完成后，应用程序在列表中显示。

   ![](/Images/aad-create-app-03.png)

6. 单击可查看详细信息。 

   ![](/Images/aad-create-app-04.png)

7. 如果设置窗口未显示，单击“**所有设置**”。

   - 单击“**属性**”，然后设置“**多租户**”至“**是**”。

     ![](/Images/aad-create-app-05.png)

     复制“**应用程序 ID**”，随后单击“**保存**”。

   - 单击“**必需的权限**”。添加以下权限：

     | API | 应用程序权限 | 委派权限 |
| ------------------------------ | ----------------------- | ---------------------------------------- |
| Microsoft Graph | 阅读目录数据 | 阅读所有用户的完整资料<br>读取目录数据<br>读取目录数据<br>以登录用户身份访问目录<br>用户登录 |
| Windows Azure Active Directory |
| 登录并读取用户资料<br>读取和写入目录数据 |

     ![](/Images/aad-create-app-06.png)

   - 单击“**密钥**”，随后添加新密钥：

     ![](Images/aad-create-app-07.png)

     单击“**保存**”，随后复制密钥“**值**”。 

   关闭“设置”窗口。

## 本地运行示例

需要下列软件：

- [Python](https://www.python.org/downloads/) 3.5.2 或更高版本
- [Django](https://www.djangoproject.com/download/) 1.11 或更高版本
- [SQLite](https://www.sqlite.org/)

运行应用程序：

1. 配置以下 **环境变量**：

   - **clientId**：使用之前创建的应用注册客户端 ID。
   - **clientId**：使用之前创建的应用密钥值。
   - **SourceCodeRepositoryURL**：使用此存储库的 URL。

2. 打开终端并导航至源代码文件夹。运行下列命令：

   ```sh
   pip install -r requirements.txt
   python manage.py runserver
   ```

3. 在浏览器中打开 http://127.0.0.1:8000/。

## 将示例部署到 Azure

**GitHub 身份验证**

1. 生成令牌

   - 在 Web 浏览器中打开 https://github.com/settings/tokens。
   - 登录分叉此存储库的 GitHub 账户。
   - 单击“**生成令牌**”
   - 在“**令牌说明**”文本框中输入数值。
   - 选择下列内容（选择内容应与以下截屏匹配）：
     - repo (all) -> repo:status, repo\_deployment, public\_repo
     - admin:repo\_hook -> read:repo\_hook

   ![](Images/github-new-personal-access-token.png)

   - 单击“**生成令牌**”
   - 复制令牌

2. 在 Azure 资源浏览器中添加 GitHub 令牌至 Azure

   - 在 Web 浏览器中打开 https://resources.azure.com/providers/Microsoft.Web/sourcecontrols/GitHub。
   - 使用你的 Azure 帐户登录。
   - 已选择正确的 Azure 订阅。
   - 选择“**读/写**”模式。
   - 单击“**编辑**”。
   - 粘贴令牌到“**令牌参数**”中。

   ![](Images/update-github-token-in-azure-resource-explorer.png)

   - 单击“**放入**”

**从 GitHub 部署 Azure 组件**

1. 检查确保该构建正在传递 VSTS Build。

2. 分叉此存储库至你的 GitHub 账户。

3. 单击“部署到 Azure”按钮：

   [![部署到 Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2FO365-EDU-Python-Samples%2Fmaster%2Fazuredeploy.json)

4. 填写部署页面中的数值并选中“**我同意上述条款和条件**”复选框。

   ![](Images/azure-auto-deploy.png)

   - **资源组**：建议新建一个资源组。

   - **网站名称**：请输入名称。如 EDUGraphAPICanviz 或 EDUGraphAPI993。

     > 注意：如果输入的名称已被占用，将会出现一些验证错误：
     >
     > ![](Images/azure-auto-deploy-validation-errors-01.png)
     >
     > 单击将获得更多信息，如其他资源组/订阅中的存储账户。
     >
     > 在这种情况中，请使用其他名称。

   - **源代码存储库 URL**：使用分支的存储库名称替换 <YOUR REPOSITORY>。

   - **源代码手动集成**：**false**，因为正在从自有分支部署。

   - **Client Id**：使用之前创建的应用注册客户端 ID。

   - **Client Secret**：使用之前创建的应用密钥值。

   - 单击“**我同意上述条款和条件**”。

5. 单击“**购买**”。

**添加回复 URL 至应用程序注册**

1. 部署后，打开资源组：

   ![](Images/azure-resource-group.png)

2. 单击 Web 应用。

   ![](Images/azure-web-app.png)

   复制 URL 并更改架构为 **https**。这是重播 URL，将在下一步中使用。

3. 导航至新 Azure 门户中的应用程序注册，随后打开设置窗口。

   添加回复 URL：

   ![](Images/aad-add-reply-url.png)

   > 注意：如果要本地调试示例，确保回复 URL 是 http://127.0.0.1:8000/。

4. 单击“**保存**”。

## 了解代码

### 简介

**解决方案组件图**

![](Images/solution-component-diagram.png)

### **EDUGraphAPI**

**身份验证**

Django 附有用户身份验证系统。它处理用户账户、租户、权限和基于 cookie 的用户会话。有关详细信息，请参阅“[Django 身份验证系统](https://docs.djangoproject.com/en/1.10/topics/auth/)”。

本地用户身份验证基于内置 API：

* [验证](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.authenticate)
* [登录](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.login)
* [注销](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.logout)

O365 用户身份验证通过 Open ID Connect 实现。 

**数据访问**

在此示例中，[Django 的内置 ORM](https://docs.djangoproject.com/en/1.11/topics/db/) 用于从后端 SQLite 数据库存取数据。

下面是表格：

| 表格 | 说明 |
| ------------------------------ | ---------------------------------------- |
| auth\_user | Django 内置的用户表格含有用户的身份验证信息： 用户名、电子邮件、密码... |
| user\_roles | 包含用户角色。此示例中使用了三种角色：管理员、教师和学生 |
| 配置文件 | 包含管理员的附加信息：*favoriteColor*、*organization\_id*、*o365UserId*和*o365Email*。后两个用于将本地用户与 O365 用户连接 |
| 组织 | 此表格中的一行代表 AAD 中的租户。<br>*isAdminConsented* 列记录租户是否经过管理员同意。|
| token\_cache | 包含用户的访问/刷新令牌。|
| classroom\_seating\_arrangements | 包含教室座位排列数据。|

模型在 **/models/db.py** 中定义。

**视图**

下面是视图：

| 路径 | 说明 |
| -------------------- | ---------------------------------------- |
| /account/views.py | 包含用户注册、登录和注销的视图。 |
| /admin/views.py | 包含管理视图，如许可租户、管理关联账户。|
| /link/views.py | 包含用于关联用户账户的视图。|
| /management/views.py | 包含关于我页面的视图。|
| /schools/view.py | 包含教育视图，如学校、课程和课程详情。 |

**修饰器**

此应用程序中使用了下列修饰器：

| 修饰器 | 说明 |
| ----------------- | ---------------------------------------- |
| login\_required | 仅允许登录的用户访问保护资源。|
| admin\_only | 仅允许管理员访问保护资源。 |
| linked\_users\_only | 仅允许关联用户访问保护资源。|

**服务**

服务器端应用程序使用的服务：

| 服务 | 说明 |
| ---------------- | ---------------------------------------- |
| UserService | 包含操纵数据库中用户的方法。|
| LinkService | 包含用于关联用户账户的方法。|
| AuthService | 包含用于授权和身份验证的方法。 |
| AADGraphService | 包含访问 AAD Graph API 的方法。|
| MSGraphService | 包含访问 MS Graph API 的方法。|
| EducationService | 包含两种访问 Education REST API 的方法。|
| TokenService | 包含从数据库获取令牌并更新令牌的方法。|

服务位于 **/services** 文件夹。

**中间件**

刷新令牌不存在或过期时，**`HandleRefreshTokenExceptionMiddleware`** 捕获由 `TokenService` 引发的 `RefreshTokenException`。随后将用户重定向至重新登录的页面。

**多租户应用**

此 Web 应用程序是**多租户应用**。在 AAD 中，我们启用选项：

![](Images/app-is-multi-tenant.png)

任何 Azure Active Directory 租户中的用户可访问此应用程序。用户使用应用程序前，此程序使用的部分权限需要租户管理员同意。否则，用户将看到此错误：

![](Images/app-requires-admin-to-consent.png)

有关详细信息，参见“[使用 Azure AD & OpenID Connect 生成 多租户 SaaS Web 应用程序](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapp-multitenant-openidconnect/)”。

### Office 365 教育版 API

[Office 365 教育版 API](https://msdn.microsoft.com/office/office365/api/school-rest-operations) 从由 Microsoft School Data Sync 同步至云端的任何 Office 365 租户返回数据。API 提供有关学校、章节、教师、学生和名单信息。针对教育版租户，Schools REST API 为 Office 365 中的学校实体提供访问权限。

在此示例中，**Microsoft.Education** 类别库项目封装有 Office 365 Education API。 

**EducationServiceClient** 是库的核心类。它常用于轻松获取教育数据。

**获取学校**

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

**获取课程**

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
**获取用户**

```typescript
def get_members(self, object_id, top=12, nextlink=''):
    skiptoken = self._get_skip_token(nextlink)
    url = self.api_base_uri + 'administrativeUnits/%s/members?$top=%s%s' % (object_id, top, skiptoken)
    return self.rest_api_service.get_object_list(url, self.access_token, model=EduUser, next_key='odata.nextLink')
```
下面是显示教育数据的示例应用的截屏。

![](Images/edu-schools.png)

![](Images/edu-users.png)

![](Images/edu-classes.png)

![](Images/edu-class.png)

### 身份验证流

此项目中有 4 个身份验证流。

前 2 个流（本地登录/O365登录）能够让用户使用本地账户或 Office 365 账户登录，随后关联至其它类型账户。此程序在 LinkController 中实现。

**本地登录身份验证流**

![](Images/auth-flow-local-login.png)

**O365登录身份验证流**

![](Images/auth-flow-o365-login.png)

**管理员登录身份验证流**

该流显示管理员如何登录系统并执行管理操作。

使用 Office 365 账户登录后，系统提示要求管理员关联至本地账户。此步骤不需要，可以忽略。 

如前所述，Web 应用是使用部分应用权限的多租户应用，因此租户管理员必须首先对应用程序进行许可。  

该流在 AdminController 中实现。 

![](Images/auth-flow-admin-login.png)

### 两种类型的 Graph API

此示例中使用两种不同的 Graph API：

| | [Azure AD Graph API](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api) | [Microsoft Graph API](https://graph.microsoft.io/) |
| ------------ | ---------------------------------------- | ---------------------------------------- |
| 说明 |Azure Active Directory 图形 API 通过 REST API 终结点提供对 Azure Active Directory 的编程访问权限。Apps 可使用 Azure AD Graph API 在目录数据和目录对象上执行创建、读取、更新和删除 (CRUD) 操作，如用户、组合和组织联系人 | 所有包含 Microsoft 服务中 API 的统一 API，如 Outlook、OneDrive、OneNote、 Planner 和 Office Graph，使用单独的令牌通过单独的终结点进行访问。 |
| 客户端 | 安装包 [Microsoft.Azure.ActiveDirectory.GraphClient](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/) | 安装包 [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) |
| 终结点 | https://graph.windows.net | https://graph.microsoft.com |
| API 资源管理器 | https://graphexplorer.cloudapp.net/ | https://graph.microsoft.io/graph-explorer |

> **重要说明：**Microsoft 目前正在大力投资 Microsoft Graph API，不再投资 Azure AD Graph API （安全问题修复除外）。

> 因此，请尽可能多地使用新 Microsoft Graph API，并尽量减少使用 Azure AD Graph API。

下面的代码段介绍了如何从 Microsoft Graph API 获取组文档。

```typescript
def get_documents(self, object_id):
    url = self.api_base_uri + 'groups/%s/drive/root/children' % object_id
    return self.rest_api_service.get_object_list(url, self.access_token, model=Document)
```

请注意，在 AAD 应用程序设置中，各 Graph API 权限单独配置：

![](Images/aad-create-app-06.png) 

## 问题和意见

- 如果你在运行此示例时遇到任何问题，请[记录问题](https://github.com/OfficeDev/O365-EDU-Python-Samples/issues)。
- 与GraphAPI开发相关的问题一般应发布到 [Stack Overflow](http://stackoverflow.com/questions/tagged/office-addins)。请务必将提问或评论用 \[ms-graph-api] 标记。 

## 参与

我们鼓励你参与我们的示例。有关如何继续的指南，请参阅我们的[参与指南](/CONTRIBUTING.md)。

此项目已采用 [Microsoft 开放源代码行为准则](https://opensource.microsoft.com/codeofconduct/)。有关详细信息，请参阅[行为准则常见问题解答](https://opensource.microsoft.com/codeofconduct/faq/)。如有其他任何问题或意见，也可联系 [opencode@microsoft.com](mailto:opencode@microsoft.com)。



**版权所有 (c) 2017 Microsoft。保留所有权利。**
