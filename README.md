# ShortLoop Installation Guide
Create Automated API Collections, that never goes stale.

(Estimated Installation time : ~5 mins.) 


#### Components of ShortLoop : 
* ShortLoop Control Tower
* ShortLoop Web Portal
* ShortLoop Database (postgres)
* ShortLoop SDK (Supported : Java Spring-Boot)



### Installing SDK in **Java Spring-Boot**  Web Application.
> Add OSSRH (Open Source Software Repository Hosting) Repository info in your root pom file.

```xml
<project>
    ...
    ...
	<repositories>
		<repository>
			<id>ossrh</id>
			<url>https://s01.oss.sonatype.org/content/repositories/snapshots</url>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
			<releases>
				<enabled>false</enabled>
			</releases>
		</repository>
	</repositories>
	
</project>
```

> Add Maven Dependency : 

```xml
    <dependencies>
        ...
        ...
        <dependency>
			<groupId>dev.shortloop.agent</groupId>
			<artifactId>agent-java</artifactId>
			<version>0.0.1-SNAPSHOT</version>
		</dependency>
	</dependencies>
```
> application.properties configuration

~~~text
    shortloop.enabled=true
    shortloop.ctUrl=<ShortLoop-Control-Tower-URL-Here>
    shortloop.applicationName=<Application_Name>
~~~
> Add the following piece of code on top of you root level Application.java file 

```Java
@ComponentScan(basePackages = {
    "dev.shortloop"
})
```
After adding the above, your Application.java file should look something like this : 

```Java
... 
@ComponentScan(basePackages = {
    "dev.shortloop"
})
public class Application {
  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
    ...
  }
}

```

### Installing ShortLoop (Without DB)
> Warning : Although this is the easiest way to install and try out ShortLoop, but it relies on the mounted volume of the Host for the Persistent. (DB). So Data Durability is not guaranteed in this approach. 

Step - 1: Download the docker compose file 
(TODO)
Step - 2
```bash
docker-compose --profile self-db up
```

### [Recommended Way] Installing ShortLoop (With External DB) 

Step - 1 : Create a Postgres DB. (TODO)

Step - 2 : Download the docker compose file. 
(TODO)

Step - 3 : Install the application providing the DB Creds. 
```bash
docker-compose --profile external-db up (TODO)
```


Once the installation is completed from either of the above method, ShortLoop Portal will be accessible here. 
> http://localhost:80

NOTE : ShortLoop slowly collects the API information from the network traffic, so that the impact on your system is almost negligible. So might need 30-40 mins after installation to create it's knowledge base.




#### Coming Soon : 
 - Go Lang SDK (Mux, Gin, etc.)
 - Cluster wide installation using Envoy Proxy or eBPF Agent. 
 - AWS/GCP Traffic Mirroring
 



#### Support: 
Feel free to email for a quick support, reporting bug or improvements suggestions.
**Sumit B Mulchandani** (sumit@shortloop.dev)


