# Electron webview

This is a simple Electron application to create a webview.

![Electron webview](screenshot.png)

This Electron webview application needs just these files:

- `package.json` - Points to the app's main file and lists its details and dependencies.
- `main.js` - Starts the app and creates a browser window to render website. This is the app's **main process**.
- `index.html` - A web page to render. This is the app's renderer process.
- `assets/` - Assets for the project (style, scripts, icons)

## Usage

To run this repository you'll need [Node.js](https://nodejs.org/en/download/) (which comes with [npm](http://npmjs.com)) installed on your computer. From your command line:

```bash
# Install dependencies
$ npm install
# Run the app
$ npm start
```

### Configuration

You just need to change the `src` attribute of the `webview` in `index.html` file.

### Options

In `main.js` file:

- You can show the developer tools
  `mainWindow.openDevTools();`
- You can hide the title bar of the app

### Window dimensions and responsive

This webview is responsive and supports live dimensions change of the window.
This webview remembers the window size you have before quitting the app to use it when you open it again.

If you want to change the window dimensions at the start, change `width` and `height` in `main.js` file:

```js
mainWindow = new BrowserWindow((data && data.bounds) ? data.bounds : { width: 1024, height: 768, icon: path.join(__dirname, 'assets/icons/png/64x64.png')});
```

### Menu and keyboard shortcuts

This webview integrates an Electron menu. It will also make standard keyboard shortcuts, like copy and paste, work on MacOS.

### Topbar (home button)

A topbar to show a home button to come back to your app if your website has external links is included.

You can activate/deactivate this topbar (activate by default).

#### Deactivation

##### In `index.html`:

- Comment:
    ```html
    <div id="controls">
        <button id="home" title="Go Home">Home</button>
    </div>
    ```

##### In `assets/css/style.css`:

- Uncomment:
    ```css
    #webview {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: inline-flex !important;
    }
    ```

- Comment:
    ```css
    #controls {
      padding: 5px;
      border-bottom: solid 1px #ccc;
      background-color: #f8f8f8;
      display: -webkit-flex;
      -webit-flex-direction: column;
    }
    ```

##### In `assets/js/scripts.js`:

- Comment:
    ```js
    document.querySelector('#home').onclick = function() {
        var attribute = document.getElementById('webview');
        var home = attribute.getAttribute("data-home");
        navigateTo(home);
      };

      function navigateTo(url) {
      document.querySelector('webview').src = url;
    }
    ```

- Comment:
  ```js
  var controls = document.querySelector('#controls');
  var controlsHeight = controls.offsetHeight;
  ```

- Uncomment:
  ```js
  var controlsHeight = 0;
  ```

#### Activation

- Do the opposite of what you did in the activation chapter above.
- Set the homepage of your app in the `data-home` attribute of `webview` in `index.html` file.

## Application

To create a MacOS, Windows and Linux executable with an app icon, follow these instructions.

### Electron app icon

For this we need a 1024x1024 png-icon, a .icns for macs and a .ico for windows. For Linux we only need the pngs.

- Create your app icon

- Go to [iConvert Icons](https://iconverticons.com/online/) and upload the PNG and the service will take care of creating the other icon-formats.

- Add your files in `assets/icons`: put the `.icns` file into the `mac` folder, the pngs in the png folder and the `.ico` file in the win folder.<br>Rename the `.icns` and `.ico` files in `icon`.

#### Mac

On Mac, the `.icns` icon converted with iConvert Icons doesn't work.

I recommend using [Image2icon](http://www.img2icnsapp.com), an awesome free app to create and personalize icons from your pictures, available on the Mac Store.

The `.icns` icon converted with Image2icon perfectly works on Mac.

### Electron packager

"[Electron Packager](https://github.com/electron-userland/electron-packager) is a command line tool and Node.js library that bundles Electron-based application source code with a renamed Electron executable and supporting files into folders ready for distribution."

#### Install Electron packager

```bash
# for use in npm scripts
$ npm install electron-packager --save-dev

# for use from cli
$ npm install electron-packager -g
```

#### Application name

Change the `productName` in `package.json`

#### Build MacOS, Windows and Linux package from the terminal

MacOS

```bash
$ electron-packager . --overwrite --platform=darwin --arch=x64 --icon=assets/icons/mac/icon.icns --prune=true --out=release-builds
```

Windows

```bash
$ electron-packager . --overwrite --asar=true --platform=win32 --arch=ia32 --icon=assets/icons/win/icon.ico --prune=true --out=release-builds --version-string.CompanyName=CE --version-string.FileDescription=CE --version-string.ProductName="Electron Webview"
```

Linux (Ubuntu)

```bash
$ electron-packager . --overwrite --platform=linux --arch=x64 --icon=assets/icons/png/1024x1024.png --prune=true --out=release-builds
```

#### Shortcuts

To make it easier to create new builds, scripts are added in `package.json`.

Now you can run:

```bash
$ npm run package-mac
```

```bash
$ npm run package-win
```

```bash
$ npm run package-linux
```

## Mac installer

To create a DMG installer for our Electron app we can use the [electron-installer-dmg](https://github.com/mongodb-js/electron-installer-dmg) package.

To create a DMG installer you first need to package the app as we saw in the Application chapter above.

### Install

```bash
# For use in npm scripts
$ npm install electron-installer-dmg --save-dev

# For use from cli
$ npm install electron-installer-dmg -g
```

### Create the DMG

```bash
$ electron-installer-dmg ./release-builds/Electron\ webview-darwin-x64/Electron\ webview.app electron-webview --out=release-builds --overwrite --icon=assets/icons/mac/icon.icns
```

An `electron-webview.dmg` file is now created in the `release-builds` folder.

### Shortcuts

To make it easier to create new DMG installer, a script is added in `package.json`.

Now you can run:

```bash
$ npm run create-installer-mac
```

## Source

Based on:

- [Electron Packager tutorial](https://www.christianengvall.se/electron-packager-tutorial/)
- [Browser](https://github.com/hokein/electron-sample-apps/tree/master/webview/browser)

## License

[MIT](LICENSE.md)
