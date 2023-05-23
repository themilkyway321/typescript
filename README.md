# Typescript

- 자바스크립트에서 다음과 같이 이상한 결과가 나올때가 있어

      - [1,2,3,4] + false 의 결과가 '1,2,3,4false' 로 문자로 되서 나와. 즉 배열에 boolean을 더했는데, 문자로 변해서 결과를 출력한다. 이것은,, 맞지 않지 
      - 함수의 인자가 잘못 들어가도 실행됨 return값이 NaN일 뿐, 에러가 나지 않음. 즉, 코드를 실행하도록 그냥 나둠. 
      - const a = { a: "A" }; a.hello(); 실행 시 에러 발생 → 실행 전에 에러 감지 불가

- 따라서, Typescript를 사용하여 타입스크립트는 프로그래밍 언어이다. 
- 하지만, 브라우저는 타입스크립트를 이해하지 못함. 자바스크립트을 이해할 수 있음. 따라서 타입 스크립트에서 자바스크립트를 변환하는 것이 필요. 
- 타입스크립트 테스트 : https://www.typescriptlang.org/play?#code/NoRgNATGDMYCwF0AEBqJAzAhgGwM4FMg


 
└ 명시적 정의(변수 선언 시 타입 정의)
let a: boolean = "x"
→ 🚫 boolean 타입에 string타입 할당 불가 알림

└ 변수만 생성(타입 추론)
let b = "hello"
→ b가 string 타입이라고 추론
b = 1
→ 🚫 string 타입에 number타입 할당 불가 알림

 => 타입 추론을 권장


 ## typescript 형태 


-  :를 이용해서 선언하거나 혹은
추론을 하도록 함
```
 let a : number []; // : 이용
 let a = [1,2,3]; // 추론 
 ```

<br>

- ?를 붙이면 optional 즉, 값이 없어도 오류 발생 안 시킴. 
 ```
 const player: {
    name:string,
    age?:number
} = {
    name: "Miri"
}

//player.age 를 해주는 이유는, 값을 존재했을 때, 비교를 해야 오류 발생 안 시킴. player.age>10만 주면, 유류 발생시킴 
if(player.age && player.age>10){
    
}
```
<br>

## typescript: Alias(별칭) 타입

type을 먼저 선언해주고, 계속해서 사용할 수 있음. 

```
type Player = {
    name: string,
    age?:number
}
const first : Player = {
    name: "nico"
}
const second : Player = {
    name: "miri"
}
```


```
type Player = {
    name: string,
    age?:number
}

function playerMaker1(name:string) : Player {
    return {
        name
    }
}
// 혹은 아래 형태로
const playerMaker2 = (name:string) : Player => ({name})
```

function playerMaker1(name:string) 괄호안에 name:string은 함수가 받는 인자의 타입을 정해주는 것. 

그렇다면 왜 : Player를 해줄까?
```
function playerMaker1(name:string) {
    return {
        name
    }
}

const nico = playerMaker1("nico"); //정상 동작 
const nico.age =12 //오류 발생!!! 

따라서 아래와 같이 Player타입이라고 해주면, const nico.age =12 해도 오류라고 인식 안함 

function playerMaker1(name:string) : Player {
    return {
        name
    }
}

```

## typescript: readonly 
readonly가 있으면 최초 선언 후 수정 불가 ⇒ immutability(불변성) 부여, 

하지만 자바스크립트에서는 readonly인식 못함 

```
type Player = {
    readonly name:string,
    age?:number
}

const playerMaker = (name:string) :Player =>({name})
const nico = playerMaker("noco");

nico.name ="" // nico의 이름을 수정하려는 순간 에러 발생시킴 
```

## typescript: Tuple

정해진 개수와 순서에 따라 배열 선언

하지만 자바스크립트에서는 Tuple인식 못함 단순히 배열로 인식 ["nico", 1, true]
```
const player: [string, number, boolean] = ["nico", 1, true]
```

 readonly도 사용가능

 ```
 const str: readonly string[] = ["1","2"];

str.push()//str을 수정하려는 순간 에러 발생시킴
```


## undefined, null, any

```
let a: undefined = undefined
let b : null = null;
```

any는 타입스트립트로부터 빠져나오는 방법 
```
const c: any[]= [1,2,3,4]
const d : any = true;
c +d // 오류 발생안시킴 
````

```
const c: []= [1,2,3,4]
const d : boolean = true;
c +d // 타입스크립트에 따라 오류 발생시킴 
````