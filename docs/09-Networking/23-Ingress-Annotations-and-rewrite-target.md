# Ingress Annotations and rewrite-target
  - Take me to [Lecture](https://kodekloud.com/topic/ingress-annotations-and-rewrite-target/)

> [!Summary]
> 1. URL 경로 변경:
>     - 인그레스로 들어오는 요청의 URL 경로를 백엔드 서비스가 예상하는 경로로 변경할 수 있습니다.
>     - 예: `/something/test` -> `/test`
>     
> 2. 정규 표현식 지원:
>     - 복잡한 URL 패턴을 처리하기 위해 정규 표현식을 사용할 수 있습니다.

In this section, we will take a look at **Ingress annotations and rewrite-target**
- Different Ingress controllers have different options to customize the way it works. Nginx Ingress Controller has many options but we will take a look into the one of the option "Rewrite Target" option.
- Kubernetes Version 1.18
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /pay
        backend:
          serviceName: pay-service
          servicePort: 8282

```

 // Start of Selection
`watch` 앱은 비디오 스트리밍 웹페이지를 `http://<watch-service>:<port>/`에서 표시합니다.  
`wear` 앱은 의류 웹페이지를 `http://<wear-service>:<port>/`에서 표시합니다.  

Ingress를 구성하여 아래와 같은 동작을 구현해야 합니다. 사용자가 왼쪽의 URL을 방문하면, 그의 요청은 오른쪽의 URL로 내부적으로 전달되어야 합니다. `/watch`와 `/wear` URL 경로는 ingress 컨트롤러에서 구성하는 것으로, 사용자를 백엔드의 적절한 애플리케이션으로 전달할 수 있습니다. 애플리케이션은 이 URL/경로가 구성되어 있지 않습니다:  

`http://<ingress-service>:<ingress-port>/watch` --> `http://<watch-service>:<port>/`  
`http://<ingress-service>:<ingress-port>/wear` --> `http://<wear-service>:<port>/`  

`rewrite-target` 옵션이 없으면, 다음과 같은 결과가 발생합니다:  
`http://<ingress-service>:<ingress-port>/watch` --> `http://<watch-service>:<port>/watch`  
`http://<ingress-service>:<ingress-port>/wear` --> `http://<wear-service>:<port>/wear`  

대상 URL의 끝에 `watch`와 `wear`가 붙는 것을 주목하십시오. 대상 애플리케이션은 `/watch` 또는 `/wear` 경로로 구성되어 있지 않습니다. 이들은 각각의 목적을 위해 특별히 구축된 다른 애플리케이션이므로, URL에서 `/watch` 또는 `/wear`를 기대하지 않습니다. 따라서 요청은 실패하고 `404` not found 오류가 발생합니다.  

이를 해결하기 위해 요청이 watch 또는 wear 애플리케이션으로 전달될 때 URL을 "ReWrite"하고자 합니다. 사용자가 입력한 동일한 경로를 전달하고 싶지 않습니다. 따라서 `rewrite-target` 옵션을 지정합니다. 이는 `rules->http->paths->path` 아래에 있는 값을 `rewrite-target`의 값으로 대체하여 URL을 재작성합니다. 이는 검색 및 대체 기능과 유사하게 작동합니다.  

#### Reference Docs
- https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/
- https://kubernetes.github.io/ingress-nginx/examples/
- https://kubernetes.github.io/ingress-nginx/examples/rewrite/
- https://github.com/kubernetes/ingress-nginx/blob/master/docs/troubleshooting.md