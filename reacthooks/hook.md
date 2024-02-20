# React의 Hook

## 학습 키워드

- React Hook 이란
  - HOC 고차 컴포넌트

<br/>

## React Hook

- `Hook`은 리액트 v16.8에 새로 도입된 기능이다.  
- 클래스형 컴포넌트 고유 기능인 상태(state)관리와 라이프사이클 관리 기능을 함수형 컴포넌트에서도 쓸 수 있도록 해주는 함수들을 총칭한다.

### 🌎 React Hook의 등장 배경

> 리액트 컴포넌트는 클래스형 컴포넌트(Class component)와 함수형 컴포넌트(Functional component)로 나뉜다. 기존의 개발방식은 일반적으로 함수형 컴포넌트를 주로 사용하되 state이나 Life Cycle Method를 사용해야 할 때에만 클래스형 컴포넌트를 사용하는 방식이었다.

위와 같은 방식으로 사용하니 몇가지 문제점들이 있었다.

#### 🚨 Wrapper Hell (상태로직의 재사용성 어려움)

리액트를 쓰다 보면 자주 쓰는 로직을 따로 빼내서 사용할 필요가 있는데 HOC(High order component)를 이용해서 문제를 해결했다.

> 📖 HOC(High order component) 고차 컴포넌트 <br/>
화면에서 재사용 가능한 로직만을 분리해서 component로 만들고, 재사용 불가능한 UI와 같은 다른 부분은 parameter로 받아서 처리하도록 하는 패턴
⇒ 리액트 컴포넌트를 인자로 받아서 새로운 리액트 컴포넌트를 리턴하는 함수

하지만 HOC을 사용했을 때, wrapper hell이라는 또 다른 문제가 발생 했다.
"wrapper hell"이란 중첩되는 component가 많아져 depth가 깊어지는 지게 되어 코드 추적을 어렵게 만들고 컴포넌트의 재구성을 강요하게 했다.

#### HOC 고차 컴포넌트를 사용한 코드

SomeComponent가 마우스 포지션, window 크기, 유저 위치 등의 정보가 필요하다면 아래와 같이 HOC 구조가 발생한다. 이런 구조는 가독성도 좋지않고 element를 찾기 힘들어진다.

```jsx
const HocHell = () => {
  return (
    <Hoc1>
      <WithMousePosition>
        <WithWindowSize>
          <WithUserLocation>
            <SomeComponent />
          </WithUserLocation>
        </WithWindowSize>
      </WithMousePosition>
    </Hoc1>
  );
};
```

#### Custom Hook을 사용한 코드

Custom Hook을 통해 모듈화하여 SomeComponent 전달하기 때문에 계층 구조 가 단순해지며 재사용이 가능해진다.

```jsx
const WithHook = () => {
  const mousePosition = useMousePosition()
  const windowSizes = useWindowSize()
  const userLocation = useUserLocation()
  
  return (
    <SomeComponent
      mousePosition={mousePosition}
      windowSizes={windowSizes}
      userLocation={useLocation}
    />
  )
}
```

> __✅  Hook은 계층의 변화 없이 상태 관련 로직을 재사용할 수 있도록 도와준다.__

#### 🚨 Huge Components (이해하기 어려운 복잡한 컴포넌트)

> 🤔 만약 홈 컴포넌트는 컴포넌트가 마운트 됐을 때 그리고 pathName prop이 변경됐을 때 lookups 데이터를 api에서 받아와야 한다고 했을 때?

#### Class형 컴포넌트의 LifeCycle 메서드를 사용한 코드

componentDidMount, componentDidUpdate의 LifeCycle API를 이용하여 작업을 처리 하게 되어 함수는 단일 책임 원칙을 벗어나게 되고, 코드는 복잡해지며, 테스트는 점차 어려워진다.

```jsx
class Home extends React.Component {

  constructor(props) {
    super(props);
    this.state = {
      lookups: null
    };
    // this를 사용하기 위해서 아래와 같이 바인드
    this.fetchLookups = this.fetchLookups.bind(this);
  }

  componentDidMount() {
    this.fetchLookups(this.props.pathName);
  }

  componentDidUpdate(prevProps) {
    // pathName prop이 변경 되었을 때
    if (prevProps.pathName !== this.props.pathName) {
      this.fetchLookups(this.props.pathName);
    }
  }

  fetchLookups(pathName) {
    fetchApi({ url: `/lookup?url=${pathName}` }).then((lookups) => {
     // lookups 데이터가 반환 되면 state업데이트
      this.setState({
        lookups
      });
    });
  }

  render() {
    if (!this.state.lookups) return null;
    return <pre>{JSON.stringify(this.state.lookups)}</pre>;
  }

}
```

#### 함수형 컴포넌트의 LifeCycle 관리하는 Hook을 사용한 코드

useState, useEffect 의 Hook을 사용하면 코드는 간결해지고 반복적이고 불필요한 코드들이 제거 되었다.

```jsx
const HomeWithHook = ({ pathName }) => {
  const [lookups, updateLookups] = useState(null);
  useEffect(() => {
    if (pathName) {
      fetchApi({ url: `/lookup?url=${pathName}` }).then((lookups) => {
        updateLookups(lookups);
      });
    }
  // dependency를 pathName으로 해주면 맨 처음에 한번 pathName이 변경 되었을 때 또 실행
  }, [pathName]);
  if (!lookups) return null;
  return <pre>{JSON.stringify(lookups)}</pre>;
};
```

> __✅ Hook을 통해 서로 비슷한 것을 하는 작은 함수의 묶음으로 컴포넌트를 나누는 방법으로 사용 할 수 있다.__

#### 🚨 Confusing Classes (사람과 기계를 혼동시키는 Class)

React 에서의 Class 사용을 위해서는 JavaScript의 this 키워드가 어떻게 작동하는지 한다. JavaScript의 this키워드는 대부분의 다른 언어에서와는 다르게 작동함으로 혼란을 유발하고 코드의 재사용성과 구성을 어렵게 만든다.

> __✅ Hook은 Class없이 React 기능들을 사용하는 방법을 제시한다.__

<br/>

### ✍🏻 React Hook 정리

- React hook 16.8 버전 이후 도입된 기능이다.
- 기존에 React가 가지고 있던 문제점들을 해결하기 위해 hook이 탄생하였다.
  - Class 컴포넌트의 문제 (this 키워드, extends 키워드를 통한 상속으로 반복적인 코드 작성)
  - 상태 관리 로직(컴포넌트)의 재사용 어려움 (컴포넌트의 모듈화)
  - 컴포넌트의 LifeCycle(생명주기)와 관련된 메서드로 인한 복잡한 컴포넌트
- Hook을 통해 코드가 간결하고 가독성 있게 하며, 재사용의 효율을 극대화 시키기 위한 기능

<br/>

### 🔗 참고

- [React 공식문서](https://ko.legacy.reactjs.org/docs/hooks-intro.html#motivation)
- [React Hook의 등장 배경](https://doqtqu.tistory.com/340)
- [React-hook 이 나온 이유와 사용 해야 하는 이유](https://surviveasdev.tistory.com/entry/React-hook%EC%9D%B4-%EB%82%98%EC%98%A8-%EC%9D%B4%EC%9C%A0%EC%99%80-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
