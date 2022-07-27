# AWS Configuration

## Create react app and connect to aws amplify
-since client folder was created, some scripts have to be configured in the aws build settings:

```
version: 1
frontend:
  phases:
    preBuild:
      commands: [npm run devInstall]
    # IMPORTANT - Please verify your build commands
    build:
      commands: [npm run build]
  artifacts:
    # IMPORTANT - Please verify your build output directory
    baseDirectory: client/build
    files:
      - '**/*'
  cache:
    paths: [node_modules]
```

## Configure aws amplify cli:
```
npm install -g @aws-amplify/cli
amplify configure
```
you will create an IAM user, save both keys

## AWS Amplify Console Backend
-in the console, go to Backend Environments and clik on get started
-click open admin UI/launch studio
-go back to backend environment, click on local setup instructions, and copy the command and paste in your terminal

## Add Authentication
```
npm install aws-amplify @aws-amplify/ui-react
amplify add auth

? Do you want to use the default authentication and security configuration? Default configuration
? How do you want users to be able to sign in? Username
? Do you want to configure advanced settings? No, I am done.

amplify push --y
```

-add the following to src/index.js
```
import Amplify from 'aws-amplify';
import config from './aws-exports';
Amplify.configure(config);
```

-Add the fololowing to the build settings in aws
```
backend:
  phases:
    build:
      commands:
        - '# Execute Amplify CLI with the helper script'
        - amplifyPush --simple
```

## Backend/Amplify Configurations
-amplify had trouble deploying the backend since the amplify folder was outside of the react project directory, easiest setup is to configure aws directly inside of the client folder and update build settings: 
```
version: 1
env:
    variables: 
        VERSION_AMPLIFY: 9.1.0
backend:
  phases:
    preBuild:
      commands:
        - npm i -g @aws-amplify/cli@${VERSION_AMPLIFY}
    build:
      commands:
        - '# Execute Amplify CLI with the helper script'
        - amplifyPush --simple
frontend:
  phases:
    preBuild:
      commands:
        - npm ci
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: build
    files:
      - '**/*'
  cache:
    paths:
      - node_modules/**/*
```

## Adding GraphQL API and Database
```
amplify add api
```
push changes to amplify and redeploy app

## Add Storage
```
amplify add storage
amplify push --y
```

## Removing Resources/Services
```
amplify remove auth
```

- to delete the entire project:
```
amplify delete
```
