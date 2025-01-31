# Practice Test CKA Ingress 1

  - Take me to [Practice Test](https://kodekloud.com/topic/practice-test-cka-ingress-networking-1/)

### 풀이
- Ingress Resource 의 정보를 얻기 위해서는 `kubectl get ingress`
	- Ingress Resource 의 Type이 `Ingress` 이기 때문.
	- `kubectl get all -A` 로 나오지 않음.
- 11번 : Ingress Default 확인
	- `kubectl get deploy ingress-nginx-controller -n ingress-nginx -o yaml` 로 controller에 정의된 default 를 확인해야함.
- 마지막 문제 : `nginx.ingress.kubernetes.io/rewrite-target: /`
	- [23-Ingress-Annotations-and-rewrite-target](23-Ingress-Annotations-and-rewrite-target.md)
	- 각 서비스에 path 가 연결되어 있지 않다면, 각 서비스로 연결해주기 위해서는 위 옵션을 사용해야 한다. 
		- ex. `http://<ingress-service>:<ingress-port>/watch` 가 들어오면, 
		  `http://<watch-service>:<port>/` 로 변경하여 `watch-service` 로 변경해줌.
			  - 만약 이 옵션을 사용하지 않으면 `http://<watch-service>:<port>/watch` 가 되어 연결되지 않음.
#### Solution 

  1. Check the Solution

     <details>

      ```
      Ok
      ```
     </details>
  
  2. Check the Solution

     <details>

      ```
      INGRESS-SPACE
      ```
     </details>

  3. Check the Solution

     <details>

      ```
      NGINX-INGRESS-CONTROLLER
      ```
     </details>

  4. Check the Solution

     <details>

      ```
      APP-SPACE
      ```
     </details>

  5. Check the Solution

     <details>

      ```
      3
      ```
     </details>

  6. Check the Solution

     <details>

      ```
      APP-SPACE
      ```
     </details>

  7. Check the Solution

     <details>

      ```
      INGRESS-WEAR-WATCH
      ```
     </details>

  8. Check the Solution

     <details>

      ```
      ALL-HOSTS(*)
      ```
     </details>

  9. Check the Solution

     <details>

      ```
      WEAR-SERVICE
      ```
     </details>

  10. Check the Solution

      <details>

       ```
        /WATCH
       ```
      </details>

  11. Check the Solution

      <details>

       ```
        DEFAULT-HTTP-BACKEND
       ```
      </details>

  12. Check the Solution

      <details>

       ```
        404-ERROR-PAGE
       ```
      </details>

  13. Check the Solution

      <details>

       ```
        OK
       ```
      </details>

  14. Check the Solution

      <details>
 
        ```
        kubectl edit ingress --namespace app-space
        ```
        Change the path from /watch to /stream
    
        OR
 
        ```yaml
        apiVersion: v1
        items:
        - apiVersion: extensions/v1beta1
          kind: Ingress
          metadata:
            annotations:
              nginx.ingress.kubernetes.io/rewrite-target: /
              nginx.ingress.kubernetes.io/ssl-redirect: "false"
            name: ingress-wear-watch
            namespace: app-space
          spec:
            rules:
            - http:
                paths:
                - backend:
                    serviceName: wear-service
                    servicePort: 8080
                  path: /wear
                  pathType: ImplementationSpecific
                - backend:
                    serviceName: video-service
                    servicePort: 8080
                  path: /stream
                  pathType: ImplementationSpecific
          status:
            loadBalancer:
              ingress:
              - {}
        kind: List
        metadata:
          resourceVersion: ""
          selfLink: ""
       ```
      </details>

  15. Check the Solution

      <details>

       ```
        OK
       ```
      </details>

  16. Check the Solution

      <details>

       ```
        404 ERROR PAGE
       ```
      </details>

  17. Check the Solution

      <details>

       ```
        OK
       ```
      </details>

  18. Check the Solution

      <details>

        Run the command `kubectl edit ingress --namespace app-space` and add a new Path entry for the new service.

        OR

       ```yaml
       apiVersion: v1
       items:
       - apiVersion: extensions/v1beta1
         kind: Ingress
         metadata:
           annotations:
             nginx.ingress.kubernetes.io/rewrite-target: /
             nginx.ingress.kubernetes.io/ssl-redirect: "false"
           name: ingress-wear-watch
           namespace: app-space
         spec:
           rules:
           - http:
               paths:
               - backend:
                   serviceName: wear-service
                   servicePort: 8080
                 path: /wear
                 pathType: ImplementationSpecific
               - backend:
                   serviceName: video-service
                   servicePort: 8080
                 path: /stream
                 pathType: ImplementationSpecific
               - backend:
                   serviceName: food-service
                   servicePort: 8080
                 path: /eat
                 pathType: ImplementationSpecific
         status:
           loadBalancer:
             ingress:
             - {}
       kind: List
       metadata:
         resourceVersion: ""
         selfLink: ""
       ```
      </details>

  19. Check the Solution

      <details>

       ```
        OK
       ```
      </details>

  20. Check the Solution

      <details>

       ```
        CRITICAL-SPACE
       ```
      </details>

  21. Check the Solution

      <details>

       ```
        WEBAPP-PAY
       ```
      </details>

  22. Check the Solution

      <details>

      ```yaml
      apiVersion: networking.k8s.io/v1
      kind: Ingress
      metadata:
        name: test-ingress
        namespace: critical-space
        annotations:
          nginx.ingress.kubernetes.io/rewrite-target: /
          nginx.ingress.kubernetes.io/ssl-redirect: "false"
      spec:
        rules:
        - http:
            paths:
            - path: /pay
              pathType: Prefix
              backend:
                service:
                  name: pay-service
                  port:
                    number: 8282 
       ```
        </details>

  23. Check the Solution

      <details>

       ```
        OK
       ```
      </details>
