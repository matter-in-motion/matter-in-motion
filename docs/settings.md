# Matter in Motion. Settings

Matter in motion provides very flexible settings system for your application.

## Basics

There is `Settings` class that you should inherit your settings class from. Create `settings.js` or `settings/index.js` in your app directory.

```js
'use strict';
const inherits = require('util').inherits;
const MMSettings = require('matter-in-motion').Settings;

const Settings = function() {
  MMSettings.call(this);
};
inherits(Settings, MMSettings);

Settings.prototype.init = function() {
  Settings.super_.prototype.init.call(this);
  //here you can define all settings that your application needs like:
  this.something = true;
};

module.exports = Settings;
```

## Usage

Now you can easily import any of your settings in any of unit inside your application:

```js
Module.prototype.__init = function(units) {
  this.isIt = units.require('core.settings').something
}
```

## Using different environments

You can add `dev.js` file in your settings directory.

```js
'use strict';

module.exports = function() {
  this.core.debug = true;
};

```

To use this environment, you should define a `NODE_ENV` environment variable or launch your app with `--env=dev`.

Thus you can define any number environments for your app.

## Default settings

_All matter in motion settings placed under `core` namespace._

* __root__ — read only, current working directory
* __serializers__ — object, defines serializers settings.
* __extensions__ array of extensions
* __core__
  * __debug__ — boolean, false. Debug mode, adds more logging and validates API responses if the validator is present
  * __uncaught__ — boolean, true. Adds uncaught exceptions handler, default handler just puts all exception into the main log. You can define your own by creating `uncaught.js` in your app directory. See original `uncaught.js` for more info.
  * __api__
    - path, '/api'. API URL path
    - discovery, true. Allows the discovery ('?') requests
    - throttle, false. Throttle responses at random time. Delays are defined by like `[min, max]`

  * __auth__ — object. [Authentication settings](https://github.com/matter-in-motion/mm/blob/master/docs/authentication.md).
  * __daemon__ — object. Daemon settings
    + __start__
      * __exec__ — file to execute, `require.main.filename`
      * __args__ — array, `[ 'daemon' ]`. Arguments to execute with
      * __stdout__ — filename, `var/log/app.log`. Filename for `stdout` stream
      * __stderr__ — filename, `var/log/err.log`. Filename for `stderr` stream
    + __pidFile__ — filename, `var/app/app.pid`. Filename to save pid to
    + __exitCheckInterval__ — integer, 200

  - __cluster__ — object. Cluster settings
    + __numberOfWorkers__ — integer, `cpus().length`. Number of workers in the cluster
    + __master__
      * __exec__ – file to execute, `require.main.filename`, File path to worker file
      * __args__ — array, null. String arguments passed to worker
      * __silent__ — boolean, false. Whether or not to send output to parent's stdio
    + __checks__
      * __lazy__
        - __interval__ — milliseconds, 40 * 1000
        - __maxTime__ — milliseconds, 30 * 1000
      * __zombie__
        - __interval__ — milliseconds, 40 * 1000
        - __maxTime__ — milliseconds, 30 * 1000

  - __validator__ — object. Matter in motion uses [ajv default settings](https://github.com/epoberezkin/ajv#options) except:
    + __removeAdditional__ — boolean, true. Only additional properties with additionalProperties keyword equal to false are removed
    + __useDefaults__ — boolean, true. Replace missing properties and items with the values from corresponding default keywords
    + __errorDataPath__ — string, `api`. Set dataPath to point to `api` (default) or to `property` when validating keywords `required`, `additionalProperties` and `dependencies`.

  - __logger__ — object. Logger settings, check the [pino](https://github.com/pinojs/pino) for more info

```
{
  options: { name: 'app' },
  stream: 'var/log/app.log'
}
```

## Settings class methods

### joinPath(...args)

returns joined path from the `core.root`.

### require(name)

returns the `name` section from the settings, if the section does not exist throws an error.

### apply(settings)

if `settings` is a string tries to load `settings` file. If `settings` is an object applies it to the global settings.
