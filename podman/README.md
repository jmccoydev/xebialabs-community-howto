# Podman

Use the manifests to perform podman deployments.

## Prerequisites

1. Podman Installation(https://podman.io/getting-started/installation.html)
2. Running Podman API Service (https://docs.podman.io/en/latest/markdown/podman-start.1.html)
3. DAI Deploy with xld-podman-integration installed

## How to Start Podman API Service

1. Start the API service with the following command in the System containing podman server "podman system service tcp:localhost:8081 --log-level=debug --time=0"

## How to Create Podman Infrastructure CI

1. Download the "XL CLI" file from the "https://dist.xebialabs.com/public/xl-cli/22.2.1" dist website.
2. Create a new file with the below file content, In the **podmanHost**, specify the podman host which is created once the API service is started.

**infrastructure.yaml**
```yaml
apiVersion: xl-deploy/v1
kind: Infrastructure
spec:
- name: Infrastructure/localhost
  type: podman.Engine
  podmanHost: http://127.0.0.1:8081
```
3. Run the command "xl apply -f infrastructure.yaml" from the location of xl cli application, which should create the required Infrastructure.

## How to Create Podman Environment CI

1. Download the "XL CLI" file from the "https://dist.xebialabs.com/public/xl-cli/22.2.1" dist website.
2. Create a new file with the below file content with file name "environment.yaml", In the **members**, specify the podman Infrastructure which was created in the above section.

**environment.yaml**
```yaml
apiVersion: xl-deploy/v1
kind: Environments
spec:
- name: Environments/localEnv
  type: udm.Environment
  members:
  - Infrastructure/localhost
```
3. Run the command "xl apply -f envrionment.yaml" from the location of xl cli application, which should create the required Environment.

## How to Import Podman Application from dar file

1. Download the sample dar.
2. Unpack the Sample dar using uncompressor tools such as "7-Zip" or "Winrar".
3. Edit the `deployit-manifest.xml` with the below mentioned manifest xml content and follow below steps to import the dar package.
4. You can import a deployment package from an external storage location, your computer, or the Deploy server.

### To import a package:
1. In the left pane, hover over Applications, click Explorer action menu, then select Import.
2. Select one of three options:
3. From URL:
    a.Enter the URL.
    b.If the URL requires authentication, enter the required user name and password.
    c.Click Import.
4. From your computer:
    a.Click Browse and locate the package on your computer.
    b.Click Import.
5. From Deploy server:
    a.Select the package from the list.
    b.Click Import.

## How to Create a new `Podman.Engine` Infrastructure and Test Connection

1  Create an Infrastructure using the xl cli by refering the above section or follow the below steps to create infrastructure manually.
2. Create an Infrastructure of type `Podman.Engine` by hovering over the `Infrastructure`, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select **New**, and then select **Podman** then select **Engine**.
3. In the **Name** field, specify the name of the Configuration Item that will be created.
4. In the **Podman Host**, specify the podman host which is created once the API service is started.
5. Click **save**.
6. Once the Configuration Item is created, Hover over the Configuration Item, click ![Explorer action menu](images/menu_three_dots.png) > **Check Connection** > **Execute** to Test Connection.

## How to Deploy a podman Container

1. Create an [application](/deploy/how-to/add-a-package-to-xl-deploy.html#create-a-package) and a deployment package.
2. Hover over the deployment package, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select `podman.ContainerSpec`.
3. Hover over the deployment package containing the new `podman.ContainerSpec`, click ![Explorer action menu](images/menu_three_dots.png) > **Deploy**, and select the target environment.
4. Click **Continue** and then click **Deploy** to execute the plan.

**Sample Manifest**
```bash
<?xml version="1.0" encoding="UTF-8"?>
<udm.DeploymentPackage version="1.0" application="Test-Container-Spec">
    <application />
    <orchestrator />
    <deployables>
        <podman.ContainerSpec name="/newcont">
            <tags />
            <containerName>sample-newcontainer2505</containerName>
            <image>docker.io/library/tomcat:latest</image>
            <labels />
            <environment />
            <showLogsAfter>1</showLogsAfter>
            <networks />
            <dnsOptions />
            <links />
            <portBindings />
            <volumeBindings />
        </podman.ContainerSpec>
    </deployables>
    <applicationDependencies />
    <dependencyResolution>LATEST</dependencyResolution>
    <undeployDependencies>false</undeployDependencies>
    <templates />
    <boundTemplates />
</udm.DeploymentPackage>
```

## How to Deploy a podman Pod

1. Create an [application](/deploy/how-to/add-a-package-to-xl-deploy.html#create-a-package) and a deployment package.
2. Hover over the deployment package, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select `podman.Service`.
3. Hover over the deployment package containing the new `podman.Pod`, click ![Explorer action menu](images/menu_three_dots.png) > **Deploy**.
4. Click **Continue** and then click **Deploy** to execute the plan.

**Sample Manifest**
```bash
<?xml version="1.0" encoding="UTF-8"?>
<udm.DeploymentPackage version="1.0" application="Test-Pod-Spec">
    <application />
    <orchestrator />
    <deployables>
        <podman.PodSpec name="/newpod">
            <tags />
            <PodName>sample-newpod2505</PodName>
            <labels />
            <cpu_period>1</cpu_period>
            <cpu_quota>1</cpu_quota>
            <infra_command />
            <no_infra>true</no_infra>
            <no_manage_hosts>true</no_manage_hosts>
            <start_pod>false</start_pod>
            <pod_create_command />
            <pod_devices />
            <portmappings />
        </podman.PodSpec>
    </deployables>
    <applicationDependencies />
    <dependencyResolution>LATEST</dependencyResolution>
    <undeployDependencies>false</undeployDependencies>
    <templates />
    <boundTemplates />
</udm.DeploymentPackage>
```
> **Note**: You can deploy a podman Pod.

## How to Deploy a Podman Volume

### Create an independent volume

1. Create an [application](/deploy/how-to/add-a-package-to-xl-deploy.html#create-a-package) and a deployment package.
2. Hover over the deployment package, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select `podman.VolumeSpec`.
3. Enter a name for the service in the **Name** field and specify a **Volume Name** for the volume.
4. Hover over the deployment package containing the new `podman.VolumeSpec`, click ![Explorer action menu](images/menu_three_dots.png) > **Deploy** and select the target environment.
5. Click **Continue** and then click **Deploy** to execute the plan.
6. To check if the volume is created, run this command:
    ```bash
    podman volume ls
    ```
**Sample Manifest**
```bash
<?xml version="1.0" encoding="UTF-8"?>
<udm.DeploymentPackage version="1.0" application="Test-Volume-Spec">
    <application />
    <orchestrator />
    <deployables>
        <podman.VolumeSpec name="/newvol">
            <tags />
            <volumeName>sample-newvol2505</volumeName>
            <driver>local</driver>
            <driverOptions />
            <labels />
        </podman.VolumeSpec>
    </deployables>
    <applicationDependencies />
    <dependencyResolution>LATEST</dependencyResolution>
    <undeployDependencies>false</undeployDependencies>
    <templates />
    <boundTemplates />
</udm.DeploymentPackage>
```

### Attach a volume to a Podman container

1. Hover over the created container, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select **MountedVolumeSpec**.
2. Enter a name for the application version, specify a name for the volume, enter the directory of the Podman container where the volume will be attached in the **Mountpoint** field, and set the default value to *false* in the **Read Only** field.
3. [Deploy](/deploy/how-to/deploy-an-application.html) the created package to the target environment.

The Podman container is created with the mounted volume attached at the mount point.

## How to Create a Podman Network

1. Create an [application](/deploy/how-to/add-a-package-to-xl-deploy.html#create-a-package) and a deployment package.
2. Hover over the deployment package, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select `podman.NetworkSpec`. Perform the same action again to create a `podman.ContainerSpec`.
3. In the **Network Name** field, specify the name of the private network that will be created.
4. Click **Save** to create the network.
5. For the created container, go to the **Network** tab and add the name of the network, which will bind the    
containers.
6. [Deploy](/deploy/how-to/deploy-an-application.html) the application to the target environment.
7. Log in to your Podman host and run this command:
    ```bash
    podman network inspect <network_name>
    ```
8. To verify if the network is created with the Podman host, run this command:
    ```bash
    podman network ls
    ```
**Sample Manifest**
```bash
<?xml version="1.0" encoding="UTF-8"?>
<udm.DeploymentPackage version="1.0" application="Test-Network-Spec">
    <application />
    <orchestrator />
    <deployables>
        <podman.NetworkSpec name="/netspec">
            <tags />
            <networkName>sample-newnet2505</networkName>
            <driver>bridge</driver>
            <networkOptions />
        </podman.NetworkSpec>
    </deployables>
    <applicationDependencies />
    <dependencyResolution>LATEST</dependencyResolution>
    <undeployDependencies>false</undeployDependencies>
    <templates />
    <boundTemplates />
</udm.DeploymentPackage>
```

## Map a Podman container port to a Podman host

Port mapping is used to map the host port with the container port.

### Create a port mapper

1. Create a container inside an application with a deployment package.
2. Hover over the created container, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select **PortSpec**.
3. Enter a name for the application version.
4. In the **Host Port** field, enter the port of the Podman host that will be mapped to the container, the container port, and specify the protocol over which the connection will be established.
5. [Deploy](/deploy/how-to/deploy-an-application.html) the application to the target environment.
6. Log in to your podman host and run this command:
    ```bash
    podman network inspect <network_name>
    ```
7. To verify if the network is created with the podman host, run this command:
    ```bash
    podman network ls
    ```

## How to Create a podman secret

1. Create an [application](/deploy/how-to/add-a-package-to-xl-deploy.html#create-a-package) and a deployment package.
2. Hover over the deployment package, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select `podman.SecretSpec`.
3. Enter a name for the service in the **Name** field and specify a **Secret Name** for the Secret.
4. Specify a **Secret** for the Secret value which needs to be stored.
5. Specify a **Driver** for the Secret if required.
6. Hover over the deployment package containing the new `podman.SecretSpec`, click ![Explorer action menu](images/menu_three_dots.png) > **Deploy** and select the target environment.
7. Click **Continue** and then click **Deploy** to execute the plan.
8. To check if the secret is created, run this command:
    ```
    podman secret ls
    ```

**Sample Manifest**
```bash
<?xml version="1.0" encoding="UTF-8"?>
<udm.DeploymentPackage version="1.0" application="podapp">
  <application />
  <orchestrator />
  <deployables>
    <podman.SecretSpec name="/secretspec">
      <tags />
      <secretName>secretname</secretName>
      <driver>file</driver>
      <secret>secretvalue</secret>
    </podman.SecretSpec>
  </deployables>
  <applicationDependencies />
  <dependencyResolution>LATEST</dependencyResolution>
  <undeployDependencies>false</undeployDependencies>
  <templates />
  <boundTemplates />
</udm.DeploymentPackage>
```

## How to Deploy a File to Container in Podman

1. Create an [application](/deploy/how-to/add-a-package-to-xl-deploy.html#create-a-package) and a deployment package.
2. Hover over the deployment package, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select `podman.File`. Perform the same action again to create a `podman.File`.
3. In the **Name** field, specify the name of the CI that will be created.
4. In the **Choose file**, specify the file location.
5. In the **Target Container**, Specify the Target Container ID.
6. In the **Target Path**, Specify the Target Path inside the Container.
7. Click **Save** to create `podman.File`.
8. [Deploy](/deploy/how-to/deploy-an-application.html) the application to the target environment.
9. To verify if the file is created with the Podman host, go inside the container with bash and verify the file.

**Sample Manifest**
```bash
<?xml version="1.0" encoding="UTF-8"?>
<udm.DeploymentPackage version="1.0" application="podapp">
  <application />
  <orchestrator />
  <deployables>
    <podman.File name="/filetype" file="/filetype/wls-issue.docx">
      <tags />
      <scanPlaceholders>true</scanPlaceholders>
      <preScannedPlaceholders>false</preScannedPlaceholders>
      <placeholders />
      <checksum>225ced63f77e38e436c7c595fbdbc882be239580f892a3cbdf29f1d12b11b0ef</checksum>
      <fileEncodings>
        <entry key=".+\.properties">ISO-8859-1</entry>
      </fileEncodings>
      <isRescanned>false</isRescanned>
      <targetContainer>conatinerId</targetContainer>
      <targetPath>/tmp</targetPath>
    </podman.File>
  </deployables>
  <applicationDependencies />
  <dependencyResolution>LATEST</dependencyResolution>
  <undeployDependencies>false</undeployDependencies>
  <templates />
  <boundTemplates />
</udm.DeploymentPackage>
```

## How to Deploy a Folder to Container in Podman

1. Create an [application](/deploy/how-to/add-a-package-to-xl-deploy.html#create-a-package) and a deployment package.
2. Hover over the deployment package, click ![Explorer action menu](images/menu_three_dots.png) > **New**, and then select `podman.Folder`. Perform the same action again to create a `podman.Folder`.
3. In the **Name** field, specify the name of the Configuration Item that will be created.
4. In the **Choose file**, specify the folder location (Folder should be Zipped and uploaded as a File).
5. In the **Target Container**, Specify the Target Container ID.
6. In the **Target Path**, Specify the Target Path inside the Container.
7. Click **Save** to create `podman.Folder`.
8. [Deploy](/deploy/how-to/deploy-an-application.html) the application to the target environment.
9. To verify if the foler is created with the Podman host, go inside the container with bash and verify the folder.

**Sample Manifest**
```bash
<?xml version="1.0" encoding="UTF-8"?>
<udm.DeploymentPackage version="1.0" application="podapp">
  <application />
  <orchestrator />
  <deployables>
    <podman.Folder name="/folderfile" file="/folderfile/podapp-1.0 (5).zip">
      <tags />
      <scanPlaceholders>true</scanPlaceholders>
      <preScannedPlaceholders>false</preScannedPlaceholders>
      <placeholders />
      <checksum>5e7cc41f1f5068f967f70a41ba19d1b9618eecaf778a46fb948a0b4e197bc7f9</checksum>
      <fileEncodings>
        <entry key=".+\.properties">ISO-8859-1</entry>
      </fileEncodings>
      <isRescanned>false</isRescanned>
      <targetContainer>containerID</targetContainer>
      <targetPath>/tmp</targetPath>
    </podman.Folder>
  </deployables>
  <applicationDependencies />
  <dependencyResolution>LATEST</dependencyResolution>
  <undeployDependencies>false</undeployDependencies>
  <templates />
  <boundTemplates />
</udm.DeploymentPackage>
```
