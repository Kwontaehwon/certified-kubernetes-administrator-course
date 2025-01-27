# 쿠버네티스 서비스
  - [비디오 튜토리얼](https://kodekloud.com/topic/services-3/)로 이동하기
  
이 섹션에서는 쿠버네티스의 **`Service`** 에 대해 알아보겠습니다

## 서비스
- 쿠버네티스 서비스는 애플리케이션 내부 및 외부의 다양한 구성 요소 간의 통신을 가능하게 합니다.

  ![srv1](../../images/srv1.PNG)
  
#### 네트워킹의 다른 측면들을 살펴보겠습니다

## 외부 통신
- **`외부 사용자`** 로서 우리는 어떻게 **`웹 페이지`** 에 접근할 수 있을까요?
  - 노드에서 (예상대로 애플리케이션에 도달할 수 있음)
  
    ![srv2](../../images/srv2.PNG)
    
  - 외부 세계에서 (중간에 무언가 없다면 애플리케이션에 도달할 수 없음)
    - k8s NodePort서비스 필요
  
    ![srv3](../../images/srv3.PNG)
   

## 서비스 유형 
 #### 쿠버네티스에는 3가지 유형의 서비스가 있습니다
 ![srv-types](../../images/srv-types.PNG)
 1. NodePort
    - 서비스가 내부 포트를 노드의 포트에서 접근 가능하게 만듭니다.
      - Nodeport default port 범위 : 30000 ~ 32767
      - ports 하위에서 `port`만 필수. 
        - `targetPort`를 생략할 경우 port와 동일하게 설정됨
        - `nodePort`를 생략할 경우 30000 ~ 32767 안에서 여유 있는 포트로 랜덤하게 설정됨
      ```
      apiVersion: v1
      kind: Service
      metadata:
       name: myapp-service
      spec:
       types: NodePort
       ports:
       - targetPort: 80 // 1번
         port: 80 // 2번
         nodePort: 30008 // 3번
      ```
     ![srvnp](../../images/srvnp.PNG)
      
      #### 서비스를 파드에 연결하려면
      - Pods의 Label을 Selector에 지정
      ```
      apiVersion: v1
      kind: Service
      metadata:
       name: myapp-service
      spec:
       type: NodePort
       ports:
       - targetPort: 80
         port: 80 
         nodePort: 30008 
       selector:
         app: myapp
         type: front-end
       ```

    ![srvnp1](../../images/srvnp1.PNG)
      
      #### 서비스를 생성하려면
      ```
      $ kubectl create -f service-definition.yaml
      ```
      
      #### 서비스 목록을 보려면
      ```
      $ kubectl get services
      ```
      
      #### 웹 브라우저 대신 CLI에서 애플리케이션에 접근하려면
      ```
      $ curl http://192.168.1.2:30008
      ```
      
      ![srvnp2](../../images/srvnp2.PNG)

      #### 여러 파드가 있는 서비스
      
      ![srvnp3](../../images/srvnp3.PNG)
      
      #### 파드가 여러 노드에 분산되어 있을 때
      - 파드와 노드가 어떻게 되어있든 서비스는 동일하게 동작한다.
     
      ![srvnp4](../../images/srvnp4.PNG)
     
            
 2.  ClusterIP
    - 이 경우 서비스는 클러스터 내부에 **`가상 IP`** 를 생성하여 프론트엔드 서버 집합과 백엔드 서버 집합과 같은 다른 서비스 간의 통신을 가능하게 합니다.
    
 3. LoadBalancer
    - 서비스가 지원되는 클라우드 공급자에서 애플리케이션을 위한 **`LoadBalancer`** 를 프로비저닝합니다.
    
K8s 참조 문서:
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/
