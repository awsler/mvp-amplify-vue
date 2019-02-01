# mvp-amplify-vue

Minimal application for event management built using [AWS Amplify](https://aws-amplify.github.io/docs/js/start) and [Vue.js](https://vuejs.org/) providing a GraphQL API based on [AWS AppSync](https://aws.amazon.com/appsync) and [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) deployed with [AWS CloudFormation](https://aws.amazon.com/cloudformation/).

## Deployment instructions

### Install dependencies
```
$ npm install
$ npm install -g @aws-amplify/cli
```

### Setup stack
```
$ amplify configure
> region: <region of choice>
> user name: <new or existing amplify user>
> accessKeyId: <accessKeyId>
> secretAccessKey: <secretAccessKey>
> Profile Name: <new or existing profile name>
```

```
$ amplify init
> Choose your default editor: Visual Studio Code
> Choose the type of app that you're building: javascript
> What javascript framework are you using: vue
> Source Directory Path: src
> Distribution Directory Path: dist
> Build Command: npm run-script build
> Start Command: npm run-script serve
> Do you want to use an AWS profile? Yes
> Please choose the profile you want to use: <profile name>
```

```
$ amplify add auth
> Do you want to use the default authentication and security configuration? Yes, use the default configuration.
```

```
$ amplify add api
> Please select from one of the below mentioned services: GraphQL
> Provide API name: <API name>
> Choose an authorization type for the API: API key
> Do you have an annotated GraphQL schema? Yes
> Provide your schema file path: /<path>/graphql/schema.graphql
```

```
$ amplify push
> Do you want to generate code for your newly created GraphQL API: Yes
> Choose the code generation language target: javascript
> Enter the file name pattern of graphql queries, mutations and subscriptions: src\graphql\**\*.js
> Do you want to generate/update all possible GraphQL operations - queries, mutations and subscriptions: Yes
```

### Publish app
```
$ amplify add hosting
> Select the environment setup: DEV (S3 only with HTTP)
> hosting bucket name: <bucket name>
> index doc for the website: index.html
> error doc for the website: index.html
```

```
$ amplify publish
```

### Delete stack
```
$ amplify delete
```

## Development instructions

### Compile and hot-reload for development
```
$ npm run serve
```

### Compile and minify for production
```
$ npm run build
```

### Run tests
```
$ npm run test
```

### Lint and fix files
```
$ npm run lint
```
