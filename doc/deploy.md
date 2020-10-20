# How to Deploy CcSinfo

CcSinfo is splitted into 3 repos:
* [https://github.com/saqarmax-arm/CcSinfo](https://github.com/saqarmax-arm/CcSinfo)
* [https://github.com/saqarmax-arm/CcSinfo-api](https://github.com/saqarmax-arm/CcSinfo-api)
* [https://github.com/saqarmax-arm/CcSinfo-ui](https://github.com/saqarmax-arm/CcSinfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy CcS core
1. `git clone --recursive https://github.com/saqarmax-arm/CcS.git --branch=CcSinfo`
2. Follow the instructions of [https://github.com/saqarmax-arm/CcS/blob/master/README.md#building-CcS-core](https://github.com/saqarmax-arm/CcS/blob/master/README.md#building-CcS-core) to build CcS
3. Run `CcSd` with `-logevents=1` enabled

## Deploy CcSinfo
1. `git clone https://github.com/saqarmax-arm/CcSinfo.git`
2. `cd CcSinfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `CcSinfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `CcSinfo` under a process manager (like `pm2`), to restart the process when `CcSinfo` crashes.

## Deploy CcSinfo-api
1. `git clone https://github.com/saqarmax-arm/CcSinfo-api.git`
2. `cd CcSinfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy CcSinfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/saqarmax-arm/CcSinfo-ui.git`
2. `cd CcSinfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "CcSINFO_API_BASE_CLIENT=/api/ CcSINFO_API_BASE_SERVER=http://localhost:3001/ CcSINFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `CcSinfo-ui` on port 3000
4. `npm run build`
5. `npm start`
