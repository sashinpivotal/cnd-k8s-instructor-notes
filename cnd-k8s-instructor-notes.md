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
export KUBE_EDITOR=code
alias siege1='siege -p http://development.sashin.k8s.pal.pivotal.io/ -r1000 -q'
alias refresh='source ~/.bashrc'
alias test1='curl http://development.sashin.k8s.pal.pivotal.io/'
alias test2='curl http://review.sashin.k8s.pal.pivotal.io/'
```

```
- Keyboard shortcut keys within Remote Desktop
  - ALT+F10 (maximizing window on and off)
  - ALT+Tab (move between windows)
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

- https://github.com/platform-acceleration-lab/pal-tracker-k8s-deployment

- Take the followin steps

```
- cd <pal-tracker>
- git reset --hard <solution-tag>
- cd ../pal-tracker-deployment-k8s
- git reset --hard <solution-tag>
- cp -r ./*.yaml ../pal-tracker/k8s (?? Try this)
- cd <pal-tracker>
- change DOCKER-USER-NAME in the base/deployment.yaml
- change domain name in the environments/development/ingress.yaml
- change domain name in the environments/review/ingress.yaml
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


# Containerizing an App

## Extra resources

- [What's new in Spring Boot 2.3](https://www.youtube.com/watch?v=WL7U-yGfUXA)

## Misc.

- Why the docker image that is built says 40 years old?

## Challenge exercise

- Install and try "dive" to analyze the layers of docker image

```
wget https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.deb
sudo apt install ./dive_0.9.2_linux_amd64.deb

dive pal-tracker
```

## Deploying to K8s (??)

## Intro

- Use VSC with terminal window on the right configuration
- *What is the domain name? development.sashin.k8s.pal.pivotal.io

## Challenge exercises

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
  - host: abc.development.sashin.k8s.pal.pivotal.io
    http:
      paths:
      - path: /
        backend:
          serviceName: pal-tracker
          servicePort: 8080
  backend:
    serviceName: pal-tracker
    servicePort: 8080
```

## Wrap-up

- Talk about each resource in the lab - use the diagram

  - the gray ones are the ones that get created by K8s
  - What is the role of deployment? ReplicaSet?
  - Single responsibility principle - each resource handles one thing
  
- Demo Octant 

  - Mis-configure each resource object and see how Resource Viewer responds

## Misc.

- kubectl create vs apply - imperative vs declarative (Mike G)

# Using ConfigMaps to set environment variables

- ?? We could make the following as optional exercise -Logging document from K8s ref document

```
On the Google Compute Engine (GCE) platform, the default logging support targets Stackdriver Logging, which is described in detail in the Logging With Stackdriver Logging.

This article describes how to set up a cluster to ingest logs into Elasticsearch and view them using Kibana, as an alternative to Stackdriver Logging when running on GCE.
```

- ?? Do we use this in our lab?  What is the lab that does logging?
- configmap vs using env in the deployment
- Keith says Spring Boot K8s lets having configmap to be testable in TDD?

# Actuator

## Intro

- cost and benefit analysis - free?
- monitoring and management (refresh, logging)

## Wrap-up

- Bill went through all endpoints using Postman
   - all down 
- Actuator vs Platform-provided tool (dynatrace or appdyanmics)

# Configurating Availability State Probes

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
- application should deal with transitent failure - production env - you can tune only in production
- what would be good candidates for liveness probes - out of memory, not network blip

# Configuring RollingUpdate Deployments

- The lab document says to create docker v2. How do I create a new docker image?

```
docker tag pal-tracker YOUR-DOCKER-HUB-USERNAME/pal-tracker:v0
docker push YOUR-DOCKER-HUB-USERNAME/pal-tracker:v0
```

- this is how to create a new version

```
./gradlew bootBuildImage --imageName=axykim00/pal-tracker:v2
docker push axykim00/pal-tracker:v2
```

## Wrap-up

- ??two messages race condition
  - show architecure document - controller is a loop checking etds for messages
  -delete pod (pod controller picked up) - will wait 10 seconds before killing it
  -stop routing message (by service controller)
  -?? spring drain 
  -why is prestop not built-in in k8s? k8s says it is not my problem
   it can be handled by higher level abstraction
   
