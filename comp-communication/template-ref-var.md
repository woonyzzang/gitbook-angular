# 템플릿 참조 변수

템플릿 내에서 다른 요소에 접근하기 위한 방법을 Angular는 제공합니다. 요소를 참조 하려면 `#` 기호 뒤에 고유한 식별자 이름을 사용하여 참조 변수를 설정합니다.

예를 들어 사용자가 입력하는 값을 템플릿 내에서 받아 사용하려면 인풋 요소에 템플릿 레퍼런스 변수를 설정한 후, 버튼 요소에서 이를 컴포넌트 메서드의 인자로 전달할 수 있습니다.

{% code-tabs %}
{% code-tabs-item title="Template HTML" %}
```markup
<div class="form-group">
  <label for="food-name">식사</label>
  <input
    type="text"
    id="food-name"
    class="form-control"
    placeholder="식사 이름 입력"
    #foodName> <!-- #템플릿 참조 변수 설정 -->
</div>

...

<button
  type="button"
  class="btn btn-danger"
  (click)="onAddFood(foodName.value)">식사 추가</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}



