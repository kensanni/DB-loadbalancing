# DB-loadbalancing

## Prerequisites
To follow along with this automation, you need to have the following

1. Ansible installed on your local machine
2. Three Ubuntu(One for the master database and the other two for the slaves database) instances with SSH key-based authentication
3. Your SSH keypair from `step 2`


## Installation and set up

### Setting up the database replication
To setup the master and slave database replication with loadbalancing, follow the steps below

1. Open and edit the `inventory` file in the root directory to pass in the values of your server IP address. An example is already set in the file.

2. We need to add our variables which are required by the master database for the setup to be successful.

   Open the `main.yml` in the `masterDB/defaults` directory and pass in your values

    ```yml
      ---
      mysql_user: root
      mysql_password: YOUR_MYSQL_ROOT_PASSWORD
      database_name: YOUR_MASTER_DATABASE_NAME
      database_password: YOUR_MASTER_DATABASE_PASSWORD
    ```

3. Open and edit the `main.yml` file in the `slaveDB/defaults` directory to set the slave database credentials
    ```yml
      ---
      mysql_user: root
      mysql_password: YOUR_SQL_ROOT_PASSWORD
      database_name: SLAVE_DATABASE
      database_password: YOUR_DATABASE_PASSWORD
      master_host: YOUR_MASTER_DATABASE_IP
    ```
4. Open and the edit the `main.yml` file in the `HAProxy/defaults`  directory to set HAProxy loadbalancer credentials
   ```yml
      ---
      master_db_ip: master_db_ip
      slave_db_ip_one: slave_db_ip_one
      slave_db_ip_two: slave_db_ip_two
   ```

5. Now we are done setting the credentials needed by the deployment script, we can now run the ansible command to apply the playbook on our infrastructure.

   Ensure you're in the root directory of this project and run the command below on your terminal
   ```command
      $ ansible-playbook -i inventory database.yml
   ```
   This command above will do the following

    - Create the master database.
    - Create the slaves database.
    - Setup the replication between the master and slave database.
    - Install HAProxy.
    - Configure HAProxy to loadbalance the master and slave database.
