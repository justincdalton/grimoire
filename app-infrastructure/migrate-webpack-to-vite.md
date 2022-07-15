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
  "start": "vite"
}
```