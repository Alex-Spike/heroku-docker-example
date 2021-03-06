# heroku-docker-example

This project shows an example on how to configure docker on Heroku.

## Steps to achieve the goal

- Pre-requisites
  - Github account
  - Heroku account
  - Travis account (optional)
  - Git cli
  - Heroku cli
- Create Github repo
- Create Heroku project
- In the folder type `heroku git:remote -a <project>` to link the project to an existing Heroku project (see https://stackoverflow.com/a/5129733/340760)
- Install Heroku cli plugin for containers `heroku plugins:install heroku-container-tools`
- Build and push docker image
  - Username is your Heroku email
  - The password is your Heroku API Key (https://dashboard.heroku.com/account)
  - On Travis configure env variables
    - `DOCKER_USERNAME` (Heroku email)
    - `DOCKER_PASSWORD` (Heroku api key)
  - Add .travis.yml
    - Add a `before_script` that will build the image. Name it `registry.heroku.com/<project name>/web`.
    - On `after_success` add (semicolon does matter)
      - `docker login -u="$DOCKER_USERNAME" -p="$DOCKER_PASSWORD" registry.heroku.com;`
      - `docker push registry.heroku.com/<project name>/web;`
  - Or without travis you can build and push manually
    - `docker login -u="USER" -p="HEROKU_API_KEY" registry.heroku.com;`
    - `docker push registry.heroku.com/<project name>/web;`
  - When you push the application is deployed
- Your application must listen on PORT `process.env.PORT` (`EXPOSE` on Dockerfile is completely ignored)
- You can set `ENV` variables on Dockerfile
