# 인증서 세부정보 보기
  - [비디오 튜토리얼](https://kodekloud.com/topic/view-certificate-details/)로 이동하기
  
이 섹션에서는 Kubernetes 클러스터에서 인증서를 보는 방법을 살펴본다.

## 인증서 보기 
 ![hrd](../../images/hrd.PNG)

 ![hrd1](../../images/hrd1.PNG)
`/etc/kubernetes/manifests` 아래에 있는 kube-api server.yaml 을 확인하고 이 파일에 정의되어 있는 하위 인증서를 확인

 - 인증서의 세부정보를 보려면
   ```
   $ openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
   ```
   
   ![hrd2](../../images/hrd2.PNG)
   
#### 다른 모든 인증서에 대한 정보를 식별하기 위해 동일한 절차를 따르기

   ![hrd3](../../images/hrd3.PNG)
   
## 서버 로그 검사 - 하드웨어 직접 설정
- journalctl을 사용하여 서버 로그 검사
  ```
  $ journalctl -u etcd.service -l
  ```
  
  ![hrd4](../../images/hrd4.PNG)
  
## 서버 로그 검사 - kubeadm 설정
- kubectl을 사용하여 로그 보기
  ```
  $ kubectl logs etcd-master
  ```
  ![hrd5](../../images/hrd5.PNG)
  
- docker ps 및 docker logs를 사용하여 로그 보기
  ```
  $ docker ps -a
  $ docker logs <container-id>
  ```
  ![hrd6](../../images/hrd6.PNG)
  
#### K8s 참조 문서
- https://kubernetes.io/docs/setup/best-practices/certificates/#certificate-paths
