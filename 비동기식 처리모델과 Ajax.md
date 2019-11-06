## 비동기식 처리모델과 Ajax

**1.Ajax (Asynchronous JavaScript and XML)**

브라우저에서 웹페이지를 요청하거나 링트를 클릭을 하면 화면 갱신이 발생한다.이것은 브라우저와 서버와의 통신에 의한 것이다.서버는 요청 받은 페이지(html)를 반환하는데. 이때 css나 javascript가 같이 반환된다. 클라이언트의 요청에따라 정적인 파일을 만들어 반환할 수도 있고 서버 사이드 프로그램이 만들어 낸 파일이나 데이터를 반환할 수 도있따. 서버로 부터 웹페이지가 반환되면 클라이언트(브라우저)는 이를 렌더링 하여 화면에 표시한다.

Ajaxs는 자바스크립트이용하여 비동기적으로 서버와 브라우저가 데이터를 교활 할수있게 하는 통신 방식을 말한다.
서버로 부터 웹페이지가 반화됨녀 화면 전체에 갱신해야하는데 페이지 일부만을 갱신하고도 동일한 효과를 볼수있도록 하는 것이 ajax이다.페이지 전체를 렌더링하지않고 부분만 갱신하면 되므로 빠르고 부드러운 렌더링이 가능하다.

**2.Jason**
서버와 클라이언트가 데이터 교환을 위한 규칙 데이터 포맷.일반 텍스트 보다 효과 적으로 데이터 구조화가 가능하고 xml포맷 보다 가볍고 사용하기 간편,가독성이 좋다.
자바스크립트의 객체 리터럴과 유사하다. 하지만 순수한 텍스트로 이루어진 규칙이 있는 데이터 구조이다.

~~~javasc
{
"name":"Lee",
"gender":"male",
"age":20,
"alive":true
}
~~~

키는 반드시 큰따옴표로 감싸 주어야한다.

**2.1JSON.stringify**
메드는 **객체를 json형식의 문자열로 변환한다.**

~~~javascript
const o = { name: 'LEE', gender: 'male', age: 20 };

const strObject = JSON.stringify(o);
console.log(typeof strObject, strObject);
//객체를 json 형식의 문자열로

const strPrettyObject = JSON.stringify(o, null, 2);
console.log(typeof strPrettyObject, strPrettyObject);
//객체를 json형식의 문자열로 prettify
function filter(key, value) {
  return typeof value === 'number' ? undefined : value;

}
const strFilterObject = JSON.stringify(o, filter, 2);
console.log(typeof strFilterObject, strFilterObject);

const arr = [1, 5, 'false'];

const strArray = JSON.stringify(arr);
console.log(typeof strArray, strArray);
//배열객체를 문자열로

function replaceToUpper(key, value) {
  return value.toString().toUpperCase();//모든 값을 대문자로 반환한다.

}
const strFilterArray = JSON.stringify(arr, replaceToUpper);
console.log(typeof strFilterArray, strFilterArray);
~~~

**2.2JSON.parse**
**json 데이터를 가진 문자열을 객체로 변환한다.**

서버로 부터 브라우저로 전송된 json데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 객체화를 하여하는데 이를 역직렬화이라 한다. 이를 위해서 내장 객체json의 static 메소드인 JSON.parse을 사용한다.

~~~javascript
const o = { name: 'Lee', gender: 'male', age: 20 };

const strObject=JSON.stringify(o);
console.log(typeof strObject,strObject);

const arr=[1,5,'false'];

const strArray=json.stringify(arr);
console.log(typeof strArray,strArray);

const obj=JSON.parse(strObject);
console.log(typeof obj,obj);

const objArray=JSON.parse(strArray);
console.log(typeof objArray,objArray);

~~~

배열이 json형식의 문자열로 변환되어 있는 경우 json.parse는 문자열을 배열 객체로 변환한다. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

~~~javascript
const todos=[
  {id:1,content:'HTML',completed:false},
  {id:2,content:'CSS',completed:true},
  {id:3,content:'Javascript',completed:false},
];

console.log(typeof todos)

const str=JSON.stringify(todos);
console.log(typeof str,str);

const parsed=JSON.parse(str);
console.log(typeof parsed,parsed);
~~~

**3.XMLHTTPRequest**

브라우저는 XMLHTTPRequest객체를 이용하여 ajax요청을 생성하고 전송한다. 서버가 브라우저의 요청에 대해 응답을 반한하면 같은 XMLHTTPRequest객체가 그 결과를 처리한다.

AJAX 다음은 ajax요청 처리의 예이다.

~~~javascript
const xhr=new XMLHTTPRequest();
xhr.open('GET,'/users);
xhr.send();
~~~

