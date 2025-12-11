---
title: 재시도 메커니즘의 기본 기능 사용
description: 재시도 조건 및 시각적 표시기를 포함하여 복원 응용 프로그램에 대한 Adobe I/O Events의 재시도 메커니즘을 활용합니다.
landing-page-description: Adobe I/O Events의 내장된 재시도 메커니즘을 이해하고 활용하여 애플리케이션 복원력을 향상시키고 이벤트 활성화를 효과적으로 관리할 수 있습니다.
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

# 애플리케이션 복원력을 위한 Adobe I/O Events 재시도 메커니즘 활용

이 비디오에서는 애플리케이션 복원력을 향상시키기 위한 Adobe I/O Events의 기본 제공 재시도 메커니즘을 활용하는 방법에 대한 포괄적인 안내서를 간략하게 설명합니다. 특정 HTTP 응답 상태 코드가 이벤트 재시도를 트리거하는 방법에 대해 알아봅니다. Adobe I/O Events은 재시도를 위해 1분에서 15분으로 간격이 증가하는 지수 및 고정 백오프 전략을 사용합니다. 또한 이 설명서에서는 각각 실패 및 재시도된 이벤트를 나타내는 경고 아이콘 및 원형 화살표와 같은 시각적 큐를 사용하여 개발자 콘솔에 재시도 표시기가 표시되는 방법에 대해 자세히 설명합니다.

&#39;소비자&#39; 런타임 작업의 컨텍스트 내에서 다시 시도 메커니즘이 작동하는 방식을 알아보고 이벤트가 다시 시도되는지 여부를 확인합니다. 성공적인 응답에는 200 상태 코드가 표시되고, 오류 응답에는 &#39;statusCode&#39; 특성이 있는 오류 개체가 포함됩니다. &#39;소비자&#39; 런타임 작업은 다운스트림 처리 결과를 기반으로 반환할 HTTP 응답 코드를 결정하므로 효율적인 이벤트 처리와 성공적인 활성화를 보장합니다.

## 대상자

* 이벤트 재시도를 트리거하는 특정 HTTP 응답 상태 코드를 이해하려는 개발자.
* Adobe I/O Events에서 재시도를 위해 사용하는 지수 및 고정 백오프 전략에 대해 알아보고자 하는 팀.
* 개발자 콘솔에서 시각적 표시기가 실패한 이벤트와 다시 시도된 이벤트를 나타내는 방법을 이해하려는 개발자.

## 비디오 콘텐츠

* Adobe I/O Events에는 특정 HTTP 응답 상태 코드를 기반으로 이벤트 활성화를 자동으로 다시 시도하는 기본 제공 다시 시도 메커니즘이 있습니다.
* Adobe I/O Events에서 구현한 재시도 메커니즘에는 지수 및 고정 백오프 전략이 포함됩니다.
* 실패한 이벤트에 대한 경고 아이콘 및 재시도된 이벤트에 대한 원형 화살표 아이콘과 같은 개발자 콘솔의 시각적 표시기.
* &#39;소비자&#39; 런타임 작업은 이벤트 처리에 적합한 HTTP 응답 상태 코드를 결정하는 데 중요한 역할을 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 관련 설명서

* [Webhook에서 이벤트를 처리할 수 없음](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhook을 종료하고 불안정하게 표시됨](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
