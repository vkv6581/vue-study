# vue.js study 내용 정리
## vue 강의를 들으면서 내용을 메모합니다. 기본적인 문법이 아니라 특이사항 위주로 메모.


## 뷰 특징
- 반응형 (reactivity) : 데이터 변경 시 화면에 연결된 템플릿이 자동으로 업데이트 된다. (jsp에서 매번 렌더링을 새로 해줘야 했던 것과 달리 편함)
- SPA : Single Page App : 한번에 모든 리소스를 내려받은 후 데이터에 따라 보여주는 화면만 변화함 --> 초기 로딩이 길지만 이후에 리소스 크기에 따른 로딩은 적다.

### ES6 문법 메모
- import, export : export를 통해 javascript 객체, 함수 등을 타 js파일에서 사용할 수 있게 export 가능. 타 js파일에서 import를 통해 사용 가능.
- String + String이 아닌 `문자열 ${변수명}` 로 사용 가능. 필요 시 참고.

### 기본 문법 메모
- v-html : 템플릿 안의 변수 안의 내용을 html 태그로 렌더링한다. (hustache {{{}}}과 동일)

### 뷰 권고 가이드 (쿡북)
- 모든 계산은 템플릿 밖에서 처리하고, 템플릿에는 최소한의 표현만 (변수명, computed등)
- https://v3.ko.vuejs.org/cookbook/ 쿡북 참고 : 뷰 문법을 이렇게 사용하면 좋다는 가이드라인.
- data, created, computed등 뷰 기본 속성도 권고 순서가 있음. 순서대로 나열하기.

### 라우터
- routepath/:param : 문법으로 라우터 파라미터 사용 가능 (like get parameter) this.$route.param~~ 문법으로 꺼내서 사용 가능.
- 라우터 네비게이션 : 라우터 path 이동 시 이동 전, 후로 이벤트(함수) 실행 가능. (ex. beforenter..)
  
### vuex
- FLUX 디자인패턴 (https://www.huskyhoochu.com/flux-architecture/)
- 자세한 내용 한번에 정리는 힘들지만 요약해보면
- 기존 복잡한 MVC 구조에서의 오동작을 감당할 수 없어, 전역 변수를 사용하는 것?
- 단 변수를 마음대로 수정할 수는 없고 변이(mutation)를 통해서만 수정 가능.
- state에 변수 저장, mutation을 통해 값을 수정 가능, axios등 비동기로 데이터 가져올 때는 actions사용. 왜 이렇게 구분했는지 100% 이해 못함.

### HOC (High Order Component)
- 컴포넌트 코드를 재사용하기 위한 패턴.
- 장점 : 재사용 가능
- 단점 : 사용 시 컴포넌트의 깊이가 깊어져 이벤트, 등 통신 시 단점) --> HOC로 컴포넌트 생성 시 생성한 컴포넌트 상위에 HOC가 위치하게 되서 2단계로 되어버림.

### Mixin
- HOC와 비슷한 개념.
- 컴포넌트에서 공통으로 사용되는 기능(로딩바) 등을 공통화하는 기법
- 공통화한 후 컴포넌트 생성 시 mixins로 로드. HOC와 다르게 컴포넌트 깊이가 깊어지지 않음.

### 이벤트 버스 
- 컴포넌트간 이벤트 통신하는 중간다리 역할. (아직 이해x)
- https://webruden.tistory.com/109 참고
