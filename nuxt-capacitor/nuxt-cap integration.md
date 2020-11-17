**Capacitor** is an open source native runtime for building cross-platform mobile and Progressive Web Apps, with JavaScript, HTML, and CSS. 

For this first we will need a **nuxt.js** application where we will integrate our capacitor. Nuxt.js can be seen from the [docs here](https://nuxtjs.org/docs/2.x/get-started/installation). Lets start with adding Capacitor to an existing Nuxt app.

[Capacitor](https://capacitorjs.com/docs/getting-started) allows to build our app with modern web components that is cross-platform that is  Android, iOS, PWA and electron.

#### Add CAPACITOR
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
#### LINK NUXT TO CAPACITOR

Use the --web-dir flag to set the web assets folder (the default is www)
Configuring this **capacitor.config.json** will link our nuxt app to capacitor. We need to change

> "webDir": "www" property to "webDir": "dist"

Also, we can configure our "appId" and "appName". Now our app is ready to be compiled and build.

> yarn run generate 
>
> yarn dev

We have successfully added Capacitor to our Nuxt.js app.

# Lets add Platforms :

### **[Electron](https://capacitor-community.github.io/electron/#/./getting-started/index)**

Run the command below in your root directory to install the platform for use with the **@capacitor/cli**

> npm i @capacitor-community/electron

Lets initiate platform to create electron folder in our nuxt app.

> npx cap add @capacitor-community/electron

So we have successfully added the electron platfrom to our application. Lets open it

> npx cap open @capacitor-community/electron

### **[Android & iOS](https://capacitorjs.com/docs/basics/running-your-app)**

> npx cap add android

> npx cap add ios

Lets copy all of our web code from **/dist** to our native app directories for mobile platform functionalities. *Specific modifications* in IDE can made for each.

> npx cap copy

Our code will be updated too with this.

For opening Android application :

> npx cap open android

Make sure to have Android Studio with gradle build setup to generate apk.

For opening iOS application :

> npx cap open ios

**XCode** will be opened and we can run our app.

### **[Progressive Web App](https://capacitorjs.com/docs/basics/running-your-app#progressive-web-app)**

Capacitor has a tiny development web server for local testing, but it’s recommended to run your web app using your framework of choice’s server tools.

> npx cap serve

# Generating resources

* Create *resources* folder in root directory.
* Add *icon.png* with size - 1024x1024 px Add *splash.png* with size - 2732x3732 px
* So we have placed icon and splash in resources folder, now we need these in all sizes compatible with our android and ios platforms.
* Lets add script to generate it and then *sync* it to platform.
* Add the following command to scripts in **package.json** file of nuxt app

> "resources": "cordova-res-generator -p android,ios && node resources/sync.js android"

As we see, we have used **[cordova-res-generator](https://www.npmjs.com/package/cordova-res-generator#installation)** so lets install this generator by

> yarn add --dev cordova-res-generator

We have successfully add res-generator now we are ready to go but we still have **node resources/sync.js android**

This will sync the resources to android platform using sync.js, so lets add sync.js in resources where we have added our icon and splash

> [Sync.js](https://github.com/anshcena/blog/blob/main/nuxt-capacitor/sync.js)

Copy the code from above mentioned [Sync.js](https://github.com/anshcena/blog/blob/main/nuxt-capacitor/sync.js) to generate sync.js inside resources.

so our resources will have these 3 :

![Structure resources](https://github.com/anshcena/blog/blob/main/nuxt-capacitor/img/sync%20file.jpg?raw=true "Resources")

We are ready to generate and sync now

> yarn resources

🎉 Resources are generated and sync to platfrom - android 🎉

For **iOS** , we can make change in scripts of package.json as **node/sync.js ios**

*Make sure to put logo in splash screen in middle as well as small for better resource generated results and optimized images.*

[Reference - MexsonFernandes/nuxt-capacitor-app](https://github.com/MexsonFernandes/nuxt-capacitor-app#create-resources)

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

:fire: We have generated our execcutable files with this script :fire:

*App name and package can be changed from build inside package.json of electron i.e electron/package.json *
