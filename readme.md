Less parser plugin for CakePHP 3.X
==================================

This plugin has a helper to help you parsing `.less` files in CakePHP 3.0 applications.

By default, the helper will parse all less files to CSS files using [less.php](https://github.com/oyejorge/less.php) but if, for any reason, it fails, will fallback to the [less.js](http://lesscss.org/#download-options) parser both included by this plugin (this way you'll see any errors on screen).

Installation
------------

You can install this plugin into your CakePHP application using [composer](http://getcomposer.org).

The recommended way to install composer packages is:

```
composer require elboletaire/less-cake-plugin
```

After installing it you'll need to load it on your `bootstrap.php` file:

```php
Plugin::load('Less');
```

Usage
-----

By default it will compress files using the php parser with cache enabled.
This will fill your `css` folder with a bunch of files starting with `lessphp_`
used for the cache. I recommend you adding these files to your `.gitignore` file
in order to prevent commiting them:

    lessphp_*

Basically, you give the helper a less file to be loaded (usually from `/less`
directory) and it returns the html link tag to the compiled CSS:

```php
echo $this->Less->less('less/styles.less');
// will result in something like...
<link rel="stylesheet" href="/css/lessphp_8e07b9484a24787e27f9d71522ba53443d18bbd2.css" />
```

You can compile multiple files if you pass an array:

```php
echo $this->Less->less(['less/myreset.less', 'less/styles.less']);
// They will be compiled in the same file, so the result will be the same as the previous one
<link rel="stylesheet" href="/css/lessphp_e0ce907005730c33ca6ae810d15f57a4df76d330.css"/>
```

And you can pass any option to both less.js and less.php parsers:

```php
echo $this->Less->less('less/styles.less', [
    'js' => [
        // options for lessjs (will be converted to a json object)
    ],
    'parser' => [
        // options for less.php parser
    ],
    // The helper also has its own options
]);
```

If you want to use the less.js parser directly, instead of a fallback, or you
want to use the
[#!watch](http://lesscss.org/usage/#using-less-in-the-browser-watch-mode) method,
you can do it so by setting the js parser to development:

```php
echo $this->Less->less('less/styles.less', ['js' => ['env' => 'development']]);
```

This will output all the links to the less files and the needed js files to
parse the content only using the less.js parser.

> Note: if there is an error in the php parser the helper will fallback to
the js parser so you can see the errors in screen. If, for any reason, you can't
see those errors, check the `less.log` file in the logs folder; it will contain
any error generated by the less.php parser.

To load less files inside plugins you can use plugin notation:

```php
echo $this->Less->less('Bootstrap.less/styles.less');
// will load plugins/Bootstrap/webroot/less/styles.less file
```

### Options

Beside the options for
[lessjs](http://lesscss.org/#client-side-usage-browser-options) and
[less.php](https://github.com/oyejorge/less.php#lessphp) parsers you can set
three options to the helper:

+ `cache`: default's to true. If disabled, the output will be raw CSS wrapped
  with `<style>` tags.
+ `tag`: default's to true. Whether or not return the code with its proper tag
  (with cache enabled will be a link tag, whilst without cache will be a style
  tag).
+ `less`: default's to `/bootstrap/js/less.min`. You can use this var to set a
  custom lessjs file.

```php
// Get the link to the resulting file after compressing
$css_link = $this->Less->less('less/styles.less', [
    'tag'   => false
]);

// Get the compiled CSS (raw)
$compiled_css = $this->Less->less('less/styles.less', [
    'cache' => false,
    'tag'   => false
]);
```

As a default setting of the LessHelper, all the CSS generated by the less.php
parser is compresed. To override this set `compress` to `false` in the less.php
parser options:

```php
echo $this->Less->less('less/styles.less', [
  'parser' => ['compress' => false]
]);
```


License
-------

    Copyright 2015 Òscar Casajuana (a.k.a. elboletaire)

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    imitations under the License.
