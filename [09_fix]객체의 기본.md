# `[7] 객체의 기본`

<br><br><br><br><br>

## 1. 객체 생성과 사용

자바스크립트 세상에서는 거의 모든 것들이 객체이다. null 과 undefined 를 제외한 모든 원시 타입도 객체로 취급된다. 

### - 객체의 주요 키워드
 - 속성(Property)
 - 메소드(Method)
 - 생성자(constructor)

<br><br><br>

### - 객체 생성

#### * 기본 객체(Object)의 생성자 함수 이용
```js
var person = new Object();
person.name = "jackson";
person.age = 10;
console.log(person);
```

```js
function createPerson(name, age){
  var obj = new Object();
  obj.name = name;
  obj.age = age;
  return obj;
}

var person = createPerson("jackson", 10);
console.log(person); //{name: "jackson", age: 10}
```

#### * 객체 리터럴 이용
 - 방식 1
```js
var person = {
    name : "jackson",
    age : 10
}
console.log(person);
```

 - 방식 2
```js
var name = "jackson";
var age = 10;

var person = {
    name : name,
    age : age
}
console.log(person); //{name: "jackson", age: 10}
```

 - 방식 3
```js
var obj = {};
const key = "attr";
obj[key+"Key"] = "VALUE";
console.log(obj); //{attrKey: "VALUE"}
```

 - 방식 4
```js
var obj = {};
const key = "attr";
obj = {[key+"Key"] : "VALUE"};
console.log(obj); //{attrKey: "VALUE"}
```

#### * 생성자 함수 이용

```js
function Person(name, age){
    var _country = "Korea";
    this.name = name;
    this.age = age;
}
var someone = new Person("jackson", 10);
console.log(someone);
```
생성자 함수를 사용하면 마치 객체를 생성하기 위한 템플릿(클래스)처럼 사용하여 프로퍼티가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

 - 생성자 함수 이름은 일반적으로 대문자로 시작한다. 이것은 생성자 함수임을 인식하도록 도움을 준다.
 - 프로퍼티 또는 메소드명 앞에 기술한 this는 생성자 함수가 생성할 인스턴스(instance)를 가리킨다.
 - this에 연결(바인딩)되어 있는 프로퍼티와 메소드는 public(외부에서 참조 가능)하다.
 - 생성자 함수 내에서 선언된 일반 변수는 private(외부에서 참조 불가능)하다. 즉, 생성자 함수 내부에서는 자유롭게 접근이 가능하나 외부에서 접근할 수 없다.


#### * new의 원리

```js
var a1 = new A();
```
위 코드는 아래와 같은 과정을 거치게 됩니다.
```js
var a1 = new Object();
a1.__proto__ = A.prototype;
A.call(a1);
```
아래 샘플 코드의 a1과 a2는 같습니다.
```js
function A(){
	this.sayHello = function(){
  	console.log("Hello World!");
  }
}

var a1 = new Object();
a1.__proto__ = A.prototype;
A.call(a1);
console.dir(a1);
a1.sayHello();

var a2 = new A();
console.dir(a2);
a2.sayHello();
```

<br><br><br>

### - 객체 프로퍼티 접근 방식

```js
//--방식1
person.age
person.name.first

//--방식2
person['age']
person['name']['first']
```

<br><br><br>

### - 객체 매개변수와 반환

```js
function tester({name, age}){
	const obj = {
  	name:name,
    age:age
  }		
  return obj;
}

const result = tester(
	{
  	name : "John",
    age: 29
  }
)

console.log(result); // {name: "John", age: 29}
``` 

#### * 확장연산자 활용

복기를 위해 확장연산자는 다음과 같았습니다.

```js
var input = [1, 2, 3]
var arr = [0, 0, ...input, 0, 0, 0];
console.log(arr); //[0, 0, 1, 2, 3, 0, 0, 0]
```

위 원리를 이용해여 composition 방식을 구현할 수 있습니다.

```js
function aboutHuman(){
	return {
		isLimited : false,
    characteristic : "creative"
  }
}

function speakable(){
	return {
		speak : function(){
			console.log("Blah Blah~");
		}
  }
}

function movable(){
	return {
  	move : function(){
    	console.log("Boom, bop!")
    }
  }
}


function newPerson({name, age}){
	const obj = {
  	name:name,
    age:age
  }
  return {
  	...obj, // diassemble object
    ...aboutHuman(),
    ...speakable(),
    ...movable()
  }; //and rebuild again
}

const someone = newPerson(
	{
  	name : "John",
    age: 29
  }
)


console.log(someone); 
/*
{
	name: "John", 
  age: 29, 
  isLimited: false, 
  characteristic: "creative", 
  speak : function(){
		console.log("Blah Blah~");
	},
  move : function(){
   	console.log("Boom, bop!")
  }
}
*/

someone.move(); //Boom, bop!
```

