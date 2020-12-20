# Docker for Desktop

- *Kube version in Docker for Desktop is 1.16, is there a newer version?
  Latest version is using 1.18.x
  
## Ingress controller - installation

- [Installation of the controller](https://kubernetes.github.io/ingress-nginx/deploy/#docker-for-mac)

```
< k8s/base + master > kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.40.2/deploy/static/provider/cloud/deploy.yaml
namespace/ingress-nginx created
serviceaccount/ingress-nginx created
configmap/ingress-nginx-controller created
clusterrole.rbac.authorization.k8s.io/ingress-nginx created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created
role.rbac.authorization.k8s.io/ingress-nginx created
rolebinding.rbac.authorization.k8s.io/ingress-nginx created
service/ingress-nginx-controller-admission created
service/ingress-nginx-controller created
deployment.apps/ingress-nginx-controller created
validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
serviceaccount/ingress-nginx-admission created
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
role.rbac.authorization.k8s.io/ingress-nginx-admission created
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
job.batch/ingress-nginx-admission-create created
job.batch/ingress-nginx-admission-patch created
```
  
## Ingres controller - Use of it

- ?? If I deployed the application using locally running "Docker
  for Desktop" or "Minikube", what is the hostname of the ingress?
  By default, ingress controller is not installed in Docker for
  Desktop container.
  
```
< workspace/pal-tracker + master > k get all --namespace=development
NAME                                          READY   STATUS    RESTARTS   AGE
pod/pal-tracker-development-b8d6dbb6c-rzt68   1/1     Running   0          31m

NAME                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/pal-tracker-development   ClusterIP   10.102.219.45   <none>        8080/TCP   31m

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pal-tracker-development   1/1     1            1           31m

NAME                                                DESIRED   CURRENT   READY   AGE
replicaset.apps/pal-tracker-development-b8d6dbb6c   1         1         1       31m
```

```
< workspace/pal-tracker + master > k get ingress --namespace=development
NAME                      CLASS    HOSTS                           ADDRESS     PORTS   AGE
pal-tracker-development   <none>   development.tracker.localhost   localhost   80      28m
```

```
< workspace/pal-tracker + master > cat /etc/hosts
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	localhost
127.0.0.1   passion
255.255.255.255	broadcasthost
::1             localhost
192.168.99.100  blue.example.com gree.example.com
# Added by Docker Desktop
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
127.0.0.1 development.tracker.localhost
127.0.0.1 review.tracker.localhost
```

```
< workspace/pal-tracker + master > curl review.tracker.localhost
hello from review
```

## Nodeport
  
- I was able to create NotePort and then can access it.
  ?? How about through Ingress?
  
```
< workspace/pal-tracker - master > k expose deployment.apps/pal-tracker --name=pal-tracker-service-nodeport --type=NodePort --port=8080service/pal-tracker-service-nodeport exposed

< workspace/pal-tracker - master > ka
NAME                              READY   STATUS    RESTARTS   AGE
pod/pal-tracker-c884fccff-qz7qm   1/1     Running   0          54m

NAME                                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes                     ClusterIP   10.96.0.1       <none>        443/TCP          58m
service/pal-tracker                    ClusterIP   10.97.100.233   <none>        8080/TCP         54m
service/pal-tracker-service-nodeport   NodePort    10.97.55.121    <none>        8080:31292/TCP   2s

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pal-tracker   1/1     1            1           54m

NAME                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/pal-tracker-c884fccff   1         1         1       54m

< workspace/pal-tracker - master > curl localhost:31292
hello from kubernetes
```



# What is this instructor-note for?

This is a collection of "tips and tricks" I've collected
in teaching PAL CND, which includes

- Additional logistics info that might be useful to the 
  the students before/during/after the cohort
- Tips and tricks used by other instructors
- Talking points/Wrap-up points for each topic
- Challenge questions/exercises for each topic
- Additional reference materials
- Trouble-shooting tips


# VDI

- Add the following to .bashrc file

```
# Kubernetes
alias k=kubectl
export KUBE_EDITOR=code  (if you want to use Visual Studio Code as editor of choice)
alias siege1='siege -p http://development.sashin.k8s.pal.pivotal.io/ -r1000 -q'
alias refresh='source ~/.bashrc'
alias test1='curl http://development.sashin.k8s.pal.pivotal.io/'
alias test2='curl http://review.sashin.k8s.pal.pivotal.io/'
```

- [Ubuntu shortcut keys](https://www.omgubuntu.co.uk/2019/09/useful-ubuntu-keyboard-shortcuts)

```
- Keyboard shortcut keys within Remote Desktop
  - ALT+F10 (maximizing window on and off)
  - ALT+Tab (move between windows - works only on Mac)
  - ALT+F9 (minimize window)
  - SHIFT+CTRL+C (copying from termimal) 
  - SHIFT+CTRL+V (copying to the terminal)
  - CTRL+C (copying) 
  - CTRL+V (pasting)
 
- Keyboard shortcut keys within IntelliJ
  - CTRL+N (find class)
  - CTRL+Shift+N (find file)
  - ALT+Return (Quick fix)
  - CTRL+SHIFT+Enter (complete the statement)
  - ALT+Insert (Generate)
  - Double SHIFT (global search)
  - F2, SHIFT+F2 (Go to next/previous error)
  - CTRL+ALT+Left (back - Does not work on Mac - I have to set it myself manually
  - CTRL+ALT+Right (forward - Does not work on Mac - I have to set it myself manually)
  - CTRL+SHIFT+F10 (run the app/test)
  - CTRL+F10(rerun the app/test)
  
- Keyboard shortcut keys within Chrome
  - Fn+(up-arrow) (page up)
  - Fn+(down-arrow) (page down)
```

# GCP

```
2. a gcp project and gke cluster have been provisioned for you

here is your environment info:

-----
Cluster URL: development.sashin.k8s.pal.pivotal.io
Cluster Name: pal-for-devs-k8s
GCP Project Name: k8s-0727-sashin
Ingress Router IP: 34.68.133.191
Concourse:
  url: concourse.sashin.k8s.pal.pivotal.io
  username: user
  password: M5q3bGWuJOUjpPl
-----

and here is your service account info:
-----
{
  "type": "service_account",
  ...
  }
-----
```


# Steps for fresh demo

- This is codebase
- https://github.com/platform-acceleration-lab/pal-tracker-k8s-deployment

- Take the followin steps

```
- cd <pal-tracker>
- git reset --hard <solution-tag>
- cd ../k8s
- git reset --hard <solution-tag>
- cp -r . ../pal-tracker/k8s (?? Try this)
- cd ../pal-tracker
- rm -rf k8s/.git
- rm -rf k8s/.gitignore
- rm -rf k8s/*.txt
- (change YOUR-DOCKERHUB-USERNAME in the k8s/base/deployment.yaml)
  gsed -i 's/YOUR-DOCKERHUB-USERNAME/axykim00/g' k8s/base/deployment.yaml
- (change domain name in the k8s/environments/development/ingress.yaml)
  gsed -i 's/UNIQUE-DOMAIN-FOR-DEVELOPMENT-ENVIRONMENT/development.tracker.localhost/g' k8s/environments/development/ingress.yaml
- (change domain name in the k8s/environments/review/ingress.yaml)
  gsed -i 's/UNIQUE-DOMAIN-FOR-REVIEW-ENVIRONMENT/review.tracker.localhost/g' k8s/environments/development/ingress.yaml
- kubectl apply -k k8s/environments/development
- kubectl apply -k k8s/environments/review
```

- pipeline.yaml

```
env:
  DOCKER_HUB_USERNAME: axykim00
  GCP_PROJECT_NAME: 
  K8S_CLUSTER_NAME: pal-for-devs-k8s
```

- *Somehow `git add .` does not add `k8s` directory?
  It was because when I did "cp -r from pal-tracker-deployment-k8s, .git directory is also copied. 
  I have to delete the .git directory under k8s to proceed.  It still did not work.  
  If I reopen Intellij, it says k8s has `invalid VCS root mapping` - you can click configure

```
IntellJ tells me that it is not a valid object name xxxx
```

# Git related

## Demo steps for a particular topic

-   If you want to start "topic" lab from "topic-start", 
    follow the steps mentioned below 
    
```
If you need to save your current unfinished work into 
“work in progress” branch so that you can work on that 
on later time, you do the following:
-   git checkout -b wip-branch
-   git add .
-   git commit -m “work in progress in my lab”
-   git push origin wip-branch --tags
-   git checkout master
```
    
```
-   git checkout master 
-   git reset --hard <topic-start>
-   ??change manifest files to reflect correct domain and route in manifest file like:
        - route: pal-tracker-sang-shin.apps.evans.pal.pivotal.io
-   Do the lab work
-   git add-commit -m “my work done”
-   git push origin master -f  (to force the remote master to sync with local master - do not to this in production! :-))
```

## Misc Git stuff

-   If you have created github repository with `README.md`, you
    will experience the following problem when you do 
    `git push origin master --tags`.
    An easy way out is `git push origin master -f --tags`

```
< workspace/movie-fun - master > git push
To https://github.com/sashinpivotal/movie-fun
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/sashinpivotal/movie-fun'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

- *When a repository is created with README file on, performing
  `git pull` results in the following error:
  
  ```
  < workspace/pal-tracker-distributed - master > git pull
  fatal: refusing to merge unrelated histories
  ```
  
  You can do the following to move forward.
  
  ```
  git pull --allow-unrelated-histories
  ```

-   If you want to go to a solution project while maintaining
    your code (??? Is this correct?)

```
- git checkout master
- git cherry-pick abort
- git cherry-pick <topic-solution-tag> and handle merge conflict
```


# Spring Boot Application

# Challenge questions

- What the purpose of using "gradle wrapper"?
- What is a fat jar?  How does Spring Boot creats one?
- What is 12 factor app?  (Google it)
- One of the features of Spring Boot (actually through the Spring Boot
  Maven/Gradle plugin) is to create a fat jar that contains everything
  including Tomcat.  Among the 12 factors, which factor is relevant to
  creating and deploying the same fat jar over different deployment
  environment?
- How does Spring Boot helps with dependency management?
- What does @SpringBootApplication do? What is it made of? (We already covered this)

# Challenge exercise

- Write web-slice testing code testing the rest controller using @WebMvcTest and MockMvc
- Write integration testing code using @SpringBootTest and TestRestTemplate


# Containerizing an App --------------------

## Intro

- ?? Explain the buildImage command result
- (Mike) Dockerfile is hard to create secure and smaill docker image
- Buildpack (link is on the top) - if you have used pcf, this should sound familar

## Extra resources

- [What's new in Spring Boot 2.3](https://www.youtube.com/watch?v=WL7U-yGfUXA)
- [Explore Docker layers using dive](https://www.upnxtblog.com/index.php/2020/04/27/explore-docker-layers-using-dive/)
- [Jib](https://cloud.google.com/blog/products/gcp/introducing-jib-build-java-docker-images-better)

## Misc.

- Why the docker image that is built says 40 years old?

## Challenge exercise

- Install and try "dive" to analyze the layers of docker image

```
cd ~
wget https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.deb
sudo apt install ./dive_0.9.2_linux_amd64.deb

dive pal-tracker
```

## Wrap-up

- how to create docker image best practices
- let experts do the job right
- OCI compatible image
- the 6 lines of code in the buildpack - how many containers are being pulled
- 2 pulling commands??
- what image we are running?
- docker hub, what linux image it is using??

## Deploying a Containerized App to Kubernetes -------

## Intro

- Use VSC or IntelliJ with terminal window on the right configuration
- Bill shows the k8s website that compares traditional,vitual machine, container ??
 "what is kubernetes"?
- (Mike) k8s is not a platfrom, it is tool for building a platform

- we already set up a credential in the config file to talk to the k8s

## Misc questions for me

- ?? How can I find out the port number the pod is listening to?
  - maybe "k describe po <pod>" and check the targetPort?
- ?? Is there any other way to 
  find the IP addresses of a node without using "ip address"
  (Answer - Octant shows it as an InternalIP for the node detail)
  

## Challenge exercises - accessing pod

- Access the pod - A kubectl exec command serves for 
  executing commands in Docker containers running inside 
  Kubernetes Pods.
  
- ?? what does CNB stand for? Cloud Native Buildpack?

```
ubuntu@mylab:~$ k exec -it pal-tracker-747dff7c4-vps7f -- /bin/bash

$ env
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_SERVICE_PORT=443
PAL_TRACKER_PORT_8080_TCP_PORT=8080
PAL_TRACKER_PORT_8080_TCP_PROTO=tcp
CNB_PLATFORM_API=0.3
HOSTNAME=pal-tracker-747dff7c4-kfhhh
PAL_TRACKER_SERVICE_PORT=8080
PAL_TRACKER_PORT=tcp://10.105.137.94:8080
CNB_DEPRECATION_MODE=quiet
HOME=/home/cnb
PAL_TRACKER_PORT_8080_TCP=tcp://10.105.137.94:8080
CNB_APP_DIR=/workspace
CNB_LAYERS_DIR=/layers
TERM=xterm
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_SERVICE_HOST=10.96.0.1
PWD=/
PAL_TRACKER_PORT_8080_TCP_ADDR=10.105.137.94
PAL_TRACKER_SERVICE_HOST=10.105.137.94. (This is clusterIP address of pal-tracker service)
$

cnb@pal-tracker-747dff7c4-vps7f:/$ ps -aef
UID          PID    PPID  C STIME TTY          TIME CMD
cnb            1       0  0 00:38 ?        00:00:50 java org.springframework.boot.loader.JarLauncher
cnb          222       0  0 12:20 pts/0    00:00:00 /bin/bash
cnb          234     222  0 12:20 pts/0    00:00:00 ps -aef
cnb@pal-tracker-747dff7c4-vps7f:/$ pwd
/
cnb@pal-tracker-747dff7c4-vps7f:/$ ls
bin  boot  cnb  dev  etc  home  layers  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  workspace

cnb@pal-tracker-747dff7c4-vps7f:/$ hostname 
pal-tracker-747dff7c4-vps7f
cnb@pal-tracker-747dff7c4-vps7f:/$ hostname -I
192.168.15.9 

cnb@pal-tracker-747dff7c4-vps7f:/$ find . -name \*.class -print

./workspace/org/springframework/boot/loader/data/RandomAccessDataFile.class
./workspace/org/springframework/boot/loader/data/RandomAccessDataFile$1.class
./workspace/org/springframework/boot/loader/ExecutableArchiveLauncher.class
./workspace/org/springframework/boot/loader/PropertiesLauncher$ClassPathArchives.class
./workspace/BOOT-INF/classes/io/pivotal/pal/tracker/PalTrackerApplication.class
./workspace/BOOT-INF/classes/io/pivotal/pal/tracker/WelcomeController.class

cnb@pal-tracker-747dff7c4-vps7f:/workspace$ ls BOOT-INF
classes  classpath.idx  lib
cnb@pal-tracker-747dff7c4-vps7f:/workspace$ ls BOOT-INF/lib
jackson-annotations-2.11.0.jar             jakarta.el-3.0.3.jar       snakeyaml-1.26.jar                           spring-boot-starter-logging-2.3.1.RELEASE.jar  spring-jcl-5.2.7.RELEASE.jar
jackson-core-2.11.0.jar                    jul-to-slf4j-1.7.30.jar    spring-aop-5.2.7.RELEASE.jar                 spring-boot-starter-tomcat-2.3.1.RELEASE.jar   spring-web-5.2.7.RELEASE.jar
jackson-databind-2.11.0.jar                log4j-api-2.13.3.jar       spring-beans-5.2.7.RELEASE.jar               spring-boot-starter-web-2.3.1.RELEASE.jar      spring-webmvc-5.2.7.RELEASE.jar
jackson-datatype-jdk8-2.11.0.jar           log4j-to-slf4j-2.13.3.jar  spring-boot-2.3.1.RELEASE.jar                spring-cloud-bindings-1.6.0.jar                tomcat-embed-core-9.0.36.jar
jackson-datatype-jsr310-2.11.0.jar         logback-classic-1.2.3.jar  spring-boot-autoconfigure-2.3.1.RELEASE.jar  spring-context-5.2.7.RELEASE.jar               tomcat-embed-websocket-9.0.36.jar
jackson-module-parameter-names-2.11.0.jar  logback-core-1.2.3.jar     spring-boot-starter-2.3.1.RELEASE.jar        spring-core-5.2.7.RELEASE.jar
jakarta.annotation-api-1.3.5.jar           slf4j-api-1.7.30.jar       spring-boot-starter-json-2.3.1.RELEASE.jar   spring-expression-5.2.7.RELEASE.jar

```

## Challenge questions/exercises - Service

- How does Service keeps track of the internal IP addresses
  of the pods it is front-end'ing?
  
- Try to create NodePort type

- You can create nodeport as following

```
ubuntu@mylab:~$ k expose deployment.apps/pal-tracker --name=pal-tracker-servie-nodeport --type=NodePort --port=8080 

ubuntu@mylab:~$ k get svc
NAME                          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
pal-tracker                   ClusterIP   10.107.244.109   <none>        8080/TCP         12h
pal-tracker-servie-nodeport   NodePort    10.99.210.37     <none>        8080:30612/TCP   31s

ubuntu@mylab:~$ ip address
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc mq state UP group default qlen 1000
    link/ether 02:55:ed:e9:61:cf brd ff:ff:ff:ff:ff:ff
    inet 172.31.19.166/20 brd 172.31.31.255 scope global dynamic eth0
       valid_lft 2558sec preferred_lft 2558sec
    inet6 fe80::55:edff:fee9:61cf/64 scope link 
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:2d:01:f7:ae brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:2dff:fe01:f7ae/64 scope link 
    
ubuntu@mylab:~$ curl 172.31.19.166:30612
hello

ubuntu@mylab:~$ curl mylab:30612. (You can use node name)
hello

ubuntu@mylab:~$ curl 54.151.120.233.nip.io (development domain - using ingress)
hello from kubernetes
```

## Challenge exercises - Ingress

- For Ingress object, try to use host

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: pal-tracker
  labels:
    app: pal-tracker
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
  #- host:  54.151.120.233.nip.io
  - http:
      paths:
      - path: /
        backend:
          serviceName: pal-tracker
          servicePort: 8080
  backend:
    serviceName: pal-tracker
    servicePort: 8080
```

?? When I tried above, I get the following

```
<html>
<head><title>308 Permanent Redirect</title></head>
<body>
<center><h1>308 Permanent Redirect</h1></center>
<hr><center>nginx/1.15.8</center>
</body>
</html>
```

- What about the following?

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: pal-tracker
  labels:
    app: pal-tracker
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix     # added this
        backend:
          serviceName: pal-tracker
          servicePort: 8080
```

I get the same error above.

- The following works - with host included

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: pal-tracker
  labels:
    app: pal-tracker
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
    - host:  3.101.115.109.nip.io
      http:
        paths:
          - path: /
            backend:
              serviceName: pal-tracker
              servicePort: 8080
```


- *what about the following?  (this is due to k 1.8 vs 1.9 difference)

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: pal-tracker
  labels:
    app: pal-tracker
  annotations:
    kubernetes.io/ingress.class: nginx
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: pal-tracker
            port:
              number: 8080
```

You the following error

```
ubuntu@mylab:~/workspace/k8s$ k apply -f .
deployment.apps/pal-tracker unchanged
service/pal-tracker unchanged
error: error validating "ingress.yaml": error validating data: ValidationError(Ingress.spec.rules[0].http.paths[0].backend): unknown field "service" in io.k8s.api.networking.v1beta1.IngressBackend; if you choose to ignore these errors, turn validation off with --validate=false
```

## DNS

- How dns works.  Each pod is configured with the nameserver with the
  ip address of the kube-dns service.
  [How k8s dns works](https://www.digitalocean.com/community/tutorials/an-introduction-to-the-kubernetes-dns-service#:~:text=A%20service%20named%20kube-dns%20and%20one%20or%20more,or%20delete%20Kubernetes%20services%20and%20their%20associated%20pods.)
  
```
NAMESPACE     NAME                              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
default       service/kubernetes                ClusterIP   10.96.0.1       <none>        443/TCP                  48m
development   service/pal-tracker-development   ClusterIP   10.102.219.45   <none>        8080/TCP                 10m
kube-system   service/kube-dns                  ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP   48m
```

```
< k8s/base + master > k exec -it pod/pal-tracker-development-b8d6dbb6c-rzt68 --namespace=development -- /bin/sh
$ cat /etc/resolv.conf
nameserver 10.96.0.10. (# this is the kube-dns service ip address)
search development.svc.cluster.local svc.cluster.local cluster.local
options ndots:5

< workspace/pal-tracker + master > k get svc --all-namespaces
NAMESPACE     NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
default       kubernetes       ClusterIP   10.96.0.1       <none>        443/TCP                  21h
development   pal-tracker      ClusterIP   10.105.137.94   <none>        8080/TCP                 13h
kube-system   kube-dns         ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP   21h
kube-system   metrics-server   ClusterIP   10.101.231.8    <none>        443/TCP                  21h
```

## RestTemplate to another service

```
@RestController
public class WelcomeController {

    private RestTemplate restTemplate;

    public WelcomeController(RestTemplateBuilder restTemplateBuilder) {
        this.restTemplate = restTemplateBuilder.build();
    }

    @GetMapping("/")
    public String sayHello() {
        return "hello";
    }

    // This works
    @GetMapping("/dest1")
    public String talkToDest1() {
        return restTemplate.getForObject("http://dest.3.101.115.109.nip.io", String.class);
    }

    // This does not work
    @GetMapping("/dest2")
    public String talkToDest2() {
        return restTemplate.getForObject("http://pal-tracker-dest", String.class);
    }
}
```

## Wrap-up

- (Bill) create vs apply (here we go and platform apply the changes)
  - declarative model is recommended. k apply -f k8s
  - we are giving a state (declarative model) - we are not creating (imperative model)
  - imperative model is k create deploy ...
  - why we choose declative model?
    - enable infrastructure of code model - store the state

- Can you access the pod directly from external client?  no, we need to have service
- clusterip, dns

- ingress, group and kind from the api document
- annotation ingress - nginx controller

- Talk about each resource in the lab - use the diagram

  - the gray ones are the ones that get created by K8s
  - What is the role of deployment? ReplicaSet?
  - Single responsibility principle - each resource handles one thing
  
- Demo Octant 

  - Mis-configure each resource object and see how Resource Viewer responds

## Misc.

- ??kubectl create vs apply - imperative vs declarative (Mike G)



# Clusers and Nodes -------------------------

## Intro

- control plane, node - used to be called master and worker nodes
- they can physical machine, more likely vm
- etcd maintains desired state 
- etcd - highly reliable, distributed data store
- api server - kubectl commands or octant talk to it - it is rest call
- controller manager - process loop to handle desired state and actual state
  - they are controllers inside the controller manager
  - controller mananger is a single process??
  - kube scheduller?

- kube-proxy - handle network traffic
- kubelet is an agent that talks to the contol plane
- container runtime interface (software interface) - there are multiple implementations
  - docker, curio??
- container networking interface
- container storage interface

- kubelet talks to these interfaces defined above
- what are three netwokring behaviors that are needed?


## Misc

```
ubuntu@mylab:~$ kubectl cluster-info
Kubernetes master is running at https://172.31.19.166:6443
KubeDNS is running at https://172.31.19.166:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://172.31.19.166:6443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

```

```
ubuntu@mylab:~$ k get nodes
NAME    STATUS   ROLES    AGE   VERSION
mylab   Ready    master   14h   v1.18.6
```

## Resources to use 

- [Udemy CKAD course architecture](https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/12299412#questions)

- [Cloud controller manager](https://kubernetes.io/docs/concepts/architecture/cloud-controller/)

- Container runtime is underlying software that is used to run containers.
  In our example, it happens to be docker.  Other options are rkt and CRI-O.

# Using ConfigMaps to set Env. variabkes-----

- *what is the development domain?

```
Try navigating to the development domain using a web browser. You will now see the welcome message you set in the ConfigMap.
```

- *We could make the following as optional exercise -Logging document from K8s ref document

```
On the Google Compute Engine (GCE) platform, the default logging support targets Stackdriver Logging, which is described in detail in the Logging With Stackdriver Logging.

This article describes how to set up a cluster to ingest logs into Elasticsearch and view them using Kibana, as an alternative to Stackdriver Logging when running on GCE.
```
- configmap vs using env in the deployment
- ?? Keith says Spring Boot K8s lets having configmap to be testable in TDD?

## Wrap-up

- Changing configmap itself does not trigger the restart the pods
  https://blog.questionable.services/article/kubernetes-deployments-configmap-change/
- (Bill) its behavior is similar to PCF where changing environment variables
  does not automatically restart the apps
- (Mike) Also when you do auto-scale, only the new apps will
  pick up the change, so if you want to make all apps use the
  new config values, redeploy all app instances fresh
- (Charles) You might be in the process of updating configuration
  files, which you do not want to get reflected right away
- Fedex is using spring cloud config server for getting common
  config data for multiple apps
- (Mike) Greenfield project vs legacy apps
- However, if the configmap is managed via a volume then, the pod
  will get restarted if it is configured with some kind of file watch
- ReplicaSet and Pods are created whenever deployment yaml file changes, however


# Actuator-----------------------------

## Intro

- cost and benefit analysis - free?
- monitoring and management (refresh, logging)

- (fedex) appdynamics
- acruator are diggnostics tools
- backingservice is simulated backing service
- spring boot will give healthindicators for various backing services
- assignment - make

## Wrap-up

- exposing these endpoints good idea in production?
- /actuator/env - does it expose database credentials?
- they are mostly development time tools
- dont use on k8s

- Healthindiator status 200 vs 503
- subsequent lab shows how k8s uses health indicator

- actuator follows spring security scheme (beyond the scope of this class)

- Bill went through all endpoints using Postman
   - all down 
- Actuator vs Platform-provided tool (dynatrace or appdyanmics)

## Trouble-shooting

- *The build info shows the following - why it does not 
  show version and group?

```
{
"build": {
"artifact": "pal-tracker",
"name": "pal-tracker",
"time": "2020-10-31T17:53:03.867Z",
"version": "unspecified",
"group": ""
}
}
```

# Availability Probes--------------------

## Intro

- readiness probe - check if the downstream service is up and running
- developer vs application operator (configuring the app) - devops
- operational concerns
- talking about actions vs roles

- ?? What does the following statement mean?

```
The code example in the PalTrackerFailure is an example of one way to define a Liveness failure event. 
```

## Wrap-up

- transistent failure to backing service - network blip, connection pool resource
- connection pool, thread pool could cause transient failre
- we don't want k8s to throw away for the transistent failures
- application should deal with transitent failure - production env - you can tune only in production
- what would be good candidates for liveness probes - out of memory, not network blip
- explain the availabilityevent code
- traffice pattern through time-series database collected by 3rd party system

## Resources

- https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/

# Configuring RollingUpdate Deployments----

## Intro

- what is rolling updates
   - default strategy - whenever you change your deployment.yaml file
     it starts a new pod while leaving the old pod running
- value of rolling updates
   - minimize the downtime (Bill clarified it is a blip?)
- just because a pod running does not mean it is ready to handle the requests
  maybe it needs backing service and the backing service is not ready yet
- we are going to exercise a few options

## Tips

- load test command

```
run-load-test -d 300 -u 10 -w 10 -n development http://54.151.120.233.nip.io
```

```
pal-tracker-7f9885f6c7-wq72v   1/1     Running   0          3h26m
loadtest                       0/1     Pending   0          0s
loadtest                       0/1     Pending   0          1s
loadtest                       0/1     ContainerCreating   0          1s
loadtest                       0/1     ContainerCreating   0          1s
loadtest                       1/1     Running             0          23s
pal-tracker-7fb74cf8fb-hh7ql   0/1     Pending             0          0s
pal-tracker-7fb74cf8fb-hh7ql   0/1     Pending             0          0s
pal-tracker-7fb74cf8fb-hh7ql   0/1     ContainerCreating   0          0s
pal-tracker-7fb74cf8fb-hh7ql   0/1     ContainerCreating   0          1s
pal-tracker-7fb74cf8fb-hh7ql   1/1     Running             0          2s
pal-tracker-7f9885f6c7-wq72v   1/1     Terminating         0          3h30m
pal-tracker-7f9885f6c7-wq72v   0/1     Terminating         0          3h30m
pal-tracker-7f9885f6c7-wq72v   0/1     Terminating         0          3h30m
pal-tracker-7f9885f6c7-wq72v   0/1     Terminating         0          3h30m
```

- The lab document says to create docker v2. How do I create a new docker image?

```
./gradlew bootBuildImage
docker tag pal-tracker YOUR-DOCKER-HUB-USERNAME/pal-tracker:v2
docker push YOUR-DOCKER-HUB-USERNAME/pal-tracker:v2
```

- this is how to create a new version

```
./gradlew bootBuildImage --imageName=axykim00/pal-tracker:v2
docker push axykim00/pal-tracker:v2
```

## Wrap-up

- Bill uses the 3 screens - loadtest on the top, k get pod --watch 
  on the bottom left, and 3rd termainl for k apply -f

- k8s life cycle hooks such as preStop?
- ??two messages race condition
  - show architecure document - controller is a loop checking, etds for messages
  - delete pod (pod controller picked up) - will wait 10 seconds before killing it
  - stop routing message (by service controller)
  - ?? spring drain 
  - why is prestop not built-in in k8s? k8s says it is not my problem
    it can be handled by higher level abstraction
   
- 12 facotor app
  - strip out as much as from yur spring boot app - faster start up
  - stopping micro-services?? challenge
  - scalabilty

- pcf gorouter handles ingress traffic
- pcf handles unregistering app from routing table first before
  terminating it, k8s is not so good on that 
- k8s is a toolbox not paas
- it does not handle networking either
- Read the references in this lab, it is using the same solution

- why we need prestop hook, the terminiating time, and routing table update
  - completely asynchronous
  - what benefit does it give? making systems simpler, no mutex required
    engineering decision by k8s, 
    routes in the etcd
  - deregistration of a terminated app
    from routing does not occur before terminaing the pod
  - empirical evidence of 10 seconds - why do not care about the dration
    as long as it is not 6 months - not much risk to the overall performance
    you apps might be different - you need to test around and tune it
    making 100 seconds will make disposability hard - you want your
    system to start fast and shut down fast 
    
    pcf platform operators are involved, security voluntability,
    developer says this is is the state of the applicastions
    rip infrastruure under you
    
    k8s could work the same way,
    
- is there any higher-level construct that unnecessiate the pre-stop?
- what about blue-green deployment? Are you relying pcf to do that or
  do you roll out your custom steps?
  
- solution pattern: drain?? prestop, what about the traffic in the process
  of being handled
  drain - I am not going to 
  
- does bootImageBuild anything about k8s?
  k8s directory does not have to be within pal-tracker
  
- changing to v0 is to make deployment change
- we need v2, but we don't need v3

- is it good practice to have k8s and code single platform
- pros and cons
- 12 factor says the separation of configuration
- simplicity in one repo (Mike G)
  
  
# Scaling an App with Kubernetes--------


- *I get the following error.  It is because I used -f instead of -k

```
ubuntu@ip-172-31-87-179:~/workspace/pal-tracker$ k apply -f k8s/environments/development
configmap/pal-tracker unchanged
ingress.networking.k8s.io/pal-tracker unchanged
error: error validating "k8s/environments/development/kustomization.yaml": error validating data: [apiVersion not set, kind not set]; if you choose to ignore these errors, turn validation off with --validate=false
```

- *After horizontal scaling, the number of pods remain to be 2 instances. 
  It should have been 1. It took time.  But eventually, it became 1.
  
## Intro

- cloud native app favors horizontal scaling
- read reference doc's
- this lab is a bit different starting and endppoint are the same lab
- read the lab document carefully, a lot of narratives
  
  
## Wrap-up

- Number of pods = (#concurrent users * workrate)/ (max allowed work rate per pod)
  - (500 * 50)/100 -> 25
  - we build the profile - by observing the load requests
  - we can experical performance test for pod 
  - linear expolartion
  - this assumes the pod is not maintaining state
- can you linear expolarate from vertical scaling?
  - cpu maybe linear on bare metal but it is hard to do that on vm
  - memory does not scale linearly
  - this is why cloud native favors horizontal scaling

- Bill - what metrics do you see to determine memory requirement?
  - for Web app - throughput is primary criteria, cpu might not
    be a good measure 
  - auto-scaler might not detect all the possible factors
    use it only after you understand the pattern loads
    
  - show the knobs and dials in the reference document - you
    have to understand the traffic pattern
    
- Mike is against auto-scaling.  ??Why

- tradeoffs and consideration section
  - throttle
  - greenfield vs legacy apps (bare metai)
- javs threads feature (not available in java 8) - jvm problem
  - exacerbated in pcf due to core
  - greenfield assumes latest java
  
# Liveness Probes-----------------------

## Intro (Charles)

- difference between readiness vs liveness recovery
- explain the failure endpoint, we used in the previous lab
  - we use it to simulate the failure of an application
- ?? read section 5 for tuning
- availability section
- scaling and availabilty are two different concepts

- imperative (scale command) vs declative (yaml file with replicas)

## Trouble-shooting

- ??step 6 - it is not 10 seconds it is 2 and half miunutes
- ??step 7 strange behavior - use siege instead of browser

- Verify the following

```
5. View 

View the Events section. You will see the correlated event of liveness probe failures, pod disposal, and creation.
```

## Wrap-up

- when liveness probe results in restarting the container, 
  note that it is still the same pod - it just restarts
  the container process within the same pod - think about
  a hypothetical case where you have 5 containers running
  in a single pod, the container that has the liveness
  probe will be restarted
- [Configure Livness, Readiness, Statup probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)


# Multiple Enviroments------------------

## Intro

- So far, we've been working in a single environment with a single
  set of k8s resources, which includes configmap, service, deployment
  and that is highly unrealistic 
  

## Trouble-shooting

- ?? When I deploy, I got readiness probe error - needs investigation
  - once I changed the default namespace to development from review,
    it worked OK
    
- ?? I kept getting the following error even with solution project
  with correct domain.  logs and describe does not show any sign
  of problems.  It has something to do with Readiness probe.
  When I removed readiness proble from the deployment.yaml file,
  it worked.  It seems like readiness probe is set as false
  as intial value.
  
```
< workspace/pal-tracker + master > !350
curl development.tracker.3.101.115.109.nip.io
<html>
<head><title>503 Service Temporarily Unavailable</title></head>
<body>
<center><h1>503 Service Temporarily Unavailable</h1></center>
<hr><center>nginx/1.15.8</center>
</body>
</html>
```

- *Several people, even after they change the configmap, 
  and then performed "kubectl apply", yet they still
  see the old message.  It is because changing configmap
  does not trigger the rolling update of the pods.
  - try to add dummy env name and value
  
- *After reseting pal-tracker-k8s-development to multi-environment 
  solution, I get the following error. (answer) I have to use -k 
  like `kubectl apply -k k8s/environments/development`

```
ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ k apply -f k8s/environments/review
configmap/pal-tracker created
ingress.networking.k8s.io/pal-tracker created
error: error validating "k8s/environments/review/kustomization.yaml": error validating data: [apiVersion not set, kind not set]; if you choose to ignore these errors, turn validation off with --validate=false
```

- When I added RestTemplateBuilder through a constructor, I get
  the following "k get logs <pod>"
  
```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'welcomeController' defined in file [/workspace/BOOT-INF/classes/io/pivotal/pal/tracker/WelcomeController.class]: Instantiation of bean failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [io.pivotal.pal.tracker.WelcomeController]: No default constructor found; nested exception is java.lang.NoSuchMethodException: io.pivotal.pal.tracker.WelcomeController.<init>()
```

## Experimentation - calling another pod

https://stackoverflow.com/questions/58733807/what-is-the-value-of-get-hosts-from-variable

- *I get the following error when accessing the review service
  using ip address - it was because downstream service returns 
  connection timeout - it was because the port number was not
  specified

```
curl development.tracker.3.101.115.109.nip.io/dest3
<html>
<head><title>504 Gateway Time-out</title></head>
<body>
<center><h1>504 Gateway Time-out</h1></center>
<hr><center>nginx/1.15.8</center>
</body>
</html>
```

## Wrap-up

-  Namespaces 
   - used to divide cluster resources
   - names of the resources need to be unique within a namespace
     but not across the namespaces and a resource can 
     be only in a single namespace
   - we now have pods running in different namespaces, how
     do they communicate?
     
-  Ingress
   - we use different host
   - it also supports two different services being routed to different path
   
## Challenge exercises

-  Write controller code in pod1 in namespace1 to access pod2 in namespace2


## Resources

- [Helm vs Kustomize](https://blog.boltops.com/2020/11/05/kustomize-vs-helm-vs-kubes-kubernetes-deploy-tools)
- [Helm vs Kustomize: How to deploy your apps oin 2020](https://medium.com/@alexander.hungenberg/helm-vs-kustomize-how-to-deploy-your-applications-in-2020-67f4d104da69)   

# Deployment Pipelines-------------------

## Intro

- What is CI, why CI?
  - many developers are working on a project, we can move faster,
  - remove merge conflict after waiting for a week
- steps for CI (bill used PCF slide)
- Bill likes concourse - versioning jar files
- what and why CD? 
  - make our app to be usable, release frequently
  - deploy smoothly and consistently
  - Bill does not like to use the term "production release" 
    diffferent companies have different prod 
- (Mike G)
  - Github 17 times per day - some cost? only github employees see them
  - github know who logs in, they use gihub to build github
  - we need feedback from users, cost and benefit
- (Bill)
  - feature toggle - what is and why we want to use it?
  - what is feature flag? conditions in the code- turn on and off
    your features -martin fowler

# Spring MVC with REST Endpoints-------

## Tips

```
ubuntu@mylab:~/workspace/pal-tracker$ get-k8s-domain
The domain for your K8s cluster is

     54.153.72.214.nip.io

The domain is added to your copy/paste buffer, feel free to paste into your ingress or CI deployment configurations.

ubuntu@mylab:~/workspace/pal-tracker$ curl review.tracker.54.153.72.214.nip.io/time-entries
[]
ubuntu@mylab:~/workspace/pal-tracker$ curl review.tracker.54.153.72.214.nip.io
hello from review
```

# Database Migrations------------------

- [psql commands](https://www.postgresqltutorial.com/psql-commands/)

```
\l
\dt (display all tables)
\d table_name (diaplay table_name details)
```

## Wrap-up

- [database](https://api.elephantsql.com/console/2c1c8b8c-ca75-4f7e-81f9-ff6d294f3122/details)

## ElephantSQL setup

- The following worked for using psql

This is from the document

```
flyway -url="jdbc:postgresql://ELEPHANT_SQL_SERVER:5432/ELEPHANT_SQL_DATABASE" -locations=filesystem:databases/tracker migrate -user=ELEPHANT_SQL_USER -password=ELEPHANT_SQL_PASSWORD
```

With my server info filled in

- Ignore url from the elephant server
- Username and database name is the same

```
postgres://malyyjbd:i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ@ruby.db.elephantsql.com:5432/malyyjbd 
```

```
~/workspace/pal-tracker$ flyway -url="jdbc:postgresql://ruby.db.elephantsql.com:5432/malyyjbd" -locations=filesystem:databases/tracker migrate -user=malyyjbd -password=i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ
Flyway Community Edition 6.3.1 by Redgate
Database: jdbc:postgresql://ruby.db.elephantsql.com:5432/malyyjbd (PostgreSQL 11.8)
Successfully validated 1 migration (execution time 00:00.022s)
Creating Schema History table "public"."flyway_schema_history" ...
Current version of schema "public": << Empty Schema >>
Migrating schema "public" to version 1 - initial schema
Successfully applied 1 migration to schema "public" (execution time 00:00.067s)
```

```
ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ psql -h ruby.db.elephantsql.com -U malyyjbd
Password for user malyyjbd: ("i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ")
psql (11.8 (Ubuntu 11.8-1.pgdg18.04+1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

malyyjbd=> \dt
                 List of relations
 Schema |         Name          | Type  |  Owner   
--------+-----------------------+-------+----------
 public | flyway_schema_history | table | malyyjbd
 public | time_entries          | table | malyyjbd
(2 rows)

malyyjbd=> \d time_entries
                              Table "public.time_entries"
   Column   |  Type   | Collation | Nullable |                 Default                  
------------+---------+-----------+----------+------------------------------------------
 id         | bigint  |           | not null | nextval('time_entries_id_seq'::regclass)
 project_id | bigint  |           |          | 
 user_id    | bigint  |           |          | 
 date       | date    |           |          | 
 hours      | integer |           |          | 
Indexes:
    "time_entries_pkey" PRIMARY KEY, btree (id)
 

```

```
~/workspace/pal-tracker$ psql -d malyyjbd -h ruby.db.elephantsql.com -U malyyjbd 
Password: (you can paste the password here - i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ)
psql (11.8 (Ubuntu 11.8-1.pgdg18.04+1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

malyyjbd=> 
```

- PostgreSQL - [https://customer.elephantsql.com/instance](https://customer.elephantsql.com/instance) - this for review

```
ELEPHANT_SQL_SERVER ruby.db.elephantsql.com (ruby-01) - don't use (ruby-01)
ELEPHANT_SQL_DATABASE gzxpwzfc
ELEPHANT_SQL_DB_USER gzxpwzfc
ELEPHANT_SQL_DB_PASSWORD LWjxpFkYO0sIcGpW2NdhHnMhgHK-myHl
```

- The following is for development

```
ELEPHANT_SQL_SERVER ruby.db.elephantsql.com (ruby-01) - don't use (ruby-01)
ELEPHANT_SQL_DATABASE malyyjbd
ELEPHANT_SQL_DB_USER malyyjbd
ELEPHANT_SQL_DB_PASSWORD i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ

```

## ElephantSQL trouble-shooting

- I get the following problem (it is fixed later - see later part in this document)

This is from the document

```
flyway -url="jdbc:postgresql://ELEPHANT_SQL_SERVER:5432/ELEPHANT_SQL_DATABASE" -locations=filesystem:databases/tracker migrate -user=ELEPHANT_SQL_USER -password=ELEPHANT_SQL_PASSWORD
```

```
< workspace/pal-tracker + master > flyway -url="jdbc:postgresql://ruby.db.elephantsql.com:5432/malyyjbd" -locations=filesystem:databases/tracker migrate -user=malyyjbd -password=i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ
Flyway Community Edition 6.5.2 by Redgate
Database: jdbc:postgresql://ruby.db.elephantsql.com:5432/malyyjbd (PostgreSQL 11.8)
Successfully validated 1 migration (execution time 00:00.340s)
Current version of schema "public": 1
Schema "public" is up to date. No migration necessary.

< workspace/pal-tracker + master > psql -h ruby.db.elephantsql.com -U malyyjbd
Password for user malyyjbd:
psql (13.0 (Ubuntu 13.0-1.pgdg20.04+1), server 11.8 (Ubuntu 11.8-1.pgdg20.04+1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

malyyjbd=> \d time_entries
                              Table "public.time_entries"
   Column   |  Type   | Collation | Nullable |                 Default
------------+---------+-----------+----------+------------------------------------------
 id         | bigint  |           | not null | nextval('time_entries_id_seq'::regclass)
 project_id | bigint  |           |          |
 user_id    | bigint  |           |          |
 date       | date    |           |          |
 hours      | integer |           |          |
Indexes:
    "time_entries_pkey" PRIMARY KEY, btree (id)

malyyjbd=> \q
```

## Trouble-shooting

- *I am experiencing the following concourse problem

```
selected worker: 041e690158c9
Cloning into '/tmp/semver-git-repo'...
warning: Could not find remote branch build to clone.
fatal: Remote branch build not found in upstream origin
error bumping version: exit status 128
```

  (Answer) Everytime you change the pipeline change, you have to
  apply it

```
fly -t concourse set-pipeline -c ci/pipeline.yaml -p pal-tracker -l ci/variables-pal-tracker.yaml
```

# Spring JDBC Template------------------



# Using Secrets to Store Credentials----

## Intro

- The term "secret" here is a misnomer because it is not encryped, it is
  Base64 encoded, which is as secure as a plain text

## lab Misc

- kubectl command to create a secret - development

```
kubectl create secret generic db-credentials --from-literal=SPRING_DATASOURCE_URL='jdbc:postgresql://ELEPHANT_SQL_SERVER:5432/ELEPHANT_SQL_DATABASE?user=ELEPHANT_SQL_DB_USER&password=ELEPHANT_SQL_DB_PASSWORD'

kubectl create secret generic db-credentials --from-literal=SPRING_DATASOURCE_URL='jdbc:postgresql://ruby.db.elephantsql.com:5432/malyyjbd?user=malyyjbd&password=i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ'
```

- kubectl command to create a secret - review

- PostgreSQL - [https://customer.elephantsql.com/instance](https://customer.elephantsql.com/instance) - this for review

```
ELEPHANT_SQL_SERVER ruby.db.elephantsql.com (ruby-01) - don't use (ruby-01)
ELEPHANT_SQL_DATABASE gzxpwzfc
ELEPHANT_SQL_DB_USER gzxpwzfc
ELEPHANT_SQL_DB_PASSWORD LWjxpFkYO0sIcGpW2NdhHnMhgHK-myHl
```

```
kubectl create secret generic db-credentials --from-literal=SPRING_DATASOURCE_URL='jdbc:postgresql://ruby.db.elephantsql.com:5432/gzxpwzfc?user= gzxpwzfc&password=LWjxpFkYO0sIcGpW2NdhHnMhgHK-myHl'
```

## Trouble-shooting

- Somehow the deployment fails - because my deployment.yaml file was wrong

```
review          pal-tracker-review-78f68bf564-tc8m7                          0/1     InvalidImageName   0          5m46s
```

- The initial status should look like (this is expected)


```
ubuntu@ip-172-31-87-179:~/workspace/pal-tracker$ k get po -n review -o wide
NAME                                  READY   STATUS             RESTARTS   AGE     IP           NODE                                              NOMINATED NODE   READINESS GATES
pal-tracker-review-58ddff5db-8jmnj    0/1     CrashLoopBackOff   6          16m     10.40.2.16   gke-pal-for-devs-k8s-default-pool-8c12662b-wqpq   <none>           <none>
pal-tracker-review-864c88bb5c-8sgwl   1/1     Running            0          2d14h   10.40.1.15   gke-pal-for-devs-k8s-default-pool-8c12662b-fnzc   <none>           <none>

```


## Wrap-up

- Mike reiterates why we do not have the security sensitive data in
  the file - it is to eliminate any possibility might be accidentally checked in
- Mike shows the similarity/difference between configmap example ??