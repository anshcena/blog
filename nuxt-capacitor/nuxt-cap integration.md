**Capacitor** is an open source native runtime for building cross-platform mobile and Progressive Web Apps, with JavaScript, HTML, and CSS. 

For this first we will need a nuxt.js application where we will integrate our capacitor. Nuxt.js can be seen from the [docs here](https://nuxtjs.org/docs/2.x/get-started/installation). Lets start with adding Capacitor to an existing Nuxt app.

[Capacitor](https://capacitorjs.com/docs/getting-started) allows to build our app with modern web components that is cross-platform that is  Android, iOS, PWA and electron.

To add Capacitor to your web app, run the following commands:

```
cd my-nuxt-app
npm install @capacitor/core @capacitor/cli
```

Lets initialize Capacitor with our app's information. This command below will generate '**capacitor.config.js'** file where we can add app information.

```
 npx cap init
```

Lets see how our capacitor.config.js files look :

```
{
  "appId": "com.example.app",
  "appName": "MyNuxtApp",
  "bundledWebRuntime": false,
  "npmClient": "npm",
  "webDir": "www",
  "plugins": {
    "SplashScreen": {
      "launchShowDuration": 0
    }
  },
  "cordova": {}
}
```

Use the --web-dir flag to set the web assets folder (the default is www)
.
Configuring this **capacitor.config.json** will link our nuxt app to capacitor. We need to change

> "webDir": "www" property to "webDir": "dist"

Also, we can configure our "appId" and "appName". Now our app is ready to be compiled and build.

> yarn run generate 
>
> yarn dev

We have successfully added Capacitor to our Nuxt.js app.

Lets add Platforms :

**[Electron](https://capacitor-community.github.io/electron/#/./getting-started/index)**

Run the command below in your root directory to install the platform for use with the @capacitor/cli.

> npm i @capacitor-community/electron

Lets initiate platform to create electron folder in our nuxt app.

> npx cap add @capacitor-community/electron

So we have successfully added the electron platfrom to our application. Lets open it

> npx cap open @capacitor-community/electron

**[Android & iOS](https://capacitorjs.com/docs/basics/running-your-app)**

> npx cap add android npx cap add ios

Lets copy all of our web code from /dist to our native app directories for mobile platform functionalities. Specific modifications in IDE can made for each.

> npx cap copy

Our code will be updated too with this.

For opening Android application :

> npx cap open android

Make sure to have Android Studio with gradle build setup to generate apk.

For opening iOS application :

> npx cap open ios

**XCode** will be opened and we can run our app.

**[Progressive Web App](https://capacitorjs.com/docs/basics/running-your-app#progressive-web-app)**

Capacitor has a tiny development web server for local testing, but it’s recommended to run your web app using your framework of choice’s server tools.

> npx cap serve

# Generating resources

* Create resources folder in root directory.
* Add icon.png with size - 1024x1024 px Add splash.png with size - 2732x3732 px
* So we have placed icon and splash in resources folder, now we need these in all sizes compatible with our android and ios platforms.
* lets add script to generate it and then sync it to platform.
* Add the following command to scripts in package.json file of nuxt app

> "resources": "cordova-res-generator -p android,ios && node resources/sync.js android"

As we see, we have used cordova-res-generator so lets install this generator by

> yarn add --dev cordova-res-generator

We have successfully add res-generator now we are ready to go but we still have **node resources/sync.js android**


This will sync the resources to android platform using sync.js, so lets add sync.js in resources where we added our icon and splash

```
const fs = require('fs')

const SOURCE_IOS_ICON = 'resources/ios/icon/'
const SOURCE_IOS_SPLASH = 'resources/ios/splash/'

const TARGET_IOS_ICON = 'ios/App/App/Assets.xcassets/AppIcon.appiconset/'
const TARGET_IOS_SPLASH = 'ios/App/App/Assets.xcassets/Splash.imageset/'

const SOURCE_ANDROID_ICON = 'resources/android/icon/'
const SOURCE_ANDROID_SPLASH = 'resources/android/splash/'

const TARGET_ANDROID_ICON = 'android/app/src/main/res/'
const TARGET_ANDROID_SPLASH = 'android/app/src/main/res/'

const IOS_ICONS = [
  { source: 'icon-20.png', target: 'AppIcon-20x20@1x.png' },
  { source: 'icon-20@2x.png', target: 'AppIcon-20x20@2x.png' },
  { source: 'icon-20@2x.png', target: 'AppIcon-20x20@2x-1.png' },
  { source: 'icon-20@3x.png', target: 'AppIcon-20x20@3x.png' },
  { source: 'icon-29.png', target: 'AppIcon-29x29@1x.png' },
  { source: 'icon-29@2x.png', target: 'AppIcon-29x29@2x.png' },
  { source: 'icon-29@2x.png', target: 'AppIcon-29x29@2x-1.png' },
  { source: 'icon-29@3x.png', target: 'AppIcon-29x29@3x.png' },
  { source: 'icon-40.png', target: 'AppIcon-40x40@1x.png' },
  { source: 'icon-40@2x.png', target: 'AppIcon-40x40@2x.png' },
  { source: 'icon-40@2x.png', target: 'AppIcon-40x40@2x-1.png' },
  { source: 'icon-40@3x.png', target: 'AppIcon-40x40@3x.png' },
  { source: 'icon-60@2x.png', target: 'AppIcon-60x60@2x.png' },
  { source: 'icon-60@3x.png', target: 'AppIcon-60x60@3x.png' },
  { source: 'icon-76.png', target: 'AppIcon-76x76@1x.png' },
  { source: 'icon-76@2x.png', target: 'AppIcon-76x76@2x.png' },
  { source: 'icon-83.5@2x.png', target: 'AppIcon-83.5x83.5@2x.png' },
  { source: 'icon-1024.png', target: 'AppIcon-512@2x.png' }
]
const IOS_SPLASHES = [
  { source: 'Default-Portrait@~ipadpro.png', target: 'splash-2732x2732.png' },
  {
    source: 'Default-Portrait@~ipadpro.png',
    target: 'splash-2732x2732-1.png'
  },
  {
    source: 'Default-Portrait@~ipadpro.png',
    target: 'splash-2732x2732-2.png'
  }
]

const ANDROID_ICONS = [
  { source: 'drawable-ldpi-icon.png', target: 'drawable-hdpi-icon.png' },
  { source: 'drawable-mdpi-icon.png', target: 'mipmap-mdpi/ic_launcher.png' },
  {
    source: 'drawable-mdpi-icon.png',
    target: 'mipmap-mdpi/ic_launcher_round.png'
  },
  {
    source: 'drawable-mdpi-icon.png',
    target: 'mipmap-mdpi/ic_launcher_foreground.png'
  },
  { source: 'drawable-hdpi-icon.png', target: 'mipmap-hdpi/ic_launcher.png' },
  {
    source: 'drawable-hdpi-icon.png',
    target: 'mipmap-hdpi/ic_launcher_round.png'
  },
  {
    source: 'drawable-hdpi-icon.png',
    target: 'mipmap-hdpi/ic_launcher_foreground.png'
  },
  {
    source: 'drawable-xhdpi-icon.png',
    target: 'mipmap-xhdpi/ic_launcher.png'
  },
  {
    source: 'drawable-xhdpi-icon.png',
    target: 'mipmap-xhdpi/ic_launcher_round.png'
  },
  {
    source: 'drawable-xhdpi-icon.png',
    target: 'mipmap-xhdpi/ic_launcher_foreground.png'
  },
  {
    source: 'drawable-xxhdpi-icon.png',
    target: 'mipmap-xxhdpi/ic_launcher.png'
  },
  {
    source: 'drawable-xxhdpi-icon.png',
    target: 'mipmap-xxhdpi/ic_launcher_round.png'
  },
  {
    source: 'drawable-xxhdpi-icon.png',
    target: 'mipmap-xxhdpi/ic_launcher_foreground.png'
  },
  {
    source: 'drawable-xxxhdpi-icon.png',
    target: 'mipmap-xxxhdpi/ic_launcher.png'
  },
  {
    source: 'drawable-xxxhdpi-icon.png',
    target: 'mipmap-xxxhdpi/ic_launcher_round.png'
  },
  {
    source: 'drawable-xxxhdpi-icon.png',
    target: 'mipmap-xxxhdpi/ic_launcher_foreground.png'
  }
]
const ANDROID_SPLASHES = [
  { source: 'drawable-land-mdpi-screen.png', target: 'drawable/splash.png' },
  {
    source: 'drawable-land-mdpi-screen.png',
    target: 'drawable-land-mdpi/splash.png'
  },
  {
    source: 'drawable-land-hdpi-screen.png',
    target: 'drawable-land-hdpi/splash.png'
  },
  {
    source: 'drawable-land-xhdpi-screen.png',
    target: 'drawable-land-xhdpi/splash.png'
  },
  {
    source: 'drawable-land-xxhdpi-screen.png',
    target: 'drawable-land-xxhdpi/splash.png'
  },
  {
    source: 'drawable-land-xxxhdpi-screen.png',
    target: 'drawable-land-xxxhdpi/splash.png'
  },
  {
    source: 'drawable-port-mdpi-screen.png',
    target: 'drawable-port-mdpi/splash.png'
  },
  {
    source: 'drawable-port-hdpi-screen.png',
    target: 'drawable-port-hdpi/splash.png'
  },
  {
    source: 'drawable-port-xhdpi-screen.png',
    target: 'drawable-port-xhdpi/splash.png'
  },
  {
    source: 'drawable-port-xxhdpi-screen.png',
    target: 'drawable-port-xxhdpi/splash.png'
  },
  {
    source: 'drawable-port-xxxhdpi-screen.png',
    target: 'drawable-port-xxxhdpi/splash.png'
  }
]

function copyImages(sourcePath, targetPath, images) {
  for (const icon of images) {
    const source = sourcePath + icon.source
    const target = targetPath + icon.target
    fs.copyFile(source, target, (err) => {
      if (err) {
        throw err
      }
      // eslint-disable-next-line no-console
      console.log(`${source} >> ${target}`)
    })
  }
}

if (process.argv.includes('android')) {
  copyImages(SOURCE_ANDROID_ICON, TARGET_ANDROID_ICON, ANDROID_ICONS)
  copyImages(SOURCE_ANDROID_SPLASH, TARGET_ANDROID_SPLASH, ANDROID_SPLASHES)
}

if (process.argv.includes('ios')) {
  copyImages(SOURCE_IOS_ICON, TARGET_IOS_ICON, IOS_ICONS)
  copyImages(SOURCE_IOS_SPLASH, TARGET_IOS_SPLASH, IOS_SPLASHES)
}
```

so our resources will have these 3 :

![Structure resources](https://github.com/anshcena/blog/blob/main/nuxt-capacitor/img/sync%20file.jpg?raw=true "Resources")

We are ready to generate and sync now

> yarn resources

🎉 Resources are generated and sync to platfrom - android 🎉

For **iOS** , we can make change in scripts of package.json as **node/sync.js ios**

*Make sure to put logo in splash screen in middle as well as small for better resource generated results and optimized images.*

#### Packaging our Electron app to executable file

We will be using [electron-builder](https://www.npmjs.com/package/electron-builder) so install using :

> npm i electron-builder

Add script in package.json inside electron root folder.

```
"electron:pack": "npm run build && electron-builder build --dir",
"electron:build-windows": "npm run build && electron-builder build --windows",
"electron:build-mac": "npm run build && electron-builder build --mac"
```
We have added build script as well as for **mac** and **windows** application.

Run Command : 
>yarn electron:build-windows // windows

>yarn electron:build-mac     // mac
