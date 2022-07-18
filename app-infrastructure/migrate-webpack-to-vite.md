## Scaffold a sample project
In order to generate a basic Vite config and starting files it's helpful to set up a sample project to copy from. Follow the Vite guide to [scaffold](https://vitejs.dev/guide/#scaffolding-your-first-vite-project) out a project with the framework your existing project is using.

## Install Vite packages
As an example, for a React project using yarn for package management:
```sh
$ yarn add -D vite @vitejs/plugin-react
```

## Copy Vite files
Copy the following files from the vite sample project you scaffolded into your existing project:

- vite.config.(js|ts)
- index.html
- src/main.tsx

If you are using typescript for the vite config file also copy `tsconfig.node.json` and add the following to your existing `tsconfig.json`:

```json
"references": [{ "path": "./tsconfig.node.json" }]
```

Also depending on your lint configuration you may need to add the `vite.config.ts` file to your `.eslintignore` list or add the config file to your `tsconfig.json` file in the `includes` array.

Also depending on your `tsconfig` target you may need additional options in the `tsconfig.node.json` file:

```json
{
  "compilerOptions": {
    "lib": [
      "ES2021"
    ],
    "target": "ES2021",
    "composite": true,
    "esModuleInterop": true,
    "module": "CommonJS",
    "moduleResolution": "node",
    "skipLibCheck": true
  },
  "include": [
    "vite.config.ts"
  ]
}
```

*Note: Vite is using esbuild as of v3 which does not support targeting ES5. With IE11 going EOL on June 15, 2022 there is no real reason to target ES5 any longer.*

## Vite config
There are several options you will likely need to add to the `vite.config` from the existing webpack config. Your should look like the one below if you are migrating a React app.

```js
export default defineConfig({
  plugins: [react()]
})
```

The following options should be added inside of the `defineConfig` object parameter.

### Add the port to run on
```js
server: {
  port: 8080
}
```

### Add import aliases
```js
resolve: {
  alias: {
    src: path.resolve(__dirname, 'src')
  }
}
```

## Add Vite scripts to package.json
```json
"scripts": {
  "start": "vite",
  "build": "vite build",
  "preview": "vite preview --port 8080"
}
```

## Common (and uncommon) migration concerns
### Using SVGs as components
If your project is importing SVGs as React or Vue components you will need a plugin for this. For React the best option seems to be `vite-plugin-svgr` and for Vue `vite-plugin-svg`.

### Environment variables
By default Vite provides it's own env object on `import.meta.env`, but many projects coming from webpack use `process.env` even in client side code. Fortunately there is a plugin for mapping env variables to `process.env` and making them available. Ideally you should either map specific envvars or specify a prefix. For example using `vite-plugin-environment` when migrating from CreateReactApp:

```js
environmentPlugin('all', 'REACT_APP_')
```

### Split up javascript bundle
Vite does not do any code splitting by default which can lead to a single large javascript bundle. It does include a 

### Update CSS modules file names
Vite recognizes CSS module files by including `.module` in the filename. For example, if you have a file named `colors.scss` which uses CSS module exports it will need to be renamed to `colors.module.scss`.

### Relative asset imports
You may see issues where assets such as font faces imported with relative paths no longer load and instead throw 404 errors. If you see this problem you can alias the assets directory and change the import to use that instead.

### Output directory
By default Vite uses `dist` as the build output directory, but your existing project may be using a different one such as `build`. To change the directory add the following to the Vite config:

```json
build: {
  outDir: 'build'
}
```
