# mvp-amplify-vue

Minimal application for event management built using [AWS Amplify](https://aws-amplify.github.io/docs/js/start) and [Vue.js](https://vuejs.org/) providing a GraphQL API based on [AWS AppSync](https://aws.amazon.com/appsync) and [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) deployed with [AWS CloudFormation](https://aws.amazon.com/cloudformation/).

## Deployment instructions

### Install dependencies
```
npm install
npm install -g @aws-amplify/cli
```

### Setup stack
```
amplify configure
amplify init
amplify add auth
amplify add api
amplify push
```

### Publish app
```
amplify add hosting
amplify publish
```

### Delete stack
```
amplify delete
```

## Development instructions

### Compile and hot-reload for development
```
npm run serve
```

### Compile and minify for production
```
npm run build
```

### Run tests
```
npm run test
```

### Lint and fix files
```
npm run lint
```
