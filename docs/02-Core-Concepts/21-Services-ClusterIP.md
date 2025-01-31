# 쿠버네티스 서비스 - ClusterIP
  - [비디오 튜토리얼](https://kodekloud.com/topic/services-cluster-ip-2/)로 이동하기
  
이 섹션에서는 쿠버네티스의 **`서비스 - ClusterIP`** 에 대해 알아보겠습니다
## ClusterIP
- Pod 는 각자 IP를 가지고 있지만 그 IP는 영구적이지 않음.
  - Pod가 죽거나 재시작되면 새로운 IP가 할당됨.
- 계층 간 통신이 필요할 떄 어떤 Pod로 요청해야할 지 알 수 없음.
- 이 경우 서비스는 클러스터 내부에 **`CluserIP`** 를 생성하여 프론트엔드 서버 집합과 백엔드 서버 집합과 같은 다른 서비스 간의 통신을 가능하게 합니다.
    
    ![srvc1](../../images/srvc1.PNG)
    
#### 이러한 서비스나 계층 간의 연결을 설정하는 올바른 방법은 무엇일까요?  
- 쿠버네티스 서비스는 파드들을 그룹화하고 그룹 내의 파드에 접근할 수 있는 단일 인터페이스를 제공할 수 있습니다.

  ![srvc2](../../images/srvc2.PNG)
  
#### To create a service of type ClusterIP
- Service 의 기본 Type은 ClusterIP.
```
apiVersion: v1
kind: Service
metadata:
 name: back-end
spec:
 types: ClusterIP
 ports:
 - targetPort: 80
   port: 80
 selector:
   app: myapp
   type: back-end
```
```
$ kubectl create -f service-definition.yaml
```

#### To list the services
```
$ kubectl get services
```
  ![srvc3](../../images/srvc3.PNG)
   
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/

#### 기본서비스인 `kunbernetes`도 CusterIP임.