- 12 facotor app
  -strip out as much as from yur spring boot app - faster start up
  -stopping micro-services?? challenge
  -scalabiltu
  
  
  
# Liveness probe

- ??step 6 - it is not 10 seconds it is 2 hand half miunutes
- ??step 7 strange behavior - use siege instead of browser

# Multi-enviroment

- *After reseting pal-tracker-k8s-development to multi-environment solution, I get the following error. (answer) I have to use -k like `kubectl apply -k k8s/environments/development`

```
ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ k apply -f k8s/environments/review
configmap/pal-tracker created
ingress.networking.k8s.io/pal-tracker created
error: error validating "k8s/environments/review/kustomization.yaml": error validating data: [apiVersion not set, kind not set]; if you choose to ignore these errors, turn validation off with --validate=false

ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ k apply -f k8s/environments/development
configmap/pal-tracker configured
ingress.networking.k8s.io/pal-tracker configured
error: error validating "k8s/environments/development/kustomization.yaml": error validating data: [apiVersion not set, kind not set]; if you choose to ignore these errors, turn validation off with --validate=false
```

# Pipeline

## Intro

- What is CI, why CI?
  - many developers are working on a project, we can move faster,
  - remove merge conflict after waiting for a week
- steps for CI (bill used PCF slide)
- Bill likes concourse - versioning jar files
- what and why CD? 
  - make our app to be usable, release frequently
  - deploy smmothly and consistently
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

# Scaling apps with Kubernetes

- *I get the following error.  It is because I used -f instead of -k

```
ubuntu@ip-172-31-87-179:~/workspace/pal-tracker$ k apply -f k8s/environments/development
configmap/pal-tracker unchanged
ingress.networking.k8s.io/pal-tracker unchanged
error: error validating "k8s/environments/development/kustomization.yaml": error validating data: [apiVersion not set, kind not set]; if you choose to ignore these errors, turn validation off with --validate=false
```

- *After horizontal scaling, the number of pods remain to be 2. It should have been 1.
  It took time.  But eventually, it became 1.
  
  
## Wrap-up

- Bill - what metrics do you see to determine memory requirement?
  - for Web app - throughput is primary criteria, cpu might not
    be a good measure 
  - auto-scaler might not detect all the possible factors
    use it only after you understand the pattern loads
    
  - show the knobs and dials in the reference document - you
    have to understand the traffic pattern
    
- Mile is against auto-scaling.  ??Why

# Migration

Error in the lab. grab17

```
- run: sudo /etc/init.d/postgresql restart
```

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

url from the elephant server - MAKE SURE TO click the eye-icon to see the full path

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

With my server info filled in

url from the elephant server - MAKE SURE TO click the eye-icon to see the full path

```
postgres://malyyjbd:i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ@ruby.db.elephantsql.com:5432/malyyjbd 
```

```
~/workspace/pal-tracker$ flyway -url="jdbc:postgres://malyyjbd:i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ@ruby.db.elephantsql.com:5432/malyyjbd" -locations=filesystem:databases/tracker migrate -user=sashin@vmware.com -password=i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ

Flyway Community Edition 6.3.1 by Redgate
ERROR: 
Unable to obtain connection from database (jdbc:postgresql://ruby.db.elephantsql.com:5432/malyyjbd) for user 'sashin@vmware.com': FATAL: no pg_hba.conf entry for host "3.235.179.117", user "sashin@vmware.com", database "malyyjbd", SSL on
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
SQL State  : 28000
Error Code : 0
Message    : FATAL: no pg_hba.conf entry for host "3.235.179.117", user "sashin@vmware.com", database "malyyjbd", SSL on

```

```
ubuntu@ip-172-31-87-179:~/workspace/pal-tracker$ flyway -url="jdbc: postgresql://malyyjbd:i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ@ruby.db.elephantsql.com:5432/malyyjbd" -locations=filesystem:databases/tracker migrate -user=sashin@vmware.com -password=i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ
ERROR: Unable to autodetect JDBC driver for url: jdbc: postgresql://malyyjbd:i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ@ruby.db.elephantsql.com:5432/malyyjbd
```