확장연산자를 활용하여 객체를 보다 쉽게 할당할 수 도 있습니다.

```js
const obj = {
  key1: "one",
  key2: "two",
};

const { key1, key2, key3 = "three" } = obj;

//객체의 키 값을 변수에 할당했으며, key3같은 경우, (=)를 통해 기본값을 할당.
console.log(key1, key2, key3); //one two three
```

<br><br><br><br><br>

## 2. 프로토타입(prototype) 형식

```js
function Person(name, age){
    var _country = "Korea";
    this.name = name;
    this.age = age;
}
var someone = new Person("jackson", 10);
someone.valueOf();
```
우리는 Person이라는 객체에 name과 age라는 속성만을 주었습니다. 그런데 valueOf()라는 메소드는 어디서 나온것일까요?

valueOf() 메소드는 Object의 메소드입니다. 그러면 어떻게 person이라는 인스턴스가 Object의 메소드를 사용할 수 있게 된 것일까요? 그것은 바로 `프로토타입 체인` 덕분입니다.

someone을 보면 `__proto__`라는 것이 보일 것입니다. 그리고 아래와 같이 구성되죠
 - `constructor` : Person
 - `__proto__`

또 프로토타입이라는게 있는게 보이실 겁니다. 그리고 이 `__proto__`변수는 Object를 가리키고 있습니다.

JavaScript에서는 객체 인스턴스와 프로토타입 간에 연결(많은 브라우저들이 생성자의 prototype 속성에서 파생된 `__proto__` 속성으로 객체 인스턴스에 구현하고 있습니다.)이 구성되며 이 연결을 따라 프로토타입 체인을 타고 올라가며 속성과 메소드를 탐색합니다.

<br><br><br>

### - 프로토타입안에 있는 것과 없는 것의 차이
프로토타입에 정의된다는 것은 `상속받은 멤버들이 정의된 곳`입니다. 즉, `prototype에 정의되지 않은 멤버들은 상속되지 않습니다`. 일단 Object의 prototype을 확인해보세요
```js
Object.prototype;
```
`Object.is()`라는 함수가 보이시나요? 이 함수도 Object의 함수인데 보이질 않습니다. 바로 prototype에 정의되지 않은 함수입니다. 그러므로, 프로토타입 체인과 연결되어 있는 Person의 인스턴스는 이 함수가 없다고 생각할 것입니다.
```js
person.is(); //ReferenceError
```

<br><br><br>

### - `prototype`과 `__proto__`

```js
function Person(){
  this.name = name;
}
```
`prototype` 속성은 상속 시키려는 멤버들이 정의된 객체입니다.

프로토타입 객체는 `__proto__` 속성으로 접근 가능한 내장 객체입니다.

함수도 객체입니다. 그러므로 속성과 메서드를 가질 수 있습니다.

위의 Person 객체를 만드는 Factory Function은 `prototype`이라는 속성을 가지고 있습니다.

그리고 이 `prototype`은 construsctor를 통해 Person 함수를 가리킵니다.

둘이 순환 참조를 하고 있는 것이죠.

만일 Person 함수를 통해 인스턴스를 생성하게 된다면 `__proto__`라는 속성이 생기게 되고 이 속성은 Person의 `Prototype`을 가리키게 됩니다.

<br><br><br>

### - 프로토타입 활용하기
같은 생성자 함수로부터 생겨난 인스턴스들을 프로토타입으로 다루기
```js
function Person(name, age){
    var _country = "Korea";
    this.name = name;
    this.age = age;
}

var person1 = new Person("Maria", 20);
var person2 = new Person("Sindra", 23);

Person.prototype.sayHi = function(){
    console.log("hi~");
}

//두 인스턴스가 프로토타입을 가리킬 때 sayHi()가 생겼음을 알 수 있음
console.log(person1.__proto__)
console.log(person2.__proto__)
person1.sayHi();
person2.sayHi();
```

this를 활용해 좀더 유연하게 만들어보겠습니다. global을 가리킬 것 같지만 호출한 곳을 가리키고 이 메소드를 호출한다면 Person 객체의 인스턴스가 될 것입니다.
```js
function Person(name, age){
    var _country = "Korea";
    this.name = name;
    this.age = age;
}

var person1 = new Person("Maria", 20);
var person2 = new Person("Sindra", 23);

Person.prototype.sayHi = function(){
    console.log("hi~ " + this.name);
}

person1.sayHi();
person2.sayHi();
```

