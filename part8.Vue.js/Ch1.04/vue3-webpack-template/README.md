# 한 번에 끝내는 프론트엔드 개발 초격차 패키지 Online. Part 8. Vue.js
# 04. Vue3 Webpack Template
# 수업내용
웹팩 템플릿에서부터 vue3 프로젝트 구조 셋업  

# 진행
npx degit ParkYoungWoong/webpack-template-basic vue3-webpack-template
으로 최초 설치.  
npx는 설치하지 않고 그 다음에 나오는 패키지를 실행하게 해줌 (여기서는 degit : .git폴더 버전정보 없이 해당 소스를 다운로드 해주는 유틸리티)  

- js폴더 삭제
- src폴더 생성
- main.js, App.vue파일 생성
- npm i vue (강의에서는 과도기라 vue@next로 해야 3.x버전이 설치되나 2025.01 시점에서는 ^3.5.13이 설치됨)
- npm i -D vue-loader vue-style-loader @vue/compiler-sfc(-D는 개발의존성으로 설치, vue-style-loader는 vue파일 안의 스타일을 해석해서 보여주는 역할을 함)  
- webpack.config.js 설정 (확장자 연결, 플러그인 설정 등)
- npm run dev로 빌드 시 다음과 같은 오류 발생
```
node:internal/crypto/hash:79
  this[kHandle] = new _Hash(algorithm, xofLen, algorithmId, getHashCache());

Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:79:19)
    at Object.createHash (node:crypto:139:10)
```
- 에러메세지에 따라 npx browserslist@latest --update-db 조치 후 접속했으나 여전히 오류 발생
```
node:internal/crypto/hash:79
  this[kHandle] = new _Hash(algorithm, xofLen, algorithmId, getHashCache());
                  ^

Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:79:19)
    at Object.createHash (node:crypto:139:10)
    at BulkUpdateDecorator.hashFactory (C:\dev_study\training_frontend\part8.Vue.js\Ch1.04\vue3-webpack-template\node_modules\webpack\lib\util\createHash.js:144:18)
    at BulkUpdateDecorator.update (C:\dev_study\training_frontend\part8.Vue.js\Ch1.04\vue3-webpack-template\node_modules\webpack\lib\util\createHash.js:46:50)
    at RawSource.updateHash (C:\dev_study\training_frontend\part8.Vue.js\Ch1.04\vue3-webpack-template\node_modules\webpack-sources\lib\RawSource.js:64:8)
    at NormalModule._initBuildHash (C:\dev_study\training_frontend\part8.Vue.js\Ch1.04\vue3-webpack-template\node_modules\webpack\lib\NormalModule.js:829:17)
    at handleParseResult (C:\dev_study\training_frontend\part8.Vue.js\Ch1.04\vue3-webpack-template\node_modules\webpack\lib\NormalModule.js:894:10)
    at C:\dev_study\training_frontend\part8.Vue.js\Ch1.04\vue3-webpack-template\node_modules\webpack\lib\NormalModule.js:985:4
    at processResult (C:\dev_study\training_frontend\part8.Vue.js\Ch1.04\vue3-webpack-template\node_modules\webpack\lib\NormalModule.js:716:11)
    at C:\dev_study\training_frontend\part8.Vue.js\Ch1.04\vue3-webpack-template\node_modules\webpack\lib\NormalModule.js:768:5 {
  opensslErrorStack: [
    'error:03000086:digital envelope routines::initialization error',
    'error:0308010C:digital envelope routines::unsupported'
  ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
}
```
- 구글링 후 set NODE_OPTIONS=--openssl-legacy-provider 조치 후 재접속 해봤으나 증상 동일
- $env:NODE_OPTIONS = "--openssl-legacy-provider" 하고 접속하니 성공. vscode안의 터미널이 파워셸인가봄
- 근본원인은 오래된 SSL 버전에 의존하는 종속성 때문이라고 함
- 노드 환경변수를 설정하여 구openssl을 사용하도록 한 임시방편
- FM대로면 관련 패키지들을 최신 패키지로 업데이트해야할 것 같지만 학습목적이므로 일단 전진!
-- 확장자 없이 vue파일을 읽어오는 다음 설정을 해줬으나 적용이 안됬는데 재시작하니 적용됨
```javascript
module.exports = {
  resolve: {
    extensions: ['.js', '.vue']
  },
```
- npm i -D file-loader (이미지 경로 로드를 위한 의존성 설치, 경로별칭 사용 가능하게 됨)
- npm i -D eslint eslint-plugin-vue babel-eslint (포메터)  
- vue에 대한 구체적인 린팅 룰은 https://eslint.vuejs.org/ 참조
- 그러나 설치 후 .eslintrc.js 안 먹음.  
- 강의와 시간차가 있어서인지 최신 eslint9버전이 설치되어 기존 설정파일인 .eslintrc.js가 안 먹는 것 같음  
- 강사님 리포지터리에서 package.json파일 참조하여 동일 버전으로 다운 그레이드!
```json
    // 2025.01 현재 설치되는 버전
    "eslint": "^9.17.0",
    "eslint-plugin-vue": "^9.32.0",

    // 강의 시점 설치되는 버전
    "eslint": "^7.22.0",
    "eslint-plugin-vue": "^7.8.0",
```
- 세이브시 린팅 적용 설정하기 (vscode의 settings.json 수정)
- 그러나 설정이 안 먹어 아래 URL참고하여 수정
- https://code.visualstudio.com/Docs/languages/javascript#_code-actions-on-save
```json
    // 예전 설정
    "editor.codeActionsOnSave" : {
        "source.fixAll.eslint": true
    }
    // 2025 설정
    "editor.codeActionsOnSave" : {
        "source.fixAll": "explicit""
    }
```
- 세이브시 오토포메팅 설정 적용 후
```html
<!-- 저장전 -->
<img src="~assets/son_young.jpg" alt="" >

<!-- 저장후 룰에 의한 오토포메팅 적용 -->
  <img
    src="~assets/son_young.jpg"
    alt="" />
```
# Webpack 기본 템플릿

