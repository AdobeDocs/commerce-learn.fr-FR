---
title: Utilisation de la fonctionnalité native d’un mécanisme de reprise
description: Tirez parti du mécanisme de reprise de Adobe I/O Events pour les applications résilientes, y compris les conditions de reprise et les indicateurs visuels.
landing-page-description: Découvrez et utilisez le mécanisme de reprise intégré de Adobe I/O Events pour améliorer la résilience des applications et gérer efficacement les activations d’événements.
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# Utilisation du mécanisme de reprise de Adobe I/O Events pour la résilience des applications

La vidéo présente un guide complet sur l’utilisation du mécanisme de reprise intégré de Adobe I/O Events pour améliorer la résilience des applications. Découvrez comment des codes d’état de réponse HTTP spécifiques déclenchent des reprises d’événement. Adobe I/O Events utilise des stratégies d’interruption exponentielles et fixes pour les reprises, avec des intervalles allant d’une minute à 15 minutes. La documentation décrit également comment les indicateurs de reprise apparaissent dans Developer Console, avec des repères visuels tels que des icônes d’avertissement et des flèches circulaires indiquant respectivement les événements ayant échoué et ceux ayant fait l’objet d’une nouvelle tentative.

Découvrez comment le mécanisme de reprise fonctionne dans le contexte des actions d’exécution « consommateur », et déterminez si un événement doit être repris. Les réponses réussies sont indiquées par un code d’état 200, tandis que les réponses d’erreur incluent un objet d’erreur avec un attribut « statusCode ». L’action d’exécution consommateur détermine le code de réponse HTTP à renvoyer en fonction des résultats du traitement en aval, ce qui garantit une gestion efficace des événements et d’éventuelles activations réussies.

## Audience

* Les développeurs qui souhaitent comprendre les codes de statut de réponse HTTP spécifiques qui déclenchent des reprises d’événement.
* Les équipes qui souhaitent en savoir plus sur les stratégies d’interruption exponentielle et fixe utilisées par Adobe I/O Events pour les reprises.
* Les développeurs et développeuses qui souhaitent comprendre comment les indicateurs visuels dans Developer Console représentent des événements ayant échoué et ayant fait l’objet d’une nouvelle tentative.

## Contenu vidéo

* Les Adobe I/O Events disposent d’un mécanisme de reprise intégré qui tente automatiquement d’activer à nouveau les événements en fonction de codes d’état de réponse HTTP spécifiques.
* Le mécanisme de reprise mis en œuvre par Adobe I/O Events implique des stratégies d’interruption exponentielles et fixes.
* Indicateurs visuels dans Developer Console, tels que les icônes d’avertissement pour les événements ayant échoué et les icônes de flèche circulaire pour les événements ayant fait l’objet d’une nouvelle tentative.
* Les actions d’exécution « consommateur » jouent un rôle essentiel dans la détermination des codes d’état de réponse HTTP appropriés pour la gestion des événements.

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Documentation connexe

* [Webhook ne peut pas gérer un événement](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhook arrêté et marqué comme instable](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
