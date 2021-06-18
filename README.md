Zuerst:
`yarn init`
Damit wird eine package JSON erzeugt.

Als nächstes wird folgendes ausgeführt:

```yarn add react react-dom```

Somit werden Packages hinzugefügt, die für React genötigt werden. Es handelt sich dabei um Abhängigkeiten, die in der Produktion wichtig sind.
Jetzt werden DEV-Dependencies hinzugefügt, die für die Entwicklung benötigt werden.

Wir fügen WebPack hinzu, das gebraucht wird, um Datein zu bündeln und somit aus vielen kleinen Datein eine große zu machen. Zusätzlich wird ein DEV-Server
hinzugefügt mit dem es möglich ist ein Live-Reload im Browser zu haben. Webpack-cli wird benötigt, um webpack Komandos in der Konsole auszuführen.

```yarn add webpack webpack-dev-server webpack-cli -D```

Das -D am Ende fügt in der package.json ein devDependencies Object hinzu, wo alle weiteren Packages hinzugefügt werden.

Als nächstes benötigen wir Babel. Da React ES6 Klassen und JSX (JavaScript Extended) verwendet, müssen diese in ein "normales" JS Format bzw. browser freundlichen Code transpiliert werden. Hier kommt Babel ins Spiel.

```yarn add babel-core babel-loader babel-preset-env babel-preset-react html-webpack-plugin -D```

In der root erstellen wir eine .babelrc file in die folgende pre-sets hineinkommen:

```
  {
    "presets": ["env", "react"]
  }
```

  Immer wenn Webpack verwendet wird, benötigen wir eine config File. Also erstellen wir in der root:

  webpack.confog.js

  Da rein schreiben wir das path Module das ein Node.js core modul ist, außerdem kommt da noch das HTML-Webpack Plugon rein.
  Anschließend wird eine React Entry File spezifiziert.

  const path = require('path');
  // will create an index.html
  const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: "./src/index.js",
    output: {
      path: path.join(__dirname, "/dist"),
      filename: "index_bundle.js"
    },
    //specification of babel loader to transpile
    module: {
      rules: [
        {
          test: /\.js?$/,
          exclude: /node_modules/,
          loader: "babel-loader",
          query: {
            presets: ["@babel/preset-env"]
          }
        }
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: "./src/index.html"
      })
    ]
  };

  Dann muss eine src Folder im root erstellt werden mit eine index.js file drin und in der alle React Files untergebracht werden.
  Der `output` gibt an wo das gebündelte JS abgelegt wird.

  Anschließend kommt folgendes in die index.js:

  ```
  import React from 'react'
  import ReactDOM from 'react-dom'
  import App from './components/App'

  ReactDOM.render(<App />, document.getElementById('app'));
  ```

  Nun muss die App.js erstellt werden. Diese kommt in die src/component Folder, die noch erstellt werden muss.

  ```
  import React, { Component } from 'react';

  class App extends Component {
      return () {

          <div>My React App</div>

      };
  }

  export default App;
  ```

  Jetzt müssen noch zwei Skripte zur package.json hinzugefügt werden.
  Der eine ist dazu da, um den webpack dev Server laufen zu lassen und der andere, um alle Skripte in den dist Ordner zu bündeln und puplikationsfähig zu machen.

  ```
    "scripts": {
      "start": "webpack-dev-server --mode development --open --hot",
      "build": "webpack --mode production"
    }`

    Jetzt kann die React Anwendung mir `yarn start` gestartet werden.


  "scripts" : {"test": "echo \"Test\" "}
  ```
