---
title: SDK Installation Instructions (Meesho)
layout: default
filename: sdk-meesho.md
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
        <version>0.0.11</version>
    </dependency>
</dependencies>
```
**2. update application.properties configuration file**

```
    shortloop.enabled=true
    shortloop.url=https://meesho.shortloop.dev
    shortloop.applicationName=service-name  # your application name here
```

If you use YAML format : 
```bash
shortloop:
  enabled: 'true'
  url: https://meesho.shortloop.dev
  applicationName: service-name   # your application name here.
```

NOTE : Should be enabled only on QA environment for now. 



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


NOTE : ShortLoop slowly collects the API information from the network traffic, so that the impact on your system is almost negligible. So might need 30-40 mins after installation to create it's knowledge base to show you the APIs list; after you install the SDK.

##### You can view the your API collections on the portal : 
[https://meesho.shortloop.dev](https://meesho.shortloop.dev)

(Default Password : `admin`)
[Change the password by clicking "Change Password" on bottom right corner.]

___


#### Support: 
> Feel free to email for a quick support, reporting bug or improvements suggestions.
**Sumit B Mulchandani** (sumit@shortloop.dev) 




