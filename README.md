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
```
//player.age 를 해주는 이유는, 값을 존재했을 때, 비교를 해야 오류 발생 안 시킴. player.age>10만 주면, 오류 발생시킴 (age가 optional 이므로)
```
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

함수의 return 타입을 정해주는 방법 
문자형식은 name을 인자고 받고 Player타입 형태로 return
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
// 혹은 화살표 함수 문법 형식을 쓴다면 아래와 같이 타입 지정. 
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


```
const numbers: readonly number[] = [1, 2, 3, 4]
numbers.push(1) // 오류 발생시킴. 수정할 수 없으므로 
```

## typescript: Tuple

정해진 개수와 순서에 따라 배열 선언

하지만 자바스크립트에서는 Tuple인식 못함 단순히 배열로 인식 ["nico", 1, true]
```
const player: [string, number, boolean] = ["nico", 1, true]

player[0] =1 //오류! 왜? 문자여야 하니까. 
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

## typescript unknown

```
let a:unknown

// a 가 unknown이면 아래처럼 한정지어줄 수 있어. 
if(typeof a === 'number'){
    let b = a + 1
}
if(typeof a === 'string'){
    let b = a.toUpperCase()
}

let b = a + 1 //얘는 오류 발생, a를 한정짓지 않않으니까. 
```

## typescript void
아무것도 return하지 않는 함수에서 반환 자료형
```
function hello() {
    console.log('x')
}
```
이때 function hello():void이지 

## typescript  never

값을 반환(return)하지 않는다. 

이는 함수가 예외를 throw하거나 프로그램 실행을 종료함을 의미합니다.
아래 코드는  return 하지 않고 throw 하여 오류를 발생시키는 함수 
```
function hello():never {
    throw new Error("zzz")
}
```
그렇다면 void와 never의 차이는? 
void와 never는 둘 다 아무것도 반환하지 않는 함수의 반환 유형입니다. 그러나 이들의 차이점은 다음과 같습니다.

void는 함수가 작업을 완료하고 반환할 필요가 없음을 나타냅니다. void 함수는 보통 부작용(side-effect)이 있거나, 매개변수를 변경하거나, 예외를 throw 할 수 있지만, 반환 값은 없습니다. 예를 들어, void 함수는 콘솔에 메시지를 출력하거나 파일에 쓰기 등의 일을 수행할 수 있습니다.

never는 함수가 반환하지 않는다는 것을 나타냅니다. never 함수는 보통 런타임 오류를 throw하거나, 무한루프에 빠지는 등의 예외적인 상황에서 사용됩니다. 이러한 함수는 호출자가 도달할 수 없는 코드를 포함하기 때문에 반환 유형이 never로 지정됩니다. never는 반환 값이 없는 것이 아니라, 함수가 반환되지 않는다는 것을 나타내는 반환 유형입니다.

요약하자면, void는 함수가 작업을 수행하고 반환하지만 반환 값이 없는 경우 사용되고, never는 함수가 반환되지 않는 경우 사용됩니다.


## Call Signatures

함수 반환 타입이랑 함수 인자 타입을 알려주는 역할
```
type Add = (a:number, b:number) => number;
const add:Add = (a,b) => a+b
```

add 라는 함수에 Add라고 타입을 알려줬으니까 오류없이 실행가능. 

## Overloading

Overloading은 직접 작성하기보다 외부 라이브러리에 자주 보이는 형태로, 하나의 함수가 복수의 Call Signature를 가질 때 발생한다

예를 들어, Next.js의 라우터 push가 대충 두 가지 방법으로 페이지를 이동한다고 할 때, 다음과 같은 코드를 작성하게 된다. 
```
router.push("/home");

router.push({
path: "/home",
state: 1
});
```

이때, 패키지나 라이브러리는 아래와 같이 두 가지 경우의 Overloading으로 디자인되어 있을 것이다
```

//제대로 된 오버로딩의 예시
//
type Config = {
path: string,
state: number
}

type Push = {
(config: Config): void,
(config: string): void
}

const push: Push = (config) => {
if (typeof config === "string") console.log(config);
else console.log(config.path);
}
```

```
//파라미터의 갯수가 달라도 된다
type Add ={
(a:number,b:number):number;
(a:number,b:number,c:number):number;
}
//a , b만 return 하고 싶을 때 c까지 확인하고 싶을 때 다양하게 사용하면 된다.
const add:Add=(a,b,c?:number)=>{
if(c) return a+b+c;
return a+b;
}

