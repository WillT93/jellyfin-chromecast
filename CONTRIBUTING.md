# Contributing

## Development

### Development Environment

The development environment is setup with editorconfig. Code style is enforced by prettier and eslint for Javascript/Typescript linting

-   [editorconfig](https://editorconfig.org/)
-   [prettier](https://prettier.io/)
-   [eslint](https://eslint.org/)

### Environment variables

| name          | required | description                                               | default if not set |
| ------------- | -------- | --------------------------------------------------------- | ------------------ |
| RECEIVER_PORT | No       | The port used for the dev server when `npm start` is used | 9000               |

### Building/Using

`npm start` - Build a development version and start a dev server

`npm run build` - Build a production version

`npm run test` - Run tests

`npm run lint` - Run linting and prettier

1. Register a new [application](https://developers.google.com/cast/docs/registration). It is important that you choose a "Custom application", the rest of the details are up to you (name, description, etc).

    The Receiver Application URL it refers to is the location where this apps files will be hosted from. The Chromecast will look to that URL when it attempts to start this receiver app. You will need a web server to host the files on (which can just be the computer you are using to develop this app). Enter the IP address into this field and ensure it's accessible on your LAN from your Chromecast device.

2.  Ensure that you can use this app by making the following changes on your Jellyfin Server:
    #### For versions 10.8.x and earlier:
    - Set up a local copy of [jellyfin-web](https://github.com/jellyfin/jellyfin-web).
    - Change `applicationStable` and `applicationUnstable` in `jellyfin-web/src/plugins/chromecastPlayer/plugin.js` to your own application ID.
    - Run the local copy of jellyfin-web using the provided instructions in the repo.

    #### For versions 10.9.x and beyond:
    - Add your `CastReceiverApplication` `ID` and `Name` to the jellyfin `system.xml` in the `configuration` folder. Example below:
    ```xml
    <CastReceiverApplications>
      <CastReceiverApplication>
        <Id>F007D354</Id>
        <Name>Stable</Name>
      </CastReceiverApplication>
      <CastReceiverApplication>
        <Id>6F511C87</Id>
        <Name>Unstable</Name>
      </CastReceiverApplication>
      <CastReceiverApplication> # New Entry
        <Id>C321E78B</Id>
        <Name>Jellyfin-Test-Receiver</Name>
      </CastReceiverApplication>
    </CastReceiverApplications>
    ```
    - Your custom hosted application is now available to select next to `stable` and `unstable`. From the client of your choice.

3. Clone this repo and run `npm install`. This will install all dependencies, run tests and build a production build by default.

4. Make changes and build with `npm run build`.

5. Spin up a web server accessible by the Chromecast. An easy way to do this is to install python, navigate to the `dist` folder (or symlink it to the location you will run the webserver from and run the following command:
    ```ps
    python -m http.server 9000
    ```

    > NOTE: While running `npm start` will serve your dev server on http://localhost:9000, that won't bind to the IP of your computer, only to `localhost`. This means the Chromecast will fail to find it on the network.

    > NOTE: Attempting to access the cast app using your computer's browser will fail. **Jellyfin** will appear in your browser tab, but the page will be blank and errors will appear in your console. This is because the cast app must be run from Chromecast / Android TV hardware. If you've gotten this far, your dev server is online, you just need to use a real Chromecast.

6. To debug, open Google Chrome and navigate to [chrome://inspect/#devices](chrome://inspect/#devices). Using a cast controller such as a phone, cast the Jellyfin app to the Chromecast. Options will appear next to the device in Chrome allowing you to open developer tools.

7. Before pushing your changes, make sure to run `npm run test` and `npm run lint`.

## Pull Requests

This project uses the standard Github Fork and PR flow
