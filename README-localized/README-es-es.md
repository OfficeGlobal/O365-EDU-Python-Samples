---
page_type: sample
products:
- office
- office-365
- ms-graph
languages:
- python
- javascript
description: "Cómo integrar con los datos de la lista de participantes y de los roles educativos como así también con los servicios de O365 disponibles a través de API Graph."
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

# EDUGraphAPI: Código de ejemplo de Office 365 Educación (Python)

En este ejemplo, le mostramos cómo integrar con datos de lista o roles del centro educativo, así como servicios de Office 365 disponibles mediante la API de Graph. 

[Microsoft School Data Sync](http://sds.microsoft.com) mantiene sincronizados los datos académicos en los inquilinos de Office 365 Educación.  

**Tabla de contenido**

- [Objetivos del ejemplo](#sample-goals)
- [Requisitos previos](#prerequisites)
- [Registrar la aplicación en Azure Active Directory](#register-the-application-in-azure-active-directory)
- [Ejecutar el ejemplo de forma local](#run-the-sample-locally)
- [Implementar el ejemplo en Azure](#deploy-the-sample-to-azure)
- [Entender el código](#understand-the-code)
- [Preguntas y comentarios](#questions-and-comments)
- [Colaboradores](#contributing)

## Objetivos del ejemplo

En el ejemplo se muestra:

- Cómo llamar a las API de Graph, incluyendo:

  - [API de Microsoft Azure Active Directory Graph](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)
  - [API de Microsoft Graph](https://www.nuget.org/packages/Microsoft.Graph/)

- Cómo vincular las cuentas de usuario administradas localmente y las cuentas de usuario de Office 365 (Azure Active Directory). 

  Tras vincular las cuentas, los usuarios pueden usar cuentas locales o cuentas de Office 365 para iniciar sesión en el sitio web de ejemplo y usarlo.

- Cómo obtener centros educativos, secciones, profesores y estudiantes de Office 365 Educación:

  - [Referencia de la API de REST en centros educativos de Office 365](https://msdn.microsoft.com/office/office365/api/school-rest-operations)

Este ejemplo se implementa con lenguaje Python y el marco web de [Django](https://www.djangoproject.com/).

> [Django](https://www.djangoproject.com/) es un marco web de alto nivel de Python que fomenta un rápido desarrollo y un diseño pragmático simple. Compilado por desarrolladores experimentados, se encarga de la mayor parte de las complicaciones del desarrollo web, para que pueda centrarse en escribir la aplicación sin tener que reinventar la rueda. Es gratuita y de código abierto.

## Requisitos previos

**Implementar y ejecutar este ejemplo requiere lo siguiente**:

- Una suscripción de Azure con permisos para registrar una nueva aplicación y para implementar la aplicación web.

- Un inquilino de O365 Educación con Microsoft School Data Sync habilitado.

  - Uno de los siguientes exploradores: Microsoft Edge, Internet Explorer 9, Safari 5.0.6, Firefox 5, Chrome 13 o una versión posterior de alguno de estos exploradores.

  Además: Para desarrollar y ejecutar este ejemplo de manera local, se necesita lo siguiente:  

  - [Python](https://www.python.org/downloads/) versión 3.5.2 o superior
  - [Django](https://www.djangoproject.com/download/) versión 1.11 o superior
  - [SQLite](https://www.sqlite.org/)



## Registrar la aplicación en Azure Active Directory

1. Inicie sesión en el nuevo Microsoft Azure Portal: [https://portal.azure.com/](https://portal.azure.com/).

2. Para elegir el inquilino de Azure AD, seleccione su cuenta en la esquina superior derecha de la página:

   ![](Images/aad-select-directory.png)

3. Haga clic en **Azure Active Directory** -> **Registros de aplicaciones** -> **+Agregar**.

   ![](Images/aad-create-app-01.png)

4. Escriba un **Nombre** y seleccione **Aplicación web o API** como **Tipo de aplicación**.

   Introduzca la **URL de inicio de sesión**: http://127.0.0.1:8000/

   ![](Images/aad-create-app-02.png)

   Haga clic en **Crear**.

5. Cuando haya finalizado, se mostrará la aplicación en la lista.

   ![](/Images/aad-create-app-03.png)

6. Haga clic en ella para ver los detalles. 

   ![](/Images/aad-create-app-04.png)

7. Haga clic en **Toda la configuración** si no se muestra la ventana de configuración.

   - Haga clic en **Propiedades** y luego establezca el campo **Multiinquilino** en **Sí**.

     ![](/Images/aad-create-app-05.png)

     Copie aparte el **Id. de aplicación** y después, haga clic en **Guardar**.

   - Haga clic en **Permisos necesarios**. Agregue los permisos siguientes:

     | API | Permisos de aplicación | Permisos delegados|
| ------------------------------ | ----------------------- | ---------------------------------------- |
| Microsoft Graph | Leer datos del directorio | Leer los perfiles completos de todos los usuarios<br>Leer datos del directorio<br>Leer datos del directorio<br>Acceder al directorio como el usuario que ha iniciado sesión<br>Iniciar sesión con usuarios |
| Windows Azure Active Directory |
| Iniciar sesión y leer el perfil del usuario<br>Leer y escribir datos en el directorio |

     ![](/Images/aad-create-app-06.png)

   - Haga clic en **Claves** y agregue una nueva clave:

     ![](Images/aad-create-app-07.png)

     Haga clic en **Guardar** y copie aparte el **VALOR** de la clave. 

   Cierre la ventana de configuración.

## Ejecutar el ejemplo de forma local

Se necesitan los siguientes software:

- [Python](https://www.python.org/downloads/) versión 3.5.2 o superior
- [Django](https://www.djangoproject.com/download/) versión 1.11 o superior
- [SQLite](https://www.sqlite.org/)

Ejecutar la aplicación:

1. Configure las siguientes **variables de entorno**:

   - **clientId**: use el Id. de cliente del registro de la aplicación que creó anteriormente.
   - **clientSecret**: use el valor de la clave del registro de la aplicación que creó anteriormente.
   - **SourceCodeRepositoryURL**: use la URL de este repositorio.

2. Abra el terminal y desplácese hasta la carpeta del código fuente. Ejecute el siguiente comando:

   ```sh
   pip install -r requirements.txt
   python manage.py runserver
   ```

3. Abra http://127.0.0.1:8000/ en un navegador.

## Implementar el ejemplo en Azure

**Autorización de GitHub**

1. Generar el token

   - Abra https://github.com/settings/tokens en el explorador web.
   - Inicie sesión en la cuenta de GitHub donde haya bifurcado este repositorio.
   - Haga clic en **Generar token**
   - Escriba un valor en el cuadro de texto **Descripción del token**
   - Seleccione lo siguiente (las selecciones deben coincidir con la siguiente captura de pantalla):
     - repo (todas) -> repo:status, repo_deployment, public_repo
     - admin:repo_hook -> read:repo_hook

   ![](Images/github-new-personal-access-token.png)

   - Haga clic en **Generar token**
   - Copie el token

2. Agregar el token de GitHub a Azure en Azure Resource Explorer

   - Abra https://resources.azure.com/providers/Microsoft.Web/sourcecontrols/GitHub en el explorador web.
   - Inicie sesión con su cuenta de Azure.
   - Seleccione la suscripción de Azure correcta.
   - Seleccione el modo **Lectura y escritura**.
   - Haga clic en **Editar**.
   - Pegue el token en el **parámetro de token**.

   ![](Images/update-github-token-in-azure-resource-explorer.png)

   - Haga clic en **COLOCAR**.

**Implementar los componentes de Azure desde GitHub**

1. Asegúrese de que la compilación pasa la compilación de VSTS.

2. Bifurque este repositorio en su cuenta de GitHub.

3. Haga clic en el botón Implementar en Azure:

   [![Implementar en Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2FO365-EDU-Python-Samples%2Fmaster%2Fazuredeploy.json)

4. Rellene los valores de la página de implementación y seleccione la casilla de verificación **Acepto los términos y condiciones indicados anteriormente**.

   ![](Images/azure-auto-deploy.png)

   - **Grupo de recursos**: le recomendamos que cree un nuevo grupo.

   - **Nombre del sitio**: escriba un nombre. Por ejemplo, EDUGraphAPICanviz o EDUGraphAPI993.

     > Nota: Si el nombre que ha escrito está ocupado, obtendrá errores de validación:
     >
     > ![](Images/azure-auto-deploy-validation-errors-01.png)
     >
     > Si hace clic en él obtendrá más información, como, por ejemplo, que la cuenta de almacenamiento ya se encuentra en otro grupo de recursos o en otra suscripción.
     >
     > En ese caso, use otro nombre.

   - **URL del repositorio de códigos fuente**: reemplace <YOUR REPOSITORY> por el nombre del repositorio de la bifurcación.

   - **Integración manual del código fuente**: seleccione **falso**, ya que va a hacer la implantación desde su propia bifurcación.

   - **Id. de cliente**: use el Id. de cliente del registro de la aplicación que creó anteriormente.

   - **Secreto de cliente**: use el valor de la clave del registro de la aplicación que creó anteriormente.

   - Marque **Acepto los términos y condiciones indicados anteriormente**.

5. Haga clic en **Comprar**.

**Agregue una URL de respuesta al registro de la aplicación.**

1. Después de la implementación, abra el grupo de recursos:

   ![](Images/azure-resource-group.png)

2. Haga clic en la aplicación web.

   ![](Images/azure-web-app.png)

   Copie la URL aparte y cambie el esquema a **https**. Esta es la URL de respuesta y se utilizará en el siguiente paso.

3. Vaya al registro de aplicaciones del nuevo Microsoft Azure Portal y abra las ventanas de configuración.

   Agregue la URL de respuesta:

   ![](Images/aad-add-reply-url.png)

   > Nota: Para depurar el ejemplo localmente, asegúrese de que http://127.0.0.1:8000/ se encuentra en las URL de respuesta.

4. Haga clic en **GUARDAR**.

## Entender el código

### Introducción

**Diagrama de componentes de la solución**

![](Images/solution-component-diagram.png)

### **EDUGraphAPI**

**Autenticación**

Django incluye un sistema de autenticación de usuarios. Maneja las cuentas de usuario, los grupos, los permisos y las sesiones de usuario basadas en cookies. Para obtener más información, consulte [Sistema de autenticación de Django](https://docs.djangoproject.com/en/1.10/topics/auth/).

La autenticación de los usuarios locales se basa en la API integrada:

* [autenticar](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.authenticate)
* [inicio de sesión](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.login)
* [cerrar sesión](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.logout)

La autenticación de usuarios de O365 se implementa con Open ID Connect. 

**Acceso a los datos**

En este ejemplo, [el ORM integrado de Django](https://docs.djangoproject.com/en/1.11/topics/db/) se usa para acceder a los datos desde la base de datos SQLite de back-end.

Debajo están las tablas:

| Tabla | Descripción |
| ------------------------------ | ---------------------------------------- |
| auth_user | La tabla de usuarios integrada de Django contiene la información de autenticación de los usuarios: nombre de usuario, correo electrónico, contraseña... |
| user_roles | Contiene los roles del usuario. En este ejemplo se utilizan tres roles: administrador, profesor y alumno. |
| perfiles | Contiene información adicional de los usuarios: *favoriteColor*, *organization_id*, *o365UserId* y *o365Email*. Las últimas dos se usan para conectar al usuario local con un usuario de O365\. |
| organizaciones | Una fila de esta tabla representa un espacio empresarial en AAD.<br>La columna *isAdminConsented* registra si el inquilino fue permitido por un administrador. |
| token_cache | Contiene los tokens de acceso y actualización de los usuarios. |
| classroom_seating_arrangements | Contiene los datos de las disposiciones del aula. |

Los modelos se definen en **/models/db.py**.

**Vistas**

Debajo están las vistas:

| Ruta | Descripción |
| -------------------- | ---------------------------------------- |
| /account/views.py | Contiene vistas para que los usuarios se registren e inicien y cierren sesión. |
| /admin/views.py | Contiene vistas administrativas como permitir inquilino, administrar cuentas vinculadas. |
| /link/views.py | Contiene vistas usadas para vincular cuentas de usuarios. |
| /management/views.py | Contiene vistas de la página de información personal. |
| /schools/view.py | Contiene vistas educativas, como escuelas, clases, y detalles de las clases. |

**Elementos Decorator**

En la aplicación de usaron los siguientes elementos Decorator:

| Elemento Decorator | Descripción |
| ----------------- | ---------------------------------------- |
| login_required | Solo permite que los usuarios que hayan iniciado sesión puedan acceder a los recursos protegidos. |
| admin_only | Solo permite que los administradores puedan acceder a los recursos protegidos. |
| linked_users_only | Solo permite que los usuarios vinculados puedan acceder a los recursos protegidos. |

**Servicios**

Los servicios utilizados por la aplicación del lado servidor:

| Servicio | Descripción |
| ---------------- | ---------------------------------------- |
| UserService | Contiene el método utilizado para manipular a los usuarios en la base de datos. |
| LinkService | Contiene el método utilizado para vincular las cuentas de usuarios. |
| AuthService | Contiene los métodos utilizados para la autorización y autenticación. |
| AADGraphService | Contiene los métodos utilizados para acceder a las API Graph de AAD. |
| MSGraphService | Contiene los métodos utilizados para acceder a las API de MS Graph. |
| EducationService | Contiene dos métodos para acceder a las API REST de Educación. |
| TokenService | Contiene el método utilizado para obtener y actualizar tokens desde la base de datos |

Los servicios están en la carpeta **/services**.

**Middleware**

**`HandleRefreshTokenExceptionMiddleware`** captura la`RefreshTokenException` iniciada por el `TokenService` cuando el token de actualización no existe o ha expirado. Luego redirecciona al usuario a una página para que el usuario pueda volver a iniciar sesión.

**Aplicación multiinquilino**

Esta aplicación web es una **aplicación multiinquilino**.  En AAD, hemos habilitado la opción:

![](Images/app-is-multi-tenant.png)

Los usuarios de cualquier inquilino de Azure Active Directory pueden obtener acceso a esta aplicación. Algunos de los permisos usados por esta aplicación requieren que un administrador del espacio empresarial les dé el consentimiento para que los usuarios puedan usar la aplicación. En caso contrario, los usuarios verán este error:

![](Images/app-requires-admin-to-consent.png)

Para obtener más información, consulte [Crear una aplicación web SaaS multiinquilino con Azure AD y OpenID Connect](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapp-multitenant-openidconnect/).

### API de Office 365 Educación

Las [API de Office 365 Educación](https://msdn.microsoft.com/office/office365/api/school-rest-operations) devuelven datos de cualquier inquilino de Office 365 que Microsoft School Data Sync ha sincronizado con la nube. Las API proporcionan información sobre centros educativos, secciones, profesores, alumnos y listas. La API de REST de centros educativos proporciona acceso a entidades escolares en Office 365 para los inquilinos de educación.

En el ejemplo, el proyecto de biblioteca de clases **Microsoft.Education** contiene la API de Office 365 Educación. 

**EducationServiceClient** es la clase principal de la biblioteca. Se usa para obtener fácilmente datos educativos.

**Obtener centros educativos**

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

**Obtener clases**

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
**Obtener usuarios**

```typescript
def get_members(self, object_id, top=12, nextlink=''):
    skiptoken = self._get_skip_token(nextlink)
    url = self.api_base_uri + 'administrativeUnits/%s/members?$top=%s%s' % (object_id, top, skiptoken)
    return self.rest_api_service.get_object_list(url, self.access_token, model=EduUser, next_key='odata.nextLink')
```
A continuación, se muestran algunas capturas de pantalla de la aplicación de ejemplo donde se muestran datos académicos.

![](Images/edu-schools.png)

![](Images/edu-users.png)

![](Images/edu-classes.png)

![](Images/edu-class.png)

### Flujos de autenticación

Hay cuatro flujos de autenticación en este proyecto.

Los primeros 2 flujos (Inicio de sesión local/Inicio de sesión de Office 365) permiten a los usuarios iniciar sesión con una cuenta local o con una cuenta de Office 365 y, después, establecer un vínculo a la cuenta del otro tipo. Este procedimiento se implementa en LinkController.

**Flujo de autenticación de inicio de sesión local**

![](Images/auth-flow-local-login.png)

**Flujo de autenticación de inicio de sesión de Office 365**

![](Images/auth-flow-o365-login.png)

**Flujo de autenticación de inicio de sesión del administrador**

Este flujo muestra cómo inicia sesión un administrador en el sistema y cómo realiza operaciones administrativas.

Cuando el administrador inicie sesión en la aplicación con una cuenta de Office 365, se le pedirá que se vincule a una cuenta local. Este paso no es obligatorio y puede omitirse. 

Como se mencionó anteriormente, la aplicación web es una aplicación multiinquilino que usa algunos permisos de aplicación, por lo que el administrador del inquilino debe aceptar primero la aplicación.  

Este flujo se implementa en AdminController. 

![](Images/auth-flow-admin-login.png)

### Dos tipos de API de Graph

En este ejemplo se usan dos API de Graph distintas:

| | [API de Azure AD Graph](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api) | [API de Microsoft Graph](https://graph.microsoft.io/) |
| ------------ | ---------------------------------------- | ---------------------------------------- |
| Descripción | La API de Azure Active Directory Graph proporciona acceso mediante programación a Azure Active Directory a través de los puntos de conexión de la API de REST. Las aplicaciones pueden usar la API de Azure AD Graph para realizar operaciones de creación, lectura, actualización y eliminación (CRUD) en datos del directorio y objetos del directorio, como usuarios, grupos y contactos de la organización | Una API unificada que también incluye las API de otros servicios de Microsoft, como Outlook, OneDrive, OneNote, Planner y Office Graph, a los que se accede a través de un único punto de conexión con un único token de acceso. |
| Cliente | Install-Package [Microsoft.Azure.ActiveDirectory.GraphClient](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/) | Install-Package [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) |
| Punto de conexión | https://graph.windows.net | https://graph.microsoft.com |
| API Explorer | https://graphexplorer.cloudapp.net/| https://graph.microsoft.io/graph-explorer |

> **NOTA IMPORTANTE:** Microsoft está realizando una inversión considerable en la nueva API de Microsoft Graph, y ya no invierte en la API de Azure AD Graph (excepto para solucionar problemas de seguridad).

> Por lo tanto, use la nueva API de Microsoft Graph tanto como sea posible y minimice el uso de la API de Azure AD Graph.

Este es un fragmento de código que muestra cómo se obtienen documentos del grupo de la API de Microsoft Graph.

```typescript
def get_documents(self, object_id):
    url = self.api_base_uri + 'groups/%s/drive/root/children' % object_id
    return self.rest_api_service.get_object_list(url, self.access_token, model=Document)
```

Tenga en cuenta que, en la configuración la aplicación de AAD, los permisos para cada API de Graph se configuran por separado:

![](Images/aad-create-app-06.png) 

## Preguntas y comentarios

- Si tiene algún problema para ejecutar este ejemplo, [registre un problema](https://github.com/OfficeDev/O365-EDU-Python-Samples/issues).
- Las preguntas generales sobre el desarrollo de GraphAPI deben publicarse en [Stack Overflow](http://stackoverflow.com/questions/tagged/office-addins). Asegúrese de que sus preguntas o comentarios se etiquetan con [ms-graph-api]. 

## Colaboradores

Le animamos a contribuir a nuestros ejemplos. Para obtener instrucciones sobre cómo continuar, consulte [nuestra guía de contribución](/CONTRIBUTING.md)

Este proyecto ha adoptado el [Código de conducta de código abierto de Microsoft](https://opensource.microsoft.com/codeofconduct/). Para obtener más información, vea [Preguntas frecuentes sobre el código de conducta](https://opensource.microsoft.com/codeofconduct/faq/) o póngase en contacto con [opencode@microsoft.com](mailto:opencode@microsoft.com) si tiene otras preguntas o comentarios.



**Copyright (c) 2017 Microsoft. Reservados todos los derechos.**
