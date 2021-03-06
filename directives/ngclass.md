# \[ngClass\] 디렉티브

인라인 스타일을 동적으로 설정하는 `[ngStyle]` 디렉티브 사용법과 CSS 클래스 속성을 동적으로 설정하는 `[ngClass]` 디렉티브 사용법은 유사합니다. 사용자가 직접 인라인 스타일을 설정하는 대신 Bootstrap 버튼 컴포넌트 클래스를 설정하는 방법을 학습해봅니다.

{% code-tabs %}
{% code-tabs-item title="app/button/button.component.ts" %}
```typescript
import { Component } from "@angular/core";

@Component({ ... })
export class ButtonComponent {

  public content:string;
  public is_disabled:boolean;

  constructor() {
    this.is_disabled = Math.random() > 0.5 ? true : false;
    this.content = this.is_disabled ? '활성' : '비활성';
  }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

`[ngClass]` 속성 디렉티브 설정을 통해 DOM에 반영될 클래스 속성은 `is_disabled` 값에 따라 달라집니다. 값이 `true`일 경우는 `btn-primary` 클래스가 설정되고, 값이 `false`일 경우는 `btn-secondary` 클래스 값이 설정됩니다.

{% code-tabs %}
{% code-tabs-item title="app/button/button.component.html" %}
```markup
<button
  type="button"
  class="btn"
  [disabled]="is_disabled"
  [ngClass]="{'btn-primary': !is_disabled, 'btn-secondary': is_disabled}"
>{{ content }}</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 메서드를 사용한 동적 클래스 속성 설정

`[ngClass]` 속성 값으로 TypeScript 식을 사용하는 방법 대신, ButtonComponent 컴포넌트 클래스에 `assignClasses()` 메서드를 추가해 활용할 수도 있습니다.

{% code-tabs %}
{% code-tabs-item title="app/button/button.component.ts" %}
```typescript
...

@Component(metadata)
export class ButtonComponent{
  ...

  assignClasses():object {
    let disable = this.is_disabled;
    return {'btn-primary': !disable, 'btn-secondary': disable};
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="app/button/button.component.html" %}
```markup
<button
  type="button"
  class="btn"
  [disabled]="is_disabled"
  [ngClass]="assignClasses()"
>{{ content }}</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

