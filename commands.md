# Command Line Commands

## Initialization

```Shell
npm install
```

or

```Shell
yarn install
```

Initializes a project.


## Development

```Shell
npm run start
```

Starts the development server running on `http://localhost:3000` or other ports

## Cleaning

```Shell
npm run clean
```

Deletes the example app, replacing it with the smallest amount of boilerplate
code necessary to start writing your app!

> Note: This command is self-destructive, once you've run it you cannot run it
> again. This is for your own safety, so you can't delete portions of your project
> irreversibly by accident.

## Server

### Development

```Shell
npm start
```

Starts the development server and makes your application accessible at
`localhost:3000`. Changes in the application code will be hot-reloaded.

### Production

```Shell
npm run start:production
```

or

```Shell
npm run production
```

- Runs tests (see `npm test`)
- Builds your app (see `npm run build`)
- Starts the production server (see `npm run start:prod` or other commands)

The app is built for optimal performance: assets are
minified and served gzipped.

### Host and Port

To change the host and/or port the app is accessible at, pass the `--host` and/or `--port` option to the command
with `--`. E.g. to make the app visible at `my-local-hostname:5000`, run the following:
`npm start -- --host my-local-hostname --port 5000`

## Building

```Shell
npm run build
```

Preps your app for deployment (does not run tests). Optimizes and minifies all files, piping them to the `build` folder.

Upload the contents of `build` to your web server to
see your work live!

## Testing

See the [testing documentation](../testing.md) for detailed information
about our testing setup!

## Unit testing

```Shell
npm run test
```

Tests your application with the unit tests specified in the `**/tests/*.js` or `**-spec.js` or  files
throughout the application.  
All the `test` commands allow an optional `-- [string]` argument to filter
the tests run by Jest. Useful if you need to run a specific test only.

```Shell
# Run only the Button component tests
npm test -- Button
```

### Watching

```Shell
npm run test:watch
```

Watches changes to your application and re-runs tests whenever a file changes.

### Remote testing

```Shell
npm run start:tunnel
```

Starts the development server and tunnels it with `ngrok`, making the website
available worldwide. Useful for testing on different devices in different locations!

### Dependency size test

```Shell
npm run analyze
```

This command will generate a `stats.json` file from your production build, which
you can upload to the [webpack analyzer](https://webpack.github.io/analyse/) or [Webpack Visualizer](https://chrisbateman.github.io/webpack-visualizer/). This
analyzer will visualize your dependencies and chunks with detailed statistics
about the bundle size.

## Linting

```Shell
npm run lint
```

Lints your JavaScript and your CSS.

```Shell
npm run lint:eslint:fix -- .
```

or

```Shell
npm run eslint .
```

Lints your code and tries to fix any errors it finds.
