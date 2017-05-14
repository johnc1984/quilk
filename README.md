![](https://img.shields.io/npm/v/quilk.svg) ![](https://img.shields.io/npm/dt/quilk.svg)

[![NPM](https://nodei.co/npm/quilk.png?downloads=true&downloadRank=true)](https://nodei.co/npm/quilk/)


#quilk
Est. 4th Sept. 2016

---

Quilk is a developer tool to build your project from a standardised JSON file, or a common js module. Both without requiring a long list of dependencies, just a single install globally of quilk.

In brief, quilk is a lightweight module runner, pre-baked modules in quilk can do the following:

* Compile [**SASS**](https://www.npmjs.com/package/node-sass) with node-sass, either by **finding scss** files or by giving it simple entry point.
* Compile [**LESS**](https://www.npmjs.com/package/less) (no find module has been written for LESS files yet).
* Generate a single CSS file from a **fixed list of CSS** files.
* [**Concat**](https://www.npmjs.com/package/concat) big client side js from a fixed list or instruct quilk to **find** js files in a folder with concat or node-minify
* [**Browserify**](https://www.npmjs.com/package/browserify) your backend modules to share with the front end
* [**Babelify**](https://www.npmjs.com/package/babelify) your code, either in one big file or broken into vendor.js and app.js with the babelify_app and babelify_vendor module.
* **Rsync** files locally to a development server, ideal for ensuring each dev has the same environment, and saves so much time!
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


### Latest commits
1. New directive for the sass find module, `include_last`. The same as the include first directive, but instead includes the array of files at the end of the file.
1. you can now control Chokidar, the default options are:
```
{
    persistent: true,
    followSymlinks: false,
    alwaysStat: true,
    awaitWriteFinish: true,
    atomic: 200
}
```
You can now take control of this by adding a `chokidar_options` in the quilk file. 
eg:
```
{
    followSymlinks: true,
    depth: 120
}
```
Which results in:
```
{
    persistent: true,
    followSymlinks: true,
    alwaysStat: true,
    awaitWriteFinish: true,
    atomic: 200,
    depth: 120
}
```
See https://www.npmjs.com/package/chokidar for more options available.


2. Updated node-minify to 2.0.4 which fixes the babili compressor bug.
3. The rsync module has a new command. Pass in `rsyncPull` as a command line arg and the module will pull all the contents from the target to the local.
4. The babelify_vendor module now knows when and when not to rebuild during quilk watch.

### Tips

1. All use of babelify or the babili compressor within the node-minify module require you to install your presets locally.
1. The options directive for node_minify module in quilk will be passed right into the actual node-minify package.