add(1,2)
add(1,2,3)
```

## Polymorphism

- poly란?:many, serveral, much, multi 등과 같은 뜻
- morphos란? :form, structure 등과 같은 뜻

polymorphos = poly + morphos = 여러 다른 구조

즉, 다형성이란, 여러 타입을 받아들임으로써 여러 형태를 가지는 것을 의미한다. 


```
type SuperPrint = {
    (arr: number[]):void
    (arr: boolean[]):void
    (arr: string[]):void
}
//3개의 call signature을 사용하고 있음 

const superPrint: SuperPrint = (arr) =>{
    arr.forEach(i=>console.log(i))
}

superPrint([1,2,3,4])
superPrint([true, false])
superPrint([1,2,true, false]) //에러 발생 
```
type SuperPrint에 추가로 (arr: (number|boolean)[]):void 이렇게 call signature를 추가할 수 도 있지만, 경우의 수가 너무 많음.<br>
따라서, 위과 같이 concrete type대신 Generic 사용 

concrete type
- number, boolean, void 등 지금까지 배운 타입

generic type
- 타입을 보고 유추하라고 하는 것 즉 [1,2,3,4]의 배열이 있다면 아 number라고 타입스크립트가 인식하는 것. 
<br><br>

<Generic이름>(형태):return할형태

```
type SuperPrint = {
    <TypePlaceholder>(arr: TypePlaceholder[]):void
    
}
const superPrint: SuperPrint = (arr) =>{
    arr.forEach(i=>console.log(i))
}

superPrint([1,2,3,4])
superPrint([true, false])
superPrint([1,2,true, false]) //이렇게 해도 오류 발생하지 않음  <TypePlaceholder>라는 generic 형태로 타입을 유추할 수 있기 때문. 
```

TypePlaceholder는 배열 형태를 받고, arr[0] 의 결과를 반환해야 하기 때문에 :void 대신 :TypePlaceholder 를 주었다.  
```
type SuperPrint = {
    <TypePlaceholder>(arr: TypePlaceholder[]):TypePlaceholder
    
}
const superPrint: SuperPrint = (arr) => arr[0]


const a = superPrint([1,2,3,4])
const b = superPrint([true, false])
const c =superPrint([1,2,true, false])
```


그렇다면 그냥 any를 넣는 것과 Generic의 차이는 무엇일까?
```
type SuperPrint = {
(arr: any[]): any
}

const superPrint: SuperPrint = (arr) => arr[0]

let a = superPrint([1, "b", true]);
// pass
a.toUpperCase();
```
any를 사용하면 위와 같은 경우에도 에러가 발생하지 않는다

```
type SuperPrint = {
(arr: T[]): T
}

const superPrint: SuperPrint = (arr) => arr[0]

let a = superPrint([1, "b", true]);
// error
a.toUpperCase();
```
Generic의 경우 에러가 발생해 보호받을 수 있다
* Call Signature를 concrete type으로 하나씩 추가하는 형태이기 때문!
<br><br>

복수의 Generic을 선언해 사용할 수 있다
```
type SuperPrint = {
(arr: T[], x: M): T
}

const superPrint: SuperPrint = (arr, x) => arr[0]

let a = superPrint([1, "b", true], "hi");

```

## 정리 

아래 1번코드와 2번코드는 같은 코드! 


1번코드
```
type SuperPrint = <T>(a: T[]) => T
const Player: SuperPrint = (a) => a[0]
```
2번코드
```
function Player<T>(a:T[]){
    return a[0]
}
```

```
//타입을 구체적으로 명시해주고 싶을 때는 <number>이런식으로 써줘도 된다. 하지만, 그냥 추론하도록 하는 것이 좋은 방법 Player([1,2,3,4])가 주어지고 타입스크립트가 숫자 배열임을 추론하도록 하는 것이 좋다. 

const a = Player<number>([1,2,3,4])
const a = Player([1,2,3,4]) //같은 표현
```

또한 generic type은 다음과 같이도 쓸 수 있다. 여러 형태로 활용할 수 있다.  


활용 1

Player 은 name과 extraInfo 두가지 인자를 갖는 함수인데, extraInfo가 함수의 형태일수도 있고, null 일 수도 있고 string 일 수도 있을 때 generic 타입으로 줄 수 있음. 
<br>
generic 타입으로 extraInfo가 함수형태인 코드 
```
type Player<F> = {
    name:string
    extraInfo:F
}

