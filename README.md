# Lando test environment

This environment is only used to reproduce a blackfire probe segfault.

Make sure lando is installed. https://docs.lando.dev/basics/installation.html

 * Clone this repository
 * Set your blackfire credentials in .lando/.blackfire.env
 * run `lando start` (takes a while)
 * Open https://drupal-lando-starterkit.lndo.site/ - you should get "Hello Drupal world!"
 * Run `lando drush cr` in the /web folder
 * Start a profiler session on https://drupal-lando-starterkit.lndo.site/
 * You should get "This URL is non reachable: https://drupal-lando-starterkit.lndo.site/ Troubleshooting?"

 `lando logs -s appserver`

 ```
appserver_1        | Mon May 31 07:49:16 2021 (1307): [Debug] Controller-name set to 'Drupal\node\Controller\NodeViewController::view'
appserver_1        | Mon May 31 07:49:16 2021 (1307): [Fatal Error] Blackfire Probe received a SIGSEGV
appserver_1        | Mon May 31 07:49:16 2021 (1307): [Error] C backtrace :
appserver_1        | Mon May 31 07:49:16 2021 (1307): [Error] [*] /usr/local/lib/php/extensions/no-debug-non-zts-20190902/blackfire.so(bf_sigsegv_handler+0x5b) [0x7f020dc476db]
 ```

Open `web/core/includes/bootstrap.inc` and

Change these lines:
```
function drupal_get_path($type, $name) {
  return dirname(drupal_get_filename($type, $name));
}
```

To:

```
function drupal_get_path($type, $name) {
  return dirname(drupal_get_filename($type, $name, NULL));
}
```

* Run the profiler again - should work without a problem.
