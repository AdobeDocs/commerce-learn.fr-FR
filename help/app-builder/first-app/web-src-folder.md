---
title: Le dossier web-src
description: Découvrez les types de fichiers du dossier web-src et les fichiers et dossiers imbriqués pour cet exemple d’application.
landing-page-description: Découvrez Adobe Developer App Builder utilisé avec Adobe Commerce et les types de fichiers situés dans le dossier web-src.
kt: 12425
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 67bbb464-1c2e-493e-9d7f-1051dfeec4ee
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Découvrez l’objectif du dossier web-src {#web-src-folder}

Le dossier web-src de cet exemple d’application contient de nombreux fichiers et dossiers JavaScript. Ce dossier est utilisé pour les applications disposant d’une interface utilisateur. Toutes les applications n’utilisent pas cette fonctionnalité. Par exemple, une intégration Commerce avec un système de gestion des stocks externe peut ne pas nécessiter d’interface frontale et de code.

## Pour qui est cette vidéo ?

* Les développeurs découvrent Adobe Commerce avec une expérience limitée de l’utilisation d’Adobe App Builder qui découvrent le dossier `web-src` et son contenu.

## Contenu vidéo

* Quel est l’objectif principal du dossier `web-src` ?
* Fichiers et dossiers généralement inclus
* Utilisation du dossier `web-src` et du contenu dans l’exemple d’application

>[!VIDEO](https://video.tv.adobe.com/v/3421039?quality=12&learn=on&captions=fre_fr)

## Exemples de code

web-src/src/components/Orders.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
import {
    Content,
    Heading,
    IllustratedMessage,
    TableView,
    TableHeader,
    TableBody,
    Column,
    Row,
    Cell,
    View,
    Flex,
    ProgressCircle
} from '@adobe/react-spectrum'
import {useCommerceOrders} from '../hooks/useCommerceOrders'

export const Orders = props => {

    const {isLoadingCommerceOrders, commerceOrders} = useCommerceOrders(props)

    const ordersColumns = [
        {name: 'Order Id', uid: 'increment_id'},
        {name: 'Status', uid: 'status'},
        {name: 'Store Name', uid: 'store_name'},
        {name: 'Total Item Count', uid: 'total_item_count'},
        {name: 'Total Quantity', uid: 'total_qty_ordered'},
        {name: 'Total Due', uid: 'total_due'},
        {name: 'Tax', uid: 'tax_amount'},
        {name: 'Created At', uid: 'created_at'}
    ]

    function renderEmptyState() {
        return (
            <IllustratedMessage>
                <Content>No data available</Content>
            </IllustratedMessage>
        )
    }

    return (

        <View>
            {isLoadingCommerceOrders ? (
                <Flex alignItems="center" justifyContent="center" height="100vh">
                    <ProgressCircle size="L" aria-label="Loading…" isIndeterminate/>
                </Flex>
            ) : (
                <View margin={10}>
                    <Heading level={1}>Fetched orders from Adobe Commerce</Heading>
                    <TableView
                        overflowMode="wrap"
                        aria-label="orders table"
                        flex
                        renderEmptyState={renderEmptyState}
                        height="static-size-1000"
                    >
                        <TableHeader columns={ordersColumns}>
                            {column => <Column key={column.uid}>{column.name}</Column>}
                        </TableHeader>
                        <TableBody items={commerceOrders}>
                            {order => (
                                <Row key={order['increment_id']}>{columnKey => <Cell>{order[columnKey]}</Cell>}</Row>
                            )}
                        </TableBody>
                    </TableView>
                </View>
            )}
        </View>
    )
}
```

web-src/src/hooks/useCommerceOrders.js

{{avoid-400-error}}

Dans l’exemple ci-dessous, l’exemple de code est `not` limitant la requête. Pour éviter une erreur 400, réduisez la taille de la réponse en utilisant `searchCriteria`.

`?searchCriteria[filter_groups][0][filters][0][field]=created_at&searchCriteria[filter_groups][0][filters][0][value]=2022-12-01&searchCriteria[filter_groups][0][filters][0][condition_type]=gt`

```javascript {line-numbers="true" start-line="1" highlight="25"}
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
import { useEffect, useState } from 'react'
import { callAction } from '../utils'

export const useCommerceOrders = props => {
    const [isLoadingCommerceOrders, setIsLoadingCommerceOrders] = useState(true)
    const [commerceOrders, setCommerceOrders] = useState([])

    const fetchCommerceOrders = async () => {
        const commerceOrdersResponse = await callAction(
            props,
            'commerce-rest-get',
            'orders?searchCriteria=all'
        )
        setCommerceOrders(commerceOrdersResponse.error ? [] : commerceOrdersResponse.items)
    }

    useEffect(() => {
        fetchCommerceOrders().then(() => setIsLoadingCommerceOrders(false))
    }, [])

    return { isLoadingCommerceOrders, commerceOrders }
}
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
