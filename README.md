# amplify-vuejs-file-upload-rekognition

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

```
yarn serve
```

### Compiles and minifies for production

```
yarn build
```

### Lints and fixes files

```
yarn lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

https://docs.amplify.aws/cli/graphql-transformer/directives#predictions

```
type Query {
  identifyLabels: String @predictions(actions: [
    identifyLabels
  ])
}
```
