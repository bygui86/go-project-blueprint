
# Blueprint for new Golang projects

This project is taken/inspired by the series "All You Need For Your Next Golang Project" by Martin Heinz (see links below).

## Tech stack

- Golang
    - Gin
    - GORM
    - Viper
- GNU Makefile
- PostgreSQL
- Docker
- GitHub Package Registry
- CodeClimate
- Travis CI
- SonarCloud

---

### Add new dependencies
```shell script
# make ?
cd cmd/blueprint
go mod vendor
```

## Run
```shell script
# make ?
cd cmd/blueprint
go run main.go
```

## Test
```shell script
# make test
cd cmd/blueprint
go test
```

## Cleanup
```shell script
# make cleanup
```

## Docker

First, to be able to authenticate ourselves with GitHub Package Registry, we need to create personal access token. 
This access token must have read:packages and write:packages scope and additionally in case you have a private repository, you will have to include repo scope as well.
See [this very nice guide](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) on GitHub.

### Build
```shell script
# make container
docker build -t  docker.pkg.github.com/bygui86/go-project-blueprint/blueprint:latest .
```
### Push
```shell script
# make push
docker login docker.pkg.github.com -u <USERNAME> -p <GITHUB_TOKEN>
docker push docker.pkg.github.com/bygui86/go-project-blueprint/blueprint:latest
```
### Pull
```shell script
docker pull docker.pkg.github.com/bygui86/go-project-blueprint/blueprint:latest
```
### Run
```shell script
docker run -d --name blueprint \ 
    -e BLUEPRINT_DSN="postgres://<USER>:<PASSWORD>@<DOCKER_SERVICE/URL>:<PORT>/<DB>?sslmode=disable" \
    -e BLUEPRINT_API_KEY="api_key" \
    -p 8080:8080 \
    docker.pkg.github.com/bygui86/go-project-blueprint/blueprint:latest
```

---

## Swagger

Application uses [gin-swagger](https://github.com/swaggo/gin-swagger).

### Generate/Update docs
```shell script
cd cmd/blueprint
swag init
cd docs
```

### View docs
```shell script
open http://localhost:8080/swagger/index.html
# or raw json
open http://localhost:8080/swagger/doc.json
```

---

## CodeClimate
- Go to <https://codeclimate.com/github/repos/new>
- Add Repository
- Go to `Test Coverage` Tab
- Copy `Test Reporter ID`
- Go to Travis and Open Settings for Your Repository
- Add Environment Variable: 

        name: `CC_TEST_REPORTER_ID`, 
        value: _Test Reporter ID from CodeClimate_

---

## SonarCloud

- On SonarCloud:
    - Click on `Plus` sign in upper right corner
    - Click on `Analyze new project`
    - Click on `GitHub app configuration` link
    - Configure SonarCloud
    - Select repository and save
    - Go back to analyze project
    - Tick `Newly added repository`
    - Click on `Set up`
    - Click on `Configure with Travis`
    - Copy the command to encrypt the Travis token
    - Run `travis encrypt --com <token_you_coppied>`
    - Populate the `secure` field in `.travis.yml` with outputted string
    - Follow steps to populate your `sonar-project.properties`
    - push

- On Travis CI:
    - Set `DOCKER_USERNAME`
    - Set `DOCKER_PASSWORD` to your GitHub registry token

---

## Links

- [Martin's Blog](https://martinheinz.dev/)
- Part 1 - Ultimate Setup for Your Next Golang Project
    - [Article](https://towardsdatascience.com/ultimate-setup-for-your-next-golang-project-1cc989ad2a96)
    - [Repo](https://github.com/MartinHeinz/go-project-blueprint)
- Part 2 - Setting up GitHub Package Registry with Docker and Golang
    - [Article](https://towardsdatascience.com/setting-up-github-package-registry-with-docker-and-golang-7a75a2533139)
    - [GitHub Webhook with RegistryPackageEvent](https://developer.github.com/v3/activity/events/types/#registrypackageevent)
    - [Doc](https://github.com/features/package-registry)
    - [Help](https://help.github.com/en/articles/about-github-package-registry)
- Part 3 - Building RESTful APIs in Golang
    - [Article](https://towardsdatascience.com/building-restful-apis-in-golang-e3fe6e3f8f95)
    - [Repo](https://github.com/MartinHeinz/go-project-blueprint/tree/rest-api)
- Part 4 - Setting Up Swagger Docs for Golang API
    - [Article](https://towardsdatascience.com/setting-up-swagger-docs-for-golang-api-8d0442263641)
    - [Repo](https://github.com/MartinHeinz/go-project-blueprint/tree/rest-api)
- Part 5 - Setting up GitHub Actions
    - [Repo](https://github.com/MartinHeinz/github-actions)
