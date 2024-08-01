---
title: Assurez la robustesse à l’aide de la fonctionnalité native d’un mécanisme de reprise.
description: Utilisation du mécanisme de reprise des événements d’Adobe I/O pour les applications résilientes, y compris les conditions de reprise et les indicateurs visuels. ​
landing-page-description: Comprendre et utiliser le mécanisme de reprise intégré des événements d’Adobe I/O pour améliorer la résilience de l’application et gérer efficacement les activations d’événement. ​
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: 11daa4b29dafe5fcecf6b75cf2269b87f65c4612
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Utilisation du mécanisme de reprise des événements d’Adobe I/O pour la résilience des applications &#x200B;

La vidéo présente un guide complet sur l’utilisation du mécanisme de reprise intégré des événements Adobe I/O pour améliorer la résilience de l’application. Découvrez comment des codes d’état de réponse HTTP spécifiques déclenchent des reprises d’événement. Les événements d’Adobe I/O utilisent des stratégies de backoff exponentiel et fixe pour les reprises, avec des intervalles passant d’une minute à 15 minutes. &#x200B; La documentation décrit également l’affichage des indicateurs de reprise dans la console de développement, avec des indices visuels tels que les icônes d’avertissement et des flèches circulaires indiquant respectivement les événements ayant échoué et repris.

Découvrez le fonctionnement du mécanisme de reprise dans le contexte des actions d’exécution &quot;consommateur&quot; et déterminez si un événement est à nouveau tenté. &#x200B;Les réponses réussies sont indiquées avec un code d’état 200, tandis que les réponses d’erreur incluent un objet d’erreur avec un attribut &#39;statusCode&#39;. L’action d’exécution &quot;consommateur&quot; détermine le code de réponse HTTP à renvoyer en fonction des résultats de traitement en aval, assurant une gestion efficace des événements et éventuellement des activations réussies. &#x200B;


## Audience

* Les développeurs qui souhaitent comprendre les codes d’état de réponse HTTP spécifiques qui déclenchent des reprises d’événement.
* Les équipes qui souhaitent en savoir plus sur les stratégies de backoff exponentiel et fixe utilisées par les événements d’Adobe I/O pour les reprises.
* Les développeurs qui souhaitent comprendre comment les indicateurs visuels dans la console de développement représentent des événements ayant échoué et ayant fait l’objet d’une nouvelle tentative.

## Contenu vidéo

* Les événements d’Adobe I/O disposent d’un mécanisme de reprise prêt à l’emploi qui tente automatiquement de relancer les activations d’événement en fonction de codes d’état de réponse HTTP spécifiques. &#x200B;
* Le mécanisme de reprise implémenté par les événements d’Adobe I/O implique des stratégies de backoff exponentiel et fixes. &#x200B;
* Indicateurs visuels dans la console de développement, tels que les icônes d’avertissement pour les événements en échec et les icônes de flèche circulaire pour les événements à nouveau testés.
* Les actions d’exécution &quot;consommateur&quot; jouent un rôle crucial pour déterminer les codes d’état de réponse HTTP appropriés pour la gestion des événements.

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Documentation connexe

* [Webhook ne pouvant pas gérer un événement](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhook down et marqué comme instable](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