생성자 함수를 정의할 때 한번에 정의한 것과 나중에 prototype으로 정의할 때의 차이는 constructor에 포함이 되느냐 안되느냐의 차이입니다. 메소드는 prototype으로 따로 정의하는게 코드 가독성을 높이는 길입니다.
```js
function PersonA(name, age){
    var _country = "Korea";
    this.name = name;
    this.age = age;
}

PersonA.prototype.sayHi = function(){
    console.log("hi~");
}

var person1 = new PersonA("Maria", 20);
console.log(person1.__proto__);

function PersonB(name, age){
    var _country = "Korea";
    this.name = name;
    this.age = age;
    this.sayHi = function(){
        console.log("hi~");
    }
}

var person2 = new PersonB("Sindra", 23);
console.log(person2.__proto__);
```

<br><br><br>

### - Getter와 Setter

```js
var o = {
  a: 7,
  get b() {
    return this.a + 1;
  },
  set c(x) {
    this.a = x / 2;
  }
};

console.log(o.a); // 7
console.log(o.b); // 8 <-- At this point the get b() method is initiated.
o.c = 50;         //   <-- At this point the set c(x) method is initiated
console.log(o.a); // 25
```
<br><br><br>

### - Delete Property

```js
// Creates a new object, myobj, with two properties, a and b.
var myobj = new Object;
myobj.a = 5;
myobj.b = 12;

// Removes the a property, leaving myobj with only the b property.
delete myobj.a;
console.log ('a' in myobj); // output: "false"
```

<br><br><br>

### - 효율적인 사용 : getPrototypeOf(), hasOwnProperty()

```js
function A(){
	this.sayHello = function(){
  	console.log("Hello World!");
  }
}

var a1 = new A();

console.log(Object.getPrototypeOf(a1) === A.prototype); //true
```

프로토타입 체인에 걸친 속성 검색은 성능 저하를 일으킬 수도 있습니다. 그러므로 프로토타입 체인을 확인하지 않고 속성만을 판단하는 Object의 `hasOwnProperty()` 사용을 권장합니다. `hasOwnProperty()`는 상속받은 속성이 아닌 자신의 속성인 경우에만 true라는 값을 반환합니다.

```js
function Person(name, age){
    var _country = "Korea";
    this.name = name;
    this.age = age;
}
var someone = new Person("jackson", 10);

//프로토타입 체인 전체를 걸쳐서 속성 검색
console.log('name' in someone); //true

//프로토타입 체인 전체를 걸쳐 확인하지 않고 속성 존재만을 판단
console.log(someone.hasOwnProperty('name')); //true
```

<br><br><br><br><br>

## 3. 클래스(class) 형식
생성자 함수 말고도 class를 이용할 수도 있습니다.
```js
class Person {
    constructor(name, age){
        this.name = name;
        this.age = age;
    }

    sayHi(){
        console.log("hi~");
    }
}

var person = new Person("Ari", 19);
person.valueOf(); //Person {name: "Ari", age: 19}
```

<br><br><br>

### - Getter와 Setter
```js
class Program{

    get name() {
        return "이 프로그램의 이름은 " + this._name + "입니다.";
    }

    set name(name){
        console.log("프로그램 이름이 " + name + "으로 설정됨");
        /*this.name이라고 설정하면 
        call stack 초과 에러가 발생합니다.
        이름을 다르게 하세요.*/
        this._name = name;
    }
}

var p = new Program();
p.name = "HR-Manager"; // 프로그램 이름이 HR-Manager으로 설정됨
console.log(p.name); // 이 프로그램의 이름은 HR-Manager입니다.
```

<br><br><br>

### - static
정적 메서드 호출
```js
class Person {

    constructor(name, age){
        this.name = name;
        this.age = age;
    }

    static walk() {
        console.log("걷는다.");
    }
}

Person.walk(); 
var taru = new Person("Taru", 32);
taru.walk(); // Error
```
정적 메서드는 인스턴스에서 호출이 불가하고 오직 클래스에서만 호출이 가능합니다.

