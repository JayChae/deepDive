2024.10.16



### 결론:

전역 상태에서 필요한 값만 select해서 가져오면 렌더링을 최적화 할 수 있다.

---



예를 들어 전역 상태가 객체 {a:1,b:2}라고 해보자.

이때 컴포넌트A가 a만을 사용하고, 컴포넌트B는 b만 사용한다.

만일 이 때 B가 b의 값을 바꾸었을 때, 컴포넌트 A는 리렌더링이 되지 말아야 한다.

전역 상태 관리 라이브러리는 이런 최적화가 되어 있다.

그래서 해당 컴포넌트에서 필요한 값만 select해서 쓰면 렌더링 최적화를 할 수 있다.

전역 상태관리 라이브러리는 기존의 상태로 새로운 상태를 만들 수 있고, select도 제공하고 있다. 이 기능을 잘 활용해 보자.

---

### 예시



```js
//jotai

const counterState = atom(0);
const biggerThan10 = atom((get)=>get(counterState) > 10);

function Temp(){
  const count = useAtomValue(biggerThan10);
}

function Temp2(){
  const [,setCount] =  useAtom(counterStae);
  ...
  //setCount 3에서 2로 변경 
}
  
```

Jotai를 예시로 들어 보자.

Atom은 전역 상태 단위를 의미한다.

counterState라는 상태로 biggerThan10이라는 상태를 만들었다. 

대신 10보다 클 경우에만 가져온다.

이 경우 biggerThan10을 사용하는 Temp 컴포넌트는 10보다 큰 경우에만 변화가 감지된다.

즉 10보다 큰 수로 바뀔 때만 렌더링이 일어난다.

아래는 나쁜 예시다.

```js
//jotai

const counterState = atom(0);
const biggerThan10 = atom((get)=>get(counterState) > 10);

function Temp(){
  const count = useAtomValue(counterStae);
  cosnt biggerThanTen = count > 10 ? count:null;
  
  ///biggerThanTen 사용하는 렌더링 있다고 하자
}
  
```

이런 경우 다른 컴포넌트에서 counterState가 10보다 작은 값으로 변경이 될 때마다, 이 컴포넌트는 무의이하게 리렌더링 될 거다.

---

다시 결론, 전역 상태 관리를 할 때 select 해서 쓰면 렌더링 최적화를 이룰 수 있다.

