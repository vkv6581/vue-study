# vuex

- vuex는 vue.js에서 사용하는 상태 관리 패턴이다.
- 상태 관리 패턴의 도입배경 및 구성 요소, 간단한 사용법을 정리한다.



### 상태 관리 패턴의 필요성.

#### 기존 MVC 패턴에서의 문제점

![MVC](https://github.com/namjunemy/TIL/blob/master/Vue/img/06.PNG?raw=true)

- 기능 추가, 변경에 따라 문제점을 예측하기 어려움
- 앱이 복잡해지면서 위와 같이 서로간의 업데이트 루프가 꼬여버리는 상황 발생. (view에서 데이터 변경 시 서로 참조하게 됨)

위와 같은 문제를 해결하기 위해 Flux패턴 도입 (React)

#### Flux패턴

![Flux](https://velog.velcdn.com/images%2Fsza0203%2Fpost%2F5ea469bd-1101-4a9f-bd4c-909317c96131%2F%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%85%E1%85%A9%E1%84%83%E1%85%B3.png)

- 위와 같이 **단방향**으로 데이터 처리(흐름)

```
action : 화면에서 발생하는 이벤트 또는 사용자의 입력

dispatcher : 데이터를 변경하는 방법, 메서드

store : 화면에 표시할 데이터

view : 사용자에게 비춰지는 화면
```

vue.js에선 데이터 전달을 상->하위 컴포넌트로밖에 할 수 없기 때문에 상태관리 패턴이 필요하다.

(이벤트를 통해서도 구현이 가능하지만 컴포넌트가 복잡해질 경우 추적이 어려움)

![event](https://joshua1988.github.io/vue-camp/assets/img/component-communication.2bb1d838.png)

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


### vuex를 언제 사용할까?

vuex의 store와 컴포넌트 data를 언제 사용해야 할지 가이드

![image](https://user-images.githubusercontent.com/23250734/185072400-1eb31f53-2868-48d3-b4d1-9ebd19f499ce.png)

#### vuex에서 데이터 요청(axios)를 몰아서 하는 이유?

화면에 데이터를 보여주고, 데이터의 변화 이벤트를 감지하는 vue 컴포넌트와

데이터를 호출하는 로직을 따로 분리해두기 위함.


--------------------------

### vuex 구성 요소

**1. state**

- 여러 컴포넌트간 공유할 데이터 상태.

```
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
```

기존 vue에서의 data와 동일하지만 사용하는 방법은 조금 다르다. (추후 helper들을 통해 쉽게 사용가능)



**2. getters**

- state값을 접근하는 속성. computed()처럼 값의 연산도 가능.
- java pojo클래스의 getter와 비슷

```
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
```


**3. mutations**

- state의 값을 변경하는 함수들.
- **state값의 변경은 무조건 mutation을 통해서만 이루어져야 한다.**


```
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
```

**state값을 mutations를 통해서만 변경해야 하는 이유**

아래와 같이 view화면에서 state를 변경한다면, 어디서 왜 변경했는지 추적이 어렵기 때문.

```
methodes: {
  increaseCounter() { this.$store.state.counter++; }
}
```

**4. actions**

- 비동기 처리 로직을 선언하는 메서드. 
- 데이터 요청(promise, axios) 로직들은 actions에 선언한다.

```
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
```

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

```
// App.vue
import { mapState } from 'vuex'
import { mapGetters } from 'vuex'
import { mapMutations } from 'vuex'
import { mapActions } from 'vuex'

export default {
  computed() { ...mapState(['num']), ...mapGetters(['countedNum']) },
  methods: { ...mapMutations([clickBtn]), ...mapActions(['asyncClickBtn']) }
}
```

...은 ES6 Spread Operator임. (참고)


**1. mapState**

- state를 뷰 컴포넌트에서 더 편하게 사용하게 해 주는 문법.
- computed에 연결

```
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
```

**2. mapGetters**

- vuex에 선언한 getters 속성을 뷰 컴포넌트에서 쉽게 사용.
- computed에 연결

```
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
```


**3. mapMutations**

- vuex에 선언한 Mutations 속성을 뷰 컴포넌트에서 쉽게 사용.
- methods에 연결.

```
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
```


**4. mapActions**

- vuex에 선언한 Actions속성을 뷰 컴포넌트에서 쉽게 사용.
- methods에 연결.

```
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
```

--------------------------
참고자료
- https://joshua1988.github.io/vue-camp/vuex/concept.html#%E1%84%87%E1%85%B2%E1%84%8B%E1%85%A6%E1%86%A8%E1%84%89%E1%85%B3-%E1%84%89%E1%85%A9%E1%84%80%E1%85%A2
- https://heewon26.tistory.com/181
- https://ict-nroo.tistory.com/106