const nico:Player<{favFood:string}>={
    name:"nico",
    extraInfo:{
        favFood:"kimchi"
    }
}
```

<br>
generic 타입으로 extraInfo의 형태가 null 인 코드 

```
type Player<F> = {
    name:string
    extraInfo:F
}

const miri:Player<null>={
    name:"miri",
    extraInfo:null
}
```



활용 2
```
type Player<F> = {
    name:string
    extraInfo:F
}

type NicoExtra = {
    favFood:string
}

type NicoPlayer = Player<NicoExtra>

const nico:NicoPlayer={
    name:"nico",
    extraInfo:{
        favFood:"kimchi"
    }
}

```



활용 3
number[]대신 generic 타입을 쓴다면 Array<number>로 대신 쓸 수 있다. 

```
type A = Array<number> //이렇게도 쓸 수 있다. 

let a:A =[1,2,3,4];


// function printAllNumbers(arr: number[]){}
function printAllNumbers(arr: Array<number>){}

```


## classes and interfaces

타입스크립트를 활용하여 클래스를 선언하는 방식은 다음과 같다. 
단적으로 타입스크립트를 쓰면 좋은 이유는 private 을 주면 데이터에 접근을 못하도록 설정할 수 있다.


타입스크립트의 클래스(Class)
- constructor의 매개변수를 지정해주면 this.firstName = firstName 같은 자바스크립트 코드로 자동 변환해준다.
- private 키워드: 클래스 바깥에서 프로퍼티나 메서드에 접근할 수 없게 하는 키워드. 상속받은 클래스에서도 접근할 수 없다.(자바스크립트에서는 작동x)
```
class Player {
    constructor (
        private firstName:string,
        private lastName:string,
        public nickname:string
    ) {}
}

const nico = new Player("nico", "last", "니꼬")

nico.firstName //에러 발생 
```
<br>
<br>
- 추상 클래스: 다른 클래스가 상속 받을 수는 있지만 새로운 인스턴스는 만들 수 없는 클래스

```
abstract class User {
     constructor (
        private firstName:string,
        private lastName:string,
        public nickname:string
    ) {}
}


class Player extends User {
   
}

const nico = new Player("nico", "last", "니꼬")

const nico = new User("nico", "last", "니꼬") //에러 발생 abstract는 새로운 인스턴스를 생성할수 없기 때문
```
<br><br>
- 추상 메서드: 추상 클래스 안에 만들 수 있는 메서드. 추상 클래스를 상속 받는 모든 것들이 구현 해야하는 메서드를 의미한다. 메서드를 구현해서는 안되고 call signature만 작성해야한다.


```
abstract class User {
     constructor (
        private firstName:string,
        private lastName:string,
        public nickname:string
    ) {}
    abstract getNickName():void
    getFullName(){
        return `${this.firstName} ${this.lastName}`
    }
}


class Player extends User {
   getNickName(){
    console.log(this.nickname) 
    console.log(this.lastName)// private이기때문에 에러 발생 
   }
}

const nico = new Player("nico", "last", "니꼬")
nico.getFullName();
```
<br><br>
- protected 클래스: 해당 클래스와 자식 클래스에서 접근 가능
```
abstract class User {
     constructor (
        protected firstName:string,
        protected lastName:string,
        protected nickname:string
    ) {}
    abstract getNickName():void
    getFullName(){
        return `${this.firstName} ${this.lastName}`
    }
}


class Player extends User {
   getNickName(){
    console.log(this.nickname) 
    console.log(this.lastName)// protected이기때문에 에러 발생 안 시킴. 
   }
}



const nico = new Player("nico", "last", "니꼬")
nico.getFullName();
nico.firstName //protected이기때문에 에러 발생 

```

구분　　　선언한 클래스 안　상속받은 클래스 안　인스턴스<br>
private 　 　　　⭕　　　　　　　❌　　　　　      ❌<br>
protected 　　　⭕　　　　　　　⭕　　　　      　 ❌<br>
public　　　　　⭕　　　　　　　⭕　　　　　       ⭕<br>


```
type Words = {
    [key:string]:string
}
//이런식으로 타입을 지정할 수 있음. key가 올자리에는 string이고 :뒤에는 string이 온다고

// let dict :Words = {
//     "potato":"food"
// }

class Dict {
  private words: Words
  constructor(){
    this.words = {}
  }


