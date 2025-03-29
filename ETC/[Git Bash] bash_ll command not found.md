# git bash에서 ll command가 되지 않을 때
- 기본적으로 git bash에서 `ll`을 통해 `ls -l`과 동일하게 출력이 가능하다.

- 되지 않을 경우 터미널에서 아래와 같이 입력하면 사용이 가능

```
echo "alias ll='ls --color=auto -alF'" >> ~/.bashrc
source ~/.bashrc
```

