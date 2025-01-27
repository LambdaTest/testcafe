---
layout: docs
title: Command Line Interface
permalink: /documentation/using-testcafe/command-line-interface.html
checked: true
---
# Command Line Interface

```sh
testcafe [options] <browser-list-comma-separated> <file-or-glob ...>
```

* [Browser List](#browser-list)
  * [Local Browsers](#local-browsers)
  * [Portable Browsers](#portable-browsers)
  * [Testing in Headless Mode](#testing-in-headless-mode)
  * [Using Chrome Device Emulation](#using-chrome-device-emulation)
  * [Remote Browsers](#remote-browsers)
  * [Browsers Accessed Through Browser Provider Plugins](#browsers-accessed-through-browser-provider-plugins)
  * [Starting browser with arguments](#starting-browser-with-arguments)
* [File Path/Glob Pattern](#file-pathglob-pattern)
* [Options](#options)
  * [-h, --help](#-h---help)
  * [-v, --version](#-v---version)
  * [-b, --list-browsers](#-b---list-browsers)
  * [-r \<name\[:output\],\[...\]\>, --reporter \<name\[:output\],\[...\]\>](#-r-nameoutput---reporter-nameoutput)
  * [-s \<path\>, --screenshots \<path\>](#-s-path---screenshots-path)
  * [-S, --screenshots-on-fails](#-s---screenshots-on-fails)
  * [-p \<pattern\>, --screenshot-path-pattern \<pattern\>](#-p-pattern---screenshot-path-pattern-pattern)
  * [--video \<basePath\>](#--video-basepath)
  * [--video-options \<option=value\[,option2=value2,...\]\>](#--video-options-optionvalueoption2value2)
  * [--video-encoding-options \<option=value\[,option2=value2,...\]\>](#--video-encoding-options-optionvalueoption2value2)
  * [-q, --quarantine-mode](#-q---quarantine-mode)
  * [-d, --debug-mode](#-d---debug-mode)
  * [--debug-on-fail](#--debug-on-fail)
  * [-e, --skip-js-errors](#-e---skip-js-errors)
  * [-u, --skip-uncaught-errors](#-u---skip-uncaught-errors)
  * [-t \<name\>, --test \<name\>](#-t-name---test-name)
  * [-T \<pattern\>, --test-grep \<pattern\>](#-t-pattern---test-grep-pattern)
  * [-f \<name\>, --fixture \<name\>](#-f-name---fixture-name)
  * [-F \<pattern\>, --fixture-grep \<pattern\>](#-f-pattern---fixture-grep-pattern)
  * [--test-meta \<key=value\[,key2=value2,...\]\>](#--test-meta-keyvaluekey2value2)
  * [--fixture-meta \<key=value\[,key2=value2,...\]\>](#--fixture-meta-keyvaluekey2value2)
  * [-a \<command\>, --app \<command\>](#-a-command---app-command)
  * [--app-init-delay \<ms\>](#--app-init-delay-ms)
  * [-c \<n\>, --concurrency \<n\>](#-c-n---concurrency-n)
  * [--selector-timeout \<ms\>](#--selector-timeout-ms)
  * [--assertion-timeout \<ms\>](#--assertion-timeout-ms)
  * [--page-load-timeout \<ms\>](#--page-load-timeout-ms)
  * [--speed \<factor\>](#--speed-factor)
  * [--ports \<port1,port2\>](#--ports-port1port2)
  * [--hostname \<name\>](#--hostname-name)
  * [--proxy \<host\>](#--proxy-host)
  * [--proxy-bypass \<rules\>](#--proxy-bypass-rules)
  * [--ssl \<options\>](#--ssl-options)
  * [-L, --live](#-l---live)
  * [--dev](#--dev)
  * [--qr-code](#--qr-code)
  * [--sf, --stop-on-first-fail](#--sf---stop-on-first-fail)
  * [--ts-config-path \<path\>](#--ts-config-path-path)
  * [--color](#--color)
  * [--no-color](#--no-color)

When you execute the `testcafe` command, TestCafe first reads settings from the `.testcaferc.json` [configuration file](configuration-file.md) if this file exists, and then applies the settings from the command line. Command line settings override values from the configuration file in case they differ. TestCafe prints information about every overridden property in the console.

If the [browsers](configuration-file.md#browsers) and [src](configuration-file.md#src) properties are specified in the configuration file, you can omit them in the command line.

> Important! Make sure to keep the browser tab that is running tests active. Do not minimize the browser window.
> Inactive tabs and minimized browser windows switch to a lower resource consumption mode
> where tests do not always execute correctly.

If a browser stops responding while it executes tests, TestCafe restarts the browser and reruns the current test in a new browser instance.
If the same problem occurs with this test two more times, the test run finishes and an error is thrown.

## Browser List

The `browser-list-comma-separated` argument specifies the list of browsers (separated by commas) where tests are run.

*Related configuration file property*: [browsers](configuration-file.md#browsers).

### Local Browsers

You can specify [locally installed browsers](common-concepts/browsers/browser-support.md#locally-installed-browsers) using browser aliases or paths (with the `path:` prefix).
Use the [--list-browsers](#-b---list-browsers) command to output aliases for automatically detected browsers.

The following example demonstrates how to run a test in several browsers.
The browsers are specified differently: one using an alias, the other using a path.

```sh
testcafe chrome,path:/applications/safari.app tests/sample-fixture.js
```

Use the `all` alias to run tests in **all the installed browsers**.

```sh
testcafe all tests/sample-fixture.js
```

### Portable Browsers

You can specify [portable browsers](common-concepts/browsers/browser-support.md#portable-browsers) using paths to the browser's executable file (with the `path:` prefix), for example:

```sh
testcafe path:d:\firefoxportable\firefoxportable.exe tests/sample-fixture.js
```

If the path contains spaces, enclose it in backticks and additionally surround the whole parameter including the keyword in quotation marks.

In Windows `cmd.exe` (default command prompt), use double quotation marks:

```sh
testcafe "path:`C:\Program Files (x86)\Firefox Portable\firefox.exe`" tests/sample-fixture.js
```

In `Unix` shells like `bash`, `zsh`, `csh` (macOS, Linux, Windows Subsystem for Linux) and Windows PowerShell, use single quotation marks:

```sh
testcafe 'path:`C:\Program Files (x86)\Firefox Portable\firefox.exe`' tests/sample-fixture.js
```

> Do not use the `path:` prefix if you need to run a browser in the [headless mode](common-concepts/browsers/testing-in-headless-mode.md), use [device emulation](common-concepts/browsers/using-chrome-device-emulation.md) or [user profiles](common-concepts/browsers/user-profiles.md). Specify the [browser alias](common-concepts/browsers/browser-support.md#locally-installed-browsers) in these cases.

### Testing in Headless Mode

To run tests in the headless mode in Google Chrome or Firefox, use the `:headless` postfix:

```sh
testcafe "firefox:headless" tests/sample-fixture.js
```

See [Testing in Headless Mode](common-concepts/browsers/testing-in-headless-mode.md) for more information.

### Using Chrome Device Emulation

To run tests in Chrome's device emulation mode, specify `:emulation` and [device parameters](common-concepts/browsers/using-chrome-device-emulation.md#emulator-parameters).

```sh
testcafe "chrome:emulation:device=iphone X" tests/sample-fixture.js
```

See [Using Chrome Device Emulation](common-concepts/browsers/using-chrome-device-emulation.md) for more details.

### Remote Browsers

To run tests in a [browser on a remote device](common-concepts/browsers/browser-support.md#browsers-on-remote-devices), specify `remote` as a browser alias.

```sh
testcafe remote tests/sample-fixture.js
```

If you want to connect multiple browsers, specify `remote:` and the number of browsers. For example, if you need to use three remote browsers, specify `remote:3`.

```sh
testcafe remote:3 tests/sample-fixture.js
```

TestCafe provides URLs you should open in your remote device's browsers.

> If you run tests [concurrently](#-c-n---concurrency-n),
> specify the total number of all browsers' instances after the `remote:` keyword.

You can also use the [--qr-code](#--qr-code) option to display QR-codes that represent the same URLs.
Scan the QR-codes using the device on which you are going to test your application.
This connects the browsers to TestCafe and starts the tests.

### Browsers Accessed Through Browser Provider Plugins

To run tests in [cloud browsers](common-concepts/browsers/browser-support.md#browsers-in-cloud-testing-services) or [other browsers](common-concepts/browsers/browser-support.md#nonconventional-browsers) accessed through a [browser provider plugin](../extending-testcafe/browser-provider-plugin/README.md),
specify the browser's alias that consists of the `{browser-provider-name}` prefix and the name of a browser (the latter can be omitted); for example, `saucelabs:Chrome@52.0:Windows 8.1`.

```sh
testcafe "saucelabs:Chrome@52.0:Windows 8.1" tests/sample-fixture.js
```

### Starting browser with arguments

If you need to pass arguments for the specified browser, write them after the browser's alias. Enclose the browser call and its arguments in quotation marks.

In Windows `cmd.exe` (default command prompt), use double quotation marks:

```sh
testcafe "chrome --start-fullscreen" tests/sample-fixture.js
```

In `Unix` shells like `bash`, `zsh`, `csh` (macOS, Linux, Windows Subsystem for Linux) and Windows PowerShell, use single quotation marks:

```sh
testcafe 'chrome --start-fullscreen' tests/sample-fixture.js
```

You can also specify arguments for portable browsers. If a path to a browser contains spaces, the path should be enclosed in backticks.

For Unix shells and Windows PowerShell:

```sh
testcafe 'path:`/Users/TestCafe/Apps/Google Chrome.app` --start-fullscreen' tests/sample-fixture.js
```

For `cmd.exe`:

```sh
testcafe "path:`C:\Program Files (x86)\Google\Chrome\Application\chrome.exe` --start-fullscreen" tests/sample-fixture.js
```

Only installed and portable browsers located on the current machine can be launched with arguments.

## File Path/Glob Pattern

The `file-or-glob ...` argument specifies the files or directories (separated by a space) from which to run the tests.

TestCafe can run:

* JavaScript, TypeScript and CoffeeScript files that use [TestCafe API](../test-api/README.md),
* [TestCafe Studio](https://www.devexpress.com/products/testcafestudio/) tests (`.testcafe` files),
* Legacy TestCafe v2015.1 tests.

*Related configuration file property*: [src](configuration-file.md#src).

For example, this command runs all tests in the `my-tests` directory:

```sh
testcafe ie my-tests
```

The following command runs tests from the specified fixture files:

```sh
testcafe ie js-tests/fixture.js studio-tests/fixture.testcafe
```

You can also use [glob patterns](https://github.com/isaacs/node-glob#glob-primer) to specify a set of files.

The following command runs tests from files that match the `tests/*page*` pattern (for instance, `tests/example-page.js`, `tests/main-page.js`, or `tests/auth-page.testcafe`):

```sh
testcafe ie tests/*page*
```

If you do not specify any file or directory, TestCafe runs tests from the `test` or `tests` directories.

## Options

### -h, --help

Displays commands' usage information.

```sh
testcafe --help
```

### -v, --version

Displays the TestCafe version.

```sh
testcafe --version
```

### -b, --list-browsers

Lists the aliases of the [auto-detected browsers](common-concepts/browsers/browser-support.md#locally-installed-browsers) installed on the local machine.

```sh
testcafe --list-browsers
```

### -r \<name\[:output\],\[...\]\>, --reporter \<name\[:output\],\[...\]\>

Specifies the name of a [built-in](common-concepts/reporters.md) or [custom reporter](../extending-testcafe/reporter-plugin/README.md) that is used to generate test reports.

The following command runs tests in all available browsers and generates a report in xunit format:

```sh
testcafe all tests/sample-fixture.js -r xunit
```

The following command runs tests and generates a test report by using the custom reporter plugin:

```sh
testcafe all tests/sample-fixture.js -r my-reporter
```

The generated test report is displayed in the command prompt window.

If you need to save the report to an external file, specify the file path after the report name.

```sh
testcafe all tests/sample-fixture.js -r json:report.json
```

You can also use multiple reporters in a single test run. List them separated by commas.

```sh
testcafe all tests/sample-fixture.js -r spec,xunit:report.xml
```

Note that only one reporter can write to `stdout`. All other reporters must output to files.

*Related configuration file property*: [reporter](configuration-file.md#reporter).

### -s \<path\>, --screenshots \<path\>

Enables screenshots and specifies the base directory where they are saved.

```sh
testcafe all tests/sample-fixture.js -s screenshots
```

See [Screenshots](common-concepts/screenshots-and-videos.md#screenshots) for details.

*Related configuration file property*: [screenshotPath](configuration-file.md#screenshotpath).

### -S, --screenshots-on-fails

Takes a screenshot whenever a test fails. Screenshots are saved to the directory specified in the [-s (--screenshots)](#-s-path---screenshots-path) option.

For example, the following command runs tests from the `sample-fixture.js` file in all browsers, takes screenshots if tests fail, and saves the screenshots to the `screenshots` directory:

```sh
testcafe all tests/sample-fixture.js -S -s screenshots
```

*Related configuration file property*: [takeScreenshotsOnFails](configuration-file.md#takescreenshotsonfails).

### -p \<pattern\>, --screenshot-path-pattern \<pattern\>

Specifies a custom pattern to compose screenshot files' relative path and name.

```sh
testcafe all tests/sample-fixture.js -s screenshots -p '${DATE}_${TIME}/test-${TEST_INDEX}/${USERAGENT}/${FILE_INDEX}.png'
```

See [Path Pattern Placeholders](common-concepts/screenshots-and-videos.md#path-pattern-placeholders) for information about the available placeholders.

In Windows `cmd.exe` shell, enclose the pattern in double quotes if it contains spaces:

```sh
testcafe all tests/sample-fixture.js -s screenshots -p "${DATE} ${TIME}/test ${TEST_INDEX}/${USERAGENT}/${FILE_INDEX}.png"
```

> Use the [-s (--screenshots)](#-s-path---screenshots-path) flag to enable screenshots.

*Related configuration file property*: [screenshotPathPattern](configuration-file.md#screenshotpathpattern).

### --video \<basePath\>

Enables TestCafe to record videos of test runs and specifies the base directory to save these videos.

```sh
testcafe chrome test.js --video reports/screen-captures
```

See [Record Videos](common-concepts/screenshots-and-videos.md#record-videos) for details.

*Related configuration file property*: [videoPath](configuration-file.md#videopath).

### --video-options \<option=value\[,option2=value2,...\]\>

Specifies options that define how TestCafe records videos of test runs.

```sh
testcafe chrome test.js --video videos --video-options singleFile=true,failedOnly=true
```

See [Basic Video Options](common-concepts/screenshots-and-videos.md#basic-video-options) for details.

> Use the [--video](#--video-basepath) flag to enable video recording.

*Related configuration file property*: [videoOptions](configuration-file.md#videooptions).

### --video-encoding-options \<option=value\[,option2=value2,...\]\>

Specifies video encoding options.

```sh
testcafe chrome test.js --video videos --video-encoding-options r=20,aspect=4:3
```

See [Video Encoding Options](common-concepts/screenshots-and-videos.md#video-encoding-options) for details.

> Use the [--video](#--video-basepath) flag to enable video recording.

*Related configuration file property*: [videoEncodingOptions](configuration-file.md#videoencodingoptions).

### -q, --quarantine-mode

Enables the [quarantine mode](programming-interface/runner.md#quarantine-mode) for tests that fail.

```sh
testcafe all tests/sample-fixture.js -q
```

*Related configuration file property*: [quarantineMode](configuration-file.md#quarantinemode).

### -d, --debug-mode

Specify this option to run tests in the debugging mode. In this mode, test execution is paused before the first action or assertion allowing you to invoke the developer tools and debug.

The footer displays a status bar in which you can resume test execution or skip to the next action or assertion.

![Debugging status bar](../../images/debugging/client-debugging-footer.png)

> If the test you run in the debugging mode contains a [test hook](../test-api/test-code-structure.md#test-hooks),
> it is paused within this hook before the first action.

You can also use the **Unlock page** switch in the footer to unlock the tested page and interact with its elements.

*Related configuration file property*: [debugMode](configuration-file.md#debugmode).

### --debug-on-fail

Specifies whether to automatically enter the [debug mode](#-d---debug-mode) when a test fails.

```sh
testcafe chrome tests/sample-fixture.js --debug-on-fail
```

If this option is enabled, TestCafe pauses the test when it fails. This allows you to view the tested page and determine the cause of the fail.

When you are done, click the **Finish** button in the footer to end test execution.

*Related configuration file property*: [debugOnFail](configuration-file.md#debugonfail).

### -e, --skip-js-errors

When a JavaScript error occurs on a tested web page, TestCafe stops test execution and posts an error message and a stack trace to a report. To ignore JavaScript errors, use the `-e`(`--skip-js-errors`) option.

For example, the following command runs tests from the specified file and forces TestCafe to ignore JavaScript errors:

```sh
testcafe ie tests/sample-fixture.js -e
```

*Related configuration file property*: [skipJsErrors](configuration-file.md#skipjserrors).

### -u, --skip-uncaught-errors

When an uncaught error or unhandled promise rejection occurs on the server during test execution, TestCafe stops the test and posts an error message to a report.

To ignore these errors, use the `-u`(`--skip-uncaught-errors`) option.

For example, the following command runs tests from the specified file and forces TestCafe to ignore uncaught errors and unhandled promise rejections:

```sh
testcafe ie tests/sample-fixture.js -u
```

*Related configuration file property*: [skipUncaughtErrors](configuration-file.md#skipuncaughterrors).

### -t \<name\>, --test \<name\>

TestCafe runs a test with the specified name.

For example, the following command runs only the `"Click a label"` test from the `sample-fixture.js` file:

```sh
testcafe ie tests/sample-fixture.js -t "Click a label"
```

*Related configuration file property*: [filter.test](configuration-file.md#filtertest).

### -T \<pattern\>, --test-grep \<pattern\>

TestCafe runs tests whose names match the specified `grep` pattern.

For example, the following command runs tests whose names match `Click.*`. These can be `Click a label`, `Click a button`, etc.

```sh
testcafe ie my-tests -T "Click.*"
```

*Related configuration file property*: [filter.testGrep](configuration-file.md#filtertestgrep).

### -f \<name\>, --fixture \<name\>

TestCafe runs a fixture with the specified name.

```sh
testcafe ie my-tests -f "Sample fixture"
```

*Related configuration file property*: [filter.fixture](configuration-file.md#filterfixture).

### -F \<pattern\>, --fixture-grep \<pattern\>

TestCafe runs fixtures whose names match the specified `grep` pattern.

For example, the following command runs fixtures whose names match `Page.*`. These can be `Page1`, `Page2`, etc.

```sh
testcafe ie my-tests -F "Page.*"
```

*Related configuration file property*: [filter.fixtureGrep](configuration-file.md#filterfixturegrep).

### --test-meta \<key=value\[,key2=value2,...\]\>

TestCafe runs tests whose [metadata](../test-api/test-code-structure.md#specifying-testing-metadata) [matches](https://lodash.com/docs/#isMatch) the specified key-value pair.

For example, the following command runs tests whose metadata's `device` property is set to `mobile`, and `env` property is set to `production`.

```sh
testcafe chrome my-tests --test-meta device=mobile,env=production
```

*Related configuration file property*: [filter.testMeta](configuration-file.md#filtertestmeta).

### --fixture-meta \<key=value\[,key2=value2,...\]\>

TestCafe runs tests whose fixture's [metadata](../test-api/test-code-structure.md#specifying-testing-metadata) [matches](https://lodash.com/docs/#isMatch) the specified key-value pair.

For example, the following command runs tests whose fixture's metadata has the `device` property set to the `mobile` value and the `env` property set to the `production` value.

```sh
testcafe chrome my-tests --fixture-meta device=mobile,env=production
```

*Related configuration file property*: [filter.fixtureMeta](configuration-file.md#filterfixturemeta).

### -a \<command\>, --app \<command\>

Executes the specified shell command before running tests. Use it to set up the application you are going to test.

An application is automatically terminated after testing is finished.

```sh
testcafe chrome my-tests --app "node server.js"
```

> TestCafe adds `node_modules/.bin` to `PATH` so that you can use the binaries the locally installed dependencies provide without prefixes.

Use the [--app-init-delay](#--app-init-delay-ms) option to specify the amount of time allowed for this command to initialize the tested application.

*Related configuration file property*: [appCommand](configuration-file.md#appcommand).

### --app-init-delay \<ms\>

Specifies the time (in milliseconds) allowed for an application launched using the [--app](#-a-command---app-command) option to initialize.

TestCafe waits the specified time before it starts running tests.

**Default value**: `1000`

```sh
testcafe chrome my-tests --app "node server.js" --app-init-delay 4000
```

*Related configuration file property*: [appInitDelay](configuration-file.md#appinitdelay).

### -c \<n\>, --concurrency \<n\>

Specifies that tests should run concurrently.

TestCafe opens `n` instances of the same browser and creates a pool of browser instances.
Tests are run concurrently against this pool, that is, each test is run in the first free instance.

See [Concurrent Test Execution](common-concepts/concurrent-test-execution.md) for more information about concurrent test execution.

The following example shows how to run tests in three Chrome instances:

```sh
testcafe -c 3 chrome tests/sample-fixture.js
```

*Related configuration file property*: [concurrency](configuration-file.md#concurrency).

### --selector-timeout \<ms\>

Specifies the time (in milliseconds) within which [selectors](../test-api/selecting-page-elements/selectors/README.md) attempt to obtain a node to be returned. See [Selector Timeout](../test-api/selecting-page-elements/selectors/using-selectors.md#selector-timeout).

**Default value**: `10000`

```sh
testcafe ie my-tests --selector-timeout 500000
```

*Related configuration file property*: [selectorTimeout](configuration-file.md#selectortimeout).

### --assertion-timeout \<ms\>

Specifies the time (in milliseconds) TestCafe attempts to successfully execute an [assertion](../test-api/assertions/README.md)
if a [selector property](../test-api/selecting-page-elements/selectors/using-selectors.md#define-assertion-actual-value)
or a [client function](../test-api/obtaining-data-from-the-client/README.md) was passed as an actual value.
See [Smart Assertion Query Mechanism](../test-api/assertions/README.md#smart-assertion-query-mechanism).

**Default value**: `3000`

```sh
testcafe ie my-tests --assertion-timeout 10000
```

*Related configuration file property*: [assertionTimeout](configuration-file.md#assertiontimeout).

### --page-load-timeout \<ms\>

Specifies the time (in milliseconds) passed after the `DOMContentLoaded` event, within which TestCafe waits for the `window.load` event to fire.

After the timeout passes or the `window.load` event is raised (whichever happens first), TestCafe starts the test.

> Note that the `DOMContentLoaded` event is raised after the HTML document is loaded and parsed, while `window.load` is raised after all stylesheets, images and subframes are loaded. That is why `window.load` is fired after the `DOMContentLoaded` event with a certain delay.

**Default value**: `3000`

You can set the page load timeout to `0` to skip waiting for the `window.load` event.

```sh
testcafe ie my-tests --page-load-timeout 0
```

*Related configuration file property*: [pageLoadTimeout](configuration-file.md#pageloadtimeout).

### --speed \<factor\>

Specifies the test execution speed.

Tests are run at the maximum speed by default. You can use this option
to slow the test down.

`factor` should be a number between `1` (the fastest) and `0.01` (the slowest).

```sh
testcafe chrome my-tests --speed 0.1
```

If the speed is also specified for an [individual action](../test-api/actions/action-options.md#basic-action-options), the action's speed setting overrides the test speed.

**Default value**: `1`

*Related configuration file property*: [speed](configuration-file.md#speed).

### --ports \<port1,port2\>

Specifies custom port numbers TestCafe uses to perform testing. The number range is [0-65535].

TestCafe automatically selects ports if ports are not specified.

```sh
testcafe chrome my-tests --ports 12345,54321
```

*Related configuration file properties*: [port1, port2](configuration-file.md#port1-port2).

### --hostname \<name\>

Specifies your computer's hostname. It is used when running tests in [remote browsers](#remote-browsers).

If the hostname is not specified, TestCafe uses the operating system's hostname or the current machine's network IP address.

```sh
testcafe chrome my-tests --hostname host.mycorp.com
```

*Related configuration file property*: [hostname](configuration-file.md#hostname).

### --proxy \<host\>

Specifies the proxy server used in your local network to access the Internet.

```sh
testcafe chrome my-tests/**/*.js --proxy proxy.corp.mycompany.com
```

```sh
testcafe chrome my-tests/**/*.js --proxy 172.0.10.10:8080
```

You can also specify authentication credentials with the proxy host.

```js
testcafe chrome my-tests/**/*.js --proxy username:password@proxy.mycorp.com
```

*Related configuration file property*: [proxy](configuration-file.md#proxy).

### --proxy-bypass \<rules\>

Requires that TestCafe bypasses the proxy server to access the specified resources.

When you access the Internet through a proxy server specified using the [--proxy](#--proxy-host) option, you may still need some local or external resources to be accessed directly. In this instance, provide their URLs to the `--proxy-bypass` option.

The `rules` parameter takes a comma-separated list (without spaces) of URLs that require direct access. You can replace parts of the URL with the `*` wildcard that matches any number of characters. Wildcards at the beginning and end of the rules can be omitted (`*.mycompany.com` and `.mycompany.com` have the same effect).

The following example uses the proxy server at `proxy.corp.mycompany.com` with the `localhost:8080` address accessed directly:

```sh
testcafe chrome my-tests/**/*.js --proxy proxy.corp.mycompany.com --proxy-bypass localhost:8080
```

In the example below, two resources are accessed by bypassing the proxy: `localhost:8080` and `internal-resource.corp.mycompany.com`.

```sh
testcafe chrome my-tests/**/*.js --proxy proxy.corp.mycompany.com --proxy-bypass localhost:8080,internal-resource.corp.mycompany.com
```

The `*.mycompany.com` value means that all URLs in the `mycompany.com` subdomains are accessed directly.

```sh
testcafe chrome my-tests/**/*.js --proxy proxy.corp.mycompany.com --proxy-bypass *.mycompany.com
```

*Related configuration file property*: [proxyBypass](configuration-file.md#proxybypass).

### --ssl \<options\>

Provides options that allow you to establish an HTTPS connection between the client browser and the TestCafe server.

The `options` parameter contains options required to initialize
[a Node.js HTTPS server](https://nodejs.org/api/https.html#https_https_createserver_options_requestlistener).
The most commonly used SSL options are described in the [TLS topic](https://nodejs.org/api/tls.html#tls_tls_createsecurecontext_options) in Node.js documentation.
Options are specified in a semicolon-separated string.

```sh
testcafe --ssl pfx=path/to/file.pfx;rejectUnauthorized=true;...
```

Provide the `--ssl` flag if the tested webpage uses browser features that require
secure origin ([Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API), [Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API), [ApplePaySession](https://developer.apple.com/documentation/apple_pay_on_the_web/applepaysession), [SubtleCrypto](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto), etc).
See [Test HTTPS and HTTP/2 Websites](common-concepts/test-https-and-http2-websites.md) for more information.

*Related configuration file property*: [ssl](configuration-file.md#ssl).

### -L, --live

Enables live mode. In this mode, TestCafe watches for changes you make in the test files and all files referenced in them (like page objects or helper modules). These changes immediately restart the tests so that you can see the effect.

See [Live Mode](common-concepts/live-mode.md) for more information.

### --dev

Enables mechanisms to log and diagnose errors. You should enable this option if you are going to contact TestCafe Support to report an issue.

```sh
testcafe chrome my-tests --dev
```

*Related configuration file property*: [developmentMode](configuration-file.md#developmentmode).

### --qr-code

Outputs a QR-code that represents URLs used to connect the [remote browsers](#remote-browsers).

```sh
testcafe remote my-tests --qr-code
```

*Related configuration file property*: [qrCode](configuration-file.md#qrcode).

### --sf, --stop-on-first-fail

Stops an entire test run if any test fails. Use this option when you want to fix failed tests individually and do not need a report on all the failures.

```sh
testcafe chrome my-tests --sf
```

*Related configuration file property*: [stopOnFirstFail](configuration-file.md#stoponfirstfail).

### --ts-config-path \<path\>

Enables TestCafe to use a custom [TypeScript configuration file](../test-api/typescript-support.md#customize-compiler-options) and specifies its location.

```sh
testcafe chrome my-tests --ts-config-path /Users/s.johnson/testcafe/tsconfig.json
```

You can specify an absolute or relative path. Relative paths resolve from the current directory (the directory from which you run TestCafe).

*Related configuration file property*: [tsConfigPath](configuration-file.md#tsconfigpath).

### --color

Enables colors in the command line.

*Related configuration file property*: [color](configuration-file.md#color).

### --no-color

Disables colors in the command line.

*Related configuration file property*: [noColor](configuration-file.md#nocolor).