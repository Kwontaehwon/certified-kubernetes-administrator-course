## 후기
https://peterica.tistory.com/348

https://taronko.tistory.com/17

## Kubernetes
### Running POD, Deployment 수정 (udemy 68.)
#### POD 수정
1. 현재 실행중인 POD의 .yaml 파일 추출
	- `kubectl edit pod <pod-name>` -> vim 화면이 켜지지만 수정 및 저장은 불가능 -> 나오면 yaml 파일 생성됨
	- `kubectl get pod <pod-name> -o yaml > ouput.yaml`
2. vim 으로 yaml 파일 수정
3. `kubectl delete pod <pod-name>`
4. `kubectl create -f ouptut.yaml`
### Deployment 수정
POD는 `delete` 후 생성해야 하지만 Deployment는 `edit`만 해주면 running 중인 Deployment에 적용됨.
`kubectl edit deployment <deployment-name>`

### 빠르게 POD 삭제하는 법
`kubectl delete pod ubuntu-sleeper --force`
강의에서는 시험에선 써도 되지만 Production 에서는 쓰지 말 것을 권고
#### Cusor 프롬프트
>[!cursor 프롬프트]
> kubernetes, docker 등 관련 용어는 영어로 놔두고, 이미지와 코드블록은 번역하지 말고 나머지만 한국어로 번역해줘

## Linux 명령어
### `grep`
```
grep -i
```

### 줄 수 세기
`kubectl get clusterroles --no-headers | wc -l`

### vim
#### vim 여러줄 탭
`esc + v` 로 visual 모드 들어간 뒤 `>` `<` 로 들여쓰기 내여쓰기

#### undo, redo
`u` : undo
`ctrl + r` : redo

#### 파일 저장
`wq` : 저장 후 종료
`q!` : 저장 안하고 종료

#### 라인 지우기
https://jjeongil.tistory.com/2020
- `.` 현재 라인
- `:.d` 현재라인 지우기

### `ps`
- `ps -aux`
	- -all
	- -user
	- -x (background)


## 기타
`chown` : 파일 소유자 바꾸기
`whoami` : 현재 유저 확인