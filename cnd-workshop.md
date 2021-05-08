 # config

```
< sang/2pivotal > git clone git@github.com:platform-acceleration-lab/cnd-config-practices-eduk8s.git
Cloning into 'cnd-config-practices-eduk8s'...
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@       WARNING: POSSIBLE DNS SPOOFING DETECTED!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
The RSA host key for github.com has changed,
and the key for the corresponding IP address 140.82.112.3
is unknown. This could either mean that
DNS SPOOFING is happening or the IP address for the host
and its host key have changed at the same time.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Please contact your system administrator.
Add correct host key in /Users/sang/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/sang/.ssh/known_hosts:14
RSA host key for github.com has changed and you have requested strict checking.
Host key verification failed.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

```
< sang/2pivotal > git clone git@github.com:platform-acceleration-lab/cnd-config-practices-eduk8s.git
Cloning into 'cnd-config-practices-eduk8s'...
The authenticity of host 'github.com (140.82.112.3)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? y
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'github.com,140.82.112.3' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```


# Feedback 

- kind related activities

```
  502  cd cnd-deploy-practices-eduk8s/
  503  ./deploy/kind.sh

  505  kind get clusters
  506  brew uninstall kind
  507  brew install kind
  508  brew remove kind
  509  brew uninstall kind
  510  brew install kind
  511  kind get clusters
  
  512  docker ps
  513  docker images
  514  cd ~/.docker
  515  cat config.json
  516  cp config.json config.json.back
  517  s config.json
  518  del config.json
  519  docker login

  521  cat config.json
  522  kind get clusters
  523  kind delete clusters
  524  kind delete cluster --name=cnd-deploy-practices
  525  2p
  526  cd cnd-deploy-practices-eduk8s/
  527  make
  528  k get trainingportals
  529  watch kubectl get pods -A
  530  kind get clusters
  531  k get ns
  532  watch kubectl get pods -A
```

```
  583  docker build -t axykim00/cnd-deploy-practices:latest .
       docker image prune -a
  628  docker images
  630  k get all --namespace cnd-deploy-practices-w01
  632  k delete -f ./deploy/educates/workshop-deploy.yaml --namespace cnd-deploy-practices-w01
 
  636  k delete -f ./deploy/educates/workshop-deploy.yaml
  637  alias |grep zoom
  638  docker images
  639  docker search axykim00/cnd-deploy-practices
  640  docker images
  641  docker rmi 975cc4046786 -f
  642  docker images
  643  docker prune
  644  docker rmi -f $(docker images  --filter "dangling=true" -q)
  645  docker images
  646  docker rmi -f $(docker images  --filter "dangling=true" -q)
  647  docker images
  648  docker pull axykim00/cnd-deploy-practices:latest
  649  docker images
  650  docker search axykim00/cnd-deploy-practices
  
  651  ka -A
  652  ka --namespace cnd-deploy-practices-w01
  653  h |grep delete
  654  k apply -f ./deploy/educates/workshop-deploy.yaml
  655  ka --namespace cnd-deploy-practices-w01
  656  k get workshopsessions
  657  k delete -f ./deploy/educates/workshop-deploy.yaml
  658  env | grep TOKEN
  659  env | grep TMC
  660  env |more
  661  env |grep KUBE
  662  h
  663  k get namespaces
  664  k get trainingportal
  665  h
  666  k get trainingportal
  667  k delete trainingportal cnd-deploy-practices

  669  k get workshop
  670  k get workshopsessions
  671  k get ns
    
  ----------------
  export TMC_API_TOKEN=Gr5iU49iZ5uXMPPYjjEMvESYjLWZ6s9c7cs11aHa2bYqvgWJlqz8YeYdHAFbcWGe
  export KUBECONFIG=/Users/sang/Downloads/kubeconfig-kube-test-a351ffe.yml

  docker image prune -a
  docker delete axykim00/cnd-deploy-practices
  docker build -t axykim00/cnd-deploy-practices:latest .
  docker images  
  docker push axykim00/cnd-deploy-practices
       
  (change line number 57 of workshop-deploy.yaml
  to docker.io/axykim00/cnd-deploy-practices:latest)
  k delete trainingportals cnd-deploy-practices
  k apply -f ./deploy/educates/training-portal.yaml
  k apply -f ./deploy/educates/workshop-deploy.yaml
  k get all -A |grep cnd
  		
  k get trainingportal
  ----------------------
  
  677  k get ns
  678  k get pods --namespace cnd-deploy-practices-ui
  679  k get pods --namespace cnd-deploy-practices-ui --watch
  680  k get ns


  683  k get po -n cnd-deploy-practices-w01
  684  k describe cnd-deploy-practices-w01-docs-6684df949d-25psg -n cnd-deploy-practices-w01
  685  k describe po cnd-deploy-practices-w01-docs-6684df949d-25psg -n cnd-deploy-practices-w01
  686  history
  687  1p
  688  macdown cnd-k8s-instructor-notes/cnd-k8s-instructor-notes.md
  689  history
