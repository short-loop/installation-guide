---
title: SDK Installation Instructions
layout: default
filename: sdk.md
--- 


# ShortLoop SDK Installation Instructions

### Installing SDK in **Java Spring-Boot**  Web Application.

**1. Add Maven Dependency :**

```xml
<dependencies>
    ...
    ...
    <dependency>
        <groupId>dev.shortloop.agent</groupId>
        <artifactId>agent-java</artifactId>
        <version>0.0.7</version>
    </dependency>
</dependencies>
```
**2. update application.properties configuration file**

```
    shortloop.enabled=true
    shortloop.url=https://shortloop.company-name.com       # the deployed shortloop url here.
    shortloop.applicationName=service-name                 # your application name here
```

If you use YAML format : 
```bash
shortloop:
  enabled: 'true'
  url: https://shortloop.company-name.com   # the deployed shortloop url here.
  applicationName: service-name             # your application name here.
```


**3. Add the following piece of code on top of you root level Application.java file**


```Java
import dev.shortloop.agent.ShortloopAutoConfiguration;
```


```Java
@Import(ShortloopAutoConfiguration.class)
```


*After adding the above, your Application.java file should look something like this :*

```java
... 
import dev.shortloop.agent.ShortloopAutoConfiguration;
...
...

...
@Import(ShortloopAutoConfiguration.class)
public class Application {
  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
    ...
  }
}

```

*Quickly test if project is building after configuring SDK :  (Maybe custom to your project)
```bash
mvn clean install
```

After the changes, redeploy your Java Application.

### Installing SDK in **Go Gin**  Web Application.

**1. `shortloop-go` can be installed like any other Go Package through `go get`:**

```bash
$ go get github.com/short-loop/shortloop-go@latest
```

**2. Add the following piece of code on top of you root level main.go file where you initialize your router**

```Go
import "github.com/short-loop/shortloop-go/shortloopgin"
```

**3. Initialize the shortloop sdk**
To use shortloop-go sdk, you’ll need to initialize it with options - ShortloopEndpoint and ApplicationName as shown in below example.
```Go
router := gin.Default()
shortloopSDK, err := shortloopgin.Init(shortloopgin.Options{
    ShortloopEndpoint: "https://shortloop.company-name.com",
    ApplicationName:   "service-name",
})
if err != nil {
    fmt.Println("Error initializing shortloopSDK: ", err)
} else {
    router.Use(shortloopSDK.Filter())
}
```

*Quickly test if project is building after configuring SDK :  (Maybe custom to your project)
```bash
go build main.go
```

After the changes, redeploy your Go Application.
___

#### Coming Soon : 
 - Go Lang SDK Mux
 - Cluster wide installation using Envoy Proxy or eBPF Agent. 
 - AWS/GCP Traffic Mirroring

---

#### Support: 
> Feel free to email for a quick support, reporting bug or improvements suggestions.
**Sumit B Mulchandani** (sumit@shortloop.dev)


