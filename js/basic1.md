### 고유 특징

```jsx
==  -> 변환 했을 시 값이 동일하다면 만족
===  -> 값과 자료형이 동일할 시 만족

// 중복 선언 불가능
const // 상수
let // 변수

// 전개연산자(...)
func(...item) //파라미터를 전개해서 넣어주면 원소 하나씩 꺼내서 넘겨준다.ㅇ
```

### 고유 문법

```jsx

// for문
for ( in ) // 요소의 인덱스
for ( of ) // 요소 자체
for (basic) // 기본 for문 

// 함수
const func = function(){} // 익명
function func(){} // 선언

 
```

### 고유 메서드

```jsx
confirm("message") // return true or false
alert("message") // undefined
```

### 자료형 변환

```jsx
Number()
String()
Boolean()

// 유연한 컨버트
1+true = 2
12 + "" = "12" 
```

### 배열

```jsx
array.push(e) // 맨 뒤 추가
array.splice(3, 0, e) // 3번째 요소에 e를 추가
array[10] = e // 중간에 비어있는 경우 건너뜀, 동적 사이즈

array.splice(3, 1) // 3번쨰 원소부터 1개 삭제
v
array.indexOf(e) // e라는 원소 인덱스 찾기, 문자열에도 사용가능
```

### 문자열

```jsx

//method
str[1] // 문자열 접근
str.length// 문자열 길이

//grammer
`templet expression is comfortable ${11+11}` // 템플릿 문자열 표현

```
