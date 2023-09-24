## chown
- 파일이나 폴더 또는 하위 경로의 소유자를 변경한다.

- 명령어
```
chown [옵션] [소유자:소유그룹] [파일 또는 폴더]
```
<br>
<hr>
<br>

### ✔ 옵션
- R : 하위 경로의 소유자를 모두 변경합니다. 

- f : 소유자 변경이 안 될때 오류 메시지 표출한다.

- c : 변경된 파일을 자세히 표출한다.

- v : 작업상태를 출력한다.

- help : 도움말을 보여준다.

- version : 버전 정보를 보여준다.
<br>
<hr>
<br>

### ✔ 예시
- "testfile1.txt" 의 소유자(owner)를 "user1"으로 변경한다.
```
[root@itworld ~]# chown user1 testfile1.txt

[root@itworld ~]# -l 

-rwxr-xr-x 1 user1 user2 6  8월 18 10:08 testfile1.txt
```
<br>

- "testfile1.txt" 의 소유그룹(group)을 "user3"으로 변경한다.
```
[root@itworld ~]# chown :user3 testfile1.txt

[root@itworld ~]# -l 

-rwxr-xr-x 1 user1 user3 6  8월 18 10:18 testfile1.txt
```
<br>

- "testfile1.txt" 의 소유자를 "user2", 소유그룹을 "user5"로 변경한다.
```
[root@itworld ~]# chown user2:user5 testfile1.txt

[root@itworld ~]# -l 

-rwxr-xr-x 1 user2 user5 6  8월 18 10:10 testfile1.txt
```
<br>

- "/home/etc/test1"폴더를 포함한 하위 디렉토리의 소유자, 그룹을 모두 "user1"로 변경한다.
```
[root@itworld ~]# chown -R user1:user1 /home/etc/test1
```
<br>
<hr>
<br>

**Reference**<br>
[IT 개발자와 부동산 : [Linux]리눅스 파일, 폴더 소유자 변경 chown](https://itworld.gmax8.com/24)<br>