```

- *This is error when using existing image
  (This no longer occurs - use make kind-start, kind-stop)

```
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  12m                   default-scheduler  Successfully assigned cnd-deploy-practices-w01/cnd-deploy-practices-w01-docs-798bf8f559-8l72c to ip-10-0-5-212.us-east-2.compute.internal
  Normal   Pulling    10m (x4 over 12m)     kubelet            Pulling image "cnd-deploy-practices:latest"
  Warning  Failed     10m (x4 over 12m)     kubelet            Failed to pull image "cnd-deploy-practices:latest": rpc error: code = Unknown desc = failed to pull and unpack image "docker.io/library/cnd-deploy-practices:latest": failed to resolve reference "docker.io/library/cnd-deploy-practices:latest": pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
  Warning  Failed     10m (x4 over 12m)     kubelet            Error: ErrImagePull
  Warning  Failed     6m59s (x21 over 12m)  kubelet            Error: ImagePullBackOff
  Normal   BackOff    114s (x44 over 12m)   kubelet            Back-off pulling image "cnd-deploy-practices:latest"
```

- *Got the following error when "make" on cnd-operate module
  (It was because my current cluster cnd-config was not deleted properly)

```
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  12m                   default-scheduler  Successfully assigned cnd-operate-practices-w01/files-54d6c9449b-wb959 to cnd-config-practices-control-plane
  Normal   Pulling    11m (x4 over 12m)     kubelet            Pulling image "cnd-operate-practices"
  Warning  Failed     11m (x4 over 12m)     kubelet            Failed to pull image "cnd-operate-practices": rpc error: code = Unknown desc = failed to pull and unpack image "docker.io/library/cnd-operate-practices:latest": failed to resolve reference "docker.io/library/cnd-operate-practices:latest": pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
  Warning  Failed     11m (x4 over 12m)     kubelet            Error: ErrImagePull
  Normal   BackOff    10m (x7 over 12m)     kubelet            Back-off pulling image "cnd-operate-practices"
  Warning  Failed     2m36s (x41 over 12m)  kubelet            Error: ImagePullBackOff
```  

- Use the following command to clean up kind clusters

```
  190  make kind-stop
  191  make kind-start
  192  kind get clusters
  193  make kind-stop
  194  kind get clusters
  195*
  196  kind get clusters
  197  kind delete cluster --name cnd-config-practices
```

```
  211  ./gradlew bootBuildImage
  212  docker images
  213  docker tag pal-tracker axykim00/pal-tracker:v1
  219  docker push axykim00/pal-tracker:v1
```
## 2. Building a Spring Boot App

- ??The Slides link?
- The following statement is not true. ?Add use ("ls -lat" to see all files)

```
The project will contain a single .gitignore file as well as the (hidden) git files
```

- ??We need to change the following

```
The Spring project provides a starter to help generate your project.
```  
  
- ??Got into a funny state after "./gradlew clean" the terminal window
  is in funny state - could not get out of the ctrl/c - other 
  tab such as console, editor, slides do not work
  ??How do you get restarted?
  
### Set up your code editor

- ?? What does the following mean?

```
2. Execute the >Java: Create Project command.

3. Watch the bottom status bar as the Java project management extensions are loaded, this may take a minute. At the end of the process you will see a prompt for a project creation archetype. Dismiss it by hitting the ESC key.

You should now see a "JAVA PROJECTS" view near the bottom of the Explorer pane, the Java Project Manager will detect your Gradle built Java project.

4. Switch to the "JAVA PROJECTS" view in the editor Explorer.
```

- ?? No clip-board capability?  It is hard to copy and paste.

- ?? The following messages shows up after 60 minutes? I can dismiss it.
  But the whole session dropped.

```
The time limit for the workshop has expired. Press terminate to end the session
```

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