## nuxt
nuxt는 react의 next.js와 같이 프레임워크의 슈퍼셋
CSR의 장점과 SSR의 장점을 합친 프레임워크

vue는 라이브러리 + 프레임워크. nuxt 역시 라이브러리지만 프레임워크의 성격이 있다.

## npm create nuxt-app 생성되는 폴더 구조
- .nuxt : 빌드 시 생성되는 디렉토리
- asset : 이미지, 웹 리소스, css..
- components : vue 컴포넌트
- layouts : 페이지의 레이아웃들..
- middleware : ssr. 서버에서 브라우저로 파일을 넘기기전에 실행시키는 함수
- pages : 페이지 라우터 자동 생성. main.vue 만들면 /main으로 가면 된다.
- plugins : vue 플러그인. 인스턴스 초기화할 라이브러리, 코드
- static : 빌드 시 변경이 필요하지않는 로고, 파비콘, robot.txt 와 같은..
- store : vuex 스토어

## 프로젝트 분석 방법

entry: layouts의 default를 통해 pages에 있는 파일들이 하나씩 들어가게된다.

```javascript
<template>
  <div>
    // header
    <header />

    // routes
    <nuxt-link to="/"/>
    <nuxt-link to="/home"/>
    <nuxt-link to="/detail"/>

    // footer
    <footer />
  </div>
</template>
```
.nuxt/route.js에서 빌드시 pages 밑에있는 파일 명대로 url이 자동작성된다.

### 레이아웃 관계

가장 바깥에 레이아웃 -> 페이지 -> 컴포넌트 순으로 뿌려진다.

default.vue -> 공통 으로 넣을 수 있음(wrapper 기능). header와 footer로 모든 페이지에 동시에 넣을 수 있음.

NuxtLink to="" 를 이용해 페이지 이동이 가능하다. (React의 Link 태그와 유사)


## async data ( SSR, SEO 개선 방법 )

next의 getserversideprops와 유사
nuxt의 api 호출 방식. 페이지에 진입하기 전에 호출되는 속성이다.

```javascript
<script>
import axios from 'axios'
import SearchInput from '~/components/SearchInput.vue'
import { fetchProductsByKeyword } from '~/api'

export default {
  components: { SearchInput },
  async asyncData() {
    const response = await axios.get('http://localhost:4000/products')
    const products = response.data
  },
}
</script>
```


## 양방향 데이터 통신

상위 -> 하위
상위에서 설정한 data를 하위 컴포넌트에 props로 넘겨준다.
하위에서 받은 이벤트를 받는다.

하위->상위
하위에선 props를 준비, value로 값을 받는다. #emit을 이용해 상위로 이벤트르 넘긴다.

내려온 데이터와 위의 데이터가 동기화가 된다.

```javascript
// 상위 컴포넌트
<template>
  <div>
    <SearchInput
      :search-keyword="searchKeyword"
      @input="updateSearchKeyword"
    ></SearchInput>
  </div>
</template>

<script>

export default {
  components: [SearchInput],

  data() {
    return {
      searchKeyword: '',
    }
  },
  methods: {
    updateSearchKeyword(keyword){
      this.searchKeyword = keyword
    }
  }
}
</script>

// 하위 컴포넌트
<template>
  <div>
    <input
      type="text"
      :value="searchKeyword"
      @input="$emit('input', $event.target.value)"
    />
    <button>search</button>
  </div>
</template>

<script>
export default {
  pros: {
    searchKeyword: {
      type: String,
      default: () => '',
    },
  },
}

```
