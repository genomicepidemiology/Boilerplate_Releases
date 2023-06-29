#
This repository will guide us on hosting the packages on anaconda distribution and dockerhub.
To host the packages on specific conda channels, we follow three-step procedure:
1) Mirroring or pushing the code from bitbucket to github
2) Creating releases files
3) Hosting the packages into conda and docker

## Mirroring
  To push the code from bitbucket into github, we need to generate ssh keys and use these keys on both github and bitbucket ssh keys. Below is the clear explanation of generating and using the ssh keys. 
## Generate the SSH keys
To generate the SSH keys in **macOS** or **Linux** with OpenSSh, follow the commands below.  
$ ssh-keygen -t ecdsa -b 521 in home directory prompts to select the location for keys.          
**NOTE**: By default, all ssh keys are stored under ~/.ssh directory with filenames id_rsa(private_key) and id_rsa.pub (public key).   
Generating public/private rsa key pair.  
Enter the filepath to save the key (/home/satyakp/.ssh/id_ecdsa): <name of the keys>  
Your system will know generate the SSH key pair.     

**Output is displayed as below**
<p align="left">
  <img src="./images/ssh_key_output.png" alt="Size Limit CLI" width="538">
</p>

## Setup deploy keys in GITHUB repository

To add the deployment keys on **Github** move to **Github repository**, then to **settings/deploy keys** add our generated public key from **bitbucket** by granting **writing permissions**.

## Create a custom bitbucket-pipeline.yml file

On **bitbucket repository**, create **bitbucket-pipelines.yml** file. 
Adjust the **name** accordingly inside the **custom**.  
For example, on line 5 in the below picture (**mirror-to-github**). 
<p align="left">
  <img src="./images/bitb ucket_pipelines.png" alt="Size Limit CLI" width="538">
</p>

## Setup Github

### Create workflows

In Github account, click **actions** and click **setup the workflow yourself** which eventually **creates the workflows**  in your **.github folder**.

## Create a custom github release.yml file

Then add **release.yml** file in your **workflows directory**

<p align="left">
  <img src="./images/release.yml.png" alt="Size Limit CLI" width="538">
</p>

## Executing the workflow

We have two ways of executing workflows on **bitbucket**.
- The Branch view
- The commits section

To execute the workflow from **branch view** go to **bitbucket repository** and then click **branches** and run the **pipelines.yml** file by selecting the **run pipeline for branch**. Then click **run**.

If you would like to execute the workflow from **commits section**, click **commits view** and select **run pipeline**. Then click **run**.

## Setup Github secrets for the release to CGE channels

To **generate secrets**, 
1. move on to **settings**, 
2. click **actions** 
3. add **new repository secret**.

The below image is the example on how to setup the secrets.
<p align="left">
  <img src="./images/secrets_edited.png" alt="Size Limit CLI" width="538">
</p>

Refer these generated Tokens in the repository to use them in workflows and other configuration files.

# Releases:

## Required files for executing releases:
  
**Dockerfile** - This file contains source path of the code that is build as dockerimage.  
**meta.yaml** - This file contains the source code details of kma project which will be used to build in anaconda distribution.  
**releases.yml** - Workflow file that triggers the events, builds and helps to push the latest changes into dockerhub and anaconda distribution.

For detailed project structure, please visit the Boilerplate_Releases main page. 

## Docker

Once the tag is created, create **dockerfile** and update the **dockerimage** on **Dockerhub**
1. Dockerfile contains the **source path of the project**
2. Create a account in **Dockerhub** and the **project repository**
3. Next, we would give the **project repository** in our **version.yaml** to publish the **code** into our **dockerhub**
4. For example, you can **refer the picture below at line with tags:**
5. **secrtes** are the login credentials of **dockerhub account** that should be stored in your **github-->settings-->actions-->secrtes**.

<p align="left">
  <img src="./images/docker.png" alt="Size Limit CLI" width="538">
</p>

## Conda

We follow the same as **dockerhub** for **conda distribution**
1. We create a **conda folder** that includes **meta.yaml** file and **build.sh** file.
2. In **meta.yaml** file we will have the **package details**, **source path of project**, **requirements** and **Project information**.
3. In **build.sh** file we have the **install commands**
4. We then create **account** on **anaconda.org**
5. The **token** from **anaconda.org** is taken and passed into **github secrets** to publish the code on **anaconda distribution**.

<p align="left">
  <img src="./images/conda1.png" alt="Size Limit CLI" width="538">
</p>

## Releasing

When we want to create a new release, follow these steps:

1. Update the code changes and use (**git add --all**)
2. Commit the changes (**git commit -m v1.2.3**)
3. Tag your commit (**git tag v1.2.3**). Make sure your tag name's format is **v*.*.***. Your workflow will use this tag to detect when to create a release.
4. Push your changes to **Github** (**git push && git push --tags**)
