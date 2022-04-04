# INSTALL - MYSQL ENTERPRISE EDITION

## Introduction

Detailed Installation of MySQL Enterprise Edition 8.0 and MySQL Shell on Linux
Objective: Tarball Installation of MySQL 8 Enterprise on Linux


Tarball Installation of MySQL Enterprise 8 on Linux

Estimated Time: 15 minutes

### Objectives

In this lab, you will:
* Install MySQL Enterprise Edition
* Start and test MySQL Enterpriese Edition Install
* Install MySQL Shell and Connect to MySQL Enterprise 


### Prerequisites

This lab assumes you have:
* An Oracle account
* All previous labs successfully completed

* Lab standard  
    - ![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell> the command must be executed in the Operating System shell
    - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql> the command must be executed in a client like MySQL, MySQL Workbench
    - ![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh> the command must be executed in MySQL shell
    
## Task 1: Install MySQL Enterprise Edition

1. Usually to run mysql  the user “mysql” is used, but because it is already available we show here how create a new one.
2. Create a new user/group for your MySQL service (mysqluser/mysqlgrp) and a add ‘mysqlgrp’ group to opc to help labs execution. 

3.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo groupadd mysqlgrp</copy>
    ```
  
4.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo useradd -r -g mysqlgrp -s /bin/false mysqluser</copy>
    ```
  
5.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 

    ```
    <copy>sudo usermod -a -G mysqlgrp opc</copy>
    ```
6. Close and reopen shell session or use “newgrp” command as below

7.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>newgrp - mysqlgrp</copy>
    ```


8.	Create new directory structure:

9.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo mkdir /mysql/ /mysql/etc /mysql/data</copy>
    ```

10. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo mkdir /mysql/log /mysql/temp /mysql/binlog</copy>
    ```

11.	Extract the tarball in your /mysql folder

12. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>cd /mysql/</copy>
    ```

13. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>sudo tar xvf /workshop/mysql_8.0.28/mysql-commercial-8.0.28-linux-glibc2.12-x86&#95;64.tar.xz</copy>
    ```

14.	Create a symbolic link to mysql binary installation

15. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo ln -s mysql-commercial-8.0.28-linux-glibc2.12-x86&#95;64 mysql-latest</copy>
    ```

16.	Create a new configuration file my.cnf inside /mysql/etc
To help you we created one with some variables, please copy it

17. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo cp /workshop/my.cnf.first /mysql/etc/my.cnf</copy>
    ```

18.	For security reasons change ownership and permissions

19. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo chown -R mysqluser:mysqlgrp /mysql</copy>
    ```

20. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo chmod -R 755 /mysql</copy>
    ```

21. The following permission is for the Lab purpose so that opc account can make changes and copy files to overwrite the content

22. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo chmod -R 770 /mysql/etc</copy>
    ```

23.	initialize your database

24. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**

    ```
    <copy>sudo /mysql/mysql-latest/bin/mysqld --defaults-file=/mysql/etc/my.cnf --initialize --user=mysqluser</copy>
    ```
## Task 2: Start and test MySQL Enterprise Edition Install

1.	Start your new mysql instance

2.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>sudo /mysql/mysql-latest/bin/mysqld --defaults-file=/mysql/etc/my.cnf --user=mysqluser &</copy>
    ```

3.	Verify that process is running

4.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>ps -ef | grep mysqld</copy>
    ```

5.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>netstat -an | grep 3306</copy>
    ```


6.	Another way is searching the message “ready for connections” in error log as one of the last

7.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>grep -i ready /mysql/log/err&#95;log.log</copy>
    ```

8. Install the MySQL Shell command line utility

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
     ```
    <copy>sudo yum -y install /workshop/shell/mysql-shell-commercial-8.0.28-1.1.el8.x86_64.rpm</copy>
    ```

9.	Retrieve root password for first login:

10.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>grep -i 'temporary password' /mysql/log/err&#95;log.log</copy>
    ```

11. Login to the the mysql-enterprise installation and check the status (you will be asked to change password)

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
     ```
    <copy>mysqlsh --uri root@localhost:3306 --sql -p </copy>
    ```

12. Create New Password for MySQL Root

13. **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>ALTER USER 'root'@'localhost' IDENTIFIED BY 'Welcome1!';</copy>
    ```

14. **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>\status</copy>
    ```

15.	Shutdown the service

16. **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>\quit</copy>
    ```


17.	Create a new administrative user called 'admin' with remote access and full privileges

18. **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>** 
    ```
    <copy>mysqlsh --sql --uri root@127.0.0.1:3306 -p</copy>
    ```

19. **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>CREATE USER 'admin'@'%' IDENTIFIED BY 'Welcome1!';</copy>
    ```

20. **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;</copy>
    ```

21.	Add the mysql bin folder to the bash profile

22. **![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh>**
    ```
    <copy>\quit</copy>
    ```

23. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>nano /home/opc/.bash&#95;profile</copy>
    ```

24. At to the PATH variable “/mysql/mysql-latest/bin:” 

    after $PATH You’ll have something like

    PATH=$PATH:/mysql/mysql-latest/bin:$HOME/.local/bin:$HOME/bin

25. Save the changes, log out and log in again from the ssh for the changes to take effect on the user profile. 


## Learn More

* [MySQL Linux Installation](https://dev.mysql.com/doc/refman/8.0/en/binary-installation.html)
* [MySQL Shell Installation](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-install.html)

## Acknowledgements
* **Author** - Dale Dasker, MySQL Solution Engineering
* **Last Updated By/Date** - <Dale Dasker, April 2022
