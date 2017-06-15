![](https://img.shields.io/npm/v/quilk.svg) ![](https://img.shields.io/npm/dt/quilk.svg)

[![NPM](https://nodei.co/npm/quilk.png?downloads=true&downloadRank=true)](https://nodei.co/npm/quilk/)


#quilk
Est. 4th Sept. 2016

---

In brief, quilk is a lightweight standardised module runner. Pre-baked modules in quilk can do the following:

* **Rsync** files locally to a development server, ideal for ensuring each dev has the same environment and doesn't need to spend time managing a virtual box or its CPU overheads.
* Compile [**SASS**](https://www.npmjs.com/package/node-sass) with node-sass, either by **finding scss** files or by giving it simple entry point.
* Compile [**LESS**](https://www.npmjs.com/package/less) (no find module has been written for LESS files yet).
* Generate a single CSS file from a **fixed list of CSS** files.
* Compile JS code with [**Babelify**](https://www.npmjs.com/package/babelify) either in one big file or broken into vendor.js and app.js with the babelify_app and babelify_vendor module.
* [**Browserify**](https://www.npmjs.com/package/browserify) your backend modules to share with the front end.
* [**Concat**](https://www.npmjs.com/package/concat) big client side js from a fixed list or instruct quilk to **find** js files in a folder with concat or node-minify
* **Obfusicate, minify** javascript or css using the [**node-minify**](https://www.npmjs.com/package/node-minify) module check out their docs for more on node-minify.
* **Strip out** comments from js code.
* Configure **independent** blocks for developers.
* [**Desktop notifications**](https://www.npmjs.com/package/node-notifier) on or off or on for varying levels.
* Ping messages via [**email**](https://www.npmjs.com/package/nodemailer) when a built has finished, with success or not.
* Ping messages via **webhooks** via the [request](https://www.npmjs.com/package/request) npm when a build was successful or a giant failure, eg into a slack channel.
* **Watch** a local fileset with the watch flag ([**chokidar**](https://www.npmjs.com/package/chokidar) under the hood) to re-run build modules after file changes, doesn't rebuild all, just the affected modules)

Most of the standard jobs can be covered with a single quilk file and the baked in modules into quilk, however there is of course the ability to write your own modules for quilk, either publicy hosted of privately nested in your project repo.

---

### Documentation here 
[https://jdcrecur.github.io/quilk/](https://jdcrecur.github.io/quilk/)

---

*If anyone fancies helping [evolve](https://github.com/jdcrecur/quilk/) quilk give me a shout, many [skilled] hands make light work.*

### Example quilk file
```
module.exports = {
  modules: [
    {
      name: '(custom project specific module) Preapre the js config files based on the .env file',
      module: 'prepareJSConfigFiles'
    },
    {
      name: 'App file',
      module: 'babelify',
      configure: {
        babelrc: '.babelrc'
      },
      extensions: ['.js'],
      debug: true,
      entries: 'resources/assets/js/app.js',
      target: '/public/js/app.js'
    },
    {
      name: 'App CSS',
      module: 'sass_std',
      outputStyle: 'expanded',
      sourceComments: true,
      input_path: 'resources/assets/sass/app.scss',
      target: '/public/css/app.css'
    }
    {
      name: 'Rsync it',
      module: 'rsync',
      ignore: {
        windows: [],
        mac: [],
        linux: [],
        global: [
          '.git/*',
          '.idea/*',
          'storage/app/*',
          'storage/logs/*',
          'node_modules'
        ]
      }
    }
  ],

  dont_watch: [
    '.git/',
    'node_modules',
    'vendor',
    'public/css',
    'public/fonts',
    'public/js'
  ],

  chokidar_options: {
    atomic: 100
  },

  release_commands_or_modules: {
    prod: {
      post: [{
        name: 'minify the vendor js',
        module: 'node_minify',
        type: 'uglifyjs',
        input: ['/public/js/app.js'],
        target: '/public/js/app.js'
      }, {
        name: 'minify the css',
        module: 'node_minify',
        type: 'sqwish',
        input: ['/public/css/app.css'],
        target: '/public/css/app.css'
      }]
    }
  },

  developers: {
    default: {
      platform: 'windows',
      notifier: {
        on: false,
        style: 'WindowsBalloon',
        time: 5000,
        sound: true
      }
    },
    carmichael: {
      platform: 'windows',
      notifier: {
        on: false
      },
      rsync: {
        localPath: '/cygdrive/c/code//project-x/',
        remote: 'www-data@myserver',
        serverPath: '/var/www/vhosts/project-x/'
      }
    }
  }
}
```


### Tips

1. All use of babelify or the babili compressor within the node-minify module require you to install your presets locally.
1. The options directive for node_minify module in quilk will be passed right into the actual node-minify package.