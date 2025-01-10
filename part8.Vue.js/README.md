# 한 번에 끝내는 프론트엔드 개발 초격차 패키지 Online. Part 8. Vue.js
직전 근무처인 글로벌A사에서 2.5버전의 vue.js는 꽤 오랜기간 사용을 해봄.  
다만 깊이있게 학습한게 아닌 기존 코드를 통한 귀납적 학습 + 그때그때 찾아보는 방식으로 개발해서 깊이가 부족했는데 이번 기회에 fastcampus 강의 및 ChatGPT를 통하여 2.5와 3의 차이점 및 작동 매커니즘을 잘 알게 되었음. (오랫동안 아리까리 하던게 한꺼번에 이해되는 쾌감! 크흐..)  

# 간단하게 테스트 하는 방법
CDN 및 Codepen을 활용한다.  
[CodePen](https://codepen.io/pen/)  
[CodePen : Hello Vue3!](https://codepen.io/foenix76/pen/MYgOeVK)

# CLI로 설치하기
```bash
# yarn으로 설치  
yarn global add @vue/cli
# npm으로 설치
npm install -g @vue/cli  
```

```bash
# yarn으로 설치했더니 성공한 것처럼 메세지 출력됨
4/4] Building fresh packages...
success Installed "@vue/cli@5.0.8" with binaries:
      - vue
Done in 31.95s.

# 그러나 실제로는 실행불가
vue info
vue : 'vue' 용어가 cmdlet, 함수, 스크립트 파일 또는 실행할 수 있는 프로그램 이름으로 인식되지 않습니다. 이름이 정확한지 확인하고 경로가 포함된 경우 경로가 올바른지 검증한 다음 다시 시도하십시오.
위치 줄:1 문자:1
+ ~~~
    + CategoryInfo          : ObjectNotFound: (vue:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

# npm으로 재시도하여 성공
# 내 경우 yarn과 node, npm을 scoop으로 설치했는데 해당 영향일지도? 학습에 중요한 건 아니니 일단 전진!

# 프로젝트 생성, vue3 + npm 설정
vue create vue3-test 
Vue CLI v5.0.8
? Please pick a preset:
> Default ([Vue 3] babel, eslint) 
  Default ([Vue 2] babel, eslint) 
  Manually select features        

Vue CLI v5.0.8
? Please pick a preset: Default ([Vue 3] babel, eslint)
? Pick the package manager to use when installing dependencies: 
  Use Yarn
> Use NPM 

.
.
📄  Generating README.md...

🎉  Successfully created project vue3-test.
👉  Get started with the following commands:

# 실행 안내
 $ cd vue3-test
 $ npm run serve

> vue3-test@0.1.0 serve
> vue-cli-service serve

# 기동 완료
 INFO  Starting development server...
 DONE  Compiled successfully in 5980ms  오후 4:06:40

  App running at:
  - Local:   http://localhost:8080/
  - Network: http://192.168.50.20:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.
```

# vscode 플러그인
vue-official (vscode에 추천 플러그인으로 뜸)

# 소스 설명
main.js에서 createApp(App).mount('#app')를 통해 index.html파일의 div id="app"에 App.vue를 연결해준다.  
또한 App.vue파일은 HelloWorld.vue을 콤포넌트로 가지고 있음.  
App.vue에서 호출 시 msg의 값으로 문자열을 줘서 해당 값의 메세지를 출력한다.  
\<HelloWorld msg="Welcome to Your Vue.js App"\/>

App.vue는 크게 3부분으로 나뉜다. template, script, style  

# Vue.createApp().mount()에서 대체 무슨일이?
사실 이런거 몰라도 그냥저냥 개발 하긴 했으나 이젠 마음껏 물어볼 AI도 있으니 그러지 않기로 했다.  
vue라이브러리를 임포트하면 Vue라는 전역 객체를 사용할 수 있다. (이건 Vue3 글로벌빌드 한정)  
Vue.createApp().mount()는 Vue전역객체에 메소드체이닝으로 메소드 2개를 실행하는 것으로 시작함  
Vue.createApp().mount("css선택자")를 넘기게 되면 해당 css선택자를 가진 엘리먼트에서 viewjs문법을 사용할 수 있게 된다.  
또한 createApp메서드에 인자로 넘기는 것은 루트 컴포넌트이고 루트 컴포넌트가 return하는 데이터에 따라 반응성(Reactivity)있게 엘리먼트에 값이 표시된다.  
(참고로 Vue.으로 객체를 불러오는건 2.x대 버전 한정임) -> 아니다! 이게 틀린 내용이었다.  이하 설명함  

Vue2던 Vue3던 간에 번들러 없이 기 개발된 웹사이트에 점진적으로 Vue를 적용할때, 또는 학습목적등으로 사용시 CDN을 통한 Vue의 글로벌 빌드 임포트 방식으로 사용하는데 이 방식은 빌드과정이 없이 즉시 사용할 수 있다는 장점이 있지만 모듈화가 어렵고, 큰 프로젝트에 적합하지 않다.  
이 글로벌 빌드 임포트 방식에서는 Vue 전역 객체를 이용하여 인스턴스를 생성할 수 있다.  
단점은 싱글 파일 컴포넌트(SFC) 구문을 사용할 수 없다는 것  

Vue 2: 전역 객체 Vue를 사용하여 바로 인스턴스를 생성 (new Vue)  
```html
<!-- Vue2 글로벌 빌드 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
<script>
  // 전역 객체 Vue를 통해 인스턴스 생성
  new Vue({
    el: '#app',
    data: {
      message: 'Hello Vue 2!'
    }
  });
</script>
```
Vue 3: 전역 객체 Vue를 사용하여 createApp을 호출한 뒤, 생성된 애플리케이션 인스턴스를 마운트.  
```html
<!-- Vue3 글로벌 빌드 -->
<script src="https://cdn.jsdelivr.net/npm/vue@3"></script>
<script>
  // 전역 객체 Vue를 통해 createApp 사용
  const app = Vue.createApp({
    data() {
      return {
        message: 'Hello Vue 3!'
      };
    }
  });

  app.mount('#app');
</script>

```
Vue.createApp으로 vue인스턴스를 생성하는 인터넷의 일부 예제 때문에 헷갈리는 내용이 있었는데 Vue3는 인스턴스 생성시 다음과 같이 Vue전역객체 없이 구조분해를 통해 createApp을 불러서 생성해야 하는걸로 알았는데 이게 Vue3라이브러리가 글로벌빌드 버전이냐 일반 버전이냐의 차이에서 기인한 것이었다.  

그러니까 뷰 인스턴스 생성시
new Vue({}) 이면 2.X 글로벌/일반, Vue.createApp이면 Vue3.x글로벌, Vue전역객체 없이 createApp이면 Vue3일반설치인 셈.  
```js
// Vue3 일반 빌드 (웹팩, vite 등의 번들러가 필요함)
import { createApp } from 'vue'

const app = createApp({
  /* 최상위 컴포넌트 옵션 */
})
```

# Vue3인스턴스 생성시 data: function(){} 을 data(){}로 줄이는게 왜 가능할까?
```javascript
Vue.createApp({
  data: function () { // data는 속성이 아니고 메서드
  }
}).mount("#app")

// 메서드에서는 :(콜론)과 function 키워드가 생략이 가능함
Vue.createApp({
  data() { // 메소드
  }
}).mount("#app")
```
ChatGPT를 비롯한 AI가 정말 좋은게 수준이 너무 낮은 질문이라 남한테 물어보기 어렵거나 구글링으로도 이해가 잘 안가는걸 물어보기에 너무 좋다는 점이다.  
이제는 초급개발자만 되도 AI만 잘 쓰면 그 어떤 것도 개발이 가능할 것 같음. 사수가 필요 없을듯? (물론 좋은 동료들이 있다면 더 좋겠지만...  ^^)  
그리고 나처럼 나이가 많은 개발자는 AI를 활용하면서 경쟁자들 보다 더욱 빠르게 신기술을 익히지 않으면 경쟁에서 도태될 것이 틀림없다. 정신 바짝 차려야함!  

각설하고, 위의 소스에서 data: function(){} 을 data(){}로 줄이는게 왜 가능한건지 질문했더니 너무 훌륭한 답변이 돌아와서 첨부해본다.  
(사실 그냥 ES5, 6의 문법차이였음)

## 자바스크립트 객체 메서드 정의 방법
자바스크립트에서는 객체 내부에 메서드를 정의하는 두 가지 방법이 있습니다. 각각의 방식과 차이점을 살펴보겠습니다.
### **1. 기존 방식 (ES5)**
```javascript
{
  data: function () { }
}
```
- 이 방식에서는 `data`라는 **키(key)**에 **익명 함수**를 값(value)으로 넣습니다.
- 즉, `data`는 **속성(property)**이며, 그 값이 함수인 형태입니다.
- 코드가 조금 더 장황하게 보일 수 있습니다.

---

### **2. 축약형 (ES6)**
```javascript
{
  data() { }
}
```
- ES6(ECMAScript 2015)에서는 **메서드 정의의 축약 문법**이 도입되었습니다.
- **function 키워드**를 생략하여 더 짧고 가독성 좋게 작성할 수 있습니다.
- 이 방식은 `data`가 **메서드**임을 명시적으로 나타내 줍니다.

---

### **왜 축약형을 써도 괜찮을까?**
1. 축약형은 **기능적으로 동일**합니다.  
   → 둘 다 객체 안에 함수를 정의하는 방법이며, 호출 방법도 동일합니다.
2. 축약형은 내부적으로 **메서드**로 처리되므로, 더 직관적이고 현대적인 문법입니다.
3. ES6의 축약형은 **프로토타입 메서드**로 정의되기 때문에 성능 측면에서도 이점이 있습니다.

---

### **예제 비교**
```javascript
const obj1 = {
  data: function () {
    console.log('ES5 방식');
  }
};

const obj2 = {
  data() {
    console.log('ES6 방식');
  }
};

// 호출 방법은 동일
obj1.data(); // ES5 방식
obj2.data(); // ES6 방식
```
    
---

### **결론**  
- 최신 문법(ES6)의 축약형 `data()`를 사용하는 것이 더 간결하고 가독성이 좋습니다.  
- 여전히 ES5 문법을 사용할 수는 있지만, 새 프로젝트에서는 **ES6 방식을 추천**합니다.  

# 2.x에서 보이던 Vue 전역 객체가 안보여서 2와 3의 Vue객체 차이에 대해 물어봤다. 
import { createApp } from 'vue'
위 임포트 구문에서 'vue'를 따라가보면 vue.d.ts라는 파일이 있는데 그 안에 Vue객체가 든게 맞지?  
(이 질문을 gpt에게 할 때까지 Vue 글로벌 빌드의 존재를 몰랐다)

위 구문에서 'vue'는 Vue 패키지를 의미합니다. 이 패키지는 node_modules/vue 디렉터리 아래에 위치하며, 해당 디렉터리 내의 타입 정의 파일인 vue.d.ts를 참조합니다.

Vue 3에서는 Vue 객체가 기본적으로 더 이상 제공되지 않습니다. 대신 Composition API와 setup 함수를 기반으로 한 사용 방식이 주로 권장됩니다. Vue 2.x에서는 Vue라는 클래스를 직접 인스턴스화하여 사용할 수 있었지만, Vue 3에서는 이러한 접근이 변경되었습니다.  

(아하.. 그래서 2.x에서만 보였던 거구만! 이라고 생각했는데 3.x에서도 글로벌빌드로 Vue를 임포트 한 경우 Vue전역객체가 있다. 글로벌 빌드인 경우 Vue.createApp으로 인스턴스를 생성함)

또한 다음과 같이 임포트 구문을 사용하면 일반 빌드에서도 글로벌빌드마냥 Vue.createApp을 할 수 있나본데 이렇게 쓰는건 비추라고 한다.  
import Vue from 'vue'

# Vue 2.x vs Vue 3.x 주요 차이점

## 1. 초기화 및 마운팅 방식
### Vue 2.x
```javascript
import Vue from 'vue';
import App from './App.vue';

new Vue({
  render: (h) => h(App),
}).$mount('#app');
```

### Vue 3.x
```javascript
import { createApp } from 'vue';
import App from './App.vue';

createApp(App).mount('#app');
```

**변경점:**  
- Vue 2에서는 `Vue` 객체를 직접 인스턴스화(`new Vue`)했지만, Vue 3에서는 `createApp` 함수를 사용하여 애플리케이션 인스턴스를 생성합니다.  
- `createApp`은 플러그인 등록과 컴포넌트 설정을 캡슐화하는 새로운 구조를 제공합니다.

---

## 2. 컴포넌트 등록 방식
### Vue 2.x
```javascript
Vue.component('MyComponent', {
  template: '<div>Hello!</div>',
});
```

### Vue 3.x
```javascript
import MyComponent from './MyComponent.vue';

const app = createApp(App);
app.component('MyComponent', MyComponent);
app.mount('#app');
```

**변경점:**  
- 전역 컴포넌트 등록이 `Vue.component`에서 `app.component`로 변경되었습니다.  
- 앱 인스턴스 수준에서 관리되므로, 모듈화와 테스트가 더 쉬워졌습니다.

---

## 3. Options API vs Composition API
### Vue 2.x - Options API
```javascript
export default {
  data() {
    return {
      message: 'Hello Vue!',
    };
  },
  methods: {
    greet() {
      console.log(this.message);
    },
  },
  mounted() {
    this.greet();
  },
};
```

### Vue 3.x - Composition API
```javascript
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const message = ref('Hello Vue!');
    const greet = () => console.log(message.value);

    onMounted(() => {
      greet();
    });

    return {
      message,
      greet,
    };
  },
};
```

**변경점:**  
- Vue 3는 **Composition API**를 도입하여 코드의 재사용성과 가독성을 크게 개선했습니다.  
- `ref`와 `reactive`를 사용하여 상태 관리가 이루어집니다.  
- 라이프사이클 훅도 함수 기반으로 관리되며, 코드의 논리적 그룹화가 훨씬 용이해졌습니다.

---

## 4. 반응형 시스템 개선
### Vue 2.x
- `Object.defineProperty`를 사용하여 반응형 시스템을 구성했습니다.  
- 배열과 객체의 속성 추가/삭제에 제한이 있었습니다.  

### Vue 3.x
- **Proxy API**를 기반으로 반응형 시스템을 구축했습니다.  
- 깊은 속성도 자동 추적할 수 있고, 성능도 개선되었습니다.  

예시:  
```javascript
const state = reactive({ count: 0 });
state.count++; // 반응형 업데이트
```

---

## 5. Teleport 및 Fragments 지원
### Teleport
다른 DOM 요소로 직접 렌더링을 지원합니다.  
```html
<teleport to="#modal-root">
  <div>모달 내용</div>
</teleport>
```

### Fragments
여러 루트 요소를 지원합니다.  
```html
<template>
  <h1>Title</h1>
  <p>Content</p>
</template>
```

---

## 6. Typescript 지원 강화
Vue 3는 타입스크립트와 완벽하게 통합되도록 설계되었습니다.  
```typescript
import { defineComponent, ref } from 'vue';

export default defineComponent({
  setup() {
    const count = ref<number>(0);
    return { count };
  },
});
```

---

## 결론
- Vue 3는 함수 기반의 Composition API를 도입해 코드의 구조와 재사용성을 크게 개선했습니다.  
- Proxy를 기반으로 한 반응형 시스템은 더 강력하고 직관적입니다.  
- TypeScript와의 호환성이 강화되었고, Teleport와 Fragment 같은 새로운 기능도 추가되었습니다.  