```
ubuntu@ip-172-31-87-179:~/workspace/pal-tracker$ flyway -url="jdbc:postgres://malyyjbd:i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ@ruby.db.elephantsql.com:5432/malyyjbd" -locations=filesystem:databases/tracker migrate -user=sashin@vmware.com -password=i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ
ERROR: Unable to autodetect JDBC driver for url: jdbc:postgres://malyyjbd:i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ@ruby.db.elephantsql.com:5432/malyyjbd
```

```
psql -h ELEPHANT_SQL_SERVER -U ELEPHANT_SQL_USER
```

```
ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ psql -h ruby.db.elephantsql.com -U sashin@vmware.com
Password for user sashin@vmware.com: 
psql: FATAL:  password authentication failed for user "sashin@vmware.com"
FATAL:  password authentication failed for user "sashin@vmware.com"

ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ psql -h ruby.db.elephantsql.com -U malyyjbd
Password for user malyyjbd: 
psql: FATAL:  password authentication failed for user "malyyjbd"
FATAL:  password authentication failed for user "malyyjbd"

ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ psql -h ruby.db.elephantsql.com -U malyyjbd -W i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ
Password: 
psql: FATAL:  no pg_hba.conf entry for host "3.235.179.117", user "malyyjbd", database "i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ", SSL on
FATAL:  no pg_hba.conf entry for host "3.235.179.117", user "malyyjbd", database "i-qrVszWjVTrYoVi99yuC-Pov-ajmkdQ", SSL off


ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ psql -h ruby.db.elephantsql.com (ruby-01) -U malyyjbd
bash: syntax error near unexpected token `('

ubuntu@ip-172-31-82-120:~/workspace/pal-tracker$ psql -h "ruby.db.elephantsql.com (ruby-01)" -U malyyjbd
psql: could not translate host name "ruby.db.elephantsql.com (ruby-01)" to address: Name or service not known

```

# Secret

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

```
ubuntu@ip-172-31-87-179:~/workspace/pal-tracker$ kubectl logs -lapp=pal-tracker -f
2020-07-29 09:09:51.466  INFO 1 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.36]
2020-07-29 09:09:53.764  INFO 1 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2020-07-29 09:09:53.765  INFO 1 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 46696 ms
2020-07-29 09:10:11.073  INFO 1 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2020-07-29 09:10:20.671  INFO 1 --- [           main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 3 endpoint(s) beneath base path '/actuator'
2020-07-29 09:10:22.370  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2020-07-29 09:10:22.866  INFO 1 --- [           main] i.p.pal.tracker.PalTrackerApplication    : Started PalTrackerApplication in 91.014 seconds (JVM running for 102.645)
2020-07-29 09:10:26.470  INFO 1 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2020-07-29 09:10:26.563  INFO 1 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2020-07-29 09:10:26.871  INFO 1 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 308 ms
^C
ubuntu@ip-172-31-87-179:~/workspace/pal-tracker$ kubectl logs -lapp=pal-tracker -f -n review

Reason: Failed to determine a suitable driver class


Action:

Consider the following:
	If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
	If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).

2020-07-29 01:18:41.417  INFO 1 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.36]
2020-07-29 01:18:41.667  INFO 1 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2020-07-29 01:18:41.670  INFO 1 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 4420 ms
2020-07-29 01:18:43.175  INFO 1 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2020-07-29 01:18:44.111  INFO 1 --- [           main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 3 endpoint(s) beneath base path '/actuator'
2020-07-29 01:18:44.258  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2020-07-29 01:18:44.297  INFO 1 --- [           main] i.p.pal.tracker.PalTrackerApplication    : Started PalTrackerApplication in 8.459 seconds (JVM running for 9.588)
2020-07-29 01:18:52.790  INFO 1 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2020-07-29 01:18:52.793  INFO 1 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2020-07-29 01:18:52.823  INFO 1 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 29 ms
```

- *After secret db-credentials is created, run the following command - failure
  (answer - I have to use -k)

```
ubuntu@ip-172-31-87-179:~/workspace/pal-tracker$ k apply -f k8s/environments/development
configmap/pal-tracker configured
ingress.networking.k8s.io/pal-tracker configured
error: error validating "k8s/environments/development/kustomization.yaml": error validating data: [apiVersion not set, kind not set]; if you choose to ignore these errors, turn validation off with --validate=false
```

## Wrap-up

- Mike reiterates why we do not have the security sensitive data in
  the file - it might be accidentally checked in
- Mike shows the similarity/difference between configmap example