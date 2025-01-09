# 애플리케이션에서 ConfigMaps 구성하기
  - [비디오 튜토리얼](https://kodekloud.com/topic/configure-configmaps-in-applications/)로 이동하기
  
이 섹션에서는 애플리케이션에서 configmaps를 구성하는 방법을 살펴보겠습니다.

## ConfigMaps
- ConfigMaps 구성에는 2단계가 포함됩니다. 
  - 1. configMaps를 생성합니다.
  - 2. 이를 pod에 주입합니다.
- configmap을 생성하는 방법은 2가지가 있습니다.
  - 명령형 방법
    ```
    $ kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MODE=prod
    $ kubectl create configmap app-config --from-file=app_config.properties (다른 방법)
    ```
    ![cmi](../../images/cmi.PNG)
    
  - 선언형 방법
	  - data 아래에 key-value 포맷으로
    
    ```
    apiVersion: v1
    kind: ConfigMap
    metadata:
     name: app-config
    data:
     APP_COLOR: blue
     APP_MODE: prod
    ```
    ```
    config map 정의 파일을 생성하고 'kubectl create' 명령어를 실행하여 배포합니다.
    $ kubectl create -f config-map.yaml
    ```
    ![cmd1](../../images/cmd1.PNG)
    
 ## ConfigMaps 보기
 - configMaps를 보려면
   ```
   $ kubectl get configmaps (또는)
   $ kubectl get cm
   ```
 - configmaps를 설명하려면
   ```
   $ kubectl describe configmaps
   ```
   
   ![cmv](../../images/cmv.PNG)
   
 ## Pods에서 ConfigMap
 - pod에 configmap 주입하기
	 - `spec.envFrom` 에 `- configMapRef:` 로 적용
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: simple-webapp-color
   spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
      - containerPort: 8080
      envFrom:
      - configMapRef:
          name: app-config
   ```
   ```
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: app-config
   data:
     APP_COLOR: blue
     APP_MODE: prod
   ```
   ```
   $ kubectl create -f pod-definition.yaml
   ```
  
   ![cmp](../../images/cmp.PNG)
   
 #### pod에 구성 변수를 주입하는 다른 방법   
 - **`Single Environment Variable`**로 주입할 수 있습니다. 
 - **`Volume`**에 파일로 주입할 수 있습니다.
 
   ![cmp1](../../images/cmp1.PNG)
   
 #### K8s 참조 문서
 - https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/
 - https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#define-container-environment-variables-using-configmap-data
 - https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#create-configmaps-from-files
