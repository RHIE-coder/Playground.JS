# `[3] 연산자의 기본`

<br><br><br><br><br>

## 1. 산술 연산자

### - `+`
```js
var result = 10 + 20;
console.log(result); //30
```

#### * 숫자화 연산자
피연산자가 숫자값이 아니라면 피연산자를 숫자로 변환하기를 시도합니다.
 - +"3" 은 3을 반환합니다.
 - +true 는 1. 을 반환합니다.

#### * 문자열 연산자

문자열 + 문자열 = 문자열 확장
```js
console.log("my " + "string"); // console logs the string "my string".
```

숫자 + 문자열 = 문자열

```js
let combination = 10 + " is number"
console.log(combination); // 10 is number
console.log(typeof combination); // string
```


### - `-` 
```js
var result = 10 - 20;
console.log(result); //-10
```

#### * 부정 연산자

피연산자의 반대값(부호 바뀐값)을 반환합니다.
- x 가 3이면 -x 는 -3을 반환합니다.

### - `*` 
```js
var result = 10 * 20;
console.log(result); //200
```

### - `/` 
```js
var result = 10 / 3;
console.log(result); //3.3333333333333335
```

### - `%` 
```js
var result = 10 % 3;
console.log(result); //1
```

### - `**`
```js
var result = 10 ** 3;
console.log(result); //1000
```
<br><br><br><br><br>

## 2. 증감연산자 `++`, `--`
```js
var increasing = 10;
increasing++; //11
++increasing; //12
console.log(increasing); //12

var decreasing = 10;
decreasing--; //9
--decreasing; //8
console.log(decreasing); //8
```

### - 전위연산과 후위연산

```js
var num = 10;
var front = ++num;
console.log(front, num); //11 11

var num = 10;
var back = num++;
console.log(back, num); //11 11

var num = 10;
console.log(num--); //10
console.log(num); //9
```



<br><br><br><br><br>

## 3. 비교 연산자
|연산자|설명|참을 반환하는 예제|
|:---:|:---:|:---:|
|동등 (==)|	피연산자들이 같으면 참을 반환합니다.|	3 == var1<br>"3" == var1<br>3 == '3' |
|부등 (!=)|피연산자들이 다르면 참을 반환합니다.|	var1 != 4<br>var2 != "3"|
|일치 (===)|피연산자들이 같고 피연산자들의 같은 형태인 경우 참을 반환합니다. Object.is와 JavaScript에서의 같음을 참고하세요. | 3 === var1 |
|불일치 (!==)|	피연산자들이 다르거나 형태가 다른 경우 참을 반환합니다. |var1 !== "3"<br>3 !== '3'|
|~보다 큰 (>)|	좌변의 피연산자 보다 우변의 피연산자가 크면 참을 반환합니다.|	var2 > var1<br>"12" > 2|
|~보다 크거나 같음 (>=)|	좌변의 피연산자 보다 우변의 피연산자가 크거나 같으면 참을 반환합니다.|var2 >= var1<br>var1 >= 3|
|~보다 작음 (<)|좌변의 피연산자 보다 우변의 피연산자가 작으면 참을 반환합니다.|var1 < var2<br>"2" < 12|
|~보다 작거나 같음 (<=)|좌변의 피연산자 보다 우변의 피연산자가 작거나 같으면 참을 반환합니다.|var1<=var2<br>var2 <= 5|

<br><br><br><br><br>


## 4. 논리 연산자
|연산자|	구문|	설명|
|:---:|:----:|:-----|
|논리 AND (&&)|	expr1 && expr2|	expr1을 true로 변환할 수 있는 경우 expr2을 반환하고, 그렇지 않으면 expr1을 반환합니다.|
|논리 OR (\|\|) |	expr1 \|\| expr2 |expr1을 true로 변환할 수 있으면 expr1을 반환하고, 그렇지 않으면 expr2를 반환합니다.|
|논리 NOT (!)	|!expr|	단일 피연산자를 true로 변환할 수 있으면 false를 반환합니다. 그렇지 않으면 true를 반환합니다.|

### - 효율적인 연산

- false && anything 는 false로 단축 계산됩니다.
- true || anything 는 true로 단축 계산됩니다.

<br><br><br><br><br>

## 5. 그룹 연산자
```js
var a = 1;
var b = 2;
var c = 3;

// default precedence
a + b * c     // 7
// evaluated by default like this
a + (b * c)   // 7

// now overriding precedence 
// addition before multiplication   
(a + b) * c   // 9

// which is equivalent to
a * c + b * c // 9
```
<br><br><br><br><br>


