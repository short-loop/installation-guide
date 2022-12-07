# ShortLoop Installation Guide
Create Automated API Collections, that never goes stale.

___
### Preparation : 
###### Create a fresh EC2 instance with Docker and Docker-Compose installed. 
###### Optionally you can use our publicly available ShortLoop AMI : `ami-0c8dfd6b207aecadf`
###### This Image contains Docker (20.10) & Docker Composed installed on Ubuntu 22-LTS

**Recommended Minimum Configuration :**  
4Gb RAM + 8 Gb Disk  
[ In AWS : T2 Medium] 

___

#### Components of ShortLoop : 
* ShortLoop Control Tower
* ShortLoop Web Portal
* ShortLoop Database (postgres)

*[All 3 components can be installed via docker-compose file, as shown below.]*


___


### Method-1 : Installing ShortLoop (With External DB)  [*Recommended*]

**Step - 1 : Create a Postgres database (db_name = shortloop)**


**Step - 2 : Download the docker compose file of ShortLoop**

```bash
curl -L -o docker-compose.yaml "https://raw.githubusercontent.com/short-loop/installation-guide/main/scripts/docker-compose.yaml"
```


**Step - 3 : The docker-compose file expects a `.env` file when executed, so let's create the `.env` file using the command below.**

```bash
curl -L -o .env "https://raw.githubusercontent.com/short-loop/installation-guide/main/scripts/env_template.txt"
```

**Step - 4 : Add the configurations in the newly created `.env` file.** 
Open up the `.env` file in your preferred editor and provide the DB configuration. 
`.env` file looks something like this : 
```bash
# DB config (use only if you are providing external DB)
DB_HOST=<postgres_db_endpoint>
DB_PORT=<db_port>
DB_NAME=<db_name>
DB_USER=<db_username>
DB_PWD=<db_password>

# Portal Port Customisation (default is 80). To Change; uncomment the below line.
# UI_PORT=80

# Control Tower Port Customisation (default is 8080). To Change; uncomment the below line.
# CONTROL_TOWER_PORT=8080
```


**Step - 5 : Start ShortLoop.**
```bash
docker-compose --profile external-db up -d
```

___

### Method-2 : Installing ShortLoop (Without DB)
> Warning : Although this is the easiest way to install and try out ShortLoop, but it relies on the mounted volume of the Host for the Persistent. (DB). So Data Durability is not guaranteed in this approach. 

**Step - 1: Download the docker compose file**

```bash
curl -L -o docker-compose.yaml "https://raw.githubusercontent.com/short-loop/installation-guide/main/scripts/docker-compose.yaml"
```

**Step - 2 : Create the .env file at the same location (using the template)**

```bash
curl -L -o .env "https://raw.githubusercontent.com/short-loop/installation-guide/main/scripts/env_template.txt"
```

**Step - 3 Start ShortLoop :**
```bash
docker-compose --profile self-db up -d
```

___

### Installing SDK in **Java Spring-Boot**  Web Application.

**1. Add OSSRH (Open Source Software Repository Hosting) Repository info in your root pom file.** 


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

**2. Add Maven Dependency :**

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
**3. update application.properties configuration file**

```
    shortloop.enabled=true
    shortloop.ctUrl=http://host-name:8080       # the deployed control-tower url here.
    shortloop.applicationName=service-name      # application name here how you want discover on portal.
```

**4. Add the following piece of code on top of you root level Application.java file**

```java
@ComponentScan(basePackages = {
    "dev.shortloop"
})
```
*After adding the above, your Application.java file should look something like this :*

```java
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


Once the installation is completed from either of the above method, ShortLoop Portal will be accessible here. 
> http://localhost:80

NOTE : ShortLoop slowly collects the API information from the network traffic, so that the impact on your system is almost negligible. So might need 30-40 mins after installation to create it's knowledge base.


___

#### Coming Soon : 
 - Go Lang SDK (Mux, Gin, etc.)
 - Cluster wide installation using Envoy Proxy or eBPF Agent. 
 - AWS/GCP Traffic Mirroring
 

---

#### Support: 
> Feel free to email for a quick support, reporting bug or improvements suggestions.
**Sumit B Mulchandani** (sumit@shortloop.dev)


