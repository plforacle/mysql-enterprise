# SECURITY - MYSQL ENTERPRISE TRANSPARENT DATA ENCRYPTION

## Introduction
3c) MySQL Enterprise Transparent Data Encryption
Objective: Data Encryption in action…

This lab will walk you through encrypting InnoDB Tablespace files at rest

Estimated Lab Time: 20 minutes

### Objectives

In this lab, you will:
* Install and encrypt Data Files

### Prerequisites (Optional)

This lab assumes you have:
* An Oracle account
* All previous labs successfully completed

* Lab standard  
    - ![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell> the command must be executed in the Operating System shell
    - ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql> the command must be executed in a client like MySQL, MySQL Workbench
    - ![#ff9933](https://via.placeholder.com/15/ff9933/000000?text=+) mysqlsh> the command must be executed in MySQL shell
    

**Notes:**
- References
- https://dev.mysql.com/doc/refman/8.0/en/innodb-data-encryption.html


## Task 1: Install and setup TDE  
1.	Install MySQL Enterprise Transparent Data Encrytption on mysql-enterprise using Administrative MySQL client connections 

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>mysql -u root -p -P3306 -h127.0.0.1 </copy>
    ```
2.	Check to see if any keyring plugin is installed and load if not:

    a. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT PLUGIN_NAME, PLUGIN_STATUS FROM INFORMATION_SCHEMA.PLUGINS WHERE PLUGIN_NAME LIKE 'keyring%'; </copy>
    ```

    b. Edit the my.cnf setting in /mysql/etc/my.cnf

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>sudo nano /mysql/etc/my.cnf</copy>
    ```
    b. Add the following lines to load the plugin and set the encrypted key file

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>early-plugin-load=keyring_encrypted_file.so</copy>    
    ```
    ```
    <copy>keyring_encrypted_file_data=/mysql/data/mysql-keyring/keyring-encrypted</copy>    
    ```
    ```
    <copy>keyring_encrypted_file_password=V&rySec4eT</copy>    
    ```

    c. Restart MySQL

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>mysqladmin -uroot -p -h 127.0.0.1 -P3306 shutdown</copy>
    ```
     **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>sudo /mysql/mysql-latest/bin/mysqld --defaults-file=/mysql/etc/my.cnf $MYSQLD_OPTS &</copy>
    ```


3.	"Spy" on employees.employees table

    a. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>strings "/mysql/data/employees/employees.ibd" | head -n50</copy>
    ```


4.	Now we enable Encryption on the employees.employees table:

    a.  **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>mysql -u root -p -P3306 -h127.0.0.1 </copy>
    ```

    b. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>ALTER TABLE employees ENCRYPTION = 'Y';</copy>
    ```


5.	"Spy" on employees.employees table again:

    a. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>strings "/mysql/data/employees/employees.ibd" | head -n50</copy>
    ```



5.	Open a second SSH connection (don't close the administrative one) and use it to connect as “fwtest” and submit some commands

    a. **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>** 
    ```
    <copy>mysql -ufwtest -p -P3307 -h127.0.0.1</copy>
    ```
    b. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>USE world;</copy>
    ```
    c. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT * FROM city limit 25;</copy>
    ```
    d. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT Code, Name, Region FROM country WHERE population > 200000;</copy>
    ```
6.	Administrative connection: Return to admin session (first SSH connection terminal) and verify that there are now rules in allowlist (noticed that we interrogate temporary rules from information schema) 

    **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT * FROM information_schema.mysql_firewall_whitelist;</copy>
    ```
7.	Administrative connection: switch Firewall mode from 'recording' to 'protecting' and verify the presence of rules in allowlist

    a. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>CALL mysql.sp_set_firewall_mode('fwtest@%', 'PROTECTING');</copy>
    ```
    b. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT * FROM mysql.firewall_whitelist;</copy>
    ```
## Task 2: Use fwtest
1.	fwtest connection: run these commands. Which one’s work? Which ones fail and why?

    a. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>USE world;</copy>
    ```
    b. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>**
    ```
    <copy>SELECT * FROM city limit 25;</copy>
    ```
    c. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT Code, Name, Region FROM country WHERE population > 200000;</copy>
    ```
    d. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT * FROM countrylanguage;</copy>
    ```
    e. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT Code, Name, Region FROM country WHERE population > 500000;</copy>
    ```
    f. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT Code, Name, Region FROM country WHERE population > 200000 or 1=1;</copy>
    ```
2.	Administrative connection: set firewall in detecting mode in your 

    a. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>CALL mysql.sp_set_firewall_mode('fwtest@%', 'DETECTING');</copy>
    ```
    b. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>**
    ```
    <copy>SET GLOBAL log_error_verbosity=3;</copy>
    ```
3.	fwtest connection: Repeat a blocked command (it works? Why? ___________________)

    a. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>**
    ```
    <copy>SELECT Code, Name, Region FROM world.country WHERE population > 200000 or 1=1;</copy>
    ```
    b. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>exit</copy>
    ```
4.	Search the error in the error log

    **![#00cc00](https://via.placeholder.com/15/00cc00/000000?text=+) shell>**
    ```
    <copy>grep "MY-011191" /mysql/log/err_log.log</copy>
    ```
5.	Administrative connection: disable firewall for the user Check the Status of Firewall on the command line. Disabling firewall doesn't delete rules.

    a. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>CALL mysql.sp_set_firewall_mode('fwtest@%', 'OFF');</copy>
    ```
    b. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>** 
    ```
    <copy>SELECT MODE FROM INFORMATION_SCHEMA.MYSQL_FIREWALL_USERS WHERE USERHOST = 'fwtest@%';</copy>
    ```
    c. **![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) mysql>**
    ```
    <copy>SELECT RULE FROM INFORMATION_SCHEMA.MYSQL_FIREWALL_WHITELIST WHERE USERHOST = 'fwtest@%';</copy>
    ```
## Learn More

*(optional - include links to docs, white papers, blogs, etc)*

* [URL text 1](http://docs.oracle.com)
* [URL text 2](http://docs.oracle.com)

## Acknowledgements
* **Author** - Perside Foster, MySQL Engineering
* **Contributors** -  Marco Carlessi, MySQL Engineering
* **Last Updated By/Date** - <Perside Foster, October 2021
