# ShortLoop Installation Guide
Create Automated API Collections, that never goes stale.

___
### Preparation : 
##### 1. Create EC2 instance : 

Create a fresh EC2 instance with Docker and Docker-Compose installed. 
You can use our publicly available ShortLoop AMI : `ami-0c8dfd6b207aecadf`
This Image contains Docker (20.10) & Docker Composed installed on Ubuntu 22-LTS.


**Recommended Minimum Configuration :**  
4Gb RAM + 8 Gb Disk  
[ In AWS : T2 Medium] 

##### 2. Open up the ports : 
In the security group of your EC2 instance, edit the "Inbound Rules"
and add the inbound for port `80` & `8080` to be available from source `0.0.0.0/0` (Or your VPN IP if you are using any.)


##### 3. Add the DNS for the ease of access for the public IP. (Optional)
Suggested DNS name : `shortloop.company-name.com`

> NOTE : For the rest of the guide, we will assume that you have the DNS configured to access your instance. If not, just replace `shortloop.company-name.com` with the public IP of the instance. 

___

#### Components of ShortLoop : 
* ShortLoop Control Tower
* ShortLoop Web Portal
* ShortLoop Database (postgres)

*[All 3 components can be installed via docker-compose file, as shown below.]*


___


### Method-1 : Installing ShortLoop (With External DB)  [*Recommended*]

**Step - 1 : Create a Postgres database (db_name = shortloop)**


**Step - 2 : In the EC2 instance, download the `docker-compose.yaml` & `.env` files required for installation, using the command below**

```bash
curl -L "https://raw.githubusercontent.com/short-loop/installation-guide/main/scripts/{docker-compose.yaml,.env}" -o "#1"
```

**Step - 3 : Update the downloaded `.env` file.**

Open up the `.env` file in your preferred editor and provide the DB configuration & your Org Name.
`.env` file looks something like this : 
```bash

# Your Org/Company Name. Eg: google, facebook
ORG_NAME=<org_name>

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

**Step - 4 : Start ShortLoop.**
```bash
sudo docker-compose --profile external-db up -d
```

___

### Method-2 : Installing ShortLoop (Without DB)
> Warning : Although this is the easiest way to install and try out ShortLoop, but it relies on the mounted volume of the Host for the Persistent. (DB). So Data Durability is not guaranteed in this approach. 

**Step - 1 : In the EC2 instance, download the `docker-compose.yaml` & `.env` files required for installation, using the command below**

```bash
curl -L "https://raw.githubusercontent.com/short-loop/installation-guide/main/scripts/{docker-compose.yaml,.env}" -o "#1"
```


**Step - 2 Start ShortLoop :**
```bash
sudo docker-compose --profile self-db up -d
```

___

#### Accessing & Configuring the Shortloop Portal : 
Once all the components are installed (from either of the above methods), ShortLoop Portal will be accessible here. 
`http://shortloop.company-name.com`

Initially the Portal will ask you to add the configuration for `CT_URL` 
Add `http://shortloop.company-name.com:8080` and hit submit. 

NOTE : ShortLoop slowly collects the API information from the network traffic, so that the impact on your system is almost negligible. So might need 30-40 mins after installation to create it's knowledge base.


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
        <version>0.0.2-SNAPSHOT</version>
    </dependency>
</dependencies>
```
**3. update application.properties configuration file**

```
    shortloop.enabled=true
    shortloop.ctUrl=http://shortloop.company-name.com:8080       # the deployed control-tower url here.
    shortloop.applicationName=service-name      # application name here how you want discover on portal.
```

**4. Add the following piece of code on top of you root level Application.java file**

```Java
@Import(ShortloopAutoConfiguration.class)
```

```Java
import dev.shortloop.agent.ShortloopAutoConfiguration;
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

After the changes, redeploy your Java Application.

___

#### Coming Soon : 
 - Go Lang SDK (Mux, Gin, etc.)
 - Cluster wide installation using Envoy Proxy or eBPF Agent. 
 - AWS/GCP Traffic Mirroring
 

---

#### Support: 
> Feel free to email for a quick support, reporting bug or improvements suggestions.
**Sumit B Mulchandani** (sumit@shortloop.dev)


