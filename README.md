# Hosting a Full-Stack Application

# Udagram

The udagram application is a fairly simple application that includes all the major components of a Full-Stack web application. As a part of Udacity fullStack Nano dgree this is a project Hosting full stack Application on AWS Services.

### Dependencies

```
- Node v14.15.1 (LTS) or more recent. While older versions can work it is advisable to keep node to latest LTS version

- npm 6.14.8 (LTS) or more recent
- AWS CLI v2, v1 can work but was not tested for this project

- A RDS database running Postgres.

- A S3 bucket for hosting uploaded pictures.

- Elastic Beanstalk for hosting our server in node envirenmoent

```

### Installation

Provision the necessary AWS services needed for running the application:

1. In AWS, provision a publicly available RDS database running Postgres. <Place holder for link to classroom article>
1. In AWS, provision a s3 bucket for hosting the uploaded files. <Place holder for tlink to classroom article>
1. Export the ENV variables needed or use a package like [dotnev](https://www.npmjs.com/package/dotenv)/.
1. From the root of the repo, navigate udagram-api folder `cd starter/udagram-api` to install the node_modules `npm install`. After installation is done start the api in dev mode with `npm run dev`.
1. Without closing the terminal in step 1, navigate to the udagram-frontend `cd starter/udagram-frontend` to intall the node_modules `npm install`. After installation is done start the api in dev mode with `npm run start`.

## Testing

This project contains two different test suite: unit tests and End-To-End tests(e2e). Follow these steps to run the tests.

1. `cd starter/udagram-frontend`
1. `npm run test`
1. `npm run e2e`

There are no Unit test on the back-end

### Unit Tests:

Unit tests are using the Jasmine Framework.

### End to End Tests:

The e2e tests are using Protractor and Jasmine.

## AWS Folder in the root

There is a a folder named AWS Screen shots in the project root for All AWS Service and Architecture Diagram.

# Pipeline process

This project linked with Circle Ci account with github master branch and the pipe line process will work as the following.For Further information you can check the documentaion folder int the project root.

```
version: 2.1
orbs:
  # orbs contain basic recipes and reproducible actions (install node, aws, etc.)
  node: circleci/node@5.0.2
  eb: circleci/aws-elastic-beanstalk@2.0.1
  aws-cli: circleci/aws-cli@3.1.1
  # different jobs are calles later in the workflows sections
jobs:
  build:
    docker:
      # the base image can run most needed actions with orbs
      - image: "cimg/node:14.15"
    steps:
      # install node and checkout code
      - node/install:
          node-version: "14.15"
      - checkout
      - aws-cli/setup
      # Use root level package.json to install dependencies in the frontend app
      - run:
          name: Install Front-End Dependencies
          command: |
            npm run frontend:install
      # TODO: Install dependencies in the the backend API
      - run:
          name: Install API Dependencies
          command: |
            npm run api:install
      # TODO: Lint the frontend
      - run:
          name: Front-End Lint
          command: |
            npm run frontend:lint
      # TODO: Build the frontend app
      - run:
          name: Front-End Build
          command: |
            npm run frontend:build
      # TODO: Build the backend API
      - run:
          name: API Build
          command: |
            npm run api:build
  # deploy step will run only after manual approval
  deploy:
    docker:
      - image: "cimg/base:stable"
      # more setup needed for aws, node, elastic beanstalk
    steps:
      - node/install:
          node-version: "14.15"
      - eb/setup
      - aws-cli/setup
      - checkout
      - run:
          name: Deploy App
          # TODO: Install, build, deploy in both apps
          command: |
            npm run deploy
workflows:
  udagram:
    jobs:
      - build
      - hold:
          filters:
            branches:
              only:
                - master
          type: approval
          requires:
            - build
      - deploy:
          requires:
            - build

```

## Backend-API Files Tree

```
src
 ┣ config
 ┃ ┣ config.ts
 ┃ ┗ README.md
 ┣ controllers
 ┃ ┗ v0
 ┃ ┃ ┣ feed
 ┃ ┃ ┃ ┣ models
 ┃ ┃ ┃ ┃ ┗ FeedItem.ts
 ┃ ┃ ┃ ┗ routes
 ┃ ┃ ┃ ┃ ┗ feed.router.ts
 ┃ ┃ ┣ users
 ┃ ┃ ┃ ┣ models
 ┃ ┃ ┃ ┃ ┗ User.ts
 ┃ ┃ ┃ ┗ routes
 ┃ ┃ ┃ ┃ ┣ auth.router.ts
 ┃ ┃ ┃ ┃ ┗ user.router.ts
 ┃ ┃ ┣ index.router.ts
 ┃ ┃ ┗ model.index.ts
 ┣ migrations
 ┃ ┣ 20190325-create-feed-item.js
 ┃ ┗ 20190328-create-user.js
 ┣ aws.ts
 ┣ sequelize.ts
 ┗ server.ts
```

## FrontEnd Tree

```
src
 ┣ app
 ┃ ┣ api
 ┃ ┃ ┣ api.module.ts
 ┃ ┃ ┗ api.service.ts
 ┃ ┣ auth
 ┃ ┃ ┣ auth-login
 ┃ ┃ ┃ ┣ auth-login.component.html
 ┃ ┃ ┃ ┣ auth-login.component.scss
 ┃ ┃ ┃ ┗ auth-login.component.ts
 ┃ ┃ ┣ auth-menu-button
 ┃ ┃ ┃ ┣ auth-menu-user
 ┃ ┃ ┃ ┃ ┣ auth-menu-user.component.html
 ┃ ┃ ┃ ┃ ┣ auth-menu-user.component.scss
 ┃ ┃ ┃ ┃ ┣ auth-menu-user.component.spec.ts
 ┃ ┃ ┃ ┃ ┗ auth-menu-user.component.ts
 ┃ ┃ ┃ ┣ auth-menu-button.component.html
 ┃ ┃ ┃ ┣ auth-menu-button.component.scss
 ┃ ┃ ┃ ┗ auth-menu-button.component.ts
 ┃ ┃ ┣ auth-register
 ┃ ┃ ┃ ┣ auth-register.component.html
 ┃ ┃ ┃ ┣ auth-register.component.scss
 ┃ ┃ ┃ ┗ auth-register.component.ts
 ┃ ┃ ┣ models
 ┃ ┃ ┃ ┗ user.model.ts
 ┃ ┃ ┣ services
 ┃ ┃ ┃ ┣ auth.guard.service.ts
 ┃ ┃ ┃ ┗ auth.service.ts
 ┃ ┃ ┗ auth.module.ts
 ┃ ┣ feed
 ┃ ┃ ┣ feed-item
 ┃ ┃ ┃ ┣ feed-item.component.html
 ┃ ┃ ┃ ┣ feed-item.component.scss
 ┃ ┃ ┃ ┣ feed-item.component.spec.ts
 ┃ ┃ ┃ ┗ feed-item.component.ts
 ┃ ┃ ┣ feed-list
 ┃ ┃ ┃ ┣ feed-list.component.html
 ┃ ┃ ┃ ┣ feed-list.component.scss
 ┃ ┃ ┃ ┗ feed-list.component.ts
 ┃ ┃ ┣ feed-upload
 ┃ ┃ ┃ ┣ feed-upload-button
 ┃ ┃ ┃ ┃ ┣ feed-upload-button.component.html
 ┃ ┃ ┃ ┃ ┣ feed-upload-button.component.scss
 ┃ ┃ ┃ ┃ ┗ feed-upload-button.component.ts
 ┃ ┃ ┃ ┣ feed-upload.component.html
 ┃ ┃ ┃ ┣ feed-upload.component.scss
 ┃ ┃ ┃ ┗ feed-upload.component.ts
 ┃ ┃ ┣ models
 ┃ ┃ ┃ ┗ feed-item.model.ts
 ┃ ┃ ┣ services
 ┃ ┃ ┃ ┗ feed.provider.service.ts
 ┃ ┃ ┗ feed.module.ts
 ┃ ┣ home
 ┃ ┃ ┣ home.module.ts
 ┃ ┃ ┣ home.page.html
 ┃ ┃ ┣ home.page.scss
 ┃ ┃ ┣ home.page.spec.ts
 ┃ ┃ ┗ home.page.ts
 ┃ ┣ menubar
 ┃ ┃ ┣ menubar.component.html
 ┃ ┃ ┣ menubar.component.scss
 ┃ ┃ ┣ menubar.component.spec.ts
 ┃ ┃ ┗ menubar.component.ts
 ┃ ┣ app-routing.module.ts
 ┃ ┣ app.component.html
 ┃ ┣ app.component.spec.ts
 ┃ ┣ app.component.ts
 ┃ ┗ app.module.ts
 ┣ assets
 ┃ ┣ icon
 ┃ ┃ ┗ favicon.png
 ┃ ┗ shapes.svg
 ┣ environments
 ┃ ┣ environment.prod.ts
 ┃ ┗ environment.ts
 ┣ theme
 ┃ ┗ variables.scss
 ┣ global.scss
 ┣ index.html
 ┣ karma.conf.js
 ┣ main.ts
 ┣ polyfills.ts
 ┣ test.ts
 ┣ tsconfig.app.json
 ┗ tsconfig.spec.json

```

## Built With

- [Angular](https://angular.io/) - Single Page Application Framework
- [Node](https://nodejs.org) - Javascript Runtime
- [Express](https://expressjs.com/) - Javascript API Framework

## License

[License](LICENSE.txt)
