# Snyk Boot Web Demo - Pas DOTC Events - Updated by pas

![alt tag](https://i.ibb.co/7tnp1B6/snyk-logo.png)

Spring Boot demo application used for Snyk security scanning demos, including container image builds and Kubernetes deployment examples.

## Steps to create the container image

- Ensure you have JDK 11 installed as follows

```shell
$ java -version
java version "11.0.13" 2021-10-19 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.13+10-LTS-370)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.13+10-LTS-370, mixed mode)
```

- Build the artifact image as follows

```shell
$ mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------------< com.example:snyk-boot-web >----------------------
[INFO] Building snyk-boot-web 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
....

[INFO]
[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ snyk-boot-web ---
[INFO] Building jar: /Users/pasapicella/snyk/demos/languages-code-demos/java/snyk-boot-web/target/snyk-boot-web-0.0.1-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:2.3.10.RELEASE:repackage (repackage) @ snyk-boot-web ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  5.595 s
[INFO] Finished at: 2024-02-14T11:03:49+11:00
[INFO] ------------------------------------------------------------------------
```

- Make sure the Spring Boot JAR file was created at `target/snyk-boot-web-0.0.1-SNAPSHOT.jar`

- Create a container image as follows. These steps use Docker Hub and require Docker Desktop to be running.

```shell
$ docker build --tag=snyk-boot-web:latest .
$ docker tag snyk-boot-web:latest {userID}/snyk-boot-web:latest
$ docker push {USERID}/snyk-boot-web:latest
```
## Run on Kubernetes

- Sample Kubernetes manifests are available in the `./kubernetes` folder and can be used as a starting guide.

_Note: No service is exposed so you can't access the application without a Service or using port forward to allow access. The Kubernetes integration does not need the application exposed_ 

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: snyk-boot-web
  namespace: apples
spec:
  selector:
    matchLabels:
      app: snyk-boot-web
  replicas: 1
  template:
    metadata:
      labels:
        app: snyk-boot-web
    spec:
      containers:
        - name: snyk-boot-web
          image: pasapples/snyk-boot-web:v1
          imagePullPolicy: Always
          ports:
            - containerPort: 5000
```

---

Pas Apicella [pas at snyk.io] is a Principal Solution Engineer at Snyk APJ
