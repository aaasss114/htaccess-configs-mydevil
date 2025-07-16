![20defe2e-8b28-4546-9415-cf52ffa8c685](https://github.com/user-attachments/assets/4e221d2f-71e1-461b-b3e0-0981887a2276)
![5a212f8d-3146-4f5b-9474-73fc0bf7448e](https://github.com/user-attachments/assets/c42cacbc-7e81-4fb9-8a8f-a028668e0381)
# [MyDevil .htaccess Configs](https://github.com/rechandler12/htaccess-configs-mydevil/)

[![Build Status](https://travis-ci.com/rechandler12/htaccess-configs-mydevil.svg?branch=master)](https://travis-ci.com/rechandler12/htaccess-configs-mydevil)

**MyDevil .htaccess Configs** is a collection of configuration snippets that can help
your server improve the web site's performance and security, while also
ensuring that resources are served with the correct content-type and are
accessible, if needed, even cross-domain.

This is a fork of the [Apache Server Configs](https://github.com/h5bp/server-configs-apache) prepared for [MyDevil](https://www.mydevil.net) hosting.


## Getting Started

There are a few options for getting the MyDevil .htaccess configs:

* Download the [zip archive](https://github.com/rechandler12/htaccess-configs-mydevil/archive/v1.0.2.zip)
* Install them via [npm](https://www.npmjs.com/):
  `npm install --save-dev htaccess-configs-mydevil`

Inside the **dist/** folder, you'll find a ready-to-use **.htaccess** file.


## Usage

Copy the [`.htaccess`](https://github.com/rechandler12/htaccess-configs-mydevil/blob/master/dist/.htaccess)
file in the root of the website.

## Custom .htaccess builds

Security, mime-type, and caching best practices evolve, and so should do your *.htaccess* file. In the past, with each new *MyDevil .htaccess Configs* release it was quite tedious to find out which *.htaccess* trick was just new or only had changes in certain nuances.

The [**build script**](#build-script-buildsh) with its re-usable and customizable [**build configuration**](#configuration-file-htaccessconf) lets you easily update your *.htaccess* file. Each new *.htaccess* build will contain the updated *MyDevil .htaccess Configs* source files, enabled or commented-out according to your settings in the *htaccess.conf* of your project root.

### Configuration file: *htaccess.conf*

Allows you to define which module to [enable](#enabling-modules) or [disable](#disabling-modules) for your project. Just copy the default [**htaccess.conf**](https://github.com/rechandler12/htaccess-configs-mydevil/blob/master/bin/htaccess.conf) from this repo into your project directory. Adjust to your needs, and/or [add custom code](#adding-custom-modules) snippets you need for your project. Its syntax is straight and pretty much self-explanatory:

```
# Example Module

title   "example module"
enable  "src/example-module/images.conf"
enable  "src/example-module/web_fonts.conf"
disable "src/example-module/not-needed.conf"
omit    "src/example-module/not-needed-at-all.conf"

... more modules ...
```

#### Disabling modules

For example, the *“Cross-origin web fonts”* snippet is always included in our pre-built [*.htaccess*](https://github.com/rechandler12/htaccess-configs-mydevil/blob/master/dist/.htaccess) file and enabled. If your project does not deal with web fonts, you can **disable** or **omit** this section:

This will comment out the section:

```
disable  "src/cross-origin/web_fonts.conf"
```

…and this will exclude the section, saving lines in output:

```
omit  "src/cross-origin/web_fonts.conf"
```

#### Enabling modules

For example, the *“Forcing https://”* snippet is disabled by default, although being included in our pre-built [*.htaccess*](https://github.com/rechandler12/htaccess-configs-mydevil/blob/master/dist/.htaccess). To enable this snippet, change the **disable** keyword to **enable:**

```
enable "src/rewrites/rewrite_http_to_https.conf"
```

#### Adding custom modules

Imagine you're passing all requests to non-existing files to your favourite web framework. The according *mod_rewrite* snippet would go like this:

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^ index.php [QSA,L]
```

Store this snippet in a file, e.g. **config/framework_rewrites.conf,** and add a reference in your **htaccess.conf:**

```
# PROJECT MODULES
enable "config/framework_rewrites.conf"
```

### Build script: *build.sh*

Dive into your project root and call the build script from wherever you cloned the repo. Here are three examples:

**1. Create a default .htaccess**  
in current work directory. An existing **htaccess.conf** in this directory will be used; if none is present, the [**default configuration**](https://github.com/rechandler12/htaccess-configs-mydevil/blob/master/bin/htaccess.conf) will apply.


```bash
$ path/to/htaccess-configs-mydevil/bin/build.sh

# Output looks like:
[✔] Build .htaccess
[✔] Moved in place: './.htaccess'
```

**2. Custom output location**  
Just add output path and filename as parameter. By the way, if there's an existing *.htaccess* file, the build script will create a backup.

```bash
$ path/to/htaccess-configs-mydevil/bin/build.sh htdocs/.htaccess
[✔] Build .htaccess
[✔] Create backup: 'htdocs/.htaccess~'
[✔] Moved in place: 'htdocs/.htaccess'
```

**3. Custom .htaccess configuration**  
Why not maintain your personal **~/htaccess.conf?** This example creates a *.htaccess* in current work directory, according to your favourite settings you may have stored in your `$HOME` directory:

```bash
$ path/to/htaccess-configs-mydevil/bin/build.sh ./.htaccess ~/htaccess.conf
```


## Support

* ### __MyDevil hosting__
* ### __Apache v2.4.0+__
* ### __Browsers:__
  * Chrome
  * Firefox 4+
  * Internet Explorer 8+
  * Opera 12+
  * Safari 5+
  
### Unsupported modules from original project
#### Custom error messages/pages
MyDevil does not support ```ErrorDocument```. However, there is a workaround. You can create custom error pages in directory ```/errors/xxx.html``` when xxx is HTTP code.

Link to the official documentation (in polish): [Własne strony błędów](https://wiki.mydevil.net/PHP#W.C5.82asne_strony_b.C5.82.C4.99d.C3.B3w)

#### Disable TRACE HTTP Method

Due to the lack of access to the main configuration, the option is not available.

#### Server-side technology information

MyDevil does not support ```Header unset```. There is no workaround at the moment.

#### ServerTokens Prod

Due to the lack of access to the main configuration, the option is not available.

#### Compression

MyDevil does not support ```AddOutputFilterByType DEFLATE```. However, there is a workaround. You can enable GZIP compression at your page settings.

Link to the official documentation (in polish): [Strona WWW/PHP](https://wiki.mydevil.net/Strona_WWW#PHP)

#### Brotli/GZip pre-compressed content

Due to the lack of compression, the option is also not available.

#### ETags

MyDevil does not support ```Header unset```. There is no workaround at the moment.

#### File concatenation

MyDevil does not support
```
Options +Includes
AddOutputFilterByType INCLUDES
SetOutputFilter INCLUDES
```
There is no workaround at the moment.

## Contributing

Anyone is welcome to [contribute](.github/CONTRIBUTING.md),
however, if you decide to get involved, please take a moment to review
the [guidelines](.github/CONTRIBUTING.md):

* [Bug reports](.github/CONTRIBUTING.md#bugs)
* [Feature requests](.github/CONTRIBUTING.md#features)
* [Pull requests](.github/CONTRIBUTING.md#pull-requests)


## Acknowledgements

[MyDevil .htaccess Configs](https://github.com/rechandler12/htaccess-configs-mydevil/) is only possible thanks to all the awesome
[contributors](https://github.com/rechandler12/htaccess-configs-mydevil/graphs/contributors)!

[Apache Server Configs](https://github.com/h5bp/server-configs-apache/) is only possible thanks to all the awesome
[contributors](https://github.com/h5bp/server-configs-apache/graphs/contributors)!

## License

The code is available under the [MIT license](LICENSE).