## 6. 쉼표 연산자
 - 쉼표 연산자 (,)는 두 피연산자를 평가하고, 마지막 피연산자의 값을 반환

```js
var a, b, c

a = b = 3, c = 4; // return 4
console.log(a); // 3

var x, y, z

x = (y = 5, z = 6); // return 6
console.log(x); // 6 
```

<br><br><br><br><br>

## 7. 비트 연산자
 - 진법 표현하기
```js
console.log(0b100) // 4(2진법)
console.log(0o100) // 64(8진법)
console.log(0x100) // 256(16진법)
```
 - 10진수 <---> 2,8,16진수
```js
var decimal = 333
console.log(decimal.toString(2)); //101001101
console.log(decimal.toString(8)); //515
console.log(decimal.toString(16)); //14d

console.log(parseInt("101001101", 2)); //333
console.log(parseInt("515", 8)); //333
console.log(parseInt("14d", 16)); //333
```

|연산자|사용법|	설명|
|:---:|:---:|:---:|
|비트단위 논리곱|	a & b	|두 피연산자의 각 자리 비트의 값이 둘다 1일 경우 해당하는 자리에 1을 반환합니다.|
|비트단위 논리합|	a \| b	|두 피연산자의 각 자리 비트의 값이 둘다 0일 경우 해당하는 자리에 0을 반환합니다.|
|비트단위 배타적 논리합|	a ^ b	|두 피연산자의 각 자리 비트의 값이 같을 경우 해당하는 자리에 0을 반환합니다.<br>[두 피연산자의 각 자리 비트의 값이 다를 경우 해당하는 자리에 1을 반환합니다.]|
|비트단위 부정|	~ a	|피연산자의 각 자리의 비트를 뒤집습니다.|
|왼쪽 시프트|	a << b	|오른쪽에서 0들을 이동시키면서, a의 이진수의 각 비트를 b비트 만큼 왼쪽으로 이동시킨 값을 반환합니다.|
|부호 전파 오른쪽 시프트|	a >> b	|사라지는 비트를 버리면서, a의 이진수의 각 비트를 b비트만큼 이동시킨값을 반환합니다.|
|부호 없는 오른쪽 시프트|	a >>> b	|사라지는 비트를 버리고 왼쪽에서 0을 이동시키면서, a의 이진수의 각 비트를 b비트 만큼 이동시킨 값을 반환합니다.|

`<<` : 새로운 부호는 0
```js
5 << 2 => 20
//  5:      0..000101
// 20:      0..010100 <= adds two 0's to the right
```

`>>` : 부호전파, 가장 왼쪽의 비트가 1인경우 새로운 비트는 모두 1, 그 반대는 0
```js
20 >> 2 => 5
// 20:      0..010100
//  5:      0..000101 <= added two 0's from the left and chopped off 00 from the right

-5 >> 3 => -1
// -5:      1..111011
// -2:      1..111111 <= added three 1's from the left and chopped off 011 from the right
```
`>>>` : 제로 채우기, 새 비트는 0이됨
```js
-30 >>> 2 => 1073741816
//       -30:      111..1100010
//1073741816:      001..1111000
```

<br><br><br><br><br>

## 8. 할당 연산자
|이름|복합 할당 연산자|뜻|
|:---:|:---:|:---:|
|할당|	x = y|	x = y|
|덧셈 할당|	x += y|	x = x + y|
|뺄셈 할당|	x -= y|	x = x - y|
|곱셈 할당|	x *= y|	x = x * y|
|나눗셈 할당|	x /= y	|x = x / y|
|나머지 연산 할당|	x %= y|	x = x % y|
|지수 연산 할당 |	x **= y|	x = x ** y|
|왼쪽 이동 연산 할당|	x <<= y|	x = x << y|
|오른쪽 이동 연산 할당|	x >>= y|	x = x >> y|
|부호 없는 오른쪽 이동 연산 할당|	x >>>= y|	x = x >>> y|
|비트 AND 할당|	x &= y|	x = x & y|
|비트 XOR 할당|	x ^= y|	x = x ^ y|
|비트 OR 할당|	x \|= y|	x = x \| y|

### - 구조 분해 할당
```js
var foo = ['first', 'second', 'third'];

// 구조 분해를 활용하지 않은 경우
var one   = foo[0];
var two   = foo[1];
var three = foo[2];

// 구조 분해를 활용한 경우
var [one, two, three] = foo;
console.log(one); //first
console.log(two); //second
console.log(three); //third
```