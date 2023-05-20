---
title: 웹-src 폴더
description: 이 샘플 애플리케이션에 대 한 웹-src 폴더의 파일 유형과 중첩 된 파일 및 폴더에 대해 알아봅니다.
landing-page-description: Adobe Systems 상거래와 함께 사용 되는 Adobe Systems 개발자 앱 빌더와 웹-src 폴더에 있는 파일 유형을 알아봅니다.
kt: 12425
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
exl-id: 67bbb464-1c2e-493e-9d7f-1051dfeec4ee
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# 웹-src 폴더의 목적 Discover {#web-src-folder}

이 샘플 앱에 대 한 웹-src 폴더에는 많은 JavaScript 파일과 폴더가 포함 되어 있습니다. 이 폴더는 사용자 인터페이스가 있는 애플리케이션에 사용 됩니다. 모든 응용 프로그램이이 기능을 사용 하지는 않습니다. 예를 들어 외부 재고 관리 시스템과의 상거래 통합에는 프런트 엔드 인터페이스 및 코드가 필요 하지 않을 수 있습니다.

## 다음에 대 한이 비디오는 무엇입니까?

* 폴더 및 해당 컨텐츠를 학습 `web-src` 하는 Adobe Systems 앱 빌더를 사용 하 여 제한 된 경험를 사용 하는 Adobe Systems 상거래를 위한 개발자.

## 비디오 컨텐츠

* 폴더의 `web-src` 주요 목적은 무엇입니까?
* 일반적으로 포함 된 파일 및 폴더
* `web-src`샘플에서 폴더와 컨텐츠를 사용 하는 방법 애플리케이션

>[!VIDEO](https://video.tv.adobe.com/v/3416665?quality=12&learn=on)

## Code 샘플

web-src/src/components/Orders js

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

web-src/src/후크/useCommerceOrders

{{avoid-400-error}}

아래 예제에서 코드 샘플 `not` 은 요청를 제한 합니다. 400 오류를 방지 하려면를 사용 `searchCriteria` 하 여 응답 크기를 줄이십시오.

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
