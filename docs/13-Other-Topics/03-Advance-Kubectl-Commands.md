# Advance Kubectl Commands
  - Take me to the [Lecture](https://kodekloud.com/topic/advanced-kubectl-commands/)

### `Kubectl` 은 JSON 통신
![](images/03-Advance-Kubectl-Commands.png)
`kubectl` 은 kube-apiserver 를 통해 정보를 가져오는데 이때 사용되는 것이 **JSON**
`kubectl` 혹은 `kubectl -o wide` 를 통해 볼 수 있는 정보도 있지만 볼 수 없는 정보도 있음.
-> (Node CPU, Taints 등)

### kubectl JSON PATH 사용
![](images/03-Advance-Kubectl-Commands-1.png)


  - To get the output of **`kubectl`** in a json format: 
    ```
    kubectl get nodes -o json
    ```

    ```
    kubectl get pods -o json 
    ```
    ![pod](../../images/jpod.PNG)

  - To get the image name used by pod via json path query:
    ```
    kubectl get pods -o=jsonpath='{.items[0].spec.containers[0].image}'
    ```

  - To get the names of node in the cluster:
    ```
    kubectl get pods -o=jsonpath='{.items[*].metadata.name}'
    ```
    ![node](../../images/jnode.PNG)


  - To get the architecture of node in the cluster:
    ```
    kubectl get pods -o=jsonpath='{.items[*].status.nodeInfo.architecture}'
    ```

  - To get the count of the cpu of node in the cluster:
    ```
    kubectl get pods -o=jsonpath='{.items[*].status.status.capacity.cpu}'
    ```

#### Loops - Range
![](images/03-Advance-Kubectl-Commands-2.png)
  - To print the output in a separate column (one column with node name and other with CPU count):
    ```
    kubectl get nodes -o=custom-columns=NODE:.metadata.name ,CPU:.status.capacity.cpu
    ```
    
    ![loop](../../images/loop.PNG)
### JSON PATH for Sort
![](images/03-Advance-Kubectl-Commands-3.png)
  - Kubectl comes with a **`sort by`** property which can be combined with json path query to **`sort`** by name or **`CPU count`**
    ```
    kubectl get nodes --sort-by=.metadata.name
    ```

    ![loop](../../images/loop.PNG)

    ```
    kubectl get nodes --sort-by=..status.capacity.cpu
    ```
