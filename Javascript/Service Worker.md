## Service Worker
- 웹브라우저가 실행되지 않은 상태에서도 웹브라우저가 웹페이지와 별개로 백그라운드에서 실행되는 스크립트다.
  - 오프라인 상황에서 브라우저가 뭘 해야될지 개발자가 제어할 수 있도록 한 것.
<br>

- 캐시와 푸시 알림의 기능을 제공하여 스마트폰 앱을 사용하는 것처럼 웹 앱을 사용할 수 있게 해준다.

- 웹 앱의 일부 기능은 오프라인에서도 동작하기 때문에 웹페이지를 열지 않아도 최신 정보로 갱신될 수 있다.

- 즉, 오프라인에서도 유저에게 경험을 제공해야하는 PWA(Progressive Web Apps)의 핵심이다.
<br>
<hr>
<br>

### ✔ 동작 흐름
- 서비스 워커는 브라우저의 캐시를 뒤져서 클라이언트에게 제공하는 역할을 한다.

- 네트워크가 오프라인일 때(인터넷이 끊겼을 때), 혹은 웹 브라우저를 종료했을 때<br>
캐시 데이터가 있다면, 그리고 클라이언트가 무언가 요청을 한다면,<br>
인터넷을 통해(네트워크를 통해) 데이터를 가져와 보여주는 것이 아니라<br>
서비스 워커가 캐시의 데이터를 뒤져서 그에 맞는 데이터를 클라이언트에게 보여준다.
<br>

- 한마디로 이전에 봤었거나 다운로드했던 데이터들을 오프라인인 상태에서도 볼 수 있다는 말이다.
<br>
<hr>
<br>

### ✔ 생명 주기
![image](https://github.com/yejun95/Today-I-Learned/assets/121341413/69698c7c-d24d-4532-a34c-4e4d6e3692b0)

- register : 서비스워커를 설치 하려면 서비스워커를 등록해야 된다.
```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/sw.js').then(function() {
      console.log('ServiceWorker registration successful');
    }).catch(function(err) {
      console.log('ServiceWorker registration failed: ', err);
    });
  });
}
```
<br>
<br>

- Installing : 해당페이지를 처음 방문할 때 install 이벤트 발생, 페이지 자산을 캐시하는 곳
  - 설치 과정에서 정적 자원을 캐싱한다.
```javascript
self.addEventListener('install', event => {
 console.log('cacheName1 installing..');
event.waitUntil(
  caches.open('cacheName1').then(cache =>   cache.addAll(['/aa.do','/common/css/bb.css','/common/js/cc.js']))
 )
});
```
<br>
<br>

- Activated : 설치된 서비스 워커가 제어권을 갖은 상태, push 및 sync와 같은 함수가 처리할 준비가 됨
```javascript
self.addEventListener('activate', function(e) {
   e.waitUntil(
    caches.keys().then(function(keyList) {
          return Promise.all(
              keyList.map(function(key) {
                if ("cacheName1".indexOf(key) == -1) {
                   return caches.delete(key);
                }
              })
         );
    })
  );
});
```
<br>
<br>

- Error : 설치 과정에서 에러가 나면 그대로 종료된다.

- Idle : 사용하지 않을 때는 휴먼 상태에 있다가 필요시에만 해당 기능을 수행한다.

- Terminated : 메모리 상태에 따라 자체적으로 종료한다.
<br>

- fetch : 서비스워커를 설치 완료 후 캐시된 응답을 반환받음.
  - 해당 소스는 네트웍 데이터가 있을경우 cache를 update
```javascript
self.addEventListener('fetch', function(event) {
 event.respondWith(
  caches.open('cacheName1').then(function(cache) {
   if (event.request.clone().method == "GET") {
      return cache.match(event.request).then(function(response) {
        var fetchPromise =   
           fetch(event.request).then(function(networkResponse) {
             cache.put(event.request, networkResponse.clone());
           return networkResponse;
        });
      return response || fetchPromise;
      });
   }
  })
 );
});
```
<br>
<hr>
<br>

**Reference**<br>

[bsc0227 : PWA, Service worker에 대한 정리](https://bscnote.tistory.com/33)<br>
[개발자 찐빵이 : 서비스 워커의 라이프 사이클 (Life cycle of Service worker)](https://just-my-blog.tistory.com/111)<br>
[박상수 : Service Worker Cache](https://pakss328.medium.com/service-worker-cache-5d1097a7dd31)
