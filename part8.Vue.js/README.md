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

# 소스 설명
main.js에서 createApp(App).mount('#app')를 통해 index.html파일의 div id="app"에 App.vue를 연결해준다.  
또한 App.vue파일은 HelloWorld.vue을 콤포넌트로 가지고 있음.  
App.vue에서 호출 시 msg의 값으로 문자열을 줘서 해당 값의 메세지를 출력한다.  
\<HelloWorld msg="Welcome to Your Vue.js App"\/>

# 솔직히 처음 Vue를 불러오는 부분이 이해가 안가서 정리해봄
사실 이런거 몰라도 그냥저냥 개발 하긴 했으나 이젠 마음껏 물어볼 AI도 있으니 그러지 않기로 했다.  
vue라이브러리를 임포트하면 Vue라는 전역 객체를 사용할 수 있다.  
Vue.createApp().mount()는 Vue전역객체에 메소드체이닝으로 메소드 2개를 실행하는 것으로 시작함  
Vue.createApp().mount("css선택자")를 넘기게 되면 해당 css선택자를 가진 엘리먼트에서 viewjs문법을 사용할 수 있게 된다.  
또한 createApp메서드에 인자로 넘기는 것은 루트 컴포넌트이고 루트 컴포넌트가 return하는 데이터에 따라 반응성(Reactivity)있게 엘리먼트에 값이 표시된다.
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

# 쓸데없는 내용 추가
ChatGPT를 비롯한 AI가 정말 좋은게 수준이 너무 낮은 질문이라 남한테 물어보기 어렵거나 구글링으로도 이해가 잘 안가는걸 물어보기에 너무 좋다는 점이다.  
이제는 초급개발자만 되도 AI만 잘 쓰면 그 어떤 것도 개발이 가능할 것 같음. 사수가 필요 없을듯? (물론 좋은 동료들이 있다면 더 좋겠지만...  ^^)  
그리고 나처럼 나이가 많은 개발자는 AI를 활용하면서 빠르게 신기술을 익히지 않으면 경쟁에서 도태될 것이 틀림없다.  

위의 소스에서 data: function(){} 을 data(){}로 줄이는게 왜 가능한건지 질문했더니 너무 훌륭한 답변이 돌아와서 첨부해본다.  
(사실 그냥 ES5, 6의 문법차이였음)

## 자바스크립트 객체 메서드 정의 방법
자바스크립트에서는 객체 내부에 메서드를 정의하는 두 가지 방법이 있습니다. 각각의 방식과 차이점을 살펴보겠습니다.

---

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



# 2와 3의 Vue객체 차이에 대해 물어본 내용 (무료플랜 한계로 답변을 마크다운으로 못 옮김)
import { aaa } from 'vue'
위 임포트 구문에서 'vue'를 따라가보면 vue.d.ts라는 파일이 있는데 그 안에 Vue객체가 든게 맞지? 근데 어디서 Vue를 볼 수 있을까?

콕 찝어서 "Vue"라는 문자열이 vue.runtime.esm-bundler.js 소스 안에도 안보이던데 내가 이해할 수 있도록 좀 더 쉽게 설명 가능할까?
아니면 Vue.메소드 형식으로 어떻게 사용이 가능한지 설명해줘도 되고

Part 8. Vue.js Ch.04부터 볼 것


