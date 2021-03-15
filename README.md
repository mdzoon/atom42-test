# atom42-test

## Prerequisites:

NPM version 7.6.3
Vue version @vue/cli 4.5.11

## Project setup:
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


## Project notes:

I did not finish the full integration with Stripe, due to time constraints.
At the moment, the VueStripe plugin breaks the app due to the Content Security Policy - 3rd party script (Stripe) in the header.
I added commented out the logic of the basic XMLHttpRequest instead.
