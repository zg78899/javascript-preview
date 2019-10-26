## spread 문법

<hr>

es6에서 새롭게 도입된 spread문법은 하나로 뭉쳐있는 여러값들의 집합을 펼쳐서(전개,분산하여 ,spread) 개별적인 갑들의 목록을 만든다.
spread문법의  대상은
 <u>Array,string,Map,Set,Dom data structure(NodeList.HTML,Collection),Arguments 와 같이 for of문법으로 순회할수있는 이터러블에 한정된다</u>.

~~~javascript
console.log(...[1,2,3])//1 2 3
console.log(...'Hello')// h e l l o;
console.log(...new Map([['a','1'],['b','2']]));//['a','1'] ['b','2'];
console.log(...new Set(['a','1']));// a 1;
console.log(...new Set([1,2,3]))// 1 2 3

console.log(...{a:1,b:2});//typeError

const list=...arr//syntaxError
~~~


...[1,2,3]은 이터러블인 배열을 펼쳐서 요소값을 개별적인 값들의 목록 1 2 3으로 만든다. 이때 1 2 3은 값이 아니라 값들의 목록이다. **즉 spread문법의 결과는 값이 아니다.**

이는 spread문법 ... 이 피연산자를 연산하여 값을 생성하는 연산자가 아님을 의미한다. 따라서 spread 문법의 결과는 변수에 할당할수없다.

spread문법의 결과물은 단독으로 사용할수없고 쉼표로 구분한 값의 목록을 사용하는 문에서 사용한다.

### 1.함수 호출문의 인수 목록에서 사용하는 경우.

요소의 값을 하나의 값으로 뭉쳐있는 배열을 펼쳐서 요소들의 개별적인 값들의 목록으로 만든후,이를 함수 목록으로 전달해야하는 경우

~~~;javascript
const arr=[1,2,3];
const maxValue=Math.max(arr);
console.log(maxValue);//NaN
~~~

Math.max는 매개변수의 개수를 확정할수없는 가변인자 함수이다.여러개의 인수는 전달박아 인주중에 최대값을 반환한다.Math.max의 인수로 숫자가 아닌 배열을 주었기때문에 치대값을 정할수없다. 문제를 해결하기 위해서는 뱌열을  펼쳐 요값들의 개별적인 값들을 목록으로 만든 후 인수로 전달해야한다.

~~~javascript
const arr=[1,2,3];
const maxValue=Math.max.apply(null,arr);
console.log(maxValue);//3

const arr=[1,2,3];
const maxValue=Math.max(...arr)=Math.max(...[1,2,3]);

console.log(maxValue);
~~~

spread문법이 사용되기 전에는 function.prototype.apply을 사용하여 배열을 펼쳐서 함수의 인수로 전달하였다.

spread문법을 사용하면 훨씬 가독성이 좋다.

**주의해야할점 ** Rest파라미터와 spread 문법은 형태가 동일하기 때문에 혼동할 가능성있다.
<li>Rest파라미터는매개변수를 정의할때 함수에 전달된 인수들의 목록을 배열로 전달 받기 위해서 매개별수 이름 앞에 ...을 붙이는 것이다. 
<li>Spread문법은 하나로 뭉쳐있는 배열과 같은 이터러블을 펼쳐서 개별적인 목록으로 만드는 것이다. 
</ul>

**따라서 Rest 파라미터와 Spread문법은 서로 반대의 개념이라는 것을 명심하자.

~~~javascript
function foo(param,..arr){
  console.log(param);//1;
  console.log(...arr);//[2,3];
}
foo(...[1,2,3]);[1,2,3]->1 2 3;
~~~

<hr>

### 2.배열 리터럴의 내부에서 사용하는 경우

spread문법을 배열 리터럴에서 사용하면 간결하고 가독성이 좋게 표현이 가능하다.

~~~javascript
//es5
var arr=[1,2].concat([3,4]);
console.log(arr);//[1,2,3,4];
//es6
const arr=[...[1,2]3,4];
console.log(arr);//[1,2,3,4];
~~~

~~~javascript
//es5
var arr1=[1,2];
var arr2=[3,4];
Array.prototype.push.apply(arr1,arr2);
console.log(arr1);

//es6
const arr1=[1,2];
const arr2=[3,4];
arr1.push(..[3,4]);
console.log(arr1);

~~~

~~~javascript
//es5
var arr1=[1,4];
var arr2=[2,3];
Array.prototype.splice.apply(arr1,[1,0].concat(arr2));
console.log(arr1);//[1,2,3,4];

//es6
const arr1=[1,4];
const arr2=[2,3];

arr1.splice(1,0,...arr2);
console.log(arr1);//[1,2,3,4];
~~~

~~~javascript
//es5
var origin=[1,2];
var copy=origin.slice();

console.log(copy);//[1,2]
console.log(copy===origin);//false 저장되는 공간이 다르기때문

//es6
const origin=[1,2];
const copy=[...origin];
console.log(copy);//[1,2];
console.log(copy===origin);//false;
~~~

이때 배열의 각 요소를 얕은 복사하여 새로운 복사본을 만든다.

~~~javascript
//es5
function sum(){
  var args=Array.prototype.slice.apply(arguments);
  return args.reduce(function(pre,cur){
    return pre+cur;
  },0);
}
console.log(sum(1,2,3));

//es6
function sum(){
  const args=[...arguments];
  return args.reduce((pre,cur)=>{
    return pre+cur;
  },0);
}
console.log(sum(1,2,3));
~~~

<hr>

### 3.객체 리터럴의 내부에서 사용하는 경우

Spread문법의 대상은 이터러블이어야 하지만 Sprea 프로퍼티는 객체 리터럴 내부에서 Spread문법을 사용을 허락한다.

~~~javascript
const n={a:1,b:2,...{x:1,y:2}};
console.log(n);//{a:1,b:2,x:1,y:2};
~~~

spread문법이 도입되기 이전에는 Object.assign 메소드를 사용하요 여러개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가하였다.

~~~javascript
const merged=Object.assign({},{x:1,y:2},{y:10,z:3});
console.log(merged);//{x:1,y:10,z:3};

const change=Object.assign({},{x:1,y:20},{y:1});
console.log(change);//{x:1y:1};

const added=Object.assign({},{x:1,y:2},{z:1});
console.log(added);{x:1,y:2,z:1}
~~~

~~~javascript
const merged={...{x:1,y:2},...{y:10,z:2}};
console.log(merged)//{x:1,y:20,x:2};

const changed={...{x:1,y:2},y:100};
console.log(changed)//{x:1y:100};

const added={...{x:1,y:2},z:10};
console.log(added);//{x:1,y:2,z:10};

~~~