다른 메서드에서 정적 메서드를 사용할 수 있는지 확인해보겠습니다.
```js
class Person {

    constructor(name, age){
        this.name = name;
        this.age = age;
        Person.sayHello();
    }

    static sayHello(){
        console.log("안녕하세요 " +this.name +"입니다");
    }

    sing(){
        this.constructor.sayHello();
        console.log(`나이 ${this.age}의 ${this.name}가 [노래한다]`);
    }

    static walk() {
      console.log(`나이 ${this.age}의 ${this.name}가 [걷는다]`);
    }

    static exercise(){
        this.walk();
    }
}
var person = new Person("에이유", 19); //안녕하세요 Person입니다
Person.exercise(); //나이 undefined의 Person가 [걷는다]
Person.walk(); //나이 undefined의 Person가 [걷는다]
person.sing(); 
//안녕하세요 Person입니다
//나이 19의 에이유가 [노래한다]

```
정적 메서드가 키워드 this를 써도 인스턴스 변수(멤버 변수)를 잡아낼 수 없습니다.

<br><br><br>

### - protected

`_` 키워드를 속성 앞에 붙이면됩니다. 그러나 이것은 단지 외부에서 직접적으로 접근하지 말라는 것을 나타내기 위해 보여주는 것일 뿐입니다(Only Works for Visual Indication). 자바스크립트에서는 모든 속성은 public으로 간주하기 때문에 마음만 먹으면 언제든지 접근할 수 있습니다.

```js
class Counter{
	_value = 0;
  
  increment(){
  	this._value++;
  }
}

var test = new Counter();

console.log(test._value); //0, 접근가능
test.increment();
console.log(test._value); //1, 접근가능

```

<br><br><br>

### - private

`#` 키워드를 속성 앞에 붙이면됩니다. 이렇게하면 외부에서 접근할 수 없습니다.

```js
class Counter{
	#value = 0;
  
  increment(){
  	this.#value++;
  }
}

var test = new Counter();

console.log(test.#value); //SyntaxError
```

이제 Getter/Setter를 통해 속성을 안정적으로 보호할 수 있습니다.

```js
class Counter{
	#value = 0;
  
  increment(){
  	this.#value++;
    return this;
  }
  
  get value(){
  	return this.#value;
  }
  
  set value(number){
  	console.log(`${number} set error : 임의로 수정할 수 없습니다.`);
  }
  
  
}

var test = new Counter();
test.increment().increment(); 
console.log(test.value); //2
test.value = 20; //20 set error : 임의로 수정할 수 없습니다.

```


<br><br><br><br><br>

## 4. 상속

### - 프로토타입형

#### * call() 함수를 이용한 부모 속성 및 메서드 인스턴스화 하기

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' +
                      this.name + ' with a negative price');
  }
  
  this.nestedFunc = function(){
  	console.log("I'm the nestedFunc!");
  }
}

Product.prototype.info = function(){
    console.log("이것은 상품입니다");
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

var cheese = new Food('Mozzarella', 5);
console.log(cheese.valueOf()); // Food {name: "Mozzarella", price: 5, category: "food"}
console.log(cheese.category); // food
console.log(cheese.name); //Mozzarella
cheese.nestedFunc(); //I'm the nestedFunc!
cheese.info(); //Error
```
cheese의 `__proto__`를 보면 Product가 아니라 바로 Object인 것을 볼 수 있습니다. 그래서 Product의 프로토타입에 접근할 수가 없습니다. 그러므로 prototype을 Product로 바꿔주어야 합니다.

#### * Prototype Chaining을 이용해서 상속 구현

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

Product.prototype.info = function(){
    console.log("이것은 상품입니다");
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

Food.prototype.__proto__ = Product.prototype

var cheese = new Food('feta', 5);
console.log(cheese.valueOf());
cheese.info(); //이것은 상품입니다

console.log(cheese instanceof Food); //true
console.log(cheese instanceof Product); //true
console.log(cheese instanceof Object); //true
```

위와 같이 단순하게 프로토타입에 있는 `__proto__`가 가리키는 위치를 바꿔주면 정말 편하게 바꿀 수 있습니다. 다만 MDN에서 밝히는 표준은 아래와 같습니다.

#### * Object.create()를 이용해서 상속 구현

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

Product.prototype.info = function(){
    console.log("이것은 상품입니다");
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

var create = function(obj){
    function F() {}
    F.prototype = obj;
    return new F();
}

Food.prototype = create(Product.prototype);

var cheese = new Food('feta', 5);
console.log(cheese.valueOf());
cheese.info(); //이것은 상품입니다

console.log(cheese instanceof Food); //true
console.log(cheese instanceof Product); //true
console.log(cheese instanceof Object); //true
```
이렇게 프로토타입 체인을 형성시켜서 `cheese.info();`가 실행이 됩니다.. 그리고 우리가 만든 create라는 함수는 바로 `Object.create()`함수와 같습니다.

그러나 문제점이 있는데 바로 생성자입니다.
```js
Food.prototype.constructor
```
Food가 아닌 Product로 되어 있습니다.
```js
Food.prototype.constructor = Food
```
그러나 이러한 방식은 또 다른 주의점을 야기합니다.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' +
                      this.name + ' with a negative price');
  }
}

Product.prototype.info = function(){
    console.log("이것은 상품입니다");
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

Food.prototype.smell = function(){
  console.log("냄새가 좋습니다");
}

Food.prototype = Object.create(Product.prototype);
Food.prototype.constuctor = Food;

var cheese = new Food('feta', 5);
console.log(cheese.valueOf());
cheese.info();
cheese.smell(); //ERROR
```

Food의 프로토타입이 덮여쓰여져서 Food 프로토타입에 정의되어 있던 smell()이라는 함수가 사라지게 되었습니다. 그렇기 때문에 순서를 주의하셔야합니다.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' +
                      this.name + ' with a negative price');
  }
}

