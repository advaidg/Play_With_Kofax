# KTA in Docker
![Article cover image](https://media-exp1.licdn.com/dms/image/D5612AQFK-Mk9EDqzqA/article-cover_image-shrink_720_1280/0/1659038349468?e=1664409600&v=beta&t=faEkRx6XvjKv0YIdlAV_jN5Vm0gC66G1cUavWLQ52aQ)
### **Why Docker!**
 
Docker enables you to separate an application from its infrastructure by providing the ability to package and run an application such as Kofax Total Agility in a loosely isolated environment called a Container. Multiple containers can run on the same machine and share the OS kernel with other containers, each running as isolated processes in user space.

### **Kofax Total Agility and Docker Container**

We can deploy Kofax Total Agility into any environment (Dev, Prod) as an independent container or set of containers. You do not need to use the Kofax Total Agility installation media every time you want a new transformation server/web server. Instead, Kofax Total Agility can be packaged to a docker image (App Server/ Web Server/ Transformation Server/ Standalone), and only the relevant configuration settings, such as database connection strings, are required when the container runs.

**Note:** The SQL Server runs on another machine, which cannot run on the same Kofax Total Agility container**.**

### **Below are the Steps to Install Kofax Total Agility in a Docker container**

**Initial Setup :**

-   Extract the Kofax Total Agility install media contents to a local directory on the Docker host machine.

![KTA Installation Folder](https://media-exp1.licdn.com/dms/image/D5612AQHNnQMKTl1nuA/article-inline_image-shrink_1500_2232/0/1659039170383?e=1664409600&v=beta&t=lMMfzfagc3uWgofAdq5Sd3EbiwZc9fhBSR6STdhuYbs)

  

**Note:** The host machine must have at least 100 GB of disk space for the Kofax Total Agility Docker Installation.

```
 Create a working directory
   C:\Kofax Docker
```
-   From the Installation media, copy the contents of \Total Agility\Utilities\Docker folder to the local working directory created above.

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/D5612AQE_dJbPQMrDOw/article-inline_image-shrink_1000_1488/0/1659039807648?e=1664409600&v=beta&t=tF_Gcy7TriOVSTdDrGcHbFpLbrRh4EVUOa5tQpKTCG4)

-   Copy the Kofax Total Agility installation media into your Container Files folder in the working directory.

**Docker Installation :**

-   Next is installing Docker on the host machine before the Total Agility container. Open PowerShell and type the following command
```
Install-Package Docker -Provider Name DockerMsftProvider -Force
```
-   Check whether a reboot is required or not using the command : 
```
(Install-WindowsFeatures Containers). RestartNeeded
```
-   To Restart 
```
Restart-Computer
```
-   After the machine is up, your Docker installation should be complete and check whether the docker-engine service is running.
-   To check the information about the installed docker instance, you can use the command
```
Docker info
```
-   The next activity is to check for the default max size of a container.
-   We need to check the "daemon. json" file to do that. If not present, please create the same and append the storage criteria 
 ```
 {
 "storage-opts": ["size=50GB"] 
 }
 ```

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/D5612AQF2COc-7u3Evw/article-inline_image-shrink_1000_1488/0/1659040551811?e=1664409600&v=beta&t=KrsbvXcTSDrdx1kbBaLpYJnGhBePPOslcuPf6or4lr0)

-   After this, restart the Docker engine from the Service Console

**Creating a docker image:**

**Note:** Based on the type of installation required configure the silent-install configuration.xml

-   To begin the process, execute the below command.
```
 docker build -t AnyNameForyourContainer “C:\LocalWorkingDirectory”
```
-   Leave it for 30 minutes or more until you see a message Successfully tagged
```
AnyNameForyourContainer: latest
```
This installs the KTA components as configured in the silent install config.

**Set up the KTA DB:**

SQL Server can be installed either in a Windows Server, Azure Managed SQL instance, or the same machine.

**Note:** SQL Server login needs to be created

**Deploying kta-image to a docker container :**

-   Next up is the initialization of the Kofax Total Agility using environment variables (Configurations). For that, we need to generate **"dockersettings.env"** which will contain all the settings.
-   Launch the **KTAConfigurationUtility.exe** from utilities, select the docker mode option in the editor tool, and then click OK.

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/D5612AQGO15Dydbz9rQ/article-inline_image-shrink_1000_1488/0/1659041290930?e=1664409600&v=beta&t=wGdbG88h12MeCTiIT2OgJu3wZdRqm9j1bG1Mn3Mj7ps)

-   On the Kofax Configuration Editor tool in the Common tab, mark the changes if required or leave it to default and click the **"Save Docker Settings"** button.
-   You should be able to see the docker configuration file generated at the bottom in the Configuration Utility folder.
-   Now to spin up the container
```
 Docker run -d –name "ContainerNameHere " --env-file "Full-Path - Generated Env File" -p 5000:80 ImageName
```
### CONGRATS! , you successfully have built and deployed Kofax Total Agility inside a container

**Inspect the container:**

-   Execute the following command in your PowerShell window to check if the docker container is running successfully.
```
 docker ps -a 
```
"Docker Inspect" command will give you the details of the running container.

**Note :**

-   You can SSH into your container and run various windows commands to see the status of the deployed Kofax services.
-   Using the container IP we can access the Kofax designer.
