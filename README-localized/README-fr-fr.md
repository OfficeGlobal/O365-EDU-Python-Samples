---
page_type: sample
products:
- office
- office-365
- ms-graph
languages:
- python
- javascript
description: "Comment intégrer les rôles scolaires/données de la liste, ainsi que les services Office 365 disponibles via l’API Graph."
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

# EDUGraphAPI : exemple de code Office 365 pour l’éducation (Python)

Dans cet exemple, nous vous indiquons comment intégrer des données de liste de présence/rôles d’établissement scolaire, ainsi que les services Office 365 disponibles via l’API Graph. 

Les données scolaires sont synchronisées dans les locataires Office 365 éducation par [Microsoft School Data Sync](http://sds.microsoft.com).  

**Table des matières**

- [Exemples de objectifs](#sample-goals)
- [Conditions préalables](#prerequisites)
- [Inscription de l’application dans Azure Active Directory](#register-the-application-in-azure-active-directory)
- [Exécuter l’exemple localement](#run-the-sample-locally)
- [Déployez l’exemple sur Azure](#deploy-the-sample-to-azure)
- [Comprendre le code](#understand-the-code)
- [Questions et commentaires](#questions-and-comments)
- [Contribution](#contributing)

## Exemples de objectifs

Cet exemple illustre les éléments suivants :

- Appeler les API Graph, notamment :

  - [Microsoft Azure Active Directory Graph](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)
  - [API Microsoft Graph](https://www.nuget.org/packages/Microsoft.Graph/)

- Liaison de comptes d’utilisateur gérés localement et de comptes d’utilisateurs Office 365 (Azure Active Directory). 

  Une fois les comptes liés, les utilisateurs peuvent utiliser des comptes locaux ou Office 365 pour se connecter au site web d’exemple et l’utiliser.

- Acquisition d’établissements scolaires, de sections, d’enseignants et d’étudiants à partir d’Office 365 Éducation :

  - [Informations de référence sur les API REST Office 365 Schools](https://msdn.microsoft.com/office/office365/api/school-rest-operations)

Cet exemple est implémenté avec le langage Python et l’infrastructure web [Django](https://www.djangoproject.com/).

> [Django](https://www.djangoproject.com/) est une infrastructure web python de haut niveau qui favorise le développement rapide et la conception pragmatique et propre. Conçu par des développeurs expérimentés, elle s’occupe de la complexité du développement web. Vous pouvez ainsi vous concentrer sur la rédaction de votre application sans avoir à réinventer la roue. Il est gratuit et ouvert.

## Conditions préalables

**Déploiement et l’exécution de cet exemple nécessitent** :

- Un abonnement Azure disposant des autorisations pour inscrire une nouvelle application et déployer l’application web.

- Un client Office 365 Éducation avec Microsoft School Data Sync activé

  - Un des navigateurs suivants : Edge, Internet Explorer 9, Safari 5.0.6, Firefox 5, Chrome 13 ou une version ultérieure de l’un de ces navigateurs.

  En outre : Le développement ou l’exécution de cet exemple localement nécessite les conditions suivantes :  

  - [Python](https://www.python.org/downloads/) 3.5.2 ou version ultérieure
  - [Django](https://www.djangoproject.com/download/) 1.11 ou version ultérieure
  - [SQLite](https://www.sqlite.org/)



## Inscription de l’application dans Azure Active Directory

1. Connectez-vous au nouveau Portail Azure : [https://portal.azure.com](https://portal.azure.com/).

2. Choisissez votre client Azure AD en sélectionnant votre compte dans le coin supérieur droit de la page :

   ![](Images/aad-select-directory.png)

3. Cliquez sur **Azure Active Directory**, -> **Inscriptions des applications** -> **+Ajouter**.

   ![](Images/aad-create-app-01.png)

4. Entrez un **Nom**, puis sélectionnez **Application web/API** en tant que**Type d’application**.

   **URL de connexion**: http://127.0.0.1:8000/

   ![](Images/aad-create-app-02.png)

   Cliquez sur **Créer**.

5. Une fois l’opération terminée, l’application s’affiche dans la liste.

   ![](/Images/aad-create-app-03.png)

6. Cliquez dessus pour afficher les détails associés. 

   ![](/Images/aad-create-app-04.png)

7. Cliquez sur **Tous les paramètres**, si la fenêtre de paramètres ne s’affiche pas.

   - Cliquez sur **Propriétés**, puis configurez **Mutualisé** à **Oui**.

     ![](/Images/aad-create-app-05.png)

     Copiez **ID d’application**, puis cliquez **Enregistrer**.

   - Cliquez sur **Autorisations requises**. Ajoutez les autorisations suivantes :

     | API | Autorisation d’application | Autorisations déléguées |
| ------------------------------ | ----------------------- | ---------------------------------------- |
| Microsoft Graph | Lire les données du répertoire | Lire les profils complets de tous les utilisateurs<br>Lire les données de l’annuaire<br>Lire les données de l’annuaire<br>Accéder à l’annuaire en tant qu’utilisateur connecté<br>Connecter les utilisateurs |
| Windows Azure Active Directory |
| Se connecter et lire le profil de l’utilisateur<br>Lire et écrire les données de l’annuaire

     ![](/Images/aad-create-app-06.png)

   - Cliquez sur **Clés**, puis ajoutez une nouvelle clé :

     ![](Images/aad-create-app-07.png)

     Cliquez sur **Enregistrer**, puis copiez la **VALEUR** de la clé. 

   Fermez la fenêtre Paramètres.

## Exécuter l’exemple localement

Les logiciels suivantes sont obligatoires :

- [Python](https://www.python.org/downloads/) 3.5.2 ou version ultérieure
- [Django](https://www.djangoproject.com/download/) 1.11 ou version ultérieure
- [SQLite](https://www.sqlite.org/)

Exécuter l’application :

1. Configurez les **variables d’environnement** suivantes :

   - **clientId** : utilisez l’ID client de l’inscription de l’application que vous avez créée précédemment.
   - **clientSecret**: utilisez la valeur clé de l’inscription de l’application que vous avez créée précédemment.
   - **SourceCodeRepositoryURL** : utilisez l’URL de ce référentiel.

2. Ouvrez terminal et accédez au dossier de code source. Exécutez la commande ci-dessous :

   ```sh
   pip install -r requirements.txt
   python manage.py runserver
   ```

3. Ouvrez http://127.0.0.1:8000/ dans un navigateur.

## Déployez l’exemple sur Azure

**Autorisation GitHub**

1. Générez un jeton

   - Ouvrez https://github.com/settings/tokens dans votre navigateur web.
   - Connectez-vous à votre compte GitHub où vous avez dupliqué ce référentiel.
   - Cliquez sur **Générer un jeton**.
   - Entrez une valeur dans la zone de texte **Description du jeton**
   - Sélectionnez les options suivantes (vos sélections doivent correspondre à la capture d’écran ci-dessous) :
     - repo (all) -> repo:status, repo\_deployment, public\_repo
     - admin:repo\_hook -> read:repo\_hook

   ![](Images/github-new-personal-access-token.png)

   - Cliquez sur **Générer un jeton**
   - Copiez le jeton

2. Ajoutez le jeton GitHub à Azure dans Azure Resource Explorer

   - Ouvrez https://resources.azure.com/providers/Microsoft.Web/sourcecontrols/GitHub dans votre navigateur web.
   - Connectez-vous à l’aide de votre compte Azure.
   - Sélectionnez l’abonnement Azure approprié.
   - Sélectionnez le mode**Lecture/écriture**.
   - Cliquez sur **Modifier**.
   - Collez le jeton dans le **paramètre de jeton**.

   ![](Images/update-github-token-in-azure-resource-explorer.png)

   - Cliquez sur **PUT**.

**Déployer les composants Azure à partir de GitHub**

1. Vérifiez que la création transmet la création VSTS.

2. Transférez ce référentiel sur votre compte GitHub.

3. Cliquez sur le bouton déployer vers Azure :

   [![Déployer dans Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FOfficeDev%2FO365-EDU-Python-Samples%2Fmaster%2Fazuredeploy.json)

4. Renseignez les valeurs dans la page déploiement et sélectionnez le case à cocher **j’accepte les conditions générales mentionnées ci-dessus**.

   ![](Images/azure-auto-deploy.png)

   - **Groupe de ressources** : nous vous suggérons de créer un groupe.

   - **Nom du site**: veuillez entrer un nom. Tels que EDUGraphAPICanviz ou EDUGraphAPI993.

     > Remarque : Si le nom que vous entrez est utilisé, vous obtiendrez des erreurs de validation :
     >
     > ![](Images/azure-auto-deploy-validation-errors-01.png)
     >
     > Cliquez dessus pour obtenir davantage de détails tels que le compte de stockage est déjà dans un autre groupe de ressources/abonnement.
     >
     > Dans ce cas, veuillez utiliser un autre nom.

   - **URL du référentiel de code source**: remplacez <YOUR REPOSITORY> par le nom du référentiel de votre fourche.

   - **Intégration manuelle du code source** : sélectionnez **faux**, car vous déployez à partir de votre propre fourche.

   - **ID client** : utilisez l’ID client de l’inscription de l’application que vous avez créée précédemment.

   - **Secret client**: utilisez la valeur clé de l’inscription de l’application que vous avez créée précédemment.

   - Cliquez sur **J’accepte les termes et conditions mentionnés ci-dessus**.

5. Cliquez sur **Achat**.

**Ajouter une URL de réponse à l’inscription de l’application**

1. Après le déploiement, ouvrez le groupe de ressources :

   ![](Images/azure-resource-group.png)

2. Cliquez sur l’application web.

   ![](Images/azure-web-app.png)

   Copiez l’URL et modifiez le schéma en **https**. Il s’agit de l’URL de relecture qui sera utilisée à l’étape suivante.

3. Accédez à inscription de l’application dans le nouveau Portail Azure, puis ouvrez le paramètre Windows.

   Ajouter l’URL de réponse :

   ![](Images/aad-add-reply-url.png)

   > Remarque : pour déboguer l’exemple localement, assurez-vous que http://127.0.0.1:8000/ se trouve dans les URL de réponse.

4. Cliquez sur **ENREGISTRER**.

## Comprendre le code

### Introduction

**Diagramme de composant de solution**

![](Images/solution-component-diagram.png)

### **EDUGraphAPI**

**Authentification**

Django inclut un système d’authentification utilisateur. Elle gère les comptes d’utilisateur, les groupes, les autorisations et les sessions utilisateur basées sur les cookies. Pour en savoir plus, consultez la rubrique relative au [système d’authentification Django](https://docs.djangoproject.com/en/1.10/topics/auth/).

L’authentification des utilisateurs locaux est basée sur l’API intégrée :

* [authentifier](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.authenticate)
* [se connecter](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.login)
* [Déconnexion](https://docs.djangoproject.com/en/1.10/topics/auth/default/#django.contrib.auth.logout)

L’authentification des utilisateurs Office 365 est implémentée avec Open ID Connect. 

**Accès aux données**

Dans cet exemple, [ ORM intégrée de Django](https://docs.djangoproject.com/en/1.11/topics/db/) est utilisée pour accéder aux données à partir de la base de données SQLite.

Voici les tableaux :

| Tableau | Description |
| ------------------------------ | ---------------------------------------- |
| auth\_user | Tableau utilisateur intégrée Django qui contient les informations d’authentification des utilisateurs : nom d’utilisateur, adresse de messagerie, mot de passe... | user\_roles | Contient les rôles des utilisateurs. Cet exemple utilise trois rôles : administrateur, enseignant et étudiant. |
| profils | Contient des informations supplémentaires sur les utilisateurs : *favoriteColor*, *organization\_id*, *o365UserId*et *o365Email*. Les deux plus récents sont utilisés pour connecter l’utilisateur local avec un utilisateur Office 365\. |
| organisations | Une ligne dans ce tableau représente un client dans AAD.<br>La colonne *isAdminConsented* enregistre si le client a été envoyé par un administrateur. |
| token\_cache | Contient les jetons d’accès/d’actualisation de l’utilisateur. |
| classroom\_seating\_arrangements | Contient les données relatives aux dispositions de salle de classe. |

Les modèles sont définis dans **/models/db.py**.

**Affichages**

Voici les affichages :

| Chemin | Description |
| -------------------- | ---------------------------------------- |
| /account/views.py | Contient des affichages que les utilisateurs peuvent enregistrer, se connecter et se déconnecter. |
| /admin/views.py | Contient des affichages administratifs tels que le client de consentement, gérer les comptes liés. |
| /link/views.py | Contient des affichages utilisés pour les comptes d’utilisateurs de liens. |
| /management/views.py | Contient des affichages de la page à propos de moi. |
| /schools/view.py | Contient des affichages éducation, tels que des écoles, des classes et des détails de cours. |

**Éléments décoratifs**

Les éléments décoratifs suivants ont été utilisés dans cette application :

| Élément décoratif | Description |
| ----------------- | ---------------------------------------- |
| login\_required | Autoriser uniquement les utilisateurs connectés à accéder aux ressources protégées. |
| admin\_only | Autoriser uniquement les administrateurs à accéder aux ressources protégées. |
| linked\_users\_only | Autoriser uniquement les utilisateurs liés à accéder aux ressources protégées. |

**Services**

Les services utilisés par l’application côté serveur :

| Service | Description |
| ---------------- | ---------------------------------------- |
| UserService | Contient la méthode utilisée pour manipuler les utilisateurs dans la base de données. |
| LinkService | Contient la méthode utilisée pour lier les comptes d’utilisateurs. |
| AuthService | Contient des méthodes utilisées pour l’autorisation et l’authentification. |
| AADGraphService | Contient des méthodes utilisées pour accéder aux API de graphiques AAD. |
| MSGraphService | Contient des méthodes utilisées pour accéder aux API MS Graph. |
| EducationService | Contient deux méthodes pour accéder aux API REST de formation. |
| TokenService | Contient une méthode utilisée pour obtenir et mettre à jour des jetons à partir de la base de données |

Les services figurent dans le dossier **/services**.

**Intergiciel (middleware)**

**`HandleRefreshTokenExceptionMiddleware`** intercepte `RefreshTokenException` levé par `TokenService` lorsque le jeton d’actualisation n’existe pas ou n’a pas expiré. Il redirige ensuite l’utilisateur vers une page pour permettre à l’utilisateur de se reconnecter.

**Application mutualisée**

Cette application web est une **application mutualisée**. Dans AAD, nous avons activé l’option :

![](Images/app-is-multi-tenant.png)

Les utilisateurs de n’importe quel client Azure Active Directory peuvent accéder à cette application. Certaines autorisations utilisées par cette application nécessitent l’autorisation de l’administrateur du locataire avant que les utilisateurs puissent utiliser l’application. Dans le cas contraire, les utilisateurs verront ce message d’erreur :

![](Images/app-requires-admin-to-consent.png)

Pour plus d’informations, consultez[Créer une application web SaaS multi-locataire à l’aide d’Azure AD et OpenID Connect](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapp-multitenant-openidconnect/).

### API Office 365 Éducation

La [API Office 365 Éducation](https://msdn.microsoft.com/office/office365/api/school-rest-operations) renvoyer les données de n’importe quel locataire Office 365 qui a été synchronisé vers le cloud par Microsoft School Data Sync. Les API fournissent des informations sur les établissements scolaires, les sections, les enseignants, les étudiants et les listes de documents. L’API REST des établissements scolaires fournit l’accès aux entités scolaires dans Office 365 pour les locataires de l’éducation.

Dans cet exemple, le projet **Microsoft.Education** Class Library encapsule l’API Office 365 Éducation. 

La **EducationServiceClient** est la classe principale de la bibliothèque. Il permet de récupérer aisément des données d’éducation.

**Obtenir des établissements scolaires**

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

**Obtenir des classes**

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
**obtenir des utilisateus**

```typescript
def get_members(self, object_id, top=12, nextlink=''):
    skiptoken = self._get_skip_token(nextlink)
    url = self.api_base_uri + 'administrativeUnits/%s/members?$top=%s%s' % (object_id, top, skiptoken)
    return self.rest_api_service.get_object_list(url, self.access_token, model=EduUser, next_key='odata.nextLink')
```
Voici quelques captures d’écran de l’exemple d’application qui montrent les données d’éducation.

![](Images/edu-schools.png)

![](Images/edu-users.png)

![](Images/edu-classes.png)

![](Images/edu-class.png)

### Flux d’authentification

Ce projet comprend 4 flux d’authentification.

Les deux premiers flux (connexion locale/connexion Office 365) permettent aux utilisateurs de se connecter à l’aide d’un compte local ou d’un compte Office 365, puis de créer un lien vers le compte autre type. Cette procédure est implémentée dans la LinkController.

**Flux d’authentification de connexion locale**

![](Images/auth-flow-local-login.png)

**Flux d’authentification de connexion Office 365**

![](Images/auth-flow-o365-login.png)

**Flux d’authentification de connexion d’administrateur**

Ce flux montre comment un administrateur se connecte au système et effectue des opérations d’administration.

Une fois que vous êtes connecté à l’application à l’aide d’un compte Office 365, l’administrateur est invité à créer un lien vers un compte local. Cette étape n’est pas obligatoire et peut être ignorée. 

Comme indiqué précédemment, l’application web est une application mutualisée qui utilise des autorisations d’application, de sorte que l’administrateur locataire doit d’abord consentir l’application.  

Ce flux est implémenté dans la AdminController. 

![](Images/auth-flow-admin-login.png)

### Deux types d’API Graph

Il existe deux API Graph distinctes utilisées dans cet exemple :

|
| [API Azure AD Graph](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api) | [API Microsoft Graph](https://graph.microsoft.io/) |
| ------------ | ---------------------------------------- | ---------------------------------------- |
| Description | L’API Graph Azure Active Directory fournit un accès par programme à Azure Active Directory via les points de terminaison de l’API REST. Les applications peuvent utiliser l’API Azure AD Graph pour effectuer des opérations de création, lecture, mise à jour et suppression sur des données d’annuaire et des objets d’annuaire, tels que des utilisateurs, des groupes et des contacts d’organisation | API unifiée qui inclut également des API d’autres services Microsoft tels qu’Outlook, OneDrive, OneNote, Planner et Office Graph, accessibles via un point de terminaison unique avec un seul jeton d’accès. |
| Client | Installer-Package [Microsoft. Azure. ActiveDirectory. GraphClient](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/) | Installer-Package [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) |
| Point de terminaison | https://graph.windows.net | https://graph.microsoft.com |
| Explorateur d’API | https://graphexplorer.cloudapp.net/| https://graph.microsoft.io/graph-explorer |

> **REMARQUE IMPORTANTE** Microsoft investit énormément dans la nouvelle API Microsoft Graph et n’investit plus dans l’API Azure AD Graph (sauf pour résoudre les problèmes de sécurité).

> Par conséquent, nous vous recommandons d’utiliser la nouvelle API Microsoft Graph autant que possible et de réduire la quantité d’utilisation de l’API Azure AD Graph.

Voici une partie de code montrant comment obtenir des documents de groupe de l’API Microsoft Graph.

```typescript
def get_documents(self, object_id):
    url = self.api_base_uri + 'groups/%s/drive/root/children' % object_id
    return self.rest_api_service.get_object_list(url, self.access_token, model=Document)
```

Notez que, dans les paramètres de l’application AAD, les autorisations pour chaque API Graph sont configurées séparément :

![](Images/aad-create-app-06.png) 

## Questions et commentaires

- Si vous rencontrez des difficultés pour exécuter cet exemple, veuillez [consigner un problème](https://github.com/OfficeDev/O365-EDU-Python-Samples/issues).
- Si vous avez des questions sur le développement de GraphAPI en général, envoyez-les sur [Stack Overflow](http://stackoverflow.com/questions/tagged/office-addins). Posez vos questions avec les balises \[ms-graph-api]. 

## Contribution

Nous vous invitons à contribuer à nos exemples. Pour obtenir des instructions sur la façon de procéder, consultez [notre guide de contribution](/CONTRIBUTING.md).

Ce projet a adopté le [code de conduite Open Source de Microsoft](https://opensource.microsoft.com/codeofconduct/). Pour en savoir plus, reportez-vous à la [FAQ relative au code de conduite](https://opensource.microsoft.com/codeofconduct/faq/) ou contactez [opencode@microsoft.com](mailto:opencode@microsoft.com) pour toute question ou tout commentaire.



**Copyright (c) 2017 Microsoft. Tous droits réservés.**
