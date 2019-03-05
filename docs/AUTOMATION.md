# Automation

## Table of contents
1. [Intro](#intro)
1. [Setup Appium on a local machine](#setup-appium-on-a-local-machine)
1. [Writing tests](#writing tests)
1. [Running tests on a local machine](#running-tests-on-a-local-machine)
1. [Running tests on the Sauce Labs Real Device Cloud](#running-tests-on-the-sauce-labs-real-device-cloud)
    1. [The setup](#the-setup)
    1. [Running Android](#running-android)
    1. [Running iOS](#running-ios)
1. [FAQ](#faq)

# Intro
Tests are run with:

* [webdriver.io](http://webdriver.io/): This is the testrunner that will orchestrate the tests between different devices
* [Jasmine](https://jasmine.github.io/): The testingframework that is used to write the tests
* [Appium](http://appium.readthedocs.io/en/latest/README/): The cross-platfrom tool for native, hybrid and mobile web apps.

To setup the test environment the following needs to be installed:

- [appium-doctor](https://github.com/appium/appium-doctor) with `npm install -g appium-doctor`
- [Appium](https://github.com/appium/appium) with `npm install -g appium`
- [appium-desktop](https://github.com/appium/appium-desktop). This one needs to be downloaded from [here](https://github.com/appium/appium-desktop/releases) and pick the latest stable releases

For installation instructions check [here](#setup-appium-on-a-local-machine).

By default the following emulators and simulators are used:

- Pixel One
- iPhone X

> **Please check [tests/e2e/config/wdio.android.local.conf.js](../tests/e2e/config/wdio.android.local.conf.js) and  [tests/e2e/config/wdio.ios.local.conf.js](../tests/e2e/config/wdio.ios.local.conf.js) for the correct names and OS versions of the emulators / simulators. They need to be equal for test execution.**

## Setup Appium on a local machine
Please check [this](./APPIUM.md) document for more info about how to setup Appium on a local machine.

## Writing tests
Please check the current tests in [this](../tests/e2e/spec/)-folder to see how the tests are being created. More information about the methods that WebdriverIO supports check [this](https://webdriver.io/docs/api.html) site.

> Try to prevent using the default actions of WebdriverIO, like `.click()`, `.isDisplayed()` and so on in the specfile itself. Add them in the [screenObjects](tests/e2e/screenObjects/) because there are some differences in how iOS and Android handle their UI-hierarchy.

### Locator strategy
For selecting elements we use the accessibillityLabels. These labels can be used for both Android and iOS to select elements with 1 script. To make this possible the [`testProperties`](../src/js/config/translations/en.json)-methods has been made.
This method will hold all the logic to add accessibilityLabels on each needed component. 

We try to use the text labels from the `en.json` as much as possible so we can easily link a change in the translation to a change in the selector without breaking the automation. 


## Running tests on a local machine
> Because local automation will be ran against a build connected with the packager/metro builder, adjusting code during test execution is not wise. It will reflect immediately in the app which may cause breaking the tests.

> Android and iOS tests can't be run in parallel on a local machine with this setup, you need to create a local grid to be able to do that.

To run a complete testset use the following commands

- Android: `npm run android.local`
- iOS: `npm run ios.local`

When all tests have been executed the following will be  shown in the console

```bash
[iPhone X MAC 11.4 #0-0] Spec: /Users/wswebcreation/Git/sample-app-ios/tests/e2e/spec/cart.content.spec.js
[iPhone X MAC 11.4 #0-0] Running: iPhone X on MAC 11.4 executing /Users/wswebcreation/Git/sample-app-ios/ios/build/Build/Products/Debug-iphonesimulator/SwagLabsMobileApp.app
[iPhone X MAC 11.4 #0-0]
[iPhone X MAC 11.4 #0-0] Cart Content Page
[iPhone X MAC 11.4 #0-0]    ✓ should show 2 items in the cart
[iPhone X MAC 11.4 #0-0]    ✓ should show the items page if continue shopping is selected
[iPhone X MAC 11.4 #0-0]    ✓ should update the cart if an item is removed
[iPhone X MAC 11.4 #0-0]    ✓ should open the checkout page one page if checkout is clicked
[iPhone X MAC 11.4 #0-0]
[iPhone X MAC 11.4 #0-0] 4 passing (50.8s)
------------------------------------------------------------------
[iPhone X MAC 11.4 #0-1] Spec: /Users/wswebcreation/Git/sample-app-ios/tests/e2e/spec/checkout.complete.spec.js
[iPhone X MAC 11.4 #0-1] Running: iPhone X on MAC 11.4 executing /Users/wswebcreation/Git/sample-app-ios/ios/build/Build/Products/Debug-iphonesimulator/SwagLabsMobileApp.app
[iPhone X MAC 11.4 #0-1]
[iPhone X MAC 11.4 #0-1] Checkout Complete
[iPhone X MAC 11.4 #0-1]    ✓ should be able to finish the checkout by going back to the inventory list
[iPhone X MAC 11.4 #0-1]
[iPhone X MAC 11.4 #0-1] 1 passing (16.9s)
------------------------------------------------------------------
[iPhone X MAC 11.4 #0-2] Spec: /Users/wswebcreation/Git/sample-app-ios/tests/e2e/spec/checkout.page.one.spec.js
[iPhone X MAC 11.4 #0-2] Running: iPhone X on MAC 11.4 executing /Users/wswebcreation/Git/sample-app-ios/ios/build/Build/Products/Debug-iphonesimulator/SwagLabsMobileApp.app
[iPhone X MAC 11.4 #0-2]
[iPhone X MAC 11.4 #0-2] Checkout: Your info
[iPhone X MAC 11.4 #0-2]    ✓ should be able to submit my personal info and proceed the checkout
[iPhone X MAC 11.4 #0-2]    ✓ should be able to cancel shopping and go to the items overview page
[iPhone X MAC 11.4 #0-2]    ✓ should show an error when the first name has not been entered
[iPhone X MAC 11.4 #0-2]    ✓ should show an error when the last name has not been entered
[iPhone X MAC 11.4 #0-2]    ✓ should show an error when the postal code has not been entered
[iPhone X MAC 11.4 #0-2]
[iPhone X MAC 11.4 #0-2] 5 passing (1m 24.7s)
------------------------------------------------------------------
[iPhone X MAC 11.4 #0-3] Spec: /Users/wswebcreation/Git/sample-app-ios/tests/e2e/spec/checkout.page.two.spec.js
[iPhone X MAC 11.4 #0-3] Running: iPhone X on MAC 11.4 executing /Users/wswebcreation/Git/sample-app-ios/ios/build/Build/Products/Debug-iphonesimulator/SwagLabsMobileApp.app
[iPhone X MAC 11.4 #0-3]
[iPhone X MAC 11.4 #0-3] Checkout: Overview
[iPhone X MAC 11.4 #0-3]    ✓ should show the correct selected item in the overview
[iPhone X MAC 11.4 #0-3]    ✓ should be able to finish the checkout
[iPhone X MAC 11.4 #0-3]
[iPhone X MAC 11.4 #0-3] 2 passing (34.4s)
------------------------------------------------------------------
[iPhone X MAC 11.4 #0-4] Spec: /Users/wswebcreation/Git/sample-app-ios/tests/e2e/spec/inventory.item.spec.js
[iPhone X MAC 11.4 #0-4] Running: iPhone X on MAC 11.4 executing /Users/wswebcreation/Git/sample-app-ios/ios/build/Build/Products/Debug-iphonesimulator/SwagLabsMobileApp.app
[iPhone X MAC 11.4 #0-4]
[iPhone X MAC 11.4 #0-4] Inventory Item Page
[iPhone X MAC 11.4 #0-4]    ✓ should show the details of the selected swag
[iPhone X MAC 11.4 #0-4]    ✓ should be able to add a swag item to the cart from the details page
[iPhone X MAC 11.4 #0-4]    ✓ should be able to remove a swag item from the cart from the details page added in the inventory page
[iPhone X MAC 11.4 #0-4]    ✓ should be able to remove a swag item from the cart from the details page
[iPhone X MAC 11.4 #0-4]    ✓ should be able to get back from the swag details page through the back button
[iPhone X MAC 11.4 #0-4]
[iPhone X MAC 11.4 #0-4] 5 passing (56.9s)
------------------------------------------------------------------
[iPhone X MAC 11.4 #0-5] Spec: /Users/wswebcreation/Git/sample-app-ios/tests/e2e/spec/inventory.list.spec.js
[iPhone X MAC 11.4 #0-5] Running: iPhone X on MAC 11.4 executing /Users/wswebcreation/Git/sample-app-ios/ios/build/Build/Products/Debug-iphonesimulator/SwagLabsMobileApp.app
[iPhone X MAC 11.4 #0-5]
[iPhone X MAC 11.4 #0-5] Inventory List Page
[iPhone X MAC 11.4 #0-5]    ✓ should contain swag
[iPhone X MAC 11.4 #0-5]    ✓ should be able to select a swag item and open the details page
[iPhone X MAC 11.4 #0-5]    ✓ should be able to sort the items
[iPhone X MAC 11.4 #0-5]    ✓ should be able to open and close the selecting modal
[iPhone X MAC 11.4 #0-5]    ✓ should be able to add swag to the cart
[iPhone X MAC 11.4 #0-5]    ✓ should be able to remove swag from the cart
[iPhone X MAC 11.4 #0-5]
[iPhone X MAC 11.4 #0-5] 6 passing (1m 8.2s)
------------------------------------------------------------------
[iPhone X MAC 11.4 #0-6] Spec: /Users/wswebcreation/Git/sample-app-ios/tests/e2e/spec/login.spec.js
[iPhone X MAC 11.4 #0-6] Running: iPhone X on MAC 11.4 executing /Users/wswebcreation/Git/sample-app-ios/ios/build/Build/Products/Debug-iphonesimulator/SwagLabsMobileApp.app
[iPhone X MAC 11.4 #0-6]
[iPhone X MAC 11.4 #0-6] Login
[iPhone X MAC 11.4 #0-6]    ✓ should be able to login with a standard user
[iPhone X MAC 11.4 #0-6]    ✓ should not be able to login with a locked user
[iPhone X MAC 11.4 #0-6]    ✓ should show an error when no username is provided
[iPhone X MAC 11.4 #0-6]    ✓ should show an error when no password is provided
[iPhone X MAC 11.4 #0-6]    ✓ should show an error when no match is found
[iPhone X MAC 11.4 #0-6]
[iPhone X MAC 11.4 #0-6] 5 passing (43s)
------------------------------------------------------------------
[iPhone X MAC 11.4 #0-7] Spec: /Users/wswebcreation/Git/sample-app-ios/tests/e2e/spec/menu.spec.js
[iPhone X MAC 11.4 #0-7] Running: iPhone X on MAC 11.4 executing /Users/wswebcreation/Git/sample-app-ios/ios/build/Build/Products/Debug-iphonesimulator/SwagLabsMobileApp.app
[iPhone X MAC 11.4 #0-7]
[iPhone X MAC 11.4 #0-7] Menu
[iPhone X MAC 11.4 #0-7]    ✓ should be able to be opened and closed
[iPhone X MAC 11.4 #0-7]    ✓ should be able to bring me to the all items page
[iPhone X MAC 11.4 #0-7]    ✓ should be able reset the app state
[iPhone X MAC 11.4 #0-7]    ✓ should be able to logout
[iPhone X MAC 11.4 #0-7]    ✓ should be able to open the about page and go to a browser
[iPhone X MAC 11.4 #0-7]
[iPhone X MAC 11.4 #0-7] 5 passing (59.9s)

Test Suites:     8 passed, 8 total (100% completed)
Time:            🕙  460.00s
```

## Running tests on the Sauce Labs Real Device Cloud
This project setup also has a setup for running the tests on the Real Device Cloud of Sauce LAbs. To be able to do this there first needs to be a build of the app that can run on real devices, see [Building the app](./BUILDING.md) for more information on how to do that.

### The setup
This setup uses the WebdriverIO basics from the [`wdio.shared.conf.js`](tests/e2e/config/wdio.shared.conf.js) where all the basics (like the testframework and so on) are defined. On top of that a [`wdio.rdc.shared.js`](tests/e2e/config/wdio.rdc.shared.js) is created that holds the RDC specific configuration for both iOS and Android.

Depending on where the tests need to be run (US/EU-cloud), the correct url of the cloud need to be selected. Change this piece of code in the [`wdio.rdc.shared.js`](tests/e2e/config/wdio.rdc.shared.js)-file to select or the US or the EU cloud.

```js
// For using the EU RDC cloud, just remove the comments and comment the US url
config.hostname = 'eu1.appium.testobject.com';
// For using the US RDC cloud
// config.hostname = 'us1.appium.testobject.com';
```

### Running Android
To be able to run the Android tests on the Sauce Labs cloud please check the [`wdio.android.rdc.conf.js`](tests/e2e/config/wdio.android.rdc.conf.js)-file. There the configuration for 1 Android device can be found.

Change the `deviceName` to get the right device you want to use and or add the `platformVersion` to get a specific Android version. More information about setting up the correct device can be found on [Appium Testing on Real Devices](https://wiki.saucelabs.com/display/DOCS/Appium+Testing+on+Real+Devices) or on the Sauce Labs cloud under `/appium/basic/instructions`

> **NOTE:** Make sure the `testobject_api_key` of the correct Android project, that has been created in the Sauce Labs Real Device Cloud, has been added to the environment variables and is called `SAUCE_RDC_EU_ACCESS_KEY_ANDROID`.    

Running the test on the Sauce Labs Real Device Cloud can be done by running the following command:

```shell
$ npm run android.rdc
```

### Running iOS

> **NOTE:** iOS tests for this app can't be run on the SL cloud because we can't create a correct build of the iOS app to run on real device, see [Building iOS](./BUILDING.md#building-ios) for more information.

## FAQ
### `An unknown server-side error occurred while processing the command` while sending text to an iOS simulator
Check [this](https://gist.github.com/wswebcreation/6ac27598718eb001cd208dd691db2a84) article on how to fix it.

### Tests are extremely slow / fail very often
It could be that the debugger is still on. Run tests without the debugger

### Appium ChromeDriver error
This app also uses a Webview. To be able to automate the Webview on Android you need to have the right version of ChromeDriver installed together with Appium.
If you don't have the proper version of ChromeDriver on your machine (Appium will by default install the latest version on your machine during the installation of Appium) you might get an error like this.

```shell
[Pixel_8.1 Android 8.1 #0-0] unknown error: An unknown server-side error occurred while processing the command. Original error: No Chromedriver found that can automate Chrome '61.0.3163'. See https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/web/chromedriver.md for more details.
```

Please following the instructions [here](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/web/chromedriver.md) to know how to solve this issue.
