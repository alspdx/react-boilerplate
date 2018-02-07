#React project Setup

##Installing Packages and setting up dev environment

**Initial React and Webpack Install and Setup**
* $ npm init
* $ npm install react@15.5.4 react-dom@15.5.4 --save
* $ npm install webpack@3.4.0 --save-dev *(also must be saved globally)*

_Create webpack.config.js in the top-level of the project and add the following:_
```
const webpack = require('webpack');
const { resolve } = require('path');

module.exports = {

  entry: [
    resolve(__dirname, "src") + "/index.jsx"
  ],

  output: {
    filename: 'app.bundle.js',
    path: resolve(__dirname, 'build'),
  },

  resolve: {
    extensions: ['.js', '.jsx']
  }

};
```

**Babel and Webpack Install and Configuration**
* $ npm install babel-core@6.24.1 babel-loader@7.0.0 babel-preset-es2015@6.24.1 babel-preset-react@6.24.1 --save-dev

_Add the following to the webpack.config.js file:_
```
...
  resolve: {
    extensions: [ '.js', '.jsx' ]
  },

  module: {
    rules: [
      {
        test: /\.jsx?$/,
        loader: "babel-loader",
        exclude: /node_modules/,
        options: {
          presets: [
            "es2015",
            "react"
          ]
        }
      },
    ],
  }
};
```

**Creating Initial Components**
* $ touch index.jsx

_index.jsx, with App component linked:_

```
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";

ReactDOM.render(
  <App/>,
  document.getElementById('react-app-root')
);
```

**Creating Nested Components**
$ touch App.jsx

_App.jsx:_

```
import React from "react";

function App(){
  return (
    <div>
      <span>App works!</span>
    </div>
  );
}

export default App;
```

**Props and Prop Types**
* $ npm install prop-types@15.5.10 --save

_Passing props from ParentComponent.jsx:_

```
...
  return(
    <ChildComponent
      propOne="value one"
      propTwo="value two" />
    );
...
```

_Receiving props and declaring PropTypes in ChildComponent.jsx:_

```
import React from 'react';
import PropTypes from 'prop-types';

function ChildComponent(props) {
  return(
    <div>
      <h3>{props.propOne}</h3>
      <span>{props.propTwo}</span>
    </div>
  );
}

ChildComponent.propTypes = {
  propOne: PropTypes.string,
  propTwo: PropTypes.string
}
...
```

**Webpack Dev Server**
* $ npm install webpack-dev-server@2.5.0 --save-dev *(also must be saved globally)*

_Building_
* $ npm webpack

_Serving_
* $ npm webpack-dev-server

**Hot Module Replacement**
* $ npm install react-hot-loader@3.0.0-beta.7 --save-dev

_update webpack.config.js file:_

```
...
const { resolve } = require('path');
const webpack = require('webpack');

module.exports = {

  entry: [
    'react-hot-loader/patch',
    'webpack-dev-server/client?http://localhost:8080',
    'webpack/hot/only-dev-server',
    resolve(__dirname, "src", "index.jsx")
  ],
...
```

_add publicPath property in webpack.config.js:_

```
...
  output: {
    filename: 'app.bundle.js',
    path: resolve(__dirname, 'build'),
    publicPath: '/'
  },
....
```

_add config for dev tools and dev server webpack.config.js:_

```
...
 resolve: {
    extensions: ['.js', '.jsx']
  },

  devtool: '#source-map',

  devServer: {
    hot: true,
    contentBase: resolve(__dirname, 'build'),
    publicPath: '/'
  },
...
```

_update config rules and add plugins to rules in webpack.config.js_

```
...
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        loader: "babel-loader",
        exclude: /node_modules/,
        options: {
          presets: [
            ["es2015", {"modules": false}],
            "react",
          ],
          plugins: [
            "react-hot-loader/babel"
          ]
        }
      }
    ]
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NamedModulesPlugin(),
  ]
};
...
```

****
* $ npm install html-webpack-plugin@2.29.0 --save-dev

**ESLint Install and Setup**
* ($ npm install eslint -g) *global only!*
* ($ npm install eslint-plugin-react -g) *global only!*
* eslint --init

_This will prompt a series of questions for eslint setup_
* ? Are you using ECMAScript 6 features? **Yes**
* ? Are you using ES6 modules? **Yes**
* ? Where will your code run? **Browser**
* ? Do you use CommonJS? **No**
* ? Do you use JSX? **Yes**
* ? Do you use React? **Yes**
* ? What style of indentation do you use? **Spaces**  
* ? What quotes do you use for strings? **Single**
* ? What line endings do you use? **Unix**
* ? Do you require semicolons? **Yes**
* ? What format do you want your config file to be in? **JSON**

_Changes to make in .eslintrc:_
```
...
  "indent": [
    "error",
    2
  ],
...
```
```
...
  "rules": {
    "react/jsx-uses-vars": 2,
    "react/jsx-uses-react": 2,
    "react/jsx-no-duplicate-props": 2,
    "react/jsx-no-undef": 2,
    "react/no-multi-comp": 2,
    "react/jsx-indent-props": [
        "error",
        2
      ],
    "react/jsx-pascal-case": 2,
    "react/prop-types": 2,
    "react/jsx-indent": [
        "error",
        2
    ],
    "react/jsx-key": 2,
...
```

**Adding ESLint Scripts and Running**

_Adding ESLint in package.json:_
```
...
  "scripts": {
    "lint": "eslint src/** src/**/**; exit 0",
    "lint-fix": "eslint src/** src/**/** --fix; exit 0"
...
```
* $ npm run lint
* $ npm run lint-fix
