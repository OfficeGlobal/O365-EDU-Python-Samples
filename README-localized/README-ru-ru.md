---
page_type: sample
products:
- office
- office-365
- ms-graph
languages:
- python
- javascript
description: "Интеграция с учебными ролями и данными списков, а также доступность служб Office 365 с помощью API Graph."
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

# EDUGraphAPI — пример кода Office 365 для образования (Python)

В этом примере показано, как выполнить интеграцию с учебными ролями или данными списков, а также службами Office 365, доступными с помощью API Graph. 

Данные учебного заведения синхронизируются в клиентах Office 365 для образования с помощью [синхронизации сведений о школе (Майкрософт)](http://sds.microsoft.com).  

**Содержание**

- [Примеры целей](#sample-goals)
- [Предварительные требования](#prerequisites)
- [Регистрация приложения в Azure Active Directory](#register-the-application-in-azure-active-directory)
- [Локальный запуск примера](#run-the-sample-locally)
- [Развертывание примера в Azure](#deploy-the-sample-to-azure)
- [Разбор кода](#understand-the-code)
- [Вопросы и комментарии](#questions-and-comments)
- [Участие](#contributing)

## Примеры целей

В примере показано:

- Вызов API Graph, включая:

  - [API Graph Microsoft Azure Active Directory](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)
  - [API Microsoft Graph](https://www.nuget.org/packages/Microsoft.Graph/)

- Связывание локально управляемых учетных записей пользователей и учетных записей Office 365 (Azure Active Directory). 

  После связывания учетных записей пользователи могут использовать локальные учетные записи или учетные записи Office 365 для входа в пример веб-сайта и его использования.

- Получение учебных заведений, разделов, преподавателей и учащихся из Office 365 для образования:

  - [Справочник по API REST учебных заведений для Office 365](https://msdn.microsoft.com/office/office365/api/school-rest-operations)

Этот пример реализован на языке Python и веб-платформу [Django](https://www.djangoproject.com/).

> [Django](https://www.djangoproject.com/) — это высокоуровневая веб-платформа Python, которая способствует быстрому и практичному проектированию. Платформа создана опытными разработчиками и значительно облегчает веб-разработку, чтобы вы могли сосредоточиться на написании своего приложения, не затрачивая лишнего времени. Это бесплатные открытое программное обеспечение.

## Предварительные требования

**Для развертывания и запуска этого примера требуется следующее**:

- Подписка Azure с разрешениями на регистрацию нового приложения и развертывание веб-приложения.

- Клиент Office 365 для образования с поддержкой Microsoft School Data Sync.

  - Один из следующих браузеров: Microsoft Edge, Internet Explorer 9, Safari 5.0.6, Firefox 5, Chrome 13 или более поздние версии этих браузеров.

  Кроме того: Для разработки и работы этого примера в локальной среде требуется следующее:  

  - [Python](https://www.python.org/downloads/) 3.5.2 или выше
  - [Django](https://www.djangoproject.com/download/) 1.11 или выше
  - [SQLite](https://www.sqlite.org/)



## Регистрация приложения в Azure Active Directory

1. Войдите на новый портал Azure по адресу [https://portal.azure.com/](https://portal.azure.com/).

2. Выберите клиент Azure AD, щелкнув имя своей учетной записи в правом верхнем углу страницы:

   ![](Images/aad-select-directory.png)

3. Щелкните **Azure Active Directory** -> **Регистрация приложений** -> **+Добавить**.

   ![](Images/aad-create-app-01.png)

4. Введите **Имя** и выберите **Веб-приложение или API** в качестве **типа приложения**.

   Укажите **URL-адрес входа**: http://127.0.0.1:8000/

   ![](Images/aad-create-app-02.png)

   Нажмите **Создать**.

5. После завершения приложение отобразится в списке.

   ![](/Images/aad-create-app-03.png)

6. Щелкните его, чтобы просмотреть сведения о нем. 

   ![](/Images/aad-create-app-04.png)

7. Щелкните **Все параметры**, если окно параметров не отобразилось.

   - Нажмите **Свойства** и присвойте параметру **Мультитенантный** значение **Да**.

     ![](/Images/aad-create-app-05.png)

     Скопируйте **Идентификатор приложения** и нажмите кнопку **Сохранить**.

   - Щелкните **Необходимые разрешения**. Добавьте указанные ниже разрешения.

     | API | Разрешения приложений | Делегированные разрешения |
| ------------------------------ | ---------------------------------------- | ---------------------------------------- |
| Microsoft Graph | Чтение данных каталога | Чтение полных профилей всех пользователей<br>Чтение данных каталога<br>Чтение данных каталога<br>Доступ к каталогу, аналогичный доступу вошедшего пользователя<br>Вход в |
| Windows Azure Active Directory |
| Вход в систему и чтение профиля пользователя<br>Чтение и запись данных каталога |

     ![](/Images/aad-create-app-06.png)

   - Щелкните пункт **Ключи** и добавьте новый ключ:

     ![](Images/aad-create-app-07.png)

     Нажмите **Сохранить**, затем скопируйте **ЗНАЧЕНИЕ** ключа. 

   Закройте окно параметров.

## Локальный запуск примера

Следующее программное обеспечение обязательно:

- [Python](https://www.python.org/downloads/) 3.5.2 или выше
- [Django](https://www.djangoproject.com/download/) 1.11 или выше
- [SQLite](https://www.sqlite.org/)

Запуск приложения:

1. Настройте следующие **переменные среды**:

   - **clientId**: используйте идентификатор клиента из регистрации приложения, созданный ранее.
   - **clientSecret**: используйте значение ключа из регистрации приложения, созданное ранее.
   - **SourceCodeRepositoryURL**: используйте URL-адрес репозитория.

2. Откройте терминал и перейдите к папке исходного кода. Выполните команду ниже.

   ```sh
   pip install -r requirements.txt
   python manage.py runserver
   ```

3. Откройте http://127.0.0.1:8000/ в браузере.

## Развертывание примера в Azure

**Авторизация GitHub**

1. Создание маркера

   - Откройте страницу https://github.com/settings/tokens в веб-браузере.
   - Войдите в свою учетную запись GitHub, где находится разветвление этого репозитория.
   - Нажмите кнопку **Создать маркер**
   - Введите значение в текстовое поле **Описание маркера**
   - Выберите следующее (выбранные параметры должны соответствовать снимку экрана ниже):
     - repo (all) -> repo:status, repo\_deployment, public\_repo
     - admin:repo\_hook -> read:repo\_hook

   ![](Images/github-new-personal-access-token.png)

   - Нажмите кнопку **Создать маркер**
   - Скопируйте маркер

2. Добавление маркера GitHub в Azure в обозревателе ресурсов Azure

   - Откройте страницу https://resources.azure.com/providers/Microsoft.Web/sourcecontrols/GitHub в веб-браузере.
   - Войдите с помощью учетной записи Azure.
   - Выберите правильную подписку Azure.
   - Выберите режим **Чтение и запись**.
   - Нажмите кнопку **Изменить**.
   - Вставьте маркер в **параметр маркера**.

   ![](Images/update-github-token-in-azure-resource-explorer.png)

   - Щелкните **PUT**

**Развертывание компонентов Azure из GitHub**

1. Убедитесь, что сборка передает сборку VSTS.

2. Создайте разветвление этого репозитория в свою учетную запись GitHub.

3. Нажмите кнопку "Развернуть в Azure":

   [![Развернуть в Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2FO365-EDU-Python-Samples%2Fmaster%2Fazuredeploy.json)

4. Заполните значения на странице развертывания и установите флажок **Я принимаю условия, указанные выше**.

   ![](Images/azure-auto-deploy.png)

   - **Группа ресурсов**: рекомендуется создать новую группу.

   - **Имя сайта**: укажите имя. Например: EDUGraphAPICanviz или EDUGraphAPI993.

     > Примечание. Если указанное имя занято, возникнут ошибки проверки:
     >
     > ![](Images/azure-auto-deploy-validation-errors-01.png)
     >
     > Щелкнув по этому сообщению, вы получите дополнительные сведения, например, что учетная запись хранения уже используется в другой группе ресурсов или подписке.
     >
     > В таком случае используйте другое имя.

   - **URL-адрес репозитория исходного кода**: замените <YOUR REPOSITORY> именем репозитория вашего разветвления.

   - **Ручная интеграция исходного кода**: выберите **false**, так как вы развертываете из собственного разветвления.

   - **Идентификатор клиента**: используйте идентификатор клиента из регистрации приложения, созданный ранее.

   - **Секрет клиента**: используйте значение ключа из регистрации приложения, созданное ранее.

   - Установите флажок **Я принимаю указанные выше условия**.

5. Нажмите **Купить**.

**Добавление URL-адреса ответа в регистрацию приложения**

1. После развертывания откройте группу ресурсов:

   ![](Images/azure-resource-group.png)

2. Щелкните веб-приложение.

   ![](Images/azure-web-app.png)

   Скопируйте URL-адрес и измените схему на **https**. Это URL-адрес ответа, который будет использоваться в следующем действии.

3. Перейдите к регистрации приложения на новом портале Azure и откройте окно параметров.

   Добавьте URL-адрес ответа:

   ![](Images/aad-add-reply-url.png)

   > Примечание. Чтобы выполнить отладку примера локально, укажите https://localhost:127.0.0.1:8000/ в URL-адресах ответа.

4. Нажмите кнопку **Сохранить**.

## Разбор кода

### Введение

**Схема компонентов решения**

![](Images/solution-component-diagram.png)

### **EDUGraphAPI**

**Проверка подлинности**

Django поставляется с системой проверки подлинности пользователя. Она обрабатывает учетные записи пользователей, группы, разрешения и пользовательские сеансы на основе файлов cookie. Дополнительные сведения см. в статье [Система проверки подлинности Django](https://docs.djangoproject.com/en/1.10/topics/auth/)

Проверка подлинности локальных пользователей основана на встроенном API:

* [проверка подлинности](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.authenticate)
* [вход](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.login)
* [выход](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.logout)

Проверка подлинности пользователей Office 365 реализована с помощью подключения с Open ID Connect. 

**Доступ к данным**

В этом примере [встроенный ORM Django](https://docs.djangoproject.com/en/1.11/topics/db/), используется для доступа к данным во внутренней базе данных SQLite.

Ниже перечислены таблицы:

| Таблица | Описание |
| ------------------------------ | ---------------------------------------- |
| auth\_user | Встроенная пользовательская таблица Django, содержащая сведения о проверке подлинности пользователей: имя пользователя, электронная почта, пароль... |
| user\_roles | Содержит роли пользователей. В этом примере используются три роли: "Администратор", "преподаватель" и "ученик". |
| профили | Содержит дополнительные сведения о пользователях: *favoriteColor*, *organization\_id*, *o365UserId* и *o365Email*. Две последние версии используются для подключения локального пользователя к пользователю Office 365\. |
| Организации | Строка в этой таблице представляет клиента в AAD.<br>Столбец *IsAdminConsented* записывает, если клиент разрешен администратором. |
| token\_cache | Содержит маркеры пользователя "доступ" или "обновление". |
| classroom\_seating\_arrangements | Содержит учебные схемы рассадки. |

Модели указаны в **/models/db.py**.

**Представления**

Ниже перечислены представления:

| Путь | Описание |
| -------------------- | ---------------------------------------- |
| /account/views.py Содержит представления для регистрации пользователей, входа и выхода из системы. |
| /admin/views.py | Содержит административные представления, такие как клиент согласия, управляет связанными учетными записями. |
| /link/views.py | Содержит представления, используемые для связанных учетных записей. |
| /management/views.py | Содержит представления страницы "Обо мне". |
| /schools/view.py | Содержит представления для образовательных учреждений, такие как учебные заведения, классы и сведения о занятиях. |

**Декораторы**

В приложении использованы следующие декораторы:

| Декоратор | Описание |
| ----------------- | ---------------------------------------- |
| login\_required | Только пользователь, вошедший в учетную запись, сможет получить доступ к защищенным ресурсам. |
| admin\_only | Только администраторы могут получить доступ к защищенным ресурсам. |
| linked\_users\_only | Только связанные пользователи могут получить доступ к защищенным ресурсам. |

**Службы**

Службы, используемые серверным приложением:

| Служба | Описание |
| ---------------- | ---------------------------------------- |
| UserService | Содержит методы, используемые для управления пользователями в базе данных. |
| LinkService | Содержит методы, используемые для связи учетных записей пользователей. |
| AuthService | Содержит методы, используемые для авторизации и проверки подлинности. |
| AADGraphService | Содержит методы, используемые для доступа к API AAD Graph. |
| MSGraphService | Содержит методы, используемые для доступа к API MS Graph. |
| EducationService | Содержит два способа доступа к REST API для образования. |
| TokenService | Содержит методы, используемые для извлечения и обновления маркеров базы данных |

Службы расположены в папке **/services**.

**ПО промежуточного слоя**

**`HandleRefreshTokenExceptionMiddleware`** перехватывает`RefreshTokenException`, созданный `TokenService` когда маркера обновления не существует или его срок действия истекать. После, пользователь перенаправляется на страницу для повторно подключения к ней.

**Мультитенантное приложение**

Это **мультитенантное** веб-приложение. В AAD включен параметр:

![](Images/app-is-multi-tenant.png)

Доступ к этому приложению могут получить пользователи из любого клиента Azure Active Directory. Некоторые разрешения, используемые приложением, требуют разрешения администратора клиента до того, как пользователи смогут использовать это приложение. В противном случае, пользователи увидят ошибку:

![](Images/app-requires-admin-to-consent.png)

Дополнительные сведения см. в статье [Создание мультитенантного веб-приложения SaaS с помощью Azure AD и OpenID Connect](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapp-multitenant-openidconnect/).

### API Office 365 для образования

[API Office 365 для образования](https://msdn.microsoft.com/office/office365/api/school-rest-operations) возвращают данные из клиента Office 365, синхронизированного с облаком при помощи службы Microsoft School Data Sync. Эти API содержат сведения об учебных заведениях, разделах, преподавателях, учащихся и списках. API REST учебных заведений предоставляет доступ к объектам учебных заведений в клиентах Office 365 для образования.

В этом примере проект библиотеки классов **Microsoft.Education** объединяет API Office 365 для образования. 

**EducationServiceClient** — это основной класс библиотеки. Используется, чтобы легко получать данные образовательных учреждений.

**Получение учебных заведений**

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

**Получение курсов**

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
**Получение списка пользователей**

```typescript
def get_members(self, object_id, top=12, nextlink=''):
    skiptoken = self._get_skip_token(nextlink)
    url = self.api_base_uri + 'administrativeUnits/%s/members?$top=%s%s' % (object_id, top, skiptoken)
    return self.rest_api_service.get_object_list(url, self.access_token, model=EduUser, next_key='odata.nextLink')
```
Ниже представлены некоторые снимки экрана с примером приложения, демонстрирующие образовательные данные.

![](Images/edu-schools.png)

![](Images/edu-users.png)

![](Images/edu-classes.png)

![](Images/edu-class.png)

### Потоки проверки подлинности

В этом проекте существует 4 потока проверки подлинности.

Первые 2 потока (локальный вход/вход в O365) позволяют пользователям входить в систему с помощью локальной учетной записи или учетной записи Office 365, а затем привязывать учетную запись другого типа. Эта процедура реализована в LinkController.

**Поток проверки подлинности локального входа**

![](Images/auth-flow-local-login.png)

**Поток проверки подлинности входа в O365**

![](Images/auth-flow-o365-login.png)

**Поток проверки подлинности входа администратора**

В этом потоке показано, как администратор входит в систему и выполняет административные операции.

После входа в приложение с помощью учетной записи Office 365 администратору будет предложено подключиться к локальной учетной записи. Это необязательный шаг, который можно пропустить. 

Как указано выше, веб-приложение является мультитенантным и использует некоторые разрешения приложений, поэтому администратор клиента должен сначала согласиться на использование клиента.  

Этот поток реализован в AdminController. 

![](Images/auth-flow-admin-login.png)

### Два вида API Graph

В этом примере используются два отдельных API Graph:

| | [API Graph Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api) | [API Microsoft Graph](https://graph.microsoft.io/) |
| ------------ | ---------------------------------------- | ---------------------------------------- |
| Описание | API Graph Azure Active Directory обеспечивает программный доступ к Azure Active Directory через конечные точки API REST. Приложения могут использовать API Graph Azure AD для выполнения операций создания, чтения, обновления и удаления (CRUD) с данными и объектами каталогов, таких как пользователи, группы и контакты организации | Единый API, также включающий API из других служб Майкрософт, таких как Outlook, OneDrive, OneNote, Планировщик и Office Graph, доступен через одну конечную точку с помощью одного маркера доступа. |
| Клиент | Пакет установки [Microsoft.Azure.ActiveDirectory.GraphClient](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/) | Пакет установки [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) |
| Конечная точка | https://graph.windows.net | https://graph.microsoft.com |
| Обозреватель API | https://graphexplorer.cloudapp.net/| https://graph.microsoft.io/graph-explorer |

> **ВАЖНОЕ ПРИМЕЧАНИЕ.** Корпорация Майкрософт работает над новым API Microsoft Graph, но больше не занимается разработкой API Graph для Azure AD (за исключением проблем, связанных с безопасностью).

> Таким образом, по возможности используйте новый API Microsoft Graph, и сократите использование API Graph для Azure AD.

В примере далее показано, как получить документы группы из API Microsoft Graph.

```typescript
def get_documents(self, object_id):
    url = self.api_base_uri + 'groups/%s/drive/root/children' % object_id
    return self.rest_api_service.get_object_list(url, self.access_token, model=Document)
```

Обратите внимание, что в параметрах приложения AAD разрешения для каждого API Graph настраиваются отдельно:

![](Images/aad-create-app-06.png) 

## Вопросы и комментарии

- Если у вас возникли проблемы с запуском этого примера, [сообщите о неполадке](https://github.com/OfficeDev/O365-EDU-Python-Samples/issues).
- Общие вопросы о разработке GraphAPI следует задавать на сайте [Stack Overflow](http://stackoverflow.com/questions/tagged/office-addins). Добавляйте тег \[ms-graph-api] к своим вопросам или комментариям. 

## Участие

Мы приветствуем ваше участие в создании примеров. Сведения о дальнейших действиях см. в [руководстве по участию](/CONTRIBUTING.md).

Этот проект соответствует [Правилам поведения разработчиков открытого кода Майкрософт](https://opensource.microsoft.com/codeofconduct/). Дополнительные сведения см. в разделе [часто задаваемых вопросов о правилах поведения](https://opensource.microsoft.com/codeofconduct/faq/). Если у вас возникли вопросы или замечания, напишите нам по адресу [opencode@microsoft.com](mailto:opencode@microsoft.com).



**(c) Корпорация Майкрософт (Microsoft Corporation), 2017. Все права защищены.**
