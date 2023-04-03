# Week 4 — Postgres and RDS

## **Required Homework** 

### **Create RDS Postgres Instance**
To create an RDS instance running a postgres engine while also trying to maintain security best practices, the following approach was employed:
1. The master-username, master-user-password, and port was generated set:
    ```bash
    export POSTGRES_MASTER_USERNAME="<my-master-username>"
    gp env POSTGRES_MASTER_USERNAME=$POSTGRES_MASTER_USERNAME
    export POSTGRES_MASTER_PASSWORD="<my-master-password>"
    gp env POSTGRES_MASTER_PASSWORD=$POSTGRES_MASTER_PASSWORD
    export POSTGRES_PORT="<my-port>"
    gp env POSTGRES_PORT=$POSTGRES_PORT
    ```
2. To create a new instance in aws rds with our default aws region, the following command was run in the same terminal as above:
    ```bash
    $ aws rds create-db-instance \
      --db-instance-identifier cruddur-db-instance \
      --db-instance-class db.t3.micro \
      --engine postgres \
      --engine-version  14.6 \
      --master-username ${POSTGRES_MASTER_USERNAME} \
      --master-user-password ${POSTGRES_MASTER_PASSWORD} \
      --allocated-storage 20 \
      --availability-zone "${AWS_DEFAULT_REGION}a" \
      --backup-retention-period 0 \
      --port ${POSTGRES_PORT} \
      --no-multi-az \
      --db-name cruddur \
      --storage-type gp2 \
      --publicly-accessible \
      --storage-encrypted \
      --enable-performance-insights \
      --performance-insights-retention-period 7 \
      --no-deletion-protection
    ```
3. To ensure having the right url to the just created RDS instance, a connection link in the form of ```postgresql://[username[:password]@][netloc][:port][/dbname][?param1=value1&...]```is exported as an environment key by grabbing the endpoint name from aws rds instance and do the following:
    ```bash
    export PROD_DB_ENDPOINT="<my-endpoint>"
    gp env PROD_DB_ENDPOINT=$PROD_DB_ENDPOINT 
    export PROD_CONNECTION_URL = "postgresql://${POSTGRES_MASTER_USERNAME}:${POSTGRES_MASTER_PASSWORD}@${PROD_DB_ENDPOINT}:${POSTGRES_PORT}/cruddur"
    gp env PROD_CONNECTION_URL=$PROD_CONNECTION_URL
    ```
4. Accessing the local postgres container running, we need a similar link set in the environment:
    ```bash
    export CONNECTION_URL="postgresql://postgres:password@localhost:5432/cruddur"
    gp env CONNECTION_URL=$CONNECTION_URL
    ```
5. The link above does work with wheen the backend container tries to communicate with the postgres container, to fix that, a variation of 4. above is used and set:
    ```bash
    export CONNECTION_LOCAL_DOCKER_URL="postgresql://postgres:password@db:5432/cruddur"
    gp env CONNECTION_LOCAL_DOCKER_URL=$CONNECTION_LOCAL_DOCKER_URL
    ```
    the *netloc* from the postgres link format described in 3. above is replaces with name of the container used in the [docker-compose.yml](../docker-compose.yml) file. In my case that container is named *db*

6. Connecting to the aws RDS via the terminal can done by:
    ```bash
    psql $PROD_CONNECTION_URL

    ```
    
    ### **Bash scripting for Database Operation and SQL files**
    
    1. Each script must start with  ```#! usr/bun/bash```

2. To ensure each script is executable, one can do batch permission modification by running the following command in the terminal while in the backend-flask directory:
    ```bash
    chmod 744 bin/db-*
    ```
3. For scripts that need to know their current parent directory irrespective of where they are called in the terminal, the following was used:
    ```bash
    current_file_path=`realpath $0`
    file_parent_dir=`dirname $(dirname $current_file_path)`
    ```
  This ensures in our case that ```file_parent_dir=/workspace/aws-bootcamp-cruddur-2023/backend-flask```at all times.

4. To allow for conditional execution, the following was added:
    ```bash
    if [ "$1" = "prod" ]; then
    echo "using production url"
    CON_URL=$PROD_CONNECTION_URL
    else
        CON_URL=$CONNECTION_URL
    fi
    ```
5. Since the workspace is ephemeral, we need to set it at startup of every new workspace, the following was added to the postgres task in [.gitpod.yml](../.gitpod.yml) file:
    ```yaml
    command: |
      export GITPOD_IP=$(curl ifconfig.me)
      source "$THEIA_WORKSPACE_ROOT/backend-flask/bin/rds-update-sg-rule"
    ```
  
  ### Errors
  
1. Docker run out of space: use  ```docker system prune```

2. Couldn't crud new messages - Error in creating new activities - ```need resolving```

3. CloudWatch logs - ```need resolving```
    
