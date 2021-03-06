# `[4] 조건문과 반복문`

<br><br><br><br><br>


## 1. Block문
중괄호에 묶인 것을 말합니다.
```js
{
  statement_1;
  statement_2;
  .
  .
  .
  statement_n;
}
```

<br><br><br><br><br>


## 2. if문
```js
if (condition_1) {
  statement_1;
} else if (condition_2) {
  statement_2;
} else if (condition_n) {
  statement_n;
} else {
  statement_last;
} 
```
조건(condition)의 값이 참(true)이면 해당 Block문의 수행문(statement)을 동작하고 거짓(false)이면 동작하지 않습니다.
### -  단순 if
### - if~else
### - 다수의 if (else if의 필요성)
### - else if
### - 조건식 안에 변수 값 할당
if문 안에도 `선언된 변수`에 값을 할당시킬 수 있지만 권장하는 방법이 아닙니다. 나중에 코드를 볼 때 혼동이 있을 수 있기 때문입니다. 그러므로 조건문 안에서의 변수값 할당은 사용하지 않는 것이 좋습니다. 왜냐하면 그것은 코드를 자세히 보지 않는 경우, 동등비교연산자로 오해할 수 있기 때문입니다. 예를 들어, 다음 코드는 사용하지 마세요.
```js
var x = 5
var y = 10
if (x = y) {
     /* statements here */
}
// x = 10, y = 10
```
만일 조건식에 할당을 사용해야하는 경우, 일반적인 관행은 할당 주위에 추가 괄호를 넣는 것입니다. 예를 들어:
```js
if ((x = y)) {
  /* statements here */
}
```

### - 거짓으로 취급되는 것
```js
if (false)
if (null)
if (undefined)
if (0)
if (NaN)
if ('')
```
#### * NaN?
 - number타입
 - NaN은 JavaScript에서 유일하게 본인 스스로가 같지 않은 유일한 JS의 값이다.
     - `NaN === NaN`의 결과는 false 
 - Number.isNaN()와 global 메소드의 isNaN()이 있다.
 - 글로벌의 isNaN()은 'Not a Number'로 number 타입인지 아닌지 체크하므로 조심

```js
isNaN(NaN);       // 참
isNaN(undefined); // 참
isNaN({});        // 참

isNaN(true);      // 거짓
isNaN(null);      // 거짓
isNaN(37);        // 거짓

// 문자열
isNaN("37");      // 거짓: "37"은 NaN이 아닌 숫자 37로 변환됩니다
isNaN("37.37");   // 거짓: "37.37"은 NaN이 아닌 숫자 37.37로 변환됩니다
isNaN("123ABC");  // 참: parseInt("123ABC")는 123이지만 Number("123ABC")는 NaN입니다
isNaN("");        // 거짓: 빈 문자열은 NaN이 아닌 0으로 변환됩니다
isNaN(" ");       // 거짓: 공백이 있는 문자열은 NaN이 아닌 0으로 변환됩니다

// dates
isNaN(new Date());                // 거짓
isNaN(new Date().toString());     // 참

// 이것이 허위 양성이고 isNaN이 완전히 신뢰할 수 없는 이유이다.
isNaN("blabla")   // 참: "blabla"는 숫자로 변환됩니다.
                  // 이것을 숫자로 parsing 하는 것을 실패하고 NaN을 반환한다.
```

<br><br><br><br><br>


## 3. 삼항 조건문
삼항 조건 연산자라고 불리는 이 표현식은 아래와 같이 사용하면 됩니다.
```js
조건식 ? 참일 때 부여되는 값 : 거짓일 때 부여되는 값;
```
```js
var score = 85
var result = score > 60 ? "pass" : "fail";
console.log(result) //pass
```

<br><br><br><br><br>


## 4. switch~case문
```js
switch (expression) {
  case label_1:
    statements_1
    [break;]
  case label_2:
    statements_2
    [break;]
    ...
  default:
    statements_def
    [break;]
}
```
[break문이 없으면 어떤 현상이 벌어지는지 추가하기]

<br><br><br><br><br>


## 5. for문
```js
for ([초기문]; [조건문]; [증감문])
  문장
```
```js
var step;
for (step = 0; step < 5; step++) {
  // Runs 5 times, with values of step 0 through 4.
  console.log('Walking east one step');
}
```

<br><br><br><br><br>


## 6. continue문, break문

### - continue문

### - break문

<br><br><br><br><br>

## 7. while문
```js
while (조건문)
  문장
```
```js
n = 0;
x = 0;
while (n < 3) {
  n++;
  x += n;
}
```

### - 무한루프
```js
while (true) {
  console.log("Hello, world");
}
```
[추가하기]
 - switch의 break문과 반복문(for, while)의 break문 차이

<br><br><br><br><br>


## 8. do~while문
```js
do {
  문장
} while (조건문);
```
```js
do {
  i += 1;
  console.log(i);
} while (i < 5);
```


<br><br><br><br><br>


## 9. `for~in`문, `for~of`문
### - for...in문
```js
for (variable in object) {
  statements
}
```
```js
let myList = [10, 20, 30, 40, 50];

for (var index in myList){
    console.log(myList[index]); //10, 20, 30, 40, 50
}
```
그 안에 있는 내용을 가져오는 것이 아니라 요소의 인덱스를 반환합니다.

### - for...of문
```js
for (variable of object) {
  statement
}
```

```js
let arr = [3, 5, 7];
arr.foo = "hello"; // [3, 5, 7, foo: "hello"]

for (let i in arr) {
   console.log(i); // 0, 1, 2, foo
}

for (let i of arr) {
   console.log(i); // 3, 5, 7
}
```
그 안에 있는 요소의 내용을 가져온다.


<br><br><br><br><br>


## 10. 레이블문
```js
label :
   statement
```
```js
let str = '';

loop1:
for (let i = 0; i < 5; i++) {
  if (i === 1) {
    continue loop1;
  }
  str = str + i;
}

console.log(str);
// expected output: "0234"
```

### - 레이블문의 break 사용 예시
```js
for (i = 0; i < a.length; i++) {
  if (a[i] == theValue) {
    break;
  }
}
```
위 코드로 레이블을 사용하면
```js
var x = 0;
var z = 0
labelCancelLoops: while (true) {
  console.log("Outer loops: " + x);
  x += 1;
  z = 1;
  while (true) {
    console.log("Inner loops: " + z);
    z += 1;
    if (z === 10 && x === 10) {
      break labelCancelLoops;
    } else if (z === 10) {
      break;
    }
  }
}
```

### - 레이블문의 continue 사용 예시
```js
i = 0;
n = 0;
while (i < 5) {
  i++;
  if (i == 3) {
    continue;
  }
  n += i;
}
```
위 코드로 레이블을 사용하면
```js
checkiandj:
  while (i < 4) {
    console.log(i);
    i += 1;
    checkj:
      while (j > 4) {
        console.log(j);
        j -= 1;
        if ((j % 2) == 0) {
          continue checkj;
        }
        console.log(j + " is odd.");
      }
      console.log("i = " + i);
      console.log("j = " + j);
  }
```