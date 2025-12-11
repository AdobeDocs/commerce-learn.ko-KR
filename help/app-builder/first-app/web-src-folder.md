---
title: web-src 폴더
description: web-src 폴더의 파일 유형과 이 샘플 애플리케이션의 중첩된 파일 및 폴더에 대해 알아봅니다.
landing-page-description: Adobe Commerce에서 사용되는 Adobe Developer App Builder과 web-src 폴더에 있는 파일 유형에 대해 알아봅니다.
kt: 12425
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 67bbb464-1c2e-493e-9d7f-1051dfeec4ee
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# web-src 폴더의 목적 살펴보기 {#web-src-folder}

이 샘플 앱의 web-src 폴더에는 많은 JavaScript 파일과 폴더가 있습니다. 이 폴더는 사용자 인터페이스가 있는 애플리케이션에 사용됩니다. 일부 애플리케이션에서는 이 기능을 사용하지 않습니다. 예를 들어, 외부 인벤토리 관리 시스템과 Commerce 통합에는 프론트엔드 인터페이스와 코드가 필요하지 않을 수 있습니다.

## 이 비디오는 누구의 것입니까?

* Adobe App Builder을 사용하여 경험이 제한된 Adobe Commerce을 처음 사용하는 개발자로서 `web-src` 폴더 및 해당 콘텐츠에 대해 학습하고 있습니다.

## 비디오 콘텐츠

* `web-src` 폴더의 주요 용도는 무엇입니까?
* 일반적으로 포함된 파일 및 폴더
* 샘플 응용 프로그램에서 `web-src` 폴더와 내부 내용을 사용하는 방법

>[!VIDEO](https://video.tv.adobe.com/v/3421042?captions=kor&quality=12&learn=on)

## 코드 샘플

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

아래 예제에서 코드 샘플은 요청을 제한하는 `not`입니다. 400 오류를 방지하려면 `searchCriteria`을(를) 사용하여 응답 크기를 줄이십시오.

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
