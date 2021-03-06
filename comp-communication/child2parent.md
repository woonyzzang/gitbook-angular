# 자식 컴포넌트 ➡ 부모 컴포넌트

이번에는 자식 컴포넌트에서 부모 컴포넌트에 데이터를 전달하는 과정을 학습해보겠습니다. 자식 컴포넌트가 부모 컴포넌트에 데이터를 전달하기 위해서는 커스텀 이벤트와 이벤트 객체 방출\(Emit\)을 사용해야 합니다.

### 자식 컴포넌트 코드

자식 컴포넌트 사용자로부터 입력 받은 데이터를 부모 컴포넌트에 전달하기 위해서는 다소 복잡한 과정을 거쳐야 합니다. 먼저 [`EventEmitter`](https://angular.io/api/core/EventEmitter), [`Output`](https://angular.io/api/core/Output) 모듈을 `@angular/core`로부터 불러와야 합니다.

`EventEmitter`는 커스텀 이벤트 감지에 따른 이벤트 객체\(데이터\) 방출을 담당합니다. `Output`은 `@Output()` 데코레이터를 통해 이벤트 객체를 방출할 때 사용합니다.

{% code-tabs %}
{% code-tabs-item title="app/cockpit/cockpit.component.ts" %}
```typescript
// EventEmitter, Output 모듈 로드
import { Component, OnInput, EventEmitter, Output } from "@angular/core";

@Component({ ... })
export class CockpitComponent {

  // @output() 데코레이트를 사용해 자식 컴포넌트에서 부모 컴포넌트로 데이터 출력
  // 제너릭(Generics) 문법을 사용해 이벤트 데이터로 방출할 타입 지정
  @output() 
  addFooded = new EventEmitter<{type:string, name:string, content:string}>();

  public food_type:    string = '';
  public food_name:    string = '';
  public food_content: string = '';

  constructor(){}

  // onAddFood 메서드 실행 시,
  onAddFood():void {
    // 커스텀 이벤트 addFooded를 통해 부모 컴포넌트로 데이터 방출
    this.addFooded.emit({
      type: this.food_type,
      name: this.food_name,
      content: this.food_content
    });
  }
  onInput() {}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

자식 컴포넌트의 템플릿에는 사용자에게 입력 받을 수 있는 인풋 요소가 3개 준비되어 있고, 모두 양방향 데이터 바인딩으로 연결되어 있습니다. '식사 추가' 버튼 컴포넌트는 `(click)` 이벤트가 발생했을 때 `onAddFood()` 메서드를 실행 시킵니다.

`onAddFood()` 메서드가 실행되면 사용자로부터 입력된 데이터를 수집한 객체를 `addFooded` 커스텀 이벤트 `.emit()` 메서드를 통해 방출합니다. 이 데이터는 부모 컴포넌트에 전달됩니다.

{% code-tabs %}
{% code-tabs-item title="app/cockpit/cockpit.component.html" %}
```markup
<div class="row">
  <div class="col-xs-12">
    <p>식사 추가</p>
    <div class="form-group">
      <label for="food-name">식사 유형</label>
      <input
        type="text"
        id="food-type"
        class="form-control"
        placeholder="식사 유형 입력"
        [(ngModel)]="food_type">
    </div>
    <div class="form-group">
      <label for="food-name">식사</label>
      <input
        type="text"
        id="food-name"
        class="form-control"
        placeholder="식사 이름 입력"
        [(ngModel)]="food_name">
    </div>
    <div class="form-group">
      <label for="food-content">설명</label>
      <input
        type="text"
        id="food-content"
        class="form-control"
        placeholder="식사 설명 입력"
        [(ngModel)]="food_content">
    </div>
    <button
      type="button"
      class="btn btn-danger"
      (click)="onAddFood()">식사 추가</button>
  </div>
</div> <!-- // .row -->
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 부모 컴포넌트 코드

부모 컴포넌트 템플릿에 사용된 자식 컴포넌트 `<app-child>`에는 `(addFooded)` 커스텀 이벤트 속성이 연결되어 있습니다. 자식 컴포넌트로부터 커스텀 이벤트 방출이 감지되면, 부모 컴포넌트의 `onFoodAdded($event)` 메서드를 실행시킵니다. 메서드에 전달되는 `$event` 인자 값은 자식 컴포넌트에서 전달한 커스텀 이벤트 객체\(데이터\)입니다.

{% code-tabs %}
{% code-tabs-item title="app/parent/parent.component.html" %}
```markup
<app-child (addFooded)="onFoodAdded($event)"></app-child>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

부모 컴포넌트에 정의된 `onFoodAdded()` 메서드는 자식 컴포넌트로부터 전달 받은 이벤트 객체\(데이터\)를 `foodData` 매개변수로 받아, 자신의 `foods` 데이터를 업데이트 합니다.

{% code-tabs %}
{% code-tabs-item title="app/parent/parent.component.ts" %}
```typescript
import { Component } from "@angular/core";

@Component({ ... })
export class ParentComponent {

  public foods:object[] = [
    {
      type: '중식',
      name: '짜장면',
      content: '달콤한 짜장 소스에 면을 버무려 먹으면 참 맛있어요!'
    }
  ];

  public onFoodAdded(foodData: {type:string, name:string, content:string}):void {
    let {type, name, content} = foodData;
    this.foods.push({ type, name, content });
  }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 커스텀 이벤트 속성 별칭\(Alias\)

커스텀 이벤트 속성 이름을 줄여 사용할 별칭 속성을 자식 컴포넌트에 설정하려면 `@Output()` 데코레이터에 별칭 이름을 설정합니다.

자식 컴포넌트 코드:

{% code-tabs %}
{% code-tabs-item title="app/cockpit/cockpit.component.ts" %}
```typescript
@Component({ ... })
export class CockpitComponent {

  // 커스텀 이벤트 속성 별칭 설정
  @output('af') 
  addFooded = new EventEmitter<{type:string, name:string, content:string}>();

  ...
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

설정된 커스텀 이벤트 별칭 속성을 부모 컴포넌트 템플릿에서 사용할 수 있습니다.

부모 컴포넌트 템플릿 코드:

{% code-tabs %}
{% code-tabs-item title="app/parent/parent.component.html" %}
```markup
<!-- 커스텀 이벤트 별칭 사용 -->
<app-child (af)="onFoodAdded($event)"></app-child>
```
{% endcode-tabs-item %}
{% endcode-tabs %}