**XMLHTTPRequest.open**
XMLHTTPRequest의 객체를 생성하고 XMLHTTPRequest.open메소드를 사용하여 서버로으ㅢ 요청을 준비한다. XMLHTTPRequest.open의 사용법은 아래와 같다.

~~~javascript
XMLHTTPRequest.open(methid,url[,async])
~~~

| 매개변수 | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| method   | HTTP method('GET','POST','PUT','DELETE' 등)                  |
| url      | 요청을 보낼 URL                                              |
| async    | 비동기 조작 여부,옵션으로 default는 이며 비동기 방식을 동작한다. |

**XMLHTTPRequest.send**

XMLHTTPRequest.send메소드로 분비된 요청을 서버에 전달한다.

기본적으로 서버에 전달하는 데이터는 GET POST 메소드에 따라 그 전송방식에 차이가 있다.
GET메소드의 경우 url의 일부분인  쿼리 문자열로 데이터를 서버에 전송한다.
POST메소드이 경우,데이터를 requerst body에 담아 전송한다.
XMLHTTPRequest.send메소드네는 request.body에 담아서 전송할 인수를 전달할수있다.
만약 요청 메소드가 GET인경우 ,send메소드의 인수는 무시되고 request body은 null로 설정이 된다.

~~~javascript
xhr.send(null);
~~~



**XMLHTTPRequest.setRequestHeader**