Product.prototype.info = function(){
    console.log("이것은 상품입니다");
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}
Food.prototype = Object.create(Product.prototype); // Product의 Prototype을 가리키는 __proto__
Food.prototype.constuctor = Food; // Food에 대한 constructor 추가 = constructor가 Food를 가리키게 됨

Food.prototype.smell = function(){
  console.log("냄새가 좋습니다");
}

var cheese = new Food('feta', 5);
console.log(cheese.valueOf());
cheese.info(); //이것은 상품입니다
cheese.smell(); //냄새가 좋습니다

console.log(cheese instanceof Food); //true
console.log(cheese instanceof Product); //true
console.log(cheese instanceof Object); //true
```

<br><br><br>

### - 클래스형

```js
class Product {
    constructor(name, price){
        this.name = name;
        this.price = price;
    }

    info(){
        console.log("이것은 상품입니다");
    }
}

class Food extends Product{
    constructor(name, price){
        super(name, price)
        this.category = 'food';
    }
}

var cheese = new Food('feta', 5);
console.log(cheese.valueOf());
console.dir(cheese);
cheese.info();

console.log(cheese instanceof Food); //true
console.log(cheese instanceof Product); //true
console.log(cheese instanceof Object); //true
```

<br><br><br><br><br>

## 5. call(), apply(), bind()

call()과 apply()를 통해서 this를 지정할 수 있습니다.
```js
function doSomething(){
  console.log("do something!");
}

doSomething(); // do something!
doSomething.call(); // do something!
```

bind()를 통해서 해당 객체의 this값을 바꿀 수 있습니다.

call()과 apply()와 같지만 이 둘은 해당 함수를 호출 할 때 this값을 조절함과 달리 bind()는 호출하기 전에 this가 무엇인지 지정해놓는 것입니다.


<br><br><br>

### - call()

call() 메소드는 주어진 this 값 및 각각 전달된 인수와 함께 함수를 호출합니다.
```js
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

console.log(new Food('cheese', 5).name);
// expected output: "cheese"
```
이 함수 구문은 apply()와 거의 동일하지만, call()은 인수 목록을, 반면에 apply()는 인수 배열 하나를 받는다는 점이 중요한 차이점입니다.

<br><br><br>

### - apply()

apply() 메서드는 주어진 this 값과 배열 (또는 유사 배열 객체) 로 제공되는 arguments 로 함수를 호출합니다.

```js
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

console.log(max);
// expected output: 7

const min = Math.min.apply(null, numbers);

console.log(min);
// expected output: 2
```
```js
var array = ['a', 'b'];
var elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]
```

<br><br><br>

### - bind()

bind() 메소드가 호출되면 새로운 함수를 생성합니다. 받게되는 첫 인자의 value로는 this 키워드를 설정하고, 이어지는 인자들은 바인드된 함수의 인수에 제공됩니다.
```js
this.x = 9;
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX();
// 9 반환 - 함수가 전역 스코프에서 호출됐음

// module과 바인딩된 'this'가 있는 새로운 함수 생성
// 신입 프로그래머는 전역 변수 x와
// module의 속성 x를 혼동할 수 있음
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```

<br><br><br><br><br>

## 6. Object 객체

### - Object.create()

주로 객체를 상속하기 위해 사용하는 메서드입니다.

```js
var o;
o = {};
// 위는 아래와 같습니다:
o = Object.create(Object.prototype);
```

매개변수에 들어온 객체의 Prototype을 기반으로 `__proto__`를 가져옵니다.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

Product.prototype.info = function(){
    console.log("이것은 상품입니다");
}

console.log(Object.create(Product.prototype)); // this.__proto__의 내용(=Product.prototype의 내용)
```

 - 두번쨰 파라미터로는 `Object.defineProperties()`에 들어갈 인자처럼 들어갑니다.

