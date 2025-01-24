# Practice Test - Cluster Roles
  - Take me to [Practice Test](https://kodekloud.com/topic/practice-test-cluster-roles/)

### 풀이
- ClusterRole 생성
  - `kubectl create clusterrole my-role --verb=get,list,watch --resource=node --dry-run=client -o yaml > my-role`
- CluserRoleBinding 생성
  - `kubectl create clusterrolebinding my-rolebinding --clusterrole=my-role --user=michelle`


Solutions to practice test - cluster roles
- Run the command kubectl get clusterroles --no-headers | wc -l or kubectl get clusterroles --no-headers -o json | jq '.items | length'
  
  <details>
  
  ```
  $ kubectl get clusterroles --no-headers | wc -l (or)
  $ kubectl get clusterroles --no-headers -o json | jq '.items | length'
  ```
  
  </details>
  
- Run the command kubectl get clusterrolebindings --no-headers | wc -l or kubectl get clusterrolebindings --no-headers -o json | jq '.items | length'

  <details>
  
  ```
  $ kubectl get clusterrolebindings --no-headers | wc -l (or)
  $ kubectl get clusterrolebindings --no-headers -o json | jq '.items | length'
  ```
  
  </details>
  
- What namespace is the cluster-admin clusterrole part of?

  <details>
  
  ```
  $ Cluster roles are cluster wide and not part of any namespace
  ```
  
  </details>
  
- Run the command kubectl describe clusterrolebinding cluster-admin
  
  <details>
  
  ```
  $ kubectl describe clusterrolebinding cluster-admin
  ```
  
  </details>
  
- Run the command kubectl describe clusterrole cluster-admin

  <details>
  
  ```
  $ kubectl describe clusterrole cluster-admin
  ```
  
  </details>
  
- Check answer at /var/answers

  <details>
  
  ```
  $ kubectl create -f /var/answers/michelle-node-admin.yaml
  ```
  
  </details>
  
- Check answer at /var/answers

  <details>
  
  ```
  $ kubectl create -f /var/answers/michelle-storage-admin.yaml
  ```
  
  </details>
  
  




