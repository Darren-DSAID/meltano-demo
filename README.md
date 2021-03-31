# Meltano Demo (GitFlix) 01/04/2021
This repo contains the meltano demo project for DE Sharing Session.

# Requirements 
1. Docker 
2. Postgres 

---
### Pre-requisite setup
## Getting the project files 
1. git clone https://github.com/dsaidgovsg/meltano-demo.git to /home/<username>/meltano-demo-project/.

## Setting up Docker
1. Install docker `https://www.docker.com/`
2. Pull the latest meltano image using `docker pull meltano/meltano:latest`

## Setting up Postgres
1. Download and install postgres for your OS `https://www.postgresql.org/download/linux/ubuntu/`
2. Create a demo db with name meltano-demo-db `CREATE DATABASE meltano-demo-db`
3. Create a schema meltano-demo inside meltano-demo-db `CREATE SCHEMA meltano-demo`
---

---
### Meltano Setup
## Getting tap-csv extractor
1. cd to your project directory / where you cloned this project
2. download and install the tap-csv extractor using `docker run -v $(pwd):/projects -w /projects meltano/meltano add extractor tap-csv`

## Getting target-postgres extractor
1. cd to your project directory / where you cloned this project
2. download and install the tap-csv extractor using `docker run -v $(pwd):/projects -w /projects meltano/meltano add loader target-postgres --variant meltano`
3. Input the password to your postgres user in the .env file `TARGET_POSTGRES_PASSWORD="<password>"`

## Getting DBT Transformer
1. cd to your project directory / where you cloned this project
2. download and install the tap-csv extractor using `docker run -v $(pwd):/projects -w /projects meltano/meltano add transformer dbt`
3. Input the password to your postgres user in the .env file `TARGET_POSTGRES_PASSWORD="<password>"`

## Getting airflow 
1. cd to your project directory / where you cloned this project
2. download and install the tap-csv extractor using `docker run -v $(pwd):/projects -w /projects meltano/meltano add orchestrator airflow`
---

---
### Meltano Configuration
**NOTE: The components should already be configured. But its best to check.**
**You can run the meltano UI to configure these settings on the Web UI. `docker run -p 5000:5000 -v $(pwd):/projects -w /projects meltano/meltano ui`**

## Configuring tap-csv
1. cd to your project directory / where you cloned this project
2. Open meltano.yml in your favourite editor
3. Ensure that the config `csv_files_definition: extract/files_def.json` is present.

## Configuring target-postgres
1. cd to your project directory / where you cloned this project
2. Open meltano.yml in your favourite editor
3. Ensure that the config is present.
    ```
    dbname: meltano_demo_db
    host: 192.168.189.146
    port: 5432
    schema: meltano_demo
    user: postgres
    ```
---

---
### Running the job
## Run the job either using cli or through the web ui
1. Run the web ui using `docker run -p 5000:5000 -v $(pwd):/projects -w /projects meltano/meltano ui`
2. Run the job thorugh cli using `docker run -v $(pwd):/projects -w /projects meltano/meltano elt tap-csv target-postgres --job_id=csv-to-postgres-test --transform run`
---

---
### Analysis
## Run the web ui to get to the analyze page
1. Run the web ui using `docker run -p 5000:5000 -v $(pwd):/projects -w /projects meltano/meltano ui`
---

---
### Troubleshooting steps
1. Ensure that you have downloaded the 'Meltano' variant of target-postgres loader otherwise the anaylze step would not work
2. Be sure to transfer the transformed tables from the 'analytics' schema to the 'public' schema as theres some weird bug that meltano would read the tables from the public schema instead of the analytics schema.
`ALTER TABLE analytics.gitflix_xxx
    SET SCHEMA public;`
---