#### * Object.defineProperty()

속성을 더욱 정교하게 정의할 수 있습니다.

속성 데이터에 대한 서술을 통해 속성을 제어할 수 있습니다.

`주의! 데이터 서술자와 접근자 서술자는 동시에 적용될 수 없습니다. 충돌 발생`

 - 데이터 서술자

`configurable` : 속성의 데이터 정의를 변경할 수 있고 삭제할 수도 있으면 true, 기본값 : false, `configurable` 속성이 false가 되면 아무리 같은 `configurable` 속성이라도 다시 true로 바꾸려 시도해도 TypeError가 발생하게 된다.

`enumerable` : 열거 시 노출된다면 true, 기본값 : false

`value` : 속성에 연관된 값. 기본값 : undefined

`writable` : 할당 연산자`=`로 속성의 값 수정 가능 : true, 기본값 : false 

##### tip : `Object.keys()`를 이용해 인스턴스 key 값들을 받을 수 있다.

##### tip : `Object.getOwnPropertyDescriptor()`를 이용해 데이터 정의 값들을 확인할 수 있다.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

const song = new Product("Album",1000);
console.log(song); // Product {name: "Album", price: 1000}
console.log(Object.keys(song)); // ["name", "price"]

song.price = 2000;
console.log(song); // Product {name: "Album", price: 2000}

Object.defineProperty(song, 'price', {
  configurable: false,
  enumerable: false,
  writable: false
});


console.log(song); // Product {name: "Album", price: 3000}
song.price = 4000;
console.log(song); // Product {name: "Album", price: 3000}

console.log(Object.keys(song)); // ["name"]
console.log(Object.getOwnPropertyDescriptor(song,'price'));
//{value: 2000, writable: false, enumerable: false, configurable: false}
```

 - 접근자 서술자
 
 `get` and `set`

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

const song = new Product("Album",1000);
console.log(song); // Product {name: "Album", price: 1000}

song.price = 2000;
console.log(song); // Product {name: "Album", price: 2000}


Object.defineProperty(song, 'price', {
  get : function(){
  	return this.price;
  },
  set : function(value){
		this.price = value;
  }
});


console.log(song); 
// Product {name: "Album", price: [Exception: RangeError: Maximum call stack size exceeded at Product.get [as price]]}
```

위와 같이 선언하게 되면 this를 사용할 때마다 객체가 호출되기 때문에 무한대로 호출되어 stack overflow가 발생한다.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

const song = new Product("Album",1000);
console.log(song); // Product {name: "Album", price: 1000}

song.price = 2000;
console.log(song); // Product {name: "Album", price: 2000}

let priceValue = 10;

Object.defineProperty(song, 'price', {
  get : function(){
  	return priceValue;
  },
  set : function(value){
		priceValue = value;
  }
});


console.log(song.price); // 10
priceValue = 20;
console.log(song.price); // 20
song.price = 30;
console.log(song.price); // 30
console.log(priceValue); // 30
```

이제 Product 객체의 price 속성은 global 변수 priceValue와 값을 공유하게 된다.

#### * Object.defineProperties() 

`Object.defineProperty()`은 한 속성에 대해서 정의할 수 있는데에 반해서 `Object.defineProperties()`은 여러 속성에 대해서 정의할 수 있다.

```js
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // 등등
});
```


<br><br><br>

### - Object.create()와 비슷한 Object.assign()

동일한 키가 있으면 target의 속성은 source의 속성으로 덮어쓰여집니다.

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

복사 방향 : source --> target

#### * 객체 복사

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

const song = new Product("Album",1000);
const newSong = Object.assign({},song);
newSong.price=2000; 
console.log(song); // Product {name: "Album", price: 1000}
console.log(newSong); //{name: "Album", price: 2000}
//Product의 인스턴스가 아니다. 
```

#### * Shallow Copy

```js
let origin = {
  a : 10,
  b : {
    b1 : 20,
    b2 : 30
  }
}

let copy = Object.assign({}, origin);
console.log(origin);//{a: 10, b: {b1: 20, b2: 30}}
console.log(copy);//{a: 10, b: {b1: 20, b2: 30}}
```

```js
origin.b.b1 = 100;
console.log(origin);//{a: 10, b: {b1: 100, b2: 30}}
console.log(copy);//{a: 10, b: {b1: 100, b2: 30}}
```

