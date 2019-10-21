# Dice

To get the project up and running please make use of the versions below:
```
npm 6.7.0
node 11.10.0
yarn 1.17.3
```

For switching between versions I recommend using a tool like [Node Version Manangement](https://github.com/tj/n)


## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Lints and fixes files
```
yarn run lint
```

## Configuration
In `App.vue` you can change ` <Dice magic-number="1" />` to any number between 1 till 6. The animation will always be random and can be adjusted in `Dice.vue`. The face pointing upwards will be your configured number. 
