# 셀렉터

Angular 컴포넌트를 식별하기 위한 선택자는 다양한 방법으로 메타데이터에 설정할 수 있습니다. 각각의 유형을 살펴보면 범용적으로 사용되는 case 1 부터, 클래스 속성 값으로 식별하는 case 2, 마지막으로 웹 표준을 준수해 `data-*` 접두사 사용자 정의 속성을 사용하는 case 3까지 적용 가능한 방법은 다채롭습니다.

{% code-tabs %}
{% code-tabs-item title="app/custom/custom.component.ts" %}
```typescript
import { Component } from '@angular/core';

@Component({

  // case 1: <app-custom> 요소
  selector: 'app-custom',

  // case 2: <div class="app-custom"> 요소
  selecor: '.app-custom',

  // case 3: <div data-app-custom> 요소
  selector: '[data-app-custom]',

  ...

})
export class CustomComponent { }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
**NOTE.**  
권장되는 방법은 범용적인 방법이지만, 필요에 따라 프로젝트에 활용할 선택자 방식을 변경할 수 있습니다.   
물론, 팀 프로젝트 시 하나의 방법을 가이드화 하여 일관성을 유지해야 합니다.
{% endhint %}