[XMLHttpRequest.setRequestHeader](https://developer.mozilla.org/ko/docs/XMLHttpRequest/setRequestHeader) 메소드는 HTTP Request Header의 값을 설정한다. setRequestHeader 메소드는 반드시 XMLHttpRequest.open 메소드 호출 이후에 호출한다.

Request Header인 Content-type, Accept이다.**Content-type**

[Content-type](http://www.w3.org/Protocols/rfc1341/4_Content-Type.html)은 request body에 담아 전송할 데이터의 [MIME-type](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)의 정보를 표현한다. 자주 사용되는 MIME-type은 아래와 같다.

| 타입                        | 서브타입                                           |
| :-------------------------- | :------------------------------------------------- |
| text 타입                   | text/plain, text/html, text/css, text/javascript   |
| Application 타입            | application/json, application/x-www-form-urlencode |
| File을 업로드하기 위한 타입 | multipart/formed-data                              |

~~~javascript
xhr.open('POST','/users');
xhr.setRequestHeader('Content-type','application/json');
ocnst datat={id:3,title:'Javascript',author:'Park',price:5000};
xhr.send(JSON.stringify(data));
xhr.open('POST','/users');
xhr.setRequestHeader('Content-type','aplication/x-www-form-urlencoded');
const dta={title:'Jvascript',author:'Park',price:5000};
xhr.send(Object.keys(data).map(key=>`${key}-${data[key]}`).join('&'));
~~~

**Accept**

HTTP 클라이언트가 서버에 요청할 때 서버가 센드백할 데이터의 MIME-type을 [Accept](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Accept)로 지정할 수 있다.

다음은 서버가 센드백할 데이터의 MIME-type을 지정하는 예이다.

만약 Accept 헤더를 설정하지 않으면, send 메소드가 호출될 때 Accept 헤더가 `*/*`으로 전송된다.

**Ajax response**

다음은 Ajax 응답의 처리의 예이다.

```javascript
// XMLHttpRequest 객체의 생성
const xhr = new XMLHttpRequest();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 onreadystatechange 이벤트 핸들러가 호출된다.
xhr.onreadystatechange = function (e) {
  // readyStates는 XMLHttpRequest의 상태(state)를 반환
  // readyState: 4 => DONE(서버 응답 완료)
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status는 response 상태 코드를 반환 : 200 => 정상 응답
  if(xhr.status === 200) {
    console.log(xhr.responseText);
  } else {
    console.log('Error!');
  }
};
```

**XMLHttpRequest.send** 메소드를 통해 서버에 Request를 전송하면 서버는 Response를 반환한다. 하지만 언제 Response가 클라이언트에 도달할 지는 알 수 없다. [XMLHttpRequest.onreadystatechange](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/onreadystatechange)는 Response가 클라이언트에 도달하여 발생된 이벤트를 감지하고 콜백 함수를 실행하여 준다. 이때 이벤트는 Request에 어떠한 변화가 발생한 경우 즉 [XMLHttpRequest.readyState](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest/readyState) 프로퍼티가 변경된 경우 발생한다.



```javascript
// XMLHttpRequest 객체의 생성
var xhr = new XMLHttpRequest();
// 비동기 방식으로 Request를 오픈한다
xhr.open('GET', 'data/test.json');
// Request를 전송한다
xhr.send();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 콜백함수(이벤트 핸들러)를 호출한다.
xhr.onreadystatechange = function (e) {
  // 이 함수는 Response가 클라이언트에 도달하면 호출된다.
};
```

XMLHttpRequest 객체는 response가 클라이언트에 도달했는지를 추적할 수 있는 프로퍼티를 제공한다. 이 프로퍼티가 바로 XMLHttpRequest.readyState이다. 만일 XMLHttpRequest.readyState의 값이 4인 경우, 정상적으로 Response가 돌아온 경우이다.

readXMLHttpRequest.readyState의 값은 아래와 같다.

| Value | State            | Description                                           |
| :---: | :--------------- | :---------------------------------------------------- |
|   0   | UNSENT           | XMLHttpRequest.open() 메소드 호출 이전                |
|   1   | OPENED           | XMLHttpRequest.open() 메소드 호출 완료                |
|   2   | HEADERS_RECEIVED | XMLHttpRequest.send() 메소드 호출 완료                |
|   3   | LOADING          | 서버 응답 중(XMLHttpRequest.responseText 미완성 상태) |
|   4   | DONE             | 서버 응답 완료                                        |

```javascript
// XMLHttpRequest 객체의 생성
var xhr = new XMLHttpRequest();
// 비동기 방식으로 Request를 오픈한다
xhr.open('GET', 'data/test.json');
// Request를 전송한다
xhr.send();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 콜백함수(이벤트 핸들러)를 호출한다.
xhr.onreadystatechange = function (e) {
  // 이 함수는 Response가 클라이언트에 도달하면 호출된다.

  // readyStates는 XMLHttpRequest의 상태(state)를 반환
  // readyState: 4 => DONE(서버 응답 완료)
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status는 response 상태 코드를 반환 : 200 => 정상 응답
  if(xhr.status === 200) {
    console.log(xhr.responseText);
  } else {
    console.log('Error!');
  }
};
```

XMLHttpRequest의.readyState가 4인 경우, 서버 응답이 완료된 상태이므로 이후 [XMLHttpRequest.status](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest/status)가 200(정상 응답)임을 확인하고 정상인 경우, [XMLHttpRequest.responseText](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/responseText)를 취득한다. XMLHttpRequest.responseText에는 서버가 전송한 데이터가 담겨 있다.

**Web Server**
엡 서버는 브라우저와 같은 클라이언트로 부터 HTTP 요청을 받아들이고 HTML 문서와 같은 웹페이지를 반환하는 컴퓨터 프로그램이다.ajax는 웹 서버와의 통신이 필요하므로 웹 서버가 필요하다.node.jsrㅏ 설치 되었면 간단한 Express로 간단한 웹 서버를 생성한다

**Ajax의 예제**

**Load HTML**-ajax를 이용하여 웹페이지를 추가하기 가장 쉬운 데이터 형식은 html 이다. 별도의 작업없이 전송받은 데이터를 dom에 추가하면 된다.

**Load JSON**-서버로부터 브라우저로 전송된 json 데이터는 문자열이다. 이문자열을 객체화 해야 하는데 이를 역직렬화(Deserializing)라 한다. 역직렬화를 위해서 내장 객체json의 static 메소드인 json.pase()를 사용한다.

**Load JSONP**-  요청에 의해 웹페이지가 전달된 서버와 동일한 도메인의 서버로 부터 전달된 데이터는 문제없이 처리할 수 있다. 하지만 보안상의 이유로 다른 도베인(http와https,포트가 다르면 다른 도메인으로 간주한다.)으로의 요청(크로스 도메인 요청)은 제한된다. 이것을 동일 출처 원칙(Same-origin-policy)이라고 한다.

<u>동일출처원칙을 우회하는 세가지 방법이 있다.</u>
**1.웹서버의 프록시 파일**
-서버에 원격 서버로부터 데이터를 수집하는 별도의 기능을 추가하는 것이다. 이를 프록시 라고 한다.
**2.JSONP**
-script 태그의 원본 주소에 대한 제약은 존재하지않는데 이것을 이용하여 다른 도메인의 서버에서 데이터를 수집하는 방법이다. 자신의 서버에 함수를 정의하고 다른 도메인의 서버에 얻고자하는 데이터를 인수로 하는 함수 호출문을 로드하는 것이다.
**3.Cross-Orgin Resource Sharing**
HTTP헤더에 추가적으로 정보를 추가하여 브라우저와 서버가 서로 통신해야 한다는 사실을 알게하는 방법이다. w3c에 명세에 포함되어 있지만 최신 프라우저에서만 동작하여 서버에 HTTP헤러를 설정해 주어야한다.
Node.js로 구현한 서버의 경우,CORS package를 사용하면 간단하게 Cross-Origin-Resource Sharing을 구현할수있다.

~~~javascript
const express=require('express');
const cors=require('cors');
const app=express();

app.use(cors())
app.get('/products/:id',function(req,res,next){
  res.json({msg:'This is CORS-enable for all origins!'})
});
app.listen(80,function(){
  console.log('COR-enable web server listening on port 80')
})
~~~

