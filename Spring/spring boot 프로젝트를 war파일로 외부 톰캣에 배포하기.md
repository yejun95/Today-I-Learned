## spring boot 프로젝트를 war파일로 외부 톰캣에 배포하기
- VM arguments를 이용하여 profile을 설정하기 전 외부 톰캣에 SpringBoot 프로젝트가 배포 되어 있어야
profile 수정이 된 결과를 확인할 수 있기에 배포 먼저 진행한다.

- 배포 전 #7  에 따른 설정을 진행해야 한다.

### ✔ maven 빌드 진행
- 프로젝트 우 클릭 > Run As > Maven Build  > clean package를 입력
  - clean : 이전 빌드로 생성된 모든 파일을 제거한다.
  - package : 컴파일된 코드를 JAR 혹은 WAR 포멧으로 패키징한다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/54b24745-683a-4cbc-bfc5-ebcad5bcebba)
<br>
<br>

**error 발생**
- 이클립스에는 기본적으로 자바 실행 도구가 JRE(Java Runtime Environment) 로 연결이 되어 있다.

-  에러 해결을 위해서는 JRE 가 아닌 JDK(Java Development Kit) 로 변경해야 한다. 

- JDK 에는 프로그래밍에 필요한 컴파일러 등이 포함되어 있기 때문이다. 
 
- 즉, Maven 배포는 JDK 가 필요하다.

```
[ERROR] COMPILATION ERROR : 
[INFO] -------------------------------------------------------------
[ERROR] No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?
[INFO] 1 error
```
<br>

- 상단 탭 Window > Preferences > Java > installed JREs 에서 설치되어 있는 JDK 경로를 추가한다.
  - Add 버튼을 눌러 JDK 폴더를 지정한다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/5126f11e-bf8d-43f3-aa58-9aee5bbe4258)
<br>
<br>

- 이후 프로젝트 우클릭 > Run As > Run Configurations 에서 JDK가 잘 적용됐는지 확인한다.
  - 설정이 잘 됐다면 run을 눌러 build한다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/8c271c5e-fb6d-49c9-b649-3fd43b8d3a78)
<br>
<br>

- 이전과 다르게 BUILD SUCCESS 문구를 볼 수 있다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a297aa0b-2688-44c2-a051-042a2450d2df)
<br>
<br>

- target 폴더에 build된 war 파일 확인

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/980ac91b-ae7b-43a7-b696-8df78940ac9d)
<br>
<hr>
<br>

### ✔ 톰캣에 war 배포
- 이전에 build되어 생긴 war 파일을 tomcat 폴더 webapps에 넣는다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/05b04587-c666-4779-a012-a3ddd7de4c8f)
<br>
<br>

- 명령어를 통해 톰캣을 실행시킨다.
  - IDE 가 없다는 가정하에 실습을 하기 때문에 명령어를 사용하는 것.
  - 톰캣 / bin 폴더에서 `catalina.bat run` 실행
 - `java -jar ~~~.war` 명령어를 통해 war 파일을 직접 실행시킬 수 있으나 이는 내장 톰캣을 동작하는 것이기 
 때문에 서블릿 컨테이너에서 실행되기 위해서는 `catalina.bat run`으로 실행하여 압축 파일이 풀려야 한다.
 물론 내장 톰캣으로 서비스를 돌린다면 해당 명령어로도 무방하다.
 현재 실습 목표는 외장 톰캣에 서비스를 진행하기 위한 것이다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/2df03393-c92d-4a37-bd26-d0daa2afea40)
<br>
<br>

- war 압축 파일이 풀리면서 폴더 형태로 나타난다.

- 💡 주의점 : ~~.war 파일의 이름 자체가 contextPath로 지정된다.
  - ex) test.war 로 배포 -> localhost:8080/test 이렇게 해야 접속이 된다.
  - 기본적으로 ROOT 폴더를 root 경로로 실행하고 있으니, 해당 폴더를 지우고 ROOT 로 이름 설정해도 된다.
  - `server.xml`에서 root로 지정될 폴더의 이름 변경도 가능

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a0757a82-ee9a-4fb6-ba15-4bb74663b4e5)
<br>
<br>

- 테스트용 컨트롤러를 사용해서 문자열을 찍어 봤다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/c3493a42-131a-4a9f-8f50-fd9181bbb08c)
<br>
<hr>
<br>

### ✔ root 경로 변경
- 현재 webapps / ROOT 폴더를 root 경로로 바라보고 있는데, 이를 수정하여 bootAndvue를 켜지게 하겠다.

- `server.xml`에서 `<Host></Host>` 태그 쪽에 해당 내용을 추가한다.
  - `<Context path="/" docBase="bootAndvue"  reloadable="false" > </Context>   `
<br>

- 이렇게 하면 root 경로에 docBase로 설정한 bootAndvue가 나오게 된다.

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/3ff9ab3d-ac97-43f0-89c3-f700b9027154)
<br>
<br>

- 마지막으로 ROOT 폴더를 없애고 bootAndvue.war 파일의 이름을 ROOT.war로 적용해서 진행해본다.
  - ROOT.war로 이름 변경 후 톰캣 재시작
  - root 경로에서 웹 페이지가 잘 나온다.
<br>

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/a57c49d8-9343-406d-af5c-0a1ef2499fdf)

![image](https://github.com/BJSNuruhee/levelup/assets/121341413/eae78d40-3ac8-43ea-bffb-a09545a5148e)

