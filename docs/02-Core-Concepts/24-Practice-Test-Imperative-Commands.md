# Practice Test - Imperative Commands
  - Take me to [Practice Test](https://kodekloud.com/topic/practice-test-imperative-commands-2/)

Solutions for Practice Test - Imperative Commands

### 풀이
- 2
  - create로 pod 만드는 것 불가능 -> 단순하게 `run` 명령어 사용
  - `run` 명령어 사용시 `pods` 같은거 안붙고 바로 이름 붙여야함.
- 4
  - port 열때 serivce를 만드는게 아니라 `expose` 명령어 사용
- 9번 : pod 만들고 port 오픈 (아래 두개는 동일)
  - ```
    kubectl run httpd --image httpd:alpine --expose --port 80
    ```
  - ```
    kubectl run httpd --image httpd:alpine
    kubectl expose pods httpd --port=80
    ```
1. Information

1.  <details>
    <summary>Deploy a pod named nginx-pod using the nginx:alpine image.</summary>

    ```
    kubectl run nginx-pod --image=nginx:alpine
    ```
    </details>

1.  <details>
    <summary>Deploy a redis pod using the redis:alpine image with the labels set to tier=db.</summary>

    ```
    kubectl run redis --image=redis:alpine -l tier=db
    ```
    </details>

1.  <details>
    <summary>Create a service redis-service to expose the redis application within the cluster on port 6379.</summary>

    ```
    kubectl expose pod redis --port=6379 --name redis-service
    ```
    </details>

1.  <details>
    <summary>Create a deployment named webapp using the image kodekloud/webapp-color with 3 replicas.</summary>

    ```
    kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3
    ```
    </details>

1.  <details>
    <summary>Create a new pod called custom-nginx using the nginx image and expose it on container port 8080.</summary>

    ```
    kubectl run custom-nginx --image=nginx --port=8080
    ```
    </details>

1.  <details>
    <summary>Create a new namespace called dev-ns.</summary>

    ```
    kubectl create ns dev-ns
    ```
    </details>

1.  <details>
    <summary>Create a new deployment called redis-deploy in the dev-ns namespace with the redis image. It should have 2 replicas.</summary>

    ```
    kubectl create deployment redis-deploy -n dev-ns --image redis --replicas 2
    ```
    </details>

1.  <details>
    <summary>Create a pod called httpd using the image httpd:alpine in the default namespace.</br>Next, create a service of type ClusterIP by the same name (httpd).</br>The target port for the service should be 80.</summary>

    ```
    kubectl run httpd --image httpd:alpine --expose --port 80
    ```
    </details>


