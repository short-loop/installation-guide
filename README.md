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
and add the inbound for port `80` to be available from source `0.0.0.0/0` (Or your VPN IP if you are using any.)


##### 3. Add the DNS for the ease of access for the public IP. (Optional)
Suggested DNS name : `shortloop.company-name.com`
It's recommended that shortloop is hosted on https, otherwise certain features (eg. copy to clipboard, SSO) might not work as expected.

> NOTE : For the rest of the guide, we will assume that you have the DNS configured to access your instance. If not, just replace `shortloop.company-name.com` with the public IP of the instance. 

___

#### Components of ShortLoop : 
* ShortLoop Application (Control Tower with FrontEnd Portal)
* ShortLoop Database (postgres)

*[All 2 components can be installed via docker-compose file, as shown below.]*


___


### Method-1 : Installing ShortLoop via Docker Compose (With External DB)  [*Recommended*]

**Step - 1 : Create a Postgres database (db_name = shortloop)**
```bash
CREATE DATABASE shortloop;
CREATE USER shortloop_user WITH ENCRYPTED PASSWORD 'shortloop';
GRANT ALL PRIVILEGES ON DATABASE shortloop to shortloop_user;
```

**Step - 2 : In the EC2 instance, download the `docker-compose.yaml` & `.env` files required for installation, using the command below**

```bash
curl -L "https://raw.githubusercontent.com/short-loop/installation-guide/main/scripts/{docker-compose.yaml,.env}" -o "#1"
```

**Step - 3 : Update the downloaded `.env` file.**

Open up the `.env` file in your preferred editor and provide the DB configuration & your Org Name.
`.env` file looks something like this : 
```bash
# Your Org/Company Name. Eg: google, facebook
ORG_NAME=

# DB config (use only if you are providing external DB)
DB_HOST=
DB_PORT=
DB_NAME=
DB_USER=
DB_PWD=

# Analytics configuration. (Only to be filled by ShortLoop Org.)
ANALYTICS_KEY=

# Error Monitoring (Only to be filled by getting value from ShortLoop Org.)
#SENTRY_DSN=

##### Customisation ######

# Port Customisation (default is 80). To Change; uncomment the below line.
#SHORTLOOP_PORT=80
```

**Step - 4 : Start ShortLoop.**
```bash
sudo docker-compose --profile external-db up -d
```

___

### Method-2 : Installing ShortLoop via Docker Compose (Without DB)
> Warning : Although this is the easiest way to install and try out ShortLoop, but it relies on the mounted volume of the Host for the Persistent. (DB). So Data Durability is not guaranteed in this approach. 

**Step - 1 : In the EC2 instance, download the `docker-compose.yaml` & `.env` files required for installation, using the command below**

```bash
curl -L "https://raw.githubusercontent.com/short-loop/installation-guide/main/scripts/{docker-compose.yaml,.env-local-db}" -o "#1"
```


**Step - 2 : Update the downloaded `.env-local-db` file.**
```bash
# Your Org/Company Name. Eg: google, facebook
ORG_NAME=

# Analytics configuration. (Value to be provided by ShortLoop Org.)
ANALYTICS_KEY=

# Error Monitoring (Only to be filled by getting value from ShortLoop Org.)
#SENTRY_DSN=

##### Customisation ######

# Port Customisation (default is 80). To Change; uncomment the below line.
#SHORTLOOP_PORT=80

```

**Step - 3 Start ShortLoop :**
```bash
sudo docker-compose --profile self-db --env-file .env-local-db up -d
```

___

### Method-3 : Installing ShortLoop in K8s via Helm Chart (With External DB)

**Step - 1 : Create a Postgres database (db_name = shortloop)**
```bash
CREATE DATABASE shortloop;
CREATE USER shortloop_user WITH ENCRYPTED PASSWORD 'shortloop';
GRANT ALL PRIVILEGES ON DATABASE shortloop to shortloop_user;
```

**Step - 2 : Add Helm Repository of ShortLoop (if not already added)**
```bash
helm repo add shortloop https://short-loop.github.io/helm-charts
```

**Step - 3 : Update Helm Repositories (Will be needed whenever trying to upgrade ShortLoop to new versions.)**
```bash
helm repo update
```

Verify if the locally synced repository contains the desired version of ShortLoop
```bash
helm repo list
```
**Step - 4 : Install & run  ShortLoop. (replace the variables below with actual values)**
```bash
helm install shortloop shortloop/shortloop \
  --version="0.0.7" \
  --set dbHost=<db_host> \
  --set dbPort=<port> \
  --set dbName=<db_name> \
  --set dbUser=<db_user> \
  --set dbPwd=<db_password> \
  --set analyticsKey=<analytics_key> \
  --set env=<env_name> \
  --set orgName=<org_name>
```

___


#### Accessing & Configuring the Shortloop Portal : 
Once all the components are installed (from either of the above methods), ShortLoop Portal will be accessible here. 
`http://shortloop.company-name.com`


NOTE : ShortLoop slowly collects the API information from the network traffic, so that the impact on your system is almost negligible. So might need 30-40 mins after installation to create it's knowledge base, after you install the SDK.


___

### Installing SDK in **Java Spring-Boot**  Web Application.

**1. Add Maven Dependency :**

```xml
<dependencies>
    ...
    ...
    <dependency>
        <groupId>dev.shortloop.agent</groupId>
        <artifactId>agent-java</artifactId>
        <version>0.0.6</version>
    </dependency>
</dependencies>
```
**2. update application.properties configuration file**

```
    shortloop.enabled=true
    shortloop.url=https://shortloop.company-name.com       # the deployed shortloop url here.
    shortloop.applicationName=service-name                 # yout application name here
```

If you use YAML format : 
```bash
shortloop:
  enabled: 'true'
  url: https://shortloop.company-name.com   # the deployed shortloop url here.
  applicationName: service-name             # yout application name here.
```


**3. Add the following piece of code on top of you root level Application.java file**

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

*Quickly test if project is building after configuring SDK :  (Maybe custom to your project)
```bash
mvn clean install
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


