# barebones-react
The purpose of this project is to demonstrate setting up a very slim react environment.

## Motivation
I wanted to learn what went into building a react environment. I started using create-react-app and got curious on how to build a lean version of create-react-app.

## Prerequisites

You will need to install the following using yarn or npm:

webpack webpack-cli webpack-dev-server html-webpack-plugin path

babel-loader babel-core babel-preset-env babel-preset-react

react react-dom

## Installing

Create the directory for your app. *Note: you must have npm/yarn and node.js installed prior to creating any react project.*
 
Then run the following at the command line: 
```
npm init
```
This will run the steps in creating the package.json file, which is used as a manifest and generates the metadata about your app. Answer questions. For this demo, the defaults are fine to use. 

### Webpack

Then run the following at the command line: 
```
npm install webpack webpack-cli webpack-dev-server html-webpack-plugin path --save
```
This will install: 

* webpack --- The bundler. It will build a dependency graph for all the modules in your app, so that, the output will run in a browser

* webpack-cli ---webpack's cli. This is needed to run webpack commands. 

* webpack-dev-server --- This aides in developing an application. It runs a server, so you can develop your app and see the results 

* path --- this is a copy of node.js path module. The path module provided utilities to work with files and directory structures

* html-webpack-plugin --- this plugin helps in creating HTML files to server to webpack bundles

* --save --- npm setting, this will save the package name and version in the dependencies object in package.json

create a file called webpack.config.js and configure file with the following:
```
const path = require('path'); 

module.exports = { 
  entry: './src/index.js', 
  output: { 
    path: path.resolve('dist'), 
    filename: 'index_bundle.js' 
  }, 
  module: { 
    rules: [ 
      { test: /\.css$/, 
        use: [ 
          { loader: "style-loader" }, 
          { loader: "css-loader" } 
        ] 
      }, 
      { 
        test: /\.js$/, 
        exclude: /node_modules/, 
        use: "babel-loader" 
      }, { 
        test: /\.jsx?$/, 
        exclude: /node_modules/, 
        use: "babel-loader" 
      } 
    ] 
  } 
}
``` 
A brief explanation of this webpack.config.js settings... 
* The path variable instructs webpack where is the base path 
* The module.export object: 
  * The entry key determines where the entry point is for your application. It is important that it is set else your app will not work. By most conventions it is 'index.js', in our case we will create index.js in a directory called 'src' 
  * The output key determines where webpack will output the bundled javascript. 
    * The path key determines where the output will be. In this case, it will be a directory called 'dist' under the base path. 
    * The filename key determines the name of the output file. In this case, 'bundle.js' is the name. 
  * The module object: 
    * The rules array – conditions, results and nested rules 
      * test: /\.css$/ -- looks for .css files 
        * The use key is a UseEntry object that used in combination of loader property will help webpack determine the context for the entry. In this case css context 
      * test: /\.js$ -- looks for .js files 
        * The use key, in this case, is set to use babel 
        * The exclude key, in this case, is set to ignore the node_module directory 
      * test: /\.jsx?$/ -- looks for .jsx (XML code in React that is syntax sugar for a React element) 
        * The exclude key, in this case, is set to ignore the node_module directory. 
        * The use key, in this case, is set to use babel 

### Babel 
 
Babel transpiles your app into code that the various browsers understand. It will also tanspile any non-stage 4 JavaScript into the code needed for those browsers who have not yet supported the features you use.

Run the following at the command line: 
```
npm install babel-loader babel-core babel-preset-env babel-preset-react --save-dev 
```
This will install: 
* babel-loader --- The transpliler 
* babel-core --- Self explanitory 
* babel-preset-env --- helps determine which Babel plugins you need 
* babel-preset-react --- helps transform JSX into createElement for you 
* --save-dev --- npm setting, this will save the package name and version in the devDependencies object in package.json 
 
create a file called .babelrc and configure file with the following:  
``` 
{ 
    "presets": [ 
        "es2015", "react" 
    ] 
} 
```

A brief explanation of this .babelrc settings... 

* The presets object: 
  * The es2015 preset tells babel to compile only ES2015 to ES5 
  * The react preset tells babel to strip flow types and transform JSX to createElement calls 
 
In webpack.config.js change default port from 8080 to another port, thus isolating app from others that might use port 8080. 

