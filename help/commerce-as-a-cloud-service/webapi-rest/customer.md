---
title: Explorer de nouvelles API REST client
description: Découvrez comment utiliser les nouvelles API REST client dans Adobe Commerce Cloud Service. Idéal pour les architectes et les développeurs.
feature: REST, Customers, Saas
topic: Development, Integrations
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 225
last-substantial-update: 2026-01-27T00:00:00Z
jira: KT-20160
source-git-commit: cb3fecf5f8b23425311dc31ed526b3b9bfe07b45
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 0%

---


# API REST client

Découvrez comment utiliser les nouvelles API REST client dans Adobe Commerce as a Cloud Service. Ce tutoriel est idéal pour les architectes et les développeurs qui cherchent à intégrer et optimiser efficacement des solutions d’API.

## À qui s&#39;adresse cette vidéo ?

* Développeurs et développeuses back-end chargés de créer des intégrations avec Adobe Commerce
* Architectes techniques concevant des workflows de gestion client pour des implémentations commerciales découplées

## Contenu vidéo

* Authentifiez-vous avec Adobe IMS à l’aide des informations d’identification de serveur à serveur pour obtenir un jeton d’accès pour les requêtes API
* Utiliser le format de point d’entrée de l’API REST approprié pour Commerce as a Cloud Service
* Créez et mettez à jour des comptes clients par programmation à l’aide de requêtes POST et PUT avec des payloads JSON appropriées

>[!VIDEO](https://video.tv.adobe.com/v/3479364/?captions=fre_fr&learn=on&enablevpops)

## Exemples de code

Avant de commencer, rassemblez toutes les valeurs requises à partir de [Experience Cloud](https://experience.adobe.com) et du [Adobe Developer Console](https://developer.adobe.com/console). La préparation de ces valeurs garantit un processus de configuration fluide.

>[!NOTE]
>
>Vérifiez que vous travaillez dans la bonne organisation. La sélection de votre organisation affecte les instances et les environnements visibles dans Experience Cloud et Developer Console.

### Détails de l’instance - experience.adobe.com

Les détails de l’instance contiennent des éléments tels que votre ID d’instance, les points d’entrée GraphQL, les informations d’identification.

### Détails du développeur - https://developer.adobe.com/console/

C’est dans le Developer Console que vous gérez vos informations d’identification API, y compris les identifiants client, les secrets client et les jetons d’accès. Vous pouvez également créer de nouveaux types d’informations d’identification, tels que l’authentification de serveur à serveur ou l’authentification Native App.

## Conditions préalables

| Poste | Valeur | Où est cette valeur ? |
|--- |--- |--- |
| Identifiant de l’instance | `<instance_id>` | experience.adobe.com |
| Point d’entrée REST | `<rest_endpoint>` | experience.adobe.com |
| Identifiant client | `<client_id>` | developer.adobe.com/console |
| Secret client | `<client_secret>` | developer.adobe.com/console |


## Étape 1 : obtenir le jeton d’accès (authentification de serveur à serveur)

>[!IMPORTANT]
>
> Les variables affichées dans cet exemple ne sont pas valides. Utilisez &lt;client_id> et &lt;client_secret> à partir des informations d’identification de votre projet.

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**Exemple de réponse :**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## Étape 2 : créer un client

>[!IMPORTANT]
>
> L’URL fournie dans cet exemple n’est pas valide. Utilisez votre URL de base REST. Échangez « &lt;rest_endpoint> » avec votre URL. Il ressemble à ce `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`.
>
> /rest/ n’est pas inclus dans l’URL de ce point d’entrée. L’inclusion de entraîne une erreur.

**Endpoint:** `POST /V1/customers`

```bash
curl -X POST \
  "<rest_endpoint>/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }'
```

**Réponse:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:15",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "store_id": 1,
  "website_id": 1,
  "addresses": [],
  "disable_auto_group_change": 0
}
```

## Étape 3 : mettre à jour un client

>[!IMPORTANT]
>
> L’URL fournie dans cet exemple n’est pas valide. Utilisez votre URL de base REST. Échangez « &lt;rest_endpoint> » avec votre URL. Il ressemble à ce `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`.

Le nombre `5` dans l’exemple suivant est l’ID du client précédemment créé à l’aide de l’`"id": 5,` POST. Veillez à remplacer `5` par l’identifiant renvoyé dans votre demande.

**Endpoint:** `PUT /V1/customers/{customerId}`

```bash
curl -X PUT \
  "<rest_endpoint>/V1/customers/5" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "id": 5,
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe-Updated"
    }
  }'
```

**Réponse:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:30",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe-Updated",
  "store_id": 1,
  "website_id": 1,
  "addresses": []
}
```

## Script complet (tout-en-un)

>[!IMPORTANT]
>
> Les variables affichées dans cet exemple ne sont pas valides. Utilisez l’ID client et le secret client à partir des informations d’identification du projet. Utilisez votre URL de base REST. Échangez « &lt;rest_endpoint> » avec votre URL de point d’entrée REST à partir de experience.adobe.com. Il ressemble à ce `https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567`.

```bash
#!/bin/bash

# Configuration be sure to update these with your projects unique values
CLIENT_ID="<client_id>"
CLIENT_SECRET="<client_secret>"
REST_ENDPOINT="<rest_endpoint>"

# Step 1: Get Access Token
echo "Getting access token..."
ACCESS_TOKEN=$(curl -s -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles" | jq -r '.access_token')

echo "Token obtained: ${ACCESS_TOKEN:0:50}..."

# Step 2: Create Customer
echo ""
echo "Creating customer..."
CREATE_RESPONSE=$(curl -s -X POST \
  "${REST_ENDPOINT}/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }')

echo "Create Response:"
echo "$CREATE_RESPONSE" | jq .

# Extract customer ID
CUSTOMER_ID=$(echo "$CREATE_RESPONSE" | jq -r '.id')
echo "Customer ID: $CUSTOMER_ID"

# Step 3: Update Customer
echo ""
echo "Updating customer..."
curl -s -X PUT \
  "${REST_ENDPOINT}/V1/customers/${CUSTOMER_ID}" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d "{
    \"customer\": {
      \"id\": ${CUSTOMER_ID},
      \"email\": \"john.doe@example.com\",
      \"firstname\": \"john\",
      \"lastname\": \"Doe-Updated\"
    }
  }" | jq .
```

## Remarques importantes à propos de ce tutoriel

1. **Chemin d’accès à l’URL** : utilisez `https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` — **NOT** `https://<host>/rest/<store-view-code>/V1/customers`
1. **Authentification** : ce tutoriel a utilisé une méthode de serveur à serveur (type d’octroi `client_credentials`).
1. **Portée requise** : `commerce.accs`
1. **Expiration du jeton** : 86400 secondes (24 heures)

## Références

* [Notes De Mise À Jour D’Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/fr/docs/commerce/cloud-service/release-notes)
* [Référence de l’API REST SaaS](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [&#x200B; Guide d’authentification de l’utilisateur &#x200B;](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)
