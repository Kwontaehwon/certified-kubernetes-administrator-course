# Docker vs. ContainerD
이 섹션에서는 Docker와 ContainerD의 차이점을 살펴본다.

Docker와 `containerd`는 여러 번 접하게 될 것이다. 앞으로 오래된 블로그나 문서 페이지를 읽으면 Kubernetes와 함께 Docker가 언급되는 것을 볼 것이고, 새로운 블로그를 읽으면 `containerd`가 등장할 것이며 두 가지의 차이점이 궁금해질 것이다. 또한 `ctr`, `crictl`, `nerdctl`과 같은 몇 가지 CLI 도구가 있으며, 이 도구들이 무엇인지, 어떤 것을 사용해야 하는지에 대해 설명할 것이다.

![](../../images/02-03-01.png)

컨테이너 시대의 시작으로 돌아가 보자. 처음에는 Docker만 있었다. Rocket([rkt](https://www.redhat.com/en/topics/containers/what-is-rkt))와 같은 몇 가지 도구가 있었지만, Docker의 사용자 경험은 컨테이너 작업을 매우 간단하게 만들어 주었고, 따라서 Docker는 가장 지배적인 컨테이너 도구가 되었다. 그리고 Kubernetes가 Docker를 오케스트레이션하기 위해 등장했다. Kubernetes는 처음에 Docker를 오케스트레이션하기 위해 만들어졌기 때문에 Docker와 Kubernetes는 밀접하게 결합되어 있었고, 그 당시 Kubernetes는 Docker와만 작동하며 다른 컨테이너 솔루션을 지원하지 않았다.

![](../../images/02-03-02.png)

Kubernetes는 컨테이너 오케스트레이터로서 인기를 얻었고, 이제 `rkt`와 같은 다른 컨테이너 런타임도 필요해졌다. Kubernetes 사용자는 Docker 외의 다른 컨테이너 런타임과도 작동해야 했고, 그래서 Kubernetes는 컨테이너 런타임 인터페이스(Container Runtime Interface, CRI)라는 인터페이스를 도입했다. CRI는 공급업체가 OCI(Open Container Initiative) 표준을 준수하는 한 Kubernetes의 컨테이너 런타임으로 작동할 수 있도록 허용했다. OCI는 이미지 사양과 런타임 사양으로 구성된다. 이미지 사양은 이미지가 어떻게 구축되어야 하는지에 대한 사양을 의미하며, 런타임 사양은 모든 컨테이너 런타임이 개발되어야 하는 표준을 정의한다. 이러한 표준을 염두에 두고 누구나 Kubernetes와 함께 사용할 수 있는 컨테이너 런타임을 구축할 수 있다. 이것이 아이디어였다.

![](../../images/02-03-03.png)

`rkt`와 OCI 표준을 준수하는 다른 컨테이너 런타임은 이제 CRI를 통해 Kubernetes의 컨테이너 런타임으로 지원되지만, Docker는 CRI 표준을 지원하도록 설계되지 않았다. Docker는 CRI가 도입되기 훨씬 이전에 만들어졌고, 여전히 대부분의 사용자가 사용하는 지배적인 컨테이너 도구였다. 따라서 Kubernetes는 Docker에 대한 지원을 계속해야 했고, 이를 위해 `dockershim`이라는 임시적인 방법을 도입했다. 이는 CRI 외부에서 Docker를 지원하기 위한 해킹 방식이었다.

![](../../images/02-03-04.png)

대부분의 다른 컨테이너 런타임이 CRI에 맞춰 작동하는 동안, Docker는 계속해서 CRI 없이 작동했다. 이제 Docker는 단순한 컨테이너 런타임이 아님을 알 수 있다. Docker는 여러 도구로 구성되어 있으며, 예를 들어 Docker CLI, Docker API, 이미지를 구축하는 데 도움을 주는 빌드 도구가 있다. 볼륨, 보안 지원이 있었고, 마지막으로 `runc`라는 컨테이너 런타임과 이를 관리하는 데몬인 `containerd`가 있었다. 따라서 containerd는 CRI와 호환되며 다른 런타임과 마찬가지로 Kubernetes와 직접 작동할 수 있다. containerd는 Docker와 분리된 런타임으로 사용할 수 있다.

![](../../images/02-03-05.png)

이제 containerd는 별도의 런타임으로 존재하고 Docker는 별도로 존재한다. Kubernetes는 Docker 엔진에 대한 지원을 계속 유지했지만, dockershim을 유지하는 것은 불필요한 노력과 복잡성을 초래했다. 그래서 Kubernetes의 v1.24 릴리스에서 dockershim을 완전히 제거하기로 결정했고, Docker에 대한 지원이 제거되었다. 그러나 Docker가 제거되기 전에 구축된 모든 이미지는 여전히 작동한다. Docker는 OCI 표준의 이미지 사양을 따르기 때문에 Docker로 구축된 모든 이미지는 표준을 따르며 containerd와 계속 작동할 수 있다. 그러나 Docker 자체는 Kubernetes에서 지원되는 런타임으로부터 제거되었다. 이것이 전체 이야기이며, 이제 containerd에 대해 더 구체적으로 살펴보자.

![](../../images/02-03-06.png)

containerd는 Docker의 일부이지만 이제는 독립적인 프로젝트이며 [CNCF](https://www.cncf.io/)의 [졸업](https://www.cncf.io/projects/) 상태의 회원이다. 이제 Docker를 설치하지 않고도 containerd를 독립적으로 설치할 수 있으며, Docker의 다른 기능이 필요하지 않다면 containerd만 설치하는 것이 이상적이다. 일반적으로 Docker가 있을 때는 `docker run` 명령을 사용하여 컨테이너를 실행했지만, Docker가 설치되지 않은 경우 containerd만으로 어떻게 컨테이너를 실행할 수 있을까? containerd를 설치하면 [ctr](https://github.com/projectatomic/containerd/blob/master/docs/cli.md#client-cli)라는 명령줄 도구가 제공되며, 이 도구는 containerd를 디버깅하기 위해 만들어졌고, 사용자 친화적이지 않으며 제한된 기능만 지원한다. 이 도구에 대한 문서 페이지에서 볼 수 있는 모든 것이 그것이다. 제한된 기능 외에 containerd와 상호작용하려면 API 호출을 직접 만들어야 하며, 이는 사용자 친화적인 방법이 아니다.

So just to give you an idea, the `ctr` command can be used to perform basic container-related activities such as pull images, for example to pull redis image you would run

```
ctr images pull docker.io/library/redis:alpine
```

To run a container we use the `ctr` run command

```
ctr run docker.io/library/redis:alpine redis
```

But as I mentioned, this tool is solely for debugging containerd and is not very user friendly and is not to be used for managing containers on a production environment.
So a better alternative recommended is the [nerdctl](https://github.com/containerd/nerdctl#nerdctl-docker-compatible-cli-for-container) tool. So the `nerdctl` tool is a command line tool that’s very similar to Docker, so it’s a Docker-like CLI for containerd. It supports most of the CLI options that Docker supports and apart from that it has the added benefit that it can give us access to the newest features implemented in containerd, so for example we can work with encrypted container images or other new features that will eventually be implemented into the regular Docker command in the future. It also supports lazy pulling of images, P2P image distribution, image signing and verifying and namespaces in Kubernetes which are not available in Docker. So the `nerdctl` tool works very similar to Docker cli, so instead of Docker you would simply have to replace it with `nerdctl` so it can run almost all Docker commands that interact with containers like this

![](../../images/02-03-07.png)

So that’s pretty easy and straightforward so now that we have talked about `ctr` and the `nerdctl` tool, it’s important to talk about another command like tool known as [crictl](https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md#container-runtime-interface-cri-cli). So earlier we talked about the CRI which is a single interface used to connect CRI compatible container runtimes, containerd, `rkt` and others. So the `crictl` is a command line utility that is used to interact with the CRI compatible container runtime, so this is kind of interaction from the Kubernetes perspective. So this tool is developed and maintained by the Kubernetes community and this tool works across all the different container runtimes and because earlier you had the `ctr` and `nerdctl` utility that was built by the containerd community specifically for containerd, but this particular tool is from the Kubernetes perspective that works across different container runtimes.

So it must be installed separately and is used to inspect and debug container runtimes so this again is not ideally used to create containers unlike the Docker or the `nerdctl` utility but is again a debugging tool. You can technically create containers with the `crictl` utility but it’s not easy. It’s only to be used for some special debugging purposes. And remember that it kind of works along with the kubelet so we know that the kubelet is responsible for ensuring that a specific number of containers or pods are available on a node at time, so if you kind of go through the `crictl` utility and try and create containers with it, then eventually kubelet is going to delete them because kubelet is unaware of some of those containers or pods that are created outside of its knowledge so anything that it sees it’s going to go and delete it, so because of those things remember that the `crictl` utility is only used for debugging purposes and getting into containers and all of that.

So let’s look at some of the command line examples so you simply run  the `crictl` command for this and this can be used to perform basic container-related activities such as pull images, or list existing images, list containers, very similar to the Docker command where you use the PS commands, so in Docker you run the `ps` command, and here you run the `crictl ps` command and to run a command in since a container docker remember we use the `exec` command and it’s the same here and along with the same options such as `-i` and `-t` and you specify the container id. The view the logs, you use the `crictl` logs command, again very similar to the docker command.

![](../../images/02-03-08.png)

One major difference is that the `crictl` command is also aware of pods so you can list pods by running the `crictl` pods command so this wasn’t something that Docker was aware of. So while working with Kubernetes in the past, we used Docker commands a lot to troubleshoot containers and view logs especially on the worker nodes and now you’re going to use the `crictl` command to do so. So the syntax is a lot similar and so it shouldn’t be really hard.
So here’s a chart that lists the comparison between the Docker and `crictl` command line tools. So as you can see, a lot of command such as attach exec, images, info, inspect, logs, ps, stats, version etc., work exactly the same way, and some of the commands to create, remove and start and stop images work similarly too. So a full list of differences can be found in [this link](https://kubernetes.io/docs/reference/tools/map-crictl-dockercli/#retrieve-debugging-information).

| docker cli | crictl            | Description                                                          | Unsupported Features                |
|------------|-------------------|----------------------------------------------------------------------|-------------------------------------|
| attach     | attach            | Attach to a running container                                        | --detach-keys, --sig-proxy          |
| exec       | exec              | Run a command in a running container                                 | --privileged, --user, --detach-keys |
| images     | images            | List images                                                          |                                     |
| info       | info              | Display system-wide information                                      |                                     |
| inspect    | inspect, inspecti | Return low-level information on a container, image or task           |                                     |
| logs       | logs              | Fetch the logs of a container                                        | --details                           |
| ps         | ps                | List containers                                                      |                                     |
| stats      | stats             | Display a live stream of container(s) resource usage statistics      | Column: NET/BLOCK I/O, PIDs         |
| version    | version           | Show the runtime (Docker, ContainerD, or others) version information |                                     |

So, since as I mentioned, `crictl` can be used to connect to any CRI compatible runtime, remember to set the right endpoint if you have multiple container runtimes configured, or if you want `crictl` to interact with a specific runtime, for example if you haven’t configured anything by default it’s going to connect to these sockets in this particular order, so it’s going to try and connect to dockershim first, then containerd, then CRI-O, then the CRI-dockerd – that’s kind of the order that it falls. But if you want to override that and set a specific endpoint, you use the `--runtime-endpoint` option with the `crictl` command line, or you could use the `CONTAINER_RUNTIME_ENDPOINT` environment variable. Set the environment variable to the right endpoint.

![](../../images/02-03-09.png)

So to summarize we have the `ctr` command line utility that comes with containerd and works with containerd which is used for debugging purposes only and has a very limited set of features, so ideally you wouldn’t be using this at all so you can kind of ignore this. Then we have the `nerdctl` CLI which is again from the containerd community but this is a Docker-like CLI for containerd used for general purpose to create containers and supports the same or more features than Docker CLI, so it’s something that I think we’ll be using a lot more going forward. Then we have the `crictl` utility which is from the Kubernetes community and mainly used to interact with CRI compatible runtimes, so it’s not just for containerd – this can be used for all CRI supported runtimes – again this is mainly to be used for debugging purposes.

![](../../images/02-03-10.png)

So if we look at the comparisons here, you can see that `ctr` and `crictl` are used mainly for debugging purposes, whereas the `nerdctl` is used for general purpose. The `ctr` and `nerdctl` are from the containerd community and work with containerd, whereas `crictl` is from the Kubernetes community and works across all CRI compatible runtimes.
So our labs originally had Docker installed on all the nodes so we used the Docker commands to troubleshoot, but now it’s all containerd so remember to use the `crictl` command instead to troubleshoot.