`Object.assign()`은 속성의 값만 가져옵니다. 그렇기 때문에 만일 속성의 값이 객체를 참조하는 값이면 그대로 가져오게 됩니다.

#### * JSON 활용하기 & Deep Copy

`JSON.stringify()`

JSON.stringify() 메소드는 인수로 전달받은 자바스크립트 객체를 문자열로 변환하여 반환합니다.

`JSON.parse()` 
 
 JSON.parse()메소드는 JSON.stringify() 메소드와는 반대로 인수로 전달받은 문자열을 자바스크립트 객체로 변환하여 반환합니다.

JSON (JavaScript Object Notation)은 경량의 데이터 교환 형식

```js
let origin = {
  a : 10,
  b : {
    b1 : 20,
    b2 : 30
  }
}
let makeStringToCopy = JSON.stringify(origin);
console.log(makeStringToCopy, typeof makeStringToCopy); //{a: 10, b: {b1: 20, b2: 30}} object

let copy = JSON.parse(makeStringToCopy); 
console.log(copy, typeof copy); //{a: 10, b: {b1: 20, b2: 30}} string

origin.b.b1 = 100;
console.log(origin);//{a: 10, b: {b1: 100, b2: 30}}
console.log(copy); //{a: 10, b: {b1: 20, b2: 30}}
```

##### 열거 불가형 속성과 프로토타입 체인의 속성은 복사 불가

```js
const obj = Object.create({ foo: 1 }, { // foo 는 obj 의 프로토타입 체인상에 있음.
  bar: {
    value: 2  // bar 는 열거 불가능한 속성임.
  },
  baz: {
    value: 3,
    enumerable: true  // baz 는 자체 열거형 속성임.
  }
});

const copy = Object.assign({}, obj);
console.log(copy); // { baz: 3 }
```
위와 같이 열거가 불가능한 속성은 복사가 불가능합니다.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

Product.prototype.createSerial = function(serialNumber){
	if(typeof(serialNumber) !== 'number'){
  	return null;
  }
  this.serialNumber = serialNumber;
  return this;
}

function Album(name, price, playList){
	Product.call(this, name, price);
  
  this.playList = playList;
	
  if(Object.getPrototypeOf(this.playList).constructor.name !== Array.name){
  	throw TypeError("playList parameter must be array type");
  }
}

Album.prototype = Object.create(Product.prototype);
Album.prototype.constructor = Album;


const playList = ["라일락", "밤 편지", "너랑나","무릎", "복숭아"];
let music = new Album("IU", 1000, playList);
console.log("createSerial" in music); //true
let copyMusic = Object.assign({}, music);
console.log("createSerial" in copyMusic); //false
playList[0] = "One Love";

console.log(music.playList); //["One Love", "밤 편지", "너랑나", "무릎", "복숭아"]
console.log(copyMusic.playList); //["One Love", "밤 편지", "너랑나", "무릎", "복숭아"]
```

위 코드의 `Object.assign({},music)`에서 { }은 Object이기 때문에 Product의 prototype을 가지고 있지 않습니다. 또한 얕은 복사(Shallow Copy)이기 때문에 원본 데이터의 무결성 문제가 발생하게 됩니다.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Cannot create product ' + this.name + ' with a negative price');
  }
}

Product.prototype.createSerial = function(serialNumber){
	if(typeof(serialNumber) !== 'number'){
  	return null;
  }
  this.serialNumber = serialNumber;
  return this;
}

function Album(name, price, playList){
	Product.call(this, name, price);
  
  this.playList = playList;
	
  if(Object.getPrototypeOf(this.playList).constructor.name !== Array.name){
  	throw TypeError("playList parameter must be array type");
  }
}

Album.prototype = Object.create(Product.prototype);
Album.prototype.constructor = Album;


const playList = ["라일락", "밤 편지", "너랑나","무릎", "복숭아"];
let music = new Album("IU", 1000, playList);
console.log("createSerial" in music); //true
let copyMusic = JSON.parse(JSON.stringify(music));
copyMusic.__proto__ = Album.prototype;
console.log("createSerial" in copyMusic); //true
playList[0] = "One Love";

console.log(music.playList); // ["One Love", "밤 편지", "너랑나", "무릎", "복숭아"]
console.log(copyMusic.playList); // ["라일락", "밤 편지", "너랑나", "무릎", "복숭아"]
```


##### 객체 병합

```js
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };

const obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, 대상 객체 자체가 변경됨.
obj.a = 10

console.log(obj); // { a: 10, b: 2, c: 3 }
console.log(o1);  // { a: 10, b: 2, c: 3 }
```

#####  같은 속성을 가진 객체 병합