  add(word:Word){
    if(this.words[word.term] === undefined){
        this.words[word.term] = word.def
    }
  }
  def(term:string){
    return this.words[term]
  }
}

class Word {
    constructor(
        public term:string,
        public def:string
    ) {}
}
const kimchi = new Word("kimchi","한국의 음식")

const dict = new Dict()
dict.add(kimchi)
dict.def("kimchi")
```



```
type Words = {
    [key:string]:string
}
//이런식으로 타입을 지정할 수 있음. key가 올자리에는 string이고 :뒤에는 string이 온다고

// let dict :Words = {
//     "potato":"food"
// }

class Dict {
    //private 클래스를 주고, 이를 constructor안에 설정해줘야해. 
  private words: Words
  constructor(){
    this.words = {}
  }

    // add가 받는 인자는 Word 타입이다. class를 타입처럼 사용할 수 있다. 
  add(word:Word){
    if(this.words[word.term] === undefined){
        this.words[word.term] = word.def
    }
  }
  
  def(term:string){
    return this.words[term]
  }
}

class Word {
    constructor(
        public term:string,
        public def:string
    ) {}
}
const kimchi = new Word("kimchi","한국의 음식")

const dict = new Dict()
dict.add(kimchi)
console.log(kimchi)

```

## 타입의 여러가지 형태

```
//타입을 여러가지 방면으로 자유롭게 변형할 수 있어

//첫번째 유형 

// type Player = {
//     nickname:string,
//     healthBar:number
// }
// const Miri: Player = {
//     nickname:"Milky",
//     healthBar:10
// }

// 두번째 유형

type Nickname = string
type Health = number
type Player = {
    nickname:Nickname,
    healthBar:Health
}

const Miri: Player = {
    nickname:"Milky",
    healthBar:10
}

// 세번째 유형 

type Food = string;
const kimchi:Food = "delicicious"

type Friends = Array<string>



//네번째 유형 

type Team = "read" | "blue" | "yellow"
type Player= {
    nickname:string, 
    team:Team
}
const miri:Player = {
    nickname:"Milky",
    team:"blue",
    <!-- team:"pink" 에러! -->
}
```

## interface

interface는 오로지 객체의 형태를 타입스크립트에게 설명해주기 위한 용도로만 사용된다. 타입이랑 비슷하지만, 인터페이스는 객체형태로만 쓸 수 있다는 점이 차이점. 

```
interface Player {
    nickname:string, 
    team:Team
}
```


1번코드
```
interface User {
    name:string
}

interface Player extends User {

}
const miri : Player = {
    name:"miri"
}
```


1번 코드를 type형태로 쓴다면
```
type User = {
    name:string
}

type Player = User & {

}
const miri : Player = {
    name:"miri"
}

```

인터페이스의 또 다른 특징으로는 속성(Property)들을 ‘축적’시킬 수 있다는 것이다. type형태는 이렇게 축적 못함!

```
interface User {
    name:string
}

interface User {
    lastName:string
}
const miri : User = {
    name:"miri",
    lastName:"Kim"
}

```
<h4>그렇다면 왜 interface를 쓰는게 유용한 것일까? </h4>
https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces
아래 코드를 비교해보자. 
<br>

abstract를 사용하여 타입을 명시한 코드 
abstract를 사용하여 어떤 property와 메서드를 가질지 청사진을 제시해줄 수 있어. 실제로 자바스크립트에는 abstract 대신 그냥 class User로만 나타나지. 
하지만 우리는 알아, User로 인스턴스로 만들 수 없어. Player로만 인스턴스를 만들 수있지 (abstract의 특징.) 
결과적으로 자바스크립트에는 class User와 class Player의 코드가 존재하지만 Player만 가지고 인스턴스들을 만들게 된다. 
```
abstract class User {
    constructor(
        protected firstName:string,
        protected lastName:string
    ){}
    abstract sayHi(name:string):string
    abstract fullName():string
}

