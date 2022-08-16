## vuex

- vuex는 vue.js에서 사용하는 상태 관리 패턴이다.
- 상태 관리 패턴의 도입배경 및 구성 요소, 간단한 사용법을 정리한다.

--------------------------

### 상태 관리 패턴의 필요성.

기존 MVC 패턴에서의 문제점

![MVC](https://github.com/namjunemy/TIL/blob/master/Vue/img/06.PNG?raw=true)

- 기능 추가, 변경에 따라 문제점을 예측하기 어려움
- 앱이 복잡해지면서 위와 같이 서로간의 업데이트 루프가 꼬여버리는 상황 발생. (view에서 데이터 변경 시 서로 참조하게 됨)

위와 같은 문제를 해결하기 위해 Flux패턴 도입 (React)

Flux패턴

![Flux](https://velog.velcdn.com/images%2Fsza0203%2Fpost%2F5ea469bd-1101-4a9f-bd4c-909317c96131%2F%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%85%E1%85%A9%E1%84%83%E1%85%B3.png)

- 위와 같이 **단방향**으로 데이터 처리(흐름)

``
action : 화면에서 발생하는 이벤트 또는 사용자의 입력
dispatcher : 데이터를 변경하는 방법, 메서드
model : 화면에 표시할 데이터
view : 사용자에게 비춰지는 화면
``

vue.js에선 데이터 전달을 상->하위 컴포넌트로밖에 할 수 없기 때문에 상태관리 패턴이 필요하다.
(이벤트를 통해서도 구현이 가능하지만 컴포넌트가 복잡해질 경우 추적이 어려움)

--------------------------

### vuex - vue에서의 상태 관리 패턴

vue.js에서는 vuex라는 라이브러리를 사용해 상태 관리 패턴을 구현하고 있으며, 간략한 데이터 흐름 및 구성 요소는 아래와 같다.

![vuex](https://velog.velcdn.com/images%2Fsza0203%2Fpost%2Fedd36b0d-82fb-474c-8a61-a0c946d79676%2Fflow.png)

- State: 컴포넌트 간에 공유하는 데이터 data()
- View: 데이터를 표시하는 화면 template
- Action: 사용자의 입력에 따라 데이터를 변경하는 methods

#### vuex도입 시 해결할 수 있는 문제
- MVC패턴에서 발생하는 구조적 오류 (서로간의 참조가 많아지는 현상, 기능 추가에 따른 오동작방지)
- 컴포넌트 간 데이터 전달 명시 (데이터의 흐름을 단방향으로 파악할 수 있음)
- 여러 개의 컴포넌트에서 같은 데이터를 업데이트 할 때 동기화 문제 (view화면에선 전역 변수처럼 사용이 가능하기 때문에 동기화 문제 없음)


#### vuex에서 데이터 요청(axios)를 몰아서 하는 이유?

화면에 데이터를 보여주고, 데이터의 변화 이벤트를 감지하는 vue 컴포넌트와

데이터를 호출하는 로직을 따로 분리해두기 위함.


--------------------------

### vuex 구성 요소

![vuex 구성요소](data:image/png;base64,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)

**1. state**

- 여러 컴포넌트간 공유할 데이터 상태.

``
// Vue
data: {
	message: 'Hi Vue.js'
}

// Vuex
state: {
	message: 'Hi Vue.js'
}

<!-- Vue -->
<p>{{ message }}</p>

<!-- Vuex -->
<p>{{ this.$store.state.message }}</p>
``

기존 vue에서의 data와 동일하지만 사용하는 방법은 조금 다르다. (추후 helper들을 통해 쉽게 사용가능)



**2. getters**

- state값을 접근하는 속성. computed()처럼 값의 연산도 가능.
- java pojo클래스의 getter와 비슷

``
// store.js
state: {
	num: 10
},
getters: {
	getNumber(state) {
    	return state.num;
    },
    doubleNumber(state) {
    	return state.num*2;
    }
}
<p>{{ this.$store.getters.getNumber }}</p>
<p>{{ this.$store.getters.doubleNumber }}</p>
``


**3. mutations**

- state의 값을 변경하는 함수들.
- **state값의 변경은 무조건 mutation을 통해서만 이루어져야 한다.**


``
// store.js
state: { num: 20 },
mutations: {
	printNumbers(state){
    	return state.num
    },
    sumNumbers(state,anotherNum){
    	return state.num + anotherNum;
    }
}

// App.vue
this.$store.commit('printNumbers'); // 20반환
this.$store.commit('sumNumbers',30); // 50반환 
``

**state값을 mutations를 통해서만 변경해야 하는 이유**

아래와 같이 view화면에서 state를 변경한다면, 어디서 왜 변경했는지 추적이 어렵기 때문.

``
methodes: {
  increaseCounter() { this.$store.state.counter++; }
}
``

**4. actions**

- 비동기 처리 로직을 선언하는 메서드. 
- 데이터 요청(promise, axios) 로직들은 actions에 선언한다.

``
// store.js
state: {
  num: 10
},
mutations: {
  doubleNumber(state) {
    state.num *2;
  }
},
actions: {
  delayDoubleNumber(context) { // context로 store의 메서드와 속성에 접근한다
    context.commit('doubleNumber');
  }
}

// App.vue
this.$store.dispatch('delayDoubleNumber');
``

**비동기 처리 로직을 actions에 선언해야 하는 이유?**

언제 어느 컴포넌트에서 state를 호출하고 변경했는지 확인하기 위해.

시간 차를 두고 mutations를 호출하는 경우, 추적이 어렵기 때문에 동기/비동기를 분리해서 사용한다.

--------------------------

### vuex Helper

Store에 있는 아래 4가지 속성들을 간편하게 코딩하는 방법

- state -> mapState
- getters -> mapGetters
- mutations -> mapMutations
- actions -> mapActions

#### 헬퍼의 사용법
- 헬퍼를 사용하고자 하는 vue파일에서 아래와 같이 해당 헬퍼를 로딩

``
// App.vue
import { mapState } from 'vuex'
import { mapGetters } from 'vuex'
import { mapMutations } from 'vuex'
import { mapActions } from 'vuex'

export default {
  computed() { ...mapState(['num']), ...mapGetters(['countedNum']) },
  methods: { ...mapMutations([clickBtn]), ...mapActions(['asyncClickBtn']) }
}
``

...은 ES6 Spread Operator임. (참고)


**1. mapState**

- state를 뷰 컴포넌트에서 더 편하게 사용하게 해 주는 문법.
- computed에 연결

``
// App.vue
import { mapState } from 'vuex'

computed() {
  ...mapState(['num'])
  // num() { return this.$store.state.num;}
}

// store.js
state: {
  num: 10
}

<!-- <p>{{ this.$store.state.num }}</p> -->
<p>{{ this.num}}</p>
``

**2. mapGetters**

- vuex에 선언한 getters 속성을 뷰 컴포넌트에서 쉽게 사용.
- computed에 연결

``
// App.vue
import { mapGetters } from 'vuex'

computed() { ...mapGetters(['reverseMessage']) }

// store.js
getters: {
  reverseMessage(state) {
    return state.msg.split('').reverse().join('');
  }
}

<!-- <p>{{ this.$store.getters.reverseMessage }}</p> -->
<p>{{ this.reverseMessage }}</p>
``


**3. mapMutations**

- vuex에 선언한 Mutations 속성을 뷰 컴포넌트에서 쉽게 사용.
- methods에 연결.

``
// App.vue
import { mapMutations } from 'vuex'

methods: {
  ...mapMutations(['clickBtn']),
  authLogin() {},
  displayTable() {}
}

// store.js
mutations: {
  clickBtn(state) {
    alert(state.msg);
  }
}

<button @click='clickBtn'>popup message </button>
``


**4. mapActions**

- vuex에 선언한 Actions속성을 뷰 컴포넌트에서 쉽게 사용.
- methods에 연결.

``
// App.vue
import { mapActions } from 'vuex'

methods: {
  ...mapActions(['delayClickBtn']),
}

// store.js
actions: {
  delayClickBtn(context) {
    setTimeout(() => context.commit('clickBtn', 2000);
  }
}

<button @click="delayClickBtn">delay popup message</button>
``

--------------------------
참고자료
- https://joshua1988.github.io/vue-camp/vuex/concept.html#%E1%84%87%E1%85%B2%E1%84%8B%E1%85%A6%E1%86%A8%E1%84%89%E1%85%B3-%E1%84%89%E1%85%A9%E1%84%80%E1%85%A2
- https://heewon26.tistory.com/181
- https://ict-nroo.tistory.com/106