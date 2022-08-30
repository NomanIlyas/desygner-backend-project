# Workhub devenv (Admin, connect, scheduling)

The purpose of this repo is to have a single place dev environment to build and deploy applications locally. It contains a docker-compose file which combines all relevant apps as services.

## Prerequisites

Make sure you have the following utilities installed locally on your machine.

- Docker

## Setup

Please follow the steps below to setup admin, connect and scheduling applications locally.

1. Clone workhub-devenv repo.
2. Goto workhub-devenv directory `cd workhub-devenv`.
3. Clone the repositories inside the workhub-devenv directory.
   1. [workhub-api-admin](https://github.com/coeus-solutions/workhub-api-admin)
   2. [workhub-api-connect](https://github.com/coeus-solutions/workhub-api-connect)
   3. [workhub-api-scheduling](https://github.com/coeus-solutions/workhub-api-scheduling)
   4. [workhub-admin](https://github.com/coeus-solutions/workhub-admin)
   5. [workhub-connect](https://github.com/coeus-solutions/workhub-connect)
   6. [workhub-scheduling](https://github.com/coeus-solutions/workhub-scheduling)
   7. [workhub-socketcluster](https://github.com/coeus-solutions/workhub-socketcluster)
   8. [workhub-hub](https://github.com/coeus-solutions/workhub-hub)
4. For all frontend apps (admin, connect, scheduling) copy the .env file and rename to .env.local.
5. For all backend apps (admin, connect, scheduling) copy the .env file, rename to .env.local and adjust the environment variables if needed otherwise use the default values.
6. Create localhost aliases (in /etc/hosts file) as per API endpoints given in the .env.local files of frontend apps. Copy the below line and paste it to your /etc/hosts file.
   1. 127.0.0.1	api-connect.workhub.test api-admin.workhub.test api-scheduling.workhub.test ds.workhub.test
7. Copy docker-compose.yml.dist file from the root of workhub-devenv directory and rename it to docker-compose.yml.
8. Open up docker-compose.yml file apply the following changes if not done already.
   1. Change the ports section to 3308:3306 under `wh_db` services in case of port conflict, you can choose any other port.
   2. Change the ports section to 8000:80 under `hub` service in case of port conflict, you can choose any other port but, make sure to change it in .env.local files too against `REACT_APP_DS_SERVER_URL` in all frontend apps.
   3. Similarly, as mentioned in above points if you change any port in docker-compose.yml then accordingly adjust it in .env.local files of all frontend apps too.
9. Now from the root of workhub-devenv directory run this command `docker-compose build`, it will build all services and might take some time depending on the internet speed.
10. Once the build is successful, you can either run `docker-compose up` for running all front/backend applications at once or run individual services using following commands as per your need
    1. `docker-compose up hub socketcluster` this is mandatory to run admin, connect and scheduling
    2. `docker-compose up api_admin` for running admin backend, it will also run `api_connect` and `api_scheduling` services
    3. `docker-compose up api_connect` for running connect backend, should already be running with `api_admin`
    4. `docker-compose up api_scheduling` for running scheduling backend, should already be running with `api_admin`
    5. `docker-compose up web_admin` for running admin frontend
    6. `docker-compose up web_connect` for running connect frontend
    7. `docker-compose up web_scheduling` for running scheduling frontend
12. Once the relevant front/backend services are up & running, you can accesss the apps in the browser using following URLs
    1. http://localhost:3000 for workhub admin
    2. http://localhost:3001/connect for workhub connect
    3. http://localhost:3002/scheduling for workhub scheduling

## Login Credentials (test users)

- For admin user, use the following credentials
  - username: admin@workhub.ai
  - password: test
- For regular user, use the following credentails
  - username: test1@workhub.ai
  - password: test

## Notes

- You can use the following command to ssh into any relevant container for package install/update/remove or exploring files.
  - `docker-compose exec [container-name] sh`
- Whenever there is a change in .env.local file either front or backend you would need to restart the relevant service container.

## Known Issues

- Currently, the first time you run command `docker-compose up` you might face the following issues, although on the second run it works fine.
  - it gives database connection refused error for `api_connect`and `api_scheduling`
  - it doesn't create test users in databases so, login might not work# desygner-backend-project