class Player extends User {
    fullName(){
        return `${this.firstName} ${this.lastName}` 
    }
    sayHi(name:string){
        return `Hello ${name}. My name is ${this.fullName}`
    }
}
```
<br>
interface를 사용하여 타입을 명시한 코드 

:interface를 자바스크립트로 컴파일하지 않기때문에, 훨씬 코드가 가벼워지는 장점이 있다. 

```
interface User {
    firstName:string,
    lastName:string,
    sayHi(name:string):string
    fullName():string
}
interface Human {
    health:number
}
class Player implements User, Human {
   constructor(
        public firstName:string,
        public lastName:string,
        public health:number
   ){}
    fullName(){
        return `${this.firstName} ${this.lastName}` 
    }
    sayHi(name:string){
        return `Hello ${name}. My name is ${this.fullName}`
    }
}
```

인터페이스를 상속(implements )하는 것의 문제점 중 하나는 private혹은 protected 프로퍼티들을 사용하지 못한 다는 것 
<br><br>

이제까지 배운 것을 토대로 아래 예시를 살펴보자 

로컬스토리지 API 예시

```
interface SStorage<T> {
    [key:string]: T
}

class LocalStroage<T> {
    private storage: SStorage<T> ={}
    set(key:string, value:T){
        this.storage[key] = value;
    }
    remove(key:string){
      delete this.storage[key]
      }
    get(key:string):T{
        return this.storage[key]
    }
    clear(){
        this.storage = {}
    }
 }

 const stringsStroage = new LocalStroage<string>()

//miri를 보내주고 결과로 string으로 받는다. 제네릭 형태로 get(key:string):T 이렇게 썼으므로. 
 stringsStroage.get("miri")

 const booleanStorage = new LocalStroage<boolean>()

//xxx를 보내주고 결과로 boolean으로 받는다. 제네릭 형태로 get(key:string):T 이렇게 썼으므로. 
 booleanStorage.get("xxx")
 ```


## typescript설치
npm i -D typescript

package.json 초기화
npm init -y

## tsconfig.json설정
디렉터리에 tsconfig.json 파일이 있으면 해당 디렉터리가 TypeScript 프로젝트의 루트임을 나타냅니다. tsconfig.json 파일은 프로젝트를 컴파일하는 데 필요한 루트 파일과 컴파일러 옵션을 지정합니다.
https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#handbook-content

- Target (기본값: ES3)
최신 브라우저는 모든 ES6 기능을 지원하므로 ES6는 좋은 선택입니다. 코드가 이전 환경에 배포된 경우 더 낮은 target을 설정하거나 최신 환경에서 코드 실행이 보장되는 경우 더 높은 target을 설정하도록 선택할 수 있습니다.
ex) 화살표 함수() => this는 ES5 이하이면 함수 표현식으로 바뀝니다.

특별한 ESNext 값은 TypeScript 버전이 지원하는 가장 높은 버전을 나타냅니다. 이 설정은 다른 TypeScript 버전 간에 동일한 의미가 아니며 업그레이드를 예측하기 어렵게 만들 수 있으므로 주의해서 사용해야 합니다.
https://www.typescriptlang.org/tsconfig#target

"build": "tsc" 또는 "npx tsc"



- lib

타입스크립트에게 어떤 API를 사용하고 어떤 환경에서 코드를 실행하는 지를 지정할 수 있습니다. 예를 들어 ts파일에 localStorage.getItem()를 쓰면 타입스트립트가 알아서 추측할 수 있어. getItem(key: string): string | null 이렇게 

Math.ceil()를 쓰면 ceil(x: number): number 이렇게 알려줘. (이게 가능한 이유는 lib.d.ts파일에 누군가가 이미 해당 함수에 대해 무엇을 return할지 타입스크립트로 써두었기 때문이지) 

(target 런타임 환경이 무엇인지를 지정합니다.)
프로그램이 브라우저에서 실행되면 lib에 "DOM" 유형 정의를 할 수 있습니다.
DOM: window, document 등
ex) "lib": ["ES6","DOM"]

https://www.typescriptlang.org/tsconfig#lib





- strict:true 

모든 엄격한 타입 검사 옵션을 활성화합니다.
strict 플래그는 프로그램 정확성을 더 강력하게 보장하는 광범위한 타입 검사 동작을 가능하게 합니다.

https://www.typescriptlang.org/tsconfig#strict


- .js파일에 다음과 같이 타입을 설명할 수도 있어. 
```
/**
 * 
 * @param {object} config 
 * @param {boolean} config.debug 
 * @param {string} config.url 
 * @returns {boolean}
 */
export function init(config){
  return true;
}
/**
 * 
 * @param {number} code 
 * @returns {number}
 */
export function exit(code){
  return code +1;
}
```



- DefinitelyTyped
TypeScript type 정의를 위한 리포지토리입니다.
https://github.com/DefinitelyTyped/DefinitelyTyped
