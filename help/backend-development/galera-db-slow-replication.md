---
title: Diagnostiquer la réplication de la base de données Galera dans les journaux de requêtes lentes MySQL
description: Découvrez comment la conception de réplication de Galera DB ralentit les synchronisations des bases de données secondaires, comment identifier ces événements dans les journaux de requêtes lentes MySQL et comment minimiser l’impact.
doc-type: Technical Video
duration: 452
last-substantial-update: 2023-07-18T00:00:00.000Z
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13635
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
TQID: 'https://experienceleague.adobe.com/NYapiIjnRv5RAS1glm8do16M4jUPmbgfVCs6ICQwbUc'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
subfeature_v2:
  - id: 3c398179-d35a-51ba-b317-6c5b95feef5e
    internal-label: Logs
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%
---
# Découvrez la réplication de la base de données Galera et les requêtes lentes MySQL associées

Les clusters Galera améliorent les performances et l’évolutivité. Lorsque vous envisagez de répliquer des bases de données, il est important de comprendre que le mode de réplication des données est différent de celui du mode principal. La base de données principale peut effectuer des opérations en bloc. Lorsque la réplication se produit pour toutes les bases de données de réplication, elles effectuent des actions une par une. Par exemple, si une suppression porte sur 67 000 000 d’éléments, sur les bases de données répliquées, chacun d’eux se produit un par un. Lors de l’examen des journaux de requêtes lentes de MySQL, vous constatez que cette action peut prendre beaucoup de temps. Le fait que les bases de données répliquées effectuent des opérations de manière séquentielle est une raison pour laquelle les éléments ne sont pas synchronisés, et des impacts sur les performances peuvent être détectés.

Pour aider les bases de données de réplication à rester synchronisées avec l’instance principale, regroupez vos opérations volumineuses lorsque cela est possible. En procédant par lots, les actions peuvent être exécutées en temps opportun et les impacts sur les performances sont réduits au minimum.

## Audience prévue

* Architectes
* Développeur
* Opérations de développement

## Contenu vidéo

* Réplication de la galerie vers la base de données répliquée
* En savoir plus sur le contrôle de flux
* Recherche de numéros de threads dans les logs de requêtes lentes mysql
* Les exécutions en bloc ne se produisent que sur l’instance principale. Les réplications se produisent 1 par
* Pour que la réplication suive le rythme de l’instance principale, effectuez par lots vos validations volumineuses.

>[!VIDEO](https://video.tv.adobe.com/v/3421688?learn=on)

## Ressources utiles

* [Cluster Galera](https://mariadb.com/products/enterprise/galera-cluster/)
