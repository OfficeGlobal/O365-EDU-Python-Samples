---
page_type: sample
products:
- office
- office-365
- ms-graph
languages:
- python
- javascript
description: "Graph API を介して利用可能な学校での役割/生徒名簿のデータおよび O365 サービスを統合する方法。"
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

# EDUGraphAPI - Office 365 Education のコード サンプル (Python)

このサンプルでは、Microsoft Graph API 経由で学校での役割/生徒名簿のデータに加え、利用できる O365 サービスと統合する方法を示します。 

学校のデータは、[Microsoft School Data Sync](http://sds.microsoft.com) によって O365 Education テナントで同期されます。  

**目次**

- [サンプルの目標](#sample-goals)
- [前提条件](#prerequisites)
- [Azure Active Directory にアプリケーションを登録する](#register-the-application-in-azure-active-directory)
- [ローカルでサンプル アプリを実行する](#run-the-sample-locally)
- [Azure にサンプルを展開する](#deploy-the-sample-to-azure)
- [コードを理解する](#understand-the-code)
- [質問とコメント](#questions-and-comments)
- [投稿](#contributing)

## サンプルの目標

このサンプルでは、以下を例示します。

- 以下を含む Graph API の呼び出し:

  - [Microsoft Azure Active Directory Graph API](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)
  - [Microsoft Graph API](https://www.nuget.org/packages/Microsoft.Graph/)

- ローカル管理のユーザー アカウントと Office 365 (Azure Active Directory) ユーザー アカウントのリンク。 

  アカウントをリンクした後、ユーザーはローカルまたは Office 365 アカウントを使用してサンプル Web サイトにログインし、それを使用できます。

- Office 365 Education から学校、セクション、教師、学生を取得する:

  - [Office 365 教育機関 REST API リファレンス](https://msdn.microsoft.com/office/office365/api/school-rest-operations)

このサンプルは、Python 言語と [Django](https://www.djangoproject.com/) Web フレームワークで実装されています。

> [Django](https://www.djangoproject.com/)は、迅速な開発とクリーンで実用的な設計を促進する高度な Python Web フレームワークです。経験豊富な開発者によって構築され、Web 開発における面倒事の多くを処理します。そのため、車輪を再発明することなくアプリの作成に集中できます。無料でオープンソースです。

## 前提条件

**このサンプルを展開して実行するには、次のものが必要です**:

- 新しいアプリケーションを登録し、Web アプリを展開する権限を持つ Azure サブスクリプション。

- Microsoft School Data Sync が有効になっている O365 Education テナント

  - 次のブラウザのいずれか:Microsoft Edge、Internet Explorer 9、Safari 5.0.6、Firefox 5、Chrome 13、これらのブラウザーのいずれかの最新バージョン。

  さらに:このサンプルをローカルで開発/実行するには、次のものが必要です:  

  - [Python](https://www.python.org/downloads/) 3.5.2 以降
  - [Django](https://www.djangoproject.com/download/) 1.11 以降
  - [SQLite](https://www.sqlite.org/)



## Azure Active Directory にアプリケーションを登録する

1. Azure ポータルにサインインします: [https://portal.azure.com/](https://portal.azure.com/)。

2. ページの右上隅のアカウント名を選択して、Azure AD テナントを選択します:

   ![](Images/aad-select-directory.png)

3. [**Azure Active Directory**]、[**アプリの登録**]、[**+追加**] の順にクリックします。

   ![](Images/aad-create-app-01.png)

4. **名前** を入力し、**アプリケーション タイプ**として [**Web App / API**] (Web アプリ/API) を選択します。

   **サインオン URL** を入力します: http://127.0.0.1:8000/

   ![](Images/aad-create-app-02.png)

   [**作成**] をクリックします。

5. 完了すると、アプリが一覧に表示されます。

   ![](/Images/aad-create-app-03.png)

6. クリックして詳細を表示します。 

   ![](/Images/aad-create-app-04.png)

7. [設定] ウィンドウが表示されない場合は、[**すべての設定**] をクリックします。

   - [**プロパティ**] をクリックし、[**マルチテナント**] を**はい**に設定します。

     ![](/Images/aad-create-app-05.png)

     **アプリケーション ID** を任意の場所にコピーして、[**保存**] をクリックします。

   - [**必要なアクセス許可**] をクリックします。以下のアクセス許可を追加します。

     | API | アプリケーション アクセス許可 | 委任されたアクセス許可 |
| ------------------------------ | ----------------------- | ---------------------------------------- |
| Microsoft Graph | ディレクトリ データの読み取り | すべてのユーザーの完全なプロファイルの読み取り<br>ディレクトリ データの読み取り<br>ディレクトリ データの読み取り<br>サインインしたユーザーとしてディレクトリにアクセス<br>ユーザーをサインインする |
| Windows Azure Active Directory |
| サインインおよびユーザー プロファイルの読み取り<br>ディレクトリ データの読み取りおよび書き込み |

     ![](/Images/aad-create-app-06.png)

   - [**キー**] をクリックして、新しいキーを追加します。

     ![](Images/aad-create-app-07.png)

     [**保存**] をクリックしてから、キーの**値**を任意の場所にコピーします。 

   [設定] ウィンドウを閉じます。

## ローカルでサンプル アプリを実行する

次のソフトウェアが必要です。

- [Python](https://www.python.org/downloads/) 3.5.2 以降
- [Django](https://www.djangoproject.com/download/) 1.11 以降
- [SQLite](https://www.sqlite.org/)

アプリを実行します:

1. 以下の**環境変数**を構成します。

   - **clientId**: 前に作成したアプリ登録のクライアント ID を使用します。
   - **clientSecret**: 前に作成したアプリ登録のキーの値を使用します。
   - **SourceCodeRepositoryURL**: このリポジトリの URL を使用します。

2. [ターミナル] を開いて、ソース コード フォルダーに移動します。次のコマンドを実行します。

   ```sh
   pip install -r requirements.txt
   python manage.py runserver
   ```

3. ブラウザーで http://127.0.0.1:8000/ を開きます。

## Azure にサンプルを展開する

**GitHub 認証**

1. トークンを生成する

   - Web ブラウザーで https://github.com/settings/tokens を開きます。
   - このリポジトリをフォークした GitHub アカウントにサインインします。
   - [**トークンの生成**] をクリックします。
   - [**トークンの説明**] テキスト ボックスに値を入力します
   - 以下を選択します (選択内容は下のスクリーンショットと一致する必要があります):
     - repo (all) -> repo:status, repo_deployment, public_repo
     - admin:repo_hook -> read:repo_hook

   ![](Images/github-new-personal-access-token.png)

   - [**トークンの生成**] をクリックします
   - トークンをコピーする

2. Azure リソース エクスプローラーで GitHub トークンを Azure に追加する

   - Web ブラウザーで https://resources.azure.com/providers/Microsoft.Web/sourcecontrols/GitHub を開きます。
   - Azure アカウントでログインします。
   - 正しい Azure サブスクリプションを選択しました。
   - [**読み取り/書き込み**] モードを選択します。
   - [**編集**] をクリックします。
   - トークンを**トークン パラメーター**に貼り付けます。

   ![](Images/update-github-token-in-azure-resource-explorer.png)

   - [**PUT**] をクリックします

**GitHub から Azure コンポーネントを展開する**

1. ビルドが VSTS ビルドを通過していることを確認してください。

2. このリポジトリを GitHub アカウントにフォークします。

3. [Azure への展開] ボタンをクリックします:

   [![Azure への展開](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2FO365-EDU-Python-Samples%2Fmaster%2Fazuredeploy.json)

4. 展開ページで値を入力し、[**上記の使用条件に同意する**] チェック ボックスを選択します。

   ![](Images/azure-auto-deploy.png)

   - **リソース グループ**: 新しいグループを作成することをお勧めします。

   - **サイト名**: 名前を入力してください。EDUGraphAPICanviz や EDUGraphAPI993 などです。

     > 注:入力した名前がすでに使用されている場合、いくつかの検証エラーが発生します。
     >
     > ![](Images/azure-auto-deploy-validation-errors-01.png)
     >
     > クリックすると、ストレージ アカウントが既に他のリソース グループ/サブスクリプションにあるなどの詳細情報が表示されます。
     >
     > この場合は、別の名前を使用してください。

   - **ソース コード リポジトリの URL**: <YOUR REPOSITORY>をフォークのリポジトリ名に置き換えます。

   - **ソース コードの手動統合**: 独自のフォークからデプロイしているため、**false** を選択します。

   - **クライアント ID**: 前に作成したアプリ登録のクライアント ID を使用します。

   - **クライアント シークレット**: 前に作成したアプリ登録のキーの値を使用します。

   - [**上記の使用条件に同意する**] をチェックします。

5. [**購入**] をクリックします。

**応答 URL をアプリの登録に追加する**

1. 展開が完了したら、リソース グループを開きます。

   ![](Images/azure-resource-group.png)

2. Web アプリをクリックします。

   ![](Images/azure-web-app.png)

   URL を任意の場所にコピーして、スキーマを **https** に変更します。これは応答 URL であり、次のステップで使用されます。

3. 新しい Azure ポータルでアプリの登録に移動し、[設定] ウィンドウを開きます。

   応答 URL を追加します。

   ![](Images/aad-add-reply-url.png)

   > 注: サンプルをローカルでデバッグするには、http://127.0.0.1:8000/ が応答 URL にあることを確認してください。

4. [**保存**] をクリックします。

## コードを理解する

### 概要

**ソリューション コンポーネント図**

![](Images/solution-component-diagram.png)

### **EDUGraphAPI**

**認証**

Django にはユーザー認証システムが付属しています。ユーザー アカウント、グループ、アクセス許可、Cookie ベースのユーザー セッションを処理します。詳細については、「[Django authentication system (Django 認証システム)](https://docs.djangoproject.com/en/1.10/topics/auth/)」を参照してください。

ローカルユーザーの認証は、組み込みの API に基づいています。

* [認証](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.authenticate)
* [ログイン](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.login)
* [ログアウト](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.logout)

O365 ユーザーの認証は Open ID Connect で実装されています。 

**データ アクセス**

このサンプルでは、[Django の組み込み ORM](https://docs.djangoproject.com/en/1.11/topics/db/) を使用して、バックエンド SQLite データベースのデータにアクセスします。

テーブルは次のとおりです:

| テーブル | 説明 |
| ------------------------------ | ---------------------------------------- |
| auth_user | ユーザーの認証情報を含む Django 組み込みユーザー テーブル: ユーザー名、メール、パスワード... |
| user_roles | ユーザーの役割が含まれています。このサンプルでは、管理者、教師、および 学生 の 3 つの役割が使用されています。|
| プロファイル | ユーザーの追加情報が含まれています: *favoriteColor*、*organization_id*、*o365UserId*、および *o365Email*。後者の 2 つは、ローカル ユーザーを O365 ユーザーに接続するために使用されます。|
| 組織 | このテーブルの行は、AAD のテナントを表しています。<br>*isAdminConsented* 列は、テナントが管理者によって同意されたかどうかを記録します。|
| token_cache | ユーザーのアクセス/更新トークンが含まれています。|
| Classroom_seating_arrangements | 教室の座席配置データが含まれています。|

モデルは **/models/db.py** で定義されています。

**ビュー**

ビューは次のとおりです:

| パス | 説明 |
| -------------------- | ---------------------------------------- |
| /account/views.py | ユーザーが登録、ログイン、ログアウトするためのビューが含まれています。|
| /admin/views.py | テナントの同意、リンクされたアカウントの管理などの管理ビューが含まれています。|
| /link/views.py | ユーザー アカウントのリンクに使用されるビューが含まれています。|
| /management/views.py | プロファイル ページのビューが含まれています。|
| /schools/view.py | 学校、クラス、クラスの詳細などの教育ビューが含まれています。|

**デコレータ**

このアプリでは、以下のデコレータが使用されています:

| デコレータ | 説明 |
| ----------------- | ---------------------------------------- |
| login_required | ログインしたユーザーのみが、保護されたリソースにアクセスできるようにします。|
| admin_only | 管理者のみが、保護されたリソースにアクセスできるようにします。|
| linked_users_only | リンクされたユーザーのみが、保護されたリソースにアクセスできるようにします。|

**サービス**

サーバー側アプリで使用されているサービス:

| サービス | 説明 |
| ---------------- | ---------------------------------------- |
| UserService | データベース内のユーザーを操作するために使用されるメソッドが含まれています。|
| LinkService | ユーザー アカウントのリンクに使用されるメソッドが含まれます。|
| AuthService | 承認と認証に使用されるメソッドが含まれています。|
| AADGraphService | AAD Graph API へのアクセスに使用されるメソッドが含まれています。|
| MSGraphService | MS Graph API にアクセスするために使用されるメソッドが含まれています。|
| EducationService | Education REST API にアクセスするための 2 つのメソッドが含まれています。|
| TokenService | データベースからトークンを取得および更新するために使用されるメソッドが含まれています |

サービスは **/services** フォルダーにあります。

**ミドルウェア**

**`HandleRefreshTokenExceptionMiddleware`** は、更新トークンが存在しないか期限切れの場合に、`TokenService` によってスローされた `RefreshTokenException` をキャッチします。そして、ユーザーをページにリダイレクトして、ユーザーが再ログインできるようにします。

**マルチテナント アプリ**

この Web アプリケーションは、**マルチテナント アプリ**です。AAD では、次のオプションを有効にしました:

![](Images/app-is-multi-tenant.png)

すべての Azure Active Directory テナントのユーザーはこのアプリにアクセスできます。このアプリで使用される一部のアクセス許可では、ユーザーがアプリを使用する前にテナントの管理者が同意する必要があります。そうしないと、ユーザーには次のエラーが表示されます。

![](Images/app-requires-admin-to-consent.png)

詳細については、「[Build a multi-tenant SaaS web application using Azure AD & OpenID Connect (Azure AD と OpenID Connect を使用してマルチテナント SaaS Web アプリケーションを構築する)](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapp-multitenant-openidconnect/)」を参照してください。

### Office 365 Education API

[Office 365 Education API](https://msdn.microsoft.com/office/office365/api/school-rest-operations) は、Microsoft School Data Sync でクラウドに同期されている Office 365 テナントからデータを返します。API は、教育機関、セクション、教師、学生および名簿に関する情報を提供します。教育機関 REST API は、Education テナント用の Office 365 に含まれる教育機関エンティティへのアクセスを提供します。

このサンプルでは、**Microsoft.Education** クラス ライブラリ プロジェクトは Office 365 Education API をカプセル化します。 

**EducationServiceClient** は、ライブラリのコア クラスです。教育データを簡単に取得するために使用されます。

**教育機関の取得**

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

**クラスを取得する**

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
**ユーザーを取得する**

```typescript
def get_members(self, object_id, top=12, nextlink=''):
    skiptoken = self._get_skip_token(nextlink)
    url = self.api_base_uri + 'administrativeUnits/%s/members?$top=%s%s' % (object_id, top, skiptoken)
    return self.rest_api_service.get_object_list(url, self.access_token, model=EduUser, next_key='odata.nextLink')
```
以下は、教育データを表示するサンプル アプリのスクリーンショットです。

![](Images/edu-schools.png)

![](Images/edu-users.png)

![](Images/edu-classes.png)

![](Images/edu-class.png)

### 認証フロー

このプロジェクトには 4 つの認証フローがあります。

最初の 2 つのフロー (ローカルログイン/O365 ログイン) では、ユーザーはローカル アカウントまたは Office 365 アカウントのいずれかでログインし、他の種類のアカウントにリンクすることができます。この手順は、LinkController に実装されています。

**ローカル ログイン認証フロー**

![](Images/auth-flow-local-login.png)

**O365 ログイン認証フロー**

![](Images/auth-flow-o365-login.png)

**管理者ログイン認証フロー**

このフローは、管理者がシステムにログインして管理操作を実行する方法を示しています。

Office 365 アカウントでアプリにログインした後、管理者はローカル アカウントにリンクするよう求められます。この手順は必須ではなく、スキップできます。 

前述のように、Web アプリはいくつかのアプリケーション アクセス許可を使用するマルチテナント アプリであるため、テナント管理者は最初にアプリに同意する必要があります。  

このフローは、AdminController に実装されています。 

![](Images/auth-flow-admin-login.png)

### 2 種類の Graph API

このサンプルでは、2 つの異なる Graph API が使用されています。

| | [Azure AD Graph API](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api) | [Microsoft Graph API](https://graph.microsoft.io/) |
| ------------ | ---------------------------------------- | ---------------------------------------- |
| 説明 | Azure Active Directory Graph API を使用すると、REST API エンドポイントを介して Azure Active Directory にプログラムによってアクセスできます。アプリは、Azure AD Graph API を使用して、ユーザー、グループ、組織の連絡先などのディレクトリ データとディレクトリ オブジェクトの作成、読み取り、更新、削除 (CRUD) 操作を実行できます。| Outlook、OneDrive、OneNote、Planner、Office Graph などの他の Microsoft サービスからの API も含まれる統合 APIです。すべてが単一のアクセス トークンを持つ単一のエンドポイントからアクセスされます。|
| クライアント | インストールパッケージ [Microsoft.Azure.ActiveDirectory.GraphClient](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/) | インストールパッケージ [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) |
| エンドポイント | https://graph.windows.net | https://graph.microsoft.com |
| API エクスプローラー | https://graphexplorer.cloudapp.net/ | https://graph.microsoft.io/graph-explorer |

> **重要なメモ**Microsoft は新しい Microsoft Graph API に多額の投資を行っており、Azure AD Graph API にはもう投資していません (セキュリティ問題の修正を除く)。

> したがって、可能な限り新しい Microsoft Graph API を使用し、Azure AD Graph API の使用は最小限に抑えてください。

以下は、Microsoft Graph API からグループ ドキュメントを取得する方法を示すコードです。

```typescript
def get_documents(self, object_id):
    url = self.api_base_uri + 'groups/%s/drive/root/children' % object_id
    return self.rest_api_service.get_object_list(url, self.access_token, model=Document)
```

AAD アプリケーション設定では、各 Graph API のアクセス許可が個別に構成されていることに注意してください:

![](Images/aad-create-app-06.png) 

## 質問とコメント

- このサンプルの実行について問題がある場合は、[問題をログに記録](https://github.com/OfficeDev/O365-EDU-Python-Samples/issues)してください。
- Graph API 開発全般の質問につきましては、「[Stack Overflow](http://stackoverflow.com/questions/tagged/office-addins)」に投稿してください。ご質問またはコメントに [ms-graph-api] タグが付けられていることを確認してください。 

## 投稿

当社のサンプルに是非貢献してください。投稿方法のガイドラインについては、[投稿ガイド](/CONTRIBUTING.md)を参照してください。

このプロジェクトでは、[Microsoft Open Source Code of Conduct (Microsoft オープン ソース倫理規定)](https://opensource.microsoft.com/codeofconduct/) が採用されています。詳細については、「[Code of Conduct FAQ (倫理規定の FAQ)](https://opensource.microsoft.com/codeofconduct/faq/)」を参照してください。また、その他の質問やコメントがあれば、[opencode@microsoft.com](mailto:opencode@microsoft.com) までお問い合わせください。



**Copyright (c) 2017 Microsoft.All rights reserved.**
