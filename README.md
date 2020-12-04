## CodeLand

#### Instructions on setting up Vue.js and webpack mix
#####
##### 1. Install required dependencies
I think it goes without saying that you need NODE installed in order to use node package managers. If you're not get familliarized with npm or yarn.

Execute this command
```
npm i vue@2 vue-axios@2 axios webpack-mix cross-env
```

##### 2. Set up your environment
Next we need to set up the environment and settings that will help us row through these muddled water.

Create webpack.mix.js in root if you there isn't one already and put these settings in:

```
const mix = require('webpack-mix');

mix
  .options({
    terser: {
      extractComments: false
    }
  })
  .js(
    'path to your initializing js file',
    'path to your output directory'
  )
  .js(
    'path to your 2nd initializing js file',
    'path to your output directory'
  );
```
Note that you can chain as many js methods as you want, depending on how many files you're working on at the moment.
Here's my example
```
const mix = require('webpack-mix');

mix
  .options({
    terser: {
      extractComments: false
    }
  })
  .js(
    'codeLand/kia/js/representatives.js',
    'kmag.net/sharedcakephp/codeland/kia/js'
  );

```


##### 3. Set up your working area
Finally it's time to start working in vue. We'll create a directory in root and we're calling it 'codeland'. Every project you're working on will consist of two files: The vue component (Example.vue) and the initializing file (example.js). At this point I assume you already how the vue file is constructed so let's see how you call the component into the initilazing file (example):
```
import Representatives from '../vue/representatives/Representatives';
const representatives = new Vue({
  el: '.rep-list',
  components: {
    Representatives
  }
});
```
Note that the initializing file can use ES6. The "el" property is the root of your component when called in views.


Of course don't forget that every vue project needs vue added on top, so here's the initializing file for that (note that I also added axios). You will only need to run this once and can later remove it from the webpack config.
```
window.Vue = require('vue');
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios);
```

Important! Dont add this file to the header, but rather only to the projects that require vue. Thank you.

##### 4. Watcher
Cool, looks like you made your first project and are ready to compile it.

Add these scripts to your package.json
```
"scripts": {
 "serve": "webpack-dev-server --mode development",
 "development": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/webpack-mix/setup/webpack.config.js",
 "watch": "npm run development -- --watch",
 "watch-poll": "npm run watch -- --watch-poll"
}
```

This watcher is gonna debug and compile your every change. I also recommend you add these settings to the package.json
```
"jshintConfig": {
    "esversion": 6
  },
"prettier": {
    "singleQuote": true
  },
```

At this point if you dont have prettier installed yet you should probably do so
```
npm i -g prettier
```

Now run the watcher command
```
npm run watch-poll
```

##### 5. Adding the files and component to view
If you set up everything correctly and your project contains no errors you should have a fresh and compiled js file in your output directory.

Now all you need to do is upload the file to server and call it in view and well as the component. I believe I don't need to teach you how to add the script tag to html but here's how you add the component
```
<div class="root-class">
    <name-of-component></name-of-component>
</div>
```

Here's my example
```
<div class="rep-list">
            <representatives :param-data='`<?=$this->params['controller']?>`' :service-url='`<?=$DViewHelper::link_url('service')?>`' :translation-data='<?= json_encode($Translations['js_translations']); ?>'></representatives>
          </div>
```

##### 6. Naming conventions
In order to remove the buffers while the code is touched by multiple developers we need to use naming conventions. Here are the rules
```
1. Vue files are capitalized:
example: Component.vue
When importing the component to your vue template you also initialize it capitalized like so: <Component />

2. methods (functions), variables and state variables are camelcased:
example: 
nameOfFunction(){}
let nameOfVariable = ...
nameOfStateVariable: ...

3. components in views are lowercased:
When calling a component to a view you need to use lowercase convention and camel case is replaced by dashes like so (it will throw an error otherwise):
<name-of-component></name-of-component>

4. use shortcuts for vue bindings
example:
v-bind: becomes :
v-on: becomes @
and so on (for the full list check the vue documentation)
```


##### 7. Any questions?
You know where to find me.
