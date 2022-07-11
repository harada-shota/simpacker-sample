# Simpacker Sample

This project with using 'Ruby on Rails', Simpacker, React(Typescript), eslint, prettier.

## Create Project

```
$ mkdir simpacker-sample
$ bundle init
$ vim Gemfile → railsのコメントアウト外す
$ bundle install --path vendor/bundle
$ bundle exec rails new . --skip-javascript
```

## Serve

```
$ bundle exec rails s
```

## Simpacker Installation

https://github.com/hokaccha/simpacker

Add this line to your application's Gemfile:

```ruby
gem 'simpacker'
```

Run the following to install Simpacker:

```
$ bundle install
$ bundle exec rails simpacker:install
```

Add `javascript_pack_tag` in view.

```
<%= javascript_pack_tag 'application' %>
```

Run the folloing command to build JavaScript.

```
$ ./node_modules/.bin/webpack --watch
```

## React(Typescript) Installation

https://github.com/hokaccha/simpacker/tree/master/example/react-typescript

```
$ npm install --save react react-dom
$ npm install --save-dev typescript ts-loader @types/react @types/react-dom
```

## Edit webpack config

```diff
   entry: {
-    application: path.resolve(__dirname, "app/javascript/application.js")
+    application: path.resolve(__dirname, "app/javascript/application.tsx")
   },
   output: {
     path: path.resolve(__dirname, "public/packs"),
@@ -16,7 +16,18 @@
     filename: isProd ? "[name]-[hash].js" : "[name].js"
   },
   resolve: {
-    extensions: [".js"]
+    extensions: [".js", ".ts", ".jsx", ".tsx"]
+  },
+  module: {
+    rules: [
+      {
+        test: /\.tsx?$/,
+        loader: "ts-loader",
+        options: {
+          transpileOnly: true
+        }
+      }
+    ]
   },
   plugins: [
 };
```

## Add files

### tsconfig.json

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["es2019", "dom", "dom.iterable"],
    "module": "es2015",
    "jsx": "react",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "downlevelIteration": true,
    "sourceMap": true,
    "removeComments": false,
    "noImplicitAny": false,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true
  }
}
```

### app/javascript/application.tsx

```typescript
import React from "react";
import ReactDOM from "react-dom";
import { Hello } from "./greeter";

document.addEventListener("DOMContentLoaded", () => {
  ReactDOM.render(<Hello name="Rails" />, document.getElementById("app"));
});
```

### app/javascript/greeter.tsx

```typescript
import React, { FC } from "react";

interface Props {
  name: string;
}

export const Hello: FC<Props> = ({ name }) => {
  return <div>Hello {name}!</div>;
};
```

## ESLint and Prettier Installation

```
$ npm i -D eslint prettier eslint-config-prettier
$ npm i -D @typescript-eslint/{parser,eslint-plugin}
$ npm i -D eslint-plugin-{react,react-hooks}
```

## Add files

### .eslintrc.js

```javascript
module.exports = {
  root: true,
  env: {
    node: true,
    browser: true,
  },
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaVersion: 2018,
    ecmaFeatures: {
      jsx: true,
    },
  },
  settings: {
    react: {
      version: "detect",
    },
  },
  plugins: ["react-hooks", "react", "@typescript-eslint"],
  extends: [
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "prettier",
  ],
  ignorePatterns: ["/vendor", "/public"],
  rules: {
    "react/prop-types": "off",
  },
};
```

### .prettierrc.js

```javascript
module.exports = {};
```
