레스트콘트롤러 -> 제이슨으로 request, response

ajax에서 data 보낼 때 JSON.stringify로 보냄

그럼 콘트롤러에서 값 받을 때 RequestBody 사용
    - RequestBody는 자바 VO 객체(참조타입)만 인식 
    - 이말은 즉, 상세보기나 삭제할 때 int idx 형식으로 인자값을 받게 되면 JSON.strinfigy로 data를 전송해도 콘트로러에서 인식불가
       int, String의 경우 원시타입이기 때문
    - 그렇기에 url에 쿼리파라미터를 붙여서 보내게 되고, 매개변수에 @PathVariable을 사용