```js
const o1 = { a: 1, b: 1, c: 1 };
const o2 = { b: 2, c: 2 };
const o3 = { c: 3 };

const obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
```

##### 접근자 복사
```js
var obj = {
  foo: 1,
  get bar() {
    return 2;
  }
};

var copy = Object.assign({}, obj); 
console.log(copy); 
// { foo: 1, bar: 2 }, copy.bar 의 값은 obj.bar 의 getter 의 반환 값임.

// 모든 descriptors 를 복사하는 할당 함수
function completeAssign(target, ...sources) {
  sources.forEach(source => {
    let descriptors = Object.keys(source).reduce((descriptors, key) => {
      descriptors[key] = Object.getOwnPropertyDescriptor(source, key);
      return descriptors;
    }, {});
    // 기본적으로, Object.assign 는 열거형 Symbol 도 복사함.
    Object.getOwnPropertySymbols(source).forEach(sym => {
      let descriptor = Object.getOwnPropertyDescriptor(source, sym);
      if (descriptor.enumerable) {
        descriptors[sym] = descriptor;
      }
    });
    Object.defineProperties(target, descriptors);
  });
  return target;
}

var copy = completeAssign({}, obj);
console.log(copy);
// { foo:1, get bar() { return 2 } }
```

<br><br><br>

### - Object.freeze()

객체를 동결시켜 더이상 변경될 수 없도록 합니다.

```js
const obj = {
  a : 10
}

Object.freeze(obj);

obj.a = 100

console.log(obj); //{a: 10}
```

하지만 얕은 동결(Shallow Freeze)이기 떄문에 각 객체를 재귀적으로 동결시켜야 합니다.

<br><br><br>

### - Object.entries()

객체에서 enumerable 속성이 true인 속성을 한하여 key-value 쌍의 배열을 반환합니다.

```js
const obj = {
  a: 'somestring',
  b: 42
};

console.log(Object.entries(obj)); //[ ["a", "somestring"],["b", 42] ]

for (const [key, value] of Object.entries(obj)) {
  console.log(`${key}: ${value}`);
}
```

<br><br><br><br><br>

## 7. 객체 관련 연산자

### - 단항 연산자 `delete`, `typeof`, `void`

#### * delete

```js
x = 42;
var y = 43;
myobj = new Number();
myobj.h = 4;    // create property h
delete x;       // returns true (can delete if declared implicitly)
delete y;       // returns false (cannot delete if declared with var)
delete Math.PI; // returns false (cannot delete predefined properties)
delete myobj.h; // returns true (can delete user-defined properties)
delete myobj;   // returns true (can delete if declared implicitly)
```
배열 원소를 삭제할 경우
```js
var trees = ["redwood", "bay", "cedar", "oak", "maple"];
delete trees[3];
if (3 in trees) {
  // this does not get executed
}
```
여기서 trees의 내용은 `(5) ["redwood", "bay", "cedar", empty, "maple"]`가 되고 `delete`연산된 배열 요소는 empty로 되지만 반환할 때는 undefined로 된다.

아래 코드와 비교해보자.
```js
var trees = ["redwood", "bay", "cedar", "oak", "maple"];
trees[3] = undefined;
if (3 in trees) {
  // this gets executed
}
```

#### * typeof

typeof 연산자는 피연산자의 타입을 나타내는 `문자열을 반환!!!(중요)`

#### * void

void 연산자는 값을 반환하지 않고 평가되도록 명시
```js
function a(){
    console.log("the function of a()");
    return "hello";
}

a() /*
        the function of a()
        "hello"                */

void(a()) /*
            the function of a()
                                */
```

<br><br><br>

### - 관계 연산자 `in`, `instanceof`

#### * in

객체에 특정한 속성이 있는경우 true를 반환

```js
// Arrays
var trees = ["redwood", "bay", "cedar", "oak", "maple"];
0 in trees;        // returns true
3 in trees;        // returns true
6 in trees;        // returns false
"bay" in trees;    // returns false (you must specify the index number,
                   // not the value at that index)
"length" in trees; // returns true (length is an Array property)

// built-in objects
"PI" in Math;          // returns true
var myString = new String("coral");
"length" in myString;  // returns true

// Custom objects
var mycar = { make: "Honda", model: "Accord", year: 1998 };
"make" in mycar;  // returns true
"model" in mycar; // returns true
```

#### * instanceof

명시된 객체가 명시된 객체형인 경우 true를 반환

```js
var theDay = new Date(1995, 12, 17);
if (theDay instanceof Date) {
  // statements to execute
}
```
