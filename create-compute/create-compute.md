# Create Linux Compute Instance


## Introduction

Oracle Cloud Infrastructure Compute lets you provision and manage compute hosts, known as instances . You can create instances as needed to meet your compute and application requirements. After you create an instance, you can access it securely from your computer or cloud shell.


**Create Linux Compute Instance**

In this lab, you use Oracle Cloud Infrastructure to create an Oracle Linux instance. 

_Estimated Time:_ 10 minutes



### Objectives

In this lab, you will be guided through the following tasks:

- Create SSH Key on OCI Cloud 
- Create Compute Instance

### Prerequisites

* An Oracle Free Tier or Paid Cloud Account
* A web browser
* Should have completed Lab 1

## Task 1: Create SSH Key on OCI Cloud Shell

The Cloud Shell machine is a small virtual machine running a Bash shell which you access through the Oracle Cloud Console (Homepage). You will start the Cloud Shell and generate a SSH Key to use  for the Bastion  session.

1.  To start the Oracle Cloud shell, go to your Cloud console and click the cloud shell icon at the top right of the page. This will open the Cloud Shell in the browser, the first time it takes some time to generate it.

    ![CONNECT](./images/cloudshellopen.png " ")

    ![CONNECT](./images/cloudshell01.png " ")

    **Note:**  You can use the icons in the upper right corner of the Cloud Shell window to minimize, maximize, restart, and close your Cloud Shell session.

2.  Once the cloud shell has started, create the SSH Key using the following command:

    ```
    <copy>ssh-keygen -t rsa</copy>
    ```
    
    Press enter for each question.
    
    Here is what it should look like.  
    
    ![CONNECT](./images/ssh-key01.png " ")

3.  The public  and  private SSH keys  are stored in ~/.ssh/id_rsa.pub.

4.  Examine the two files that you just created.

    ```
    <copy>cd .ssh</copy>
    ```
    
    ```
    <copy>ls</copy>
    ```

    ![CONNECT](./images/ssh-ls-01.png " ")

    **Note:**  in the output there are two files, a *private key:* `id_rsa` and a *public key:* `id_rsa.pub`. Keep the private key safe and don't share its content with anyone. The public key will be needed for various activities and can be uploaded to certain systems as well as copied and pasted to facilitate secure communications in the cloud.

## Task 2: Create Compute instance
You will need a compute Instance to connect to your brand new MySQL database. 

1. Before creating the Compute instance open a notepad 

2. Do the followings steps to copy the public SSH key to the  notepad 

    Open the Cloud shell
    ![CONNECT](./images/cloudshell-10.png " ")    

    Enter the following command  

    ```
    <copy>cat ~/.ssh/id_rsa.pub</copy>
    ``` 
    ![CONNECT](./images/cloudshell-11.png " ") 

3. Copy the id_rsa.pub content the notepad
        
    Your notepad should look like this
    ![CONNECT](./images/notepad-rsa-key-1.png " ")  

4. To launch a Linux Compute instance, go to 
    Navigation Menu
    Compute
    Instances
    ![CONNECT](./images/05compute01.png " ")

5. On Instances in **(root)** Compartment, click  **Create Instance**
    ![CONNECT](./images/05compute02-00.png " ")

6. On Create Compute Instance 

 Enter Name
    ```
    <copy>myclient</copy>
    ```   
7. Make sure **(root)** compartment is selected 

8. On Placement, keep the selected Availability Domain

9. On Image and Shape click the **Edit** link 
    - On Image: Keep the selected Image, Oracle Linux 8 

    ![CONNECT](./images/05compute03.png " ")  

    - On Shape - Click the **change shape** button
    - Select Instance Shape: VM.Standard.E2.2

    ![CONNECT](./images/05compute-shape.png " ")  

10. On Networking, make sure '**myvcn**' is selected

    'Assign a public IP address' should be set to Yes 
   
  ![CONNECT](./images/05compute04.png " ")

11. On Add SSH keys, paste the public key from the notepad. 
  
    ![CONNECT](./images/05compute-id-rsa-paste.png " ")

12. Click '**Create**' to finish creating your Compute Instance. 

13. The New Virtual Machine will be ready to use after a few minutes. The state will be shown as 'Provisioning' during the creation

    ![CONNECT](./images/05compute07.png " ")

14.	The state 'Running' indicates that the Virtual Machine is ready to use. 

    ![CONNECT](./images/05compute08-a.png " ")

## Task 3: Connect to Compute Instance with SSH key

To connect to **myclient** you will need to properly setup your SSH command. Do the following steps:

1. Copy the public IP address of the active Compute Instance to a notepad

    a. Go to Navigation Menu 
    Compute 
    Instances
    ![CONNECT](./images/db-list.png " ")

    b. Click the `myclient` Compute Instance link
    
    ![CONNECT](./images/05compute08-b.png " ")
    
    c. Copy `myclient` plus  the `Public IP Address` to the notepad

    
2. Indicate the location of the private key you created earlier with **myclient**. 
    
    Enter the username **opc** and the Public **IP Address**.

    Note: The **myclient**  instance shows the  Public IP Address as mentioned on TASK 5: #11
    
    (Your SSH login command should look like this: 
    
    **ssh -i ~/.ssh/id_rsa opc@132.145.170...**) 

    ```
    <copy>ssh -i ~/.ssh/id_rsa opc@<your_compute_instance_ip></copy>
    ```
    ![CONNECT](./images/06connect01-signin.png " ")

    ** You are ready to install MySQL on the  Compute Instance**

**You may now proceed to the next lab**

## Acknowledgements
* **Author** - Dale Dasker, MySQL Solution Engineering
* **Last Updated By/Date** - <Dale Dasker, April 2022
