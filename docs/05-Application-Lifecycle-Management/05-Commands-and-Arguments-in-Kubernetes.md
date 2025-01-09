# Commands and Arguments in Kubernetes
  - [비디오 튜토리얼](https://kodekloud.com/topic/commands-and-arguments-in-kubernetes-2/)로 이동하기

이 섹션에서는 kubernetes의 명령어와 인수에 대해 살펴보겠습니다.

- docker run 명령어에 추가된 모든 것은 pod 정의 파일의 **`args`** 속성에 배열 형태로 들어갑니다.
- command 필드는 Dockerfile의 entrypoint 지시문에 해당하므로 요약하자면 Dockerfile의 2개의 지시문에 해당하는 2개의 필드가 있습니다.

| docker       | kubernetes |
| ------------ | ---------- |
| `ENTRYPOINT` | `command`  |
| `cmd`        | `args`     |

  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: ubuntu-sleeper-pod
  spec:
   containers:
   - name: ubuntu-sleeper
     image: ubuntu-sleeper
     command: ["sleep2.0"]
     args: ["10"]
  ```
  ![args](../../images/args.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/