Add the following module.exports ...    
```
devServer: { 
    port:9000 
} 
```

Additionally, in the webpack.config.js file, add the following to the top of the file BEFORE module.exports...
``` 
const HtmlWebpackPlugin = require('html-webpack-plugin'); 
const HtmlWebpackPluginConfig = new HtmlWebpackPlugin({ 
  template: './public/index.html', 
  filename: 'index.html', 
  inject: 'body' 
}) 
```
Then add to the webpack.config.js, in the moduel.exports object: 
```
plugins: [HtmlWebpackPluginConfig] 
``` 

This tells webpack to add Htmlwebpack to use the base index.html as our template. It will inject any JavaScript at the near the closing body tag. 
 
### The index.html file 
 
Create a directory called 'public' or 'dist' into the root directory of the current project, then create index.html with the following: 
```
<!DOCTYPE html> 
<html lang="en"> 
<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
    <meta http-equiv="X-UA-Compatible" content="ie=edge"> 
     <title>test</title> 
</head> 
<body> 
    <div id="root"></div> 
</body> 
</html> 
``` 
Add the following to the ```<head>``` section in index.html to prevent 404 favicon error: ```<link rel="shortcut icon" href="">```. Please make sure you have a div with an id of 'root' within the body tag. We will call it 'root' but you can name it something else, but you will need to remember what you named the id for your react element to be rendered. 
 
### Webpack Dev Server 
 
In package.json, add the following option to "start" entry in "scripts" section... ```webpack-dev-server --mode development```, ie ```"start" : "webpack-dev-server --mode development"``` .This will prevent the warning: ```"index_bundle.js:1 [WDS] Warnings while compiling```. The 'mode' option has not been set. Set 'mode' option to 'development' or 'production' to enable defaults for this environment."   
If you want to launch the browser when you type nmp start, add ```"--open"``` to the ```"webpack-dev-server"``` entry.  
 
### React 

Now we install the react components and setup the sample react javascript

Run the following code at the command line: 
```
npm install react react-dom --save
```

This will install: 
* react --- The react core library 
* react-dom --- Provided the DOM-specific methods and serves as the entry point for DOM-related paths 
 
### Index.js (Main react container) 
 
Create another folder for your index.js file, name this 'src' or 'client'. Create the index.js file in the folder you just created. 

Within index.js insert the following:  
```
import React from 'react'; 
import ReactDOM from 'react-dom'; 
import App from './App'; 
ReactDOM.render(<App />, document.getElementById('root')); 
``` 

A brief explanation of this  index.js... 

* import react from 'react' --- get the default module object, React in this case, from the module specifier, 'react' in this case 
* import ReactDOM from 'react-dom' --- get the default module object, ReactDOM in this case, from the module specifier, 'react-dom' in this case  
* import App from './App' --- get the default module object, App in this case, from the module specifier, the 'App' component within the current directory, in this case. Note: we will create this React element later in the document 
* ReactDOM.render(<App />, document.getElementById('root') --- call ReactDOM object's render method. This method will render the react element, <App />, into the DOM element with the id of 'root', which is in index.html. 

### App.js (A react component) 
 
Within the 'src' directory, create a file called 'App.js'. Insert the following code: 
```
import React, { Component } from 'react'; 
 
class App extends Component{ 
    render(){ 
        return( 
           <h1>Hello from React!!!</h1> 
        ); 
    } 
} 
export default App; 
```
A brief explanation of this  App.js... 
* import React, { Component } from 'react' --- get the default module object, React in this case and the Component module, from the module specifier, 'react' in this case 
* class App extends Component – This class is called 'App' and its base class is React.Component. Within the App class, we have a function called render. The render function returns a JSX element with 'Hello from React!!!' as text. 
 
## Run the app 

If you configured this correctly run the following at the command line: 
``` npm start ``` Your app will run in your browser. You should see 'http://localhost:9000/' in the address bar and *'Built with **REACT** !!!'* in the browser window.


## Built With

* [Webpack](ttps://webpack.js.org/concepts/)
* [Babel](http://babeljs.io/)
* [React](https://reactjs.org/)

## Author

* **Oscar Claros** - *Initial work* - [oclaros](https://github.com/oclaros)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to FreeCodeCamp and to coders that love to learn!!!
** Thank you to the create-react-app team for motivation.