__webpack__: 모듈(패키지) 번들러의 핵심 패키지<br>
__webpack-cli__: 터미널에서 Webpack 명령(CLI)을 사용할 수 있음<br>
__webpack-dev-server__: 개발용으로 Live Server를 실행(HMR)<br>

__html-webpack-plugin__: 최초 실행될 HTML 파일(템플릿)을 연결<br>
__copy-webpack-plugin__: 정적 파일(파비콘, 이미지 등)을 제품(`dist`) 폴더로 복사<br>

__sass-loader__: SCSS(Sass) 파일을 로드<br>
__postcss-loader__: PostCSS(Autoprefixer)로 스타일 파일을 처리<br>
__css-loader__: CSS 파일을 로드<br>
__style-loader__: 로드된 스타일(CSS)을 `<style>`로 `<head>`에 삽입<br>
__babel-loader__: JS 파일을 로드<br>

__@babel/core__: ES6 이상의 코드를 ES5 이하 버전으로 변환<br>
__@babel/preset-env__: Babel 지원 스펙을 지정<br>
__@babel/plugin-transform-runtime__: Async/Await 문법 지원<br>

__sass__: SCSS(Sass) 문법을 해석(스타일 전처리기)<br>
__postcss__: Autoprefixer 등의 다양한 스타일 후처리기 패키지<br>
__autoprefixer__: 스타일에 자동으로 공급 업체 접두사(Vendor prefix)를 적용하는 PostCSS의 플러그인<br> 

## 주의사항!

- `npm i -D webpack-dev-server@next`로 설치(webpack-cli 버전(@4^)과 일치)!<br>
- `package.json` 옵션으로 `browserslist` 추가!<br>
- `.postcssrc.js` 생성(PostCSS 구성 옵션)!<br>
- `.babelrc.js` 생성(Babel 구성 옵션)!<br>

# 후기
webpack프로젝트 구조에 대한 이해 + eslint로 코드컨벤션 설정 하는 방법에 대해 잘 배웠음  
또한 eslint의 버전에 따라 설정 방법이 다르다는 것을 알게됨.  
프로젝트는 반드시 package.json뿐만 아니라 package-lock.json을 반드시 같이 푸시해야할 것으로 보임  
또한 아무래도 강의 제작 당시와 현재의 시간차이가 있어 자꾸 시간차어택을 당해 불필요한 시간소모가 큰데 30분 이상 삽질하게 되면 정말 진행이 불가능한게 아니라면 우회하여 진도부터 뺄 것!  
**지금 이순간 중요한 것이 무엇인가? 를 잊으면 안됨**

# Ch 2. Vue 문법 중 아는것 빼고 특이사항만
따로 한번 정리를 해야겠지만 실무때 왜 참조가 안되는지 궁금했던 부분에 대한 설명이 있어 뒤늦게 추가해봄

$ref : 요소가 마운트 된 이후에만 참조 가능. (그래서 created 수명주기에는 못쓰고 mounted에서는 사용가능했던 것임)

## 자식 요소에 데이터 전달시
- props로 계속 내려주는 방법
- Provide로 부모가 제공하고 자식 또는 자식의 자식에서 Inject로 참조 (건너뛰기 가능, 단 반응성을 구현하려면 부모쪽 Provide안에서 computed로 구현해줘야 함)

# Options API vs Composition API
기존방식을 Options API라고 부르며 vue3부터 Composition API 제공  

초보자 및 간단한 로직, OOP친화적, 점진적으료 vue적용시 = Options API  
좀더 유연하고 복잡성 처리에 강점, 애플리케이션 전체 빌드 및 적용 = Composition API  

# 후기
아무래도 기존 2.x의 실무경험 때문인지 Options API형태가 익숙하지만 필요에 따라 Composition API도 사용할 수 있도록 훈련해야할 것으로 보임.  



