# Portfolio project: Explore California DevOps

#### A project implemented using key DevOps principles such as:
- Containerization with Docker
- Automated testing with RSpec, Capybara, & Selenium
- Infrastructure as Code with Terraform
- AWS
- CI/CD as code with Jenkins

---

## Problem statement:

Explore California is a (fictitious) website already catering to a few hunder thousand customers and contributes significantly to the 5B USD.
They face some issues and they want to fix them with DevOps. Analyse the problems and fix them with DevOps principles.

## Analysis

??? danger "Current state"
    - The initial state of the app:
        - Production deploys are fragile and manual
        - Development, UAT, production envs are very different
        - Testing is limited
        - getting them working on dev laptops is a chore

??? success "Final state"
    - The final expected state of the app:
        - Automated CI
        - Automated production deploys
        - Consistent environments
        - Devs to be able to use their laptops for development

## High level Design

- Full automation:
    - The entire pipeline from code change to deployment should be automated with CI and CD
    - The CD pipeline must deploy the website artifacts into AWS using terraform
    - Unit tests must be integrated with CI and run before each PR goes into review
    - Only code review and UAT must have a manual sign-off

- Containerization using docker for uniform environments

## Implementation

### Testing the app locally with Docker and Docker Compose

- Setup docker for the legacy website which is based on nginx
- The docker file will contain the following:
    
    ``` dockerfile
    FROM nginx:alpine
    COPY website /website
    COPY nginx.conf etc/nginx/nginx.conf
    EXPOSE 80
    ```
- Build and run the docker file:
 ```
   docker build --tag <name> <dir>
   docker run --publish <host port>:<docker port> <name>
 ```
- Docker compose can be used to bring all the services up in unison with networking.
- Docker compose file is as follows:
    ``` yml
    version: '3.7'
    services:
    website:
    build:
        context: .
    ports:
        - 80:80

    ```

- Bring the services up with the `docker-compose up` command
- Check all the services currently running using the `docker-compose ps` command
- Bring the services down with the `docker-compose down` command

## Demo
