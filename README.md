# awesome
The awesome collections for development

#### UI framework
* Semantic-UI: https://github.com/Semantic-Org/Semantic-UI
* magic - css animation: https://github.com/miniMAC/magic
* pure: https://github.com/yahoo/pure
* foundation: https://github.com/zurb/foundation
* react: https://github.com/facebook/react
* bootstrap-material-design: https://github.com/FezVrasta/bootstrap-material-design

#### Bootstrap Theme
* devoops: https://github.com/devoopsme/devoops
* bootswatch: https://github.com/thomaspark/bootswatch
* roots - WordPress starter theme: https://github.com/roots/roots
* bootstrap material design: https://github.com/FezVrasta/bootstrap-material-design
* startBootstrap - free Bootstrap themes: https://github.com/IronSummitMedia/startbootstrap
* adminLTE - Free Premium Admin control Panel Theme: https://github.com/almasaeed2010/AdminLTE
* startbootstrap-sb-admin: https://github.com/IronSummitMedia/startbootstrap-sb-admin-2
* 1pxdeep: https://github.com/rriepe/1pxdeep
* bootstrap-zero - themes & templates: https://github.com/iatek/bootstrap-zero
* login page: http://www.bootstrapzero.com/bootstrap-template/sign-in
* bootsketch - sketches style: http://yago.github.io/Bootsketch/
* __media-center__: http://demo.transvelo.com/media-center/
* bootstrap-rtl: https://github.com/morteza/bootstrap-rtl
* gridgum: http://gridgum.com/
* one-page: http://websites.simplesphere.net/cleany/
* one-page: http://azmind.com/premium/marco/v1-1/layout-1/index.html

#### MVC
* backboneJS: http://backbonejs.org/
* underscoreJS: http://underscorejs.org/

#### Forms
* select2 - select box: https://github.com/select2/select2
* validation: https://github.com/formvalidation/formvalidation
* full calendar: http://fullcalendar.io/
* moment - date manipulation: https://github.com/moment/moment/
* tinymce - posting: https://github.com/tinymce/tinymce
* HTML5 forms support polyfill: https://github.com/ryanseddon/H5F

#### Components
|Name             |Link                                     |Description|
|:--------------- | :---------------------------------------|:--------  |
|jQuery Knob      |https://github.com/aterrien/jQuery-Knob  |  |
|justified gallary|https://github.com/miromannino/Justified-Gallery | picutres gallary|
|spin.js          |http://fgnass.github.io/spin.js/|progress indicator|
|animate.js       |https://github.com/daneden/animate.css| 
|sweet alert      |https://github.com/t4t5/sweetalert||
|flip clock       |https://github.com/objectivehtml/FlipClock||

#### Charts & Graphs
* morris - charts: https://github.com/morrisjs/morris.js
* d3: https://github.com/mbostock/d3
* nvd3 - D3: https://github.com/novus/nvd3
* springy - A force directed graph layout algorithm: https://github.com/dhotson/springy/
* rickshaw - creating interactive real-time graphs: https://github.com/shutterstock/rickshaw

#### Face Recognition
* headtrackr: https://github.com/auduno/headtrackr
* __examples - facetrackr__: http://auduno.github.io/headtrackr/examples/facetracking.html
* face++: http://faceplusplus.com.cn

#### NodeJS
|Name     |Link                                     |Description|
|:------- | :---------------------------------------|:--------  |
|grant    |https://github.com/simov/grant           |Authentication Middleware for Express and Koa -- 84 Supported Providers|
|sequelize|https://github.com/sequelize/sequelize|ORM: MySQL, MariaDB, SQLite, PostgreSQL and MSSQL|
|node-wechat|https://github.com/node-webot/wechat|微信公共平台消息接口服务中间件|

#### jQuery
|Name     |Link                                     |Description|
|:------- | :---------------------------------------|:--------  |
|jquery-patterns|https://github.com/jquery-boilerplate/jquery-patterns|A variety of jQuery plugin patterns for jump starting your plugin development|
|jquery-windows|https://github.com/nick-jonas/nick-jonas.github.com/tree/master/windows|a handy, loosely-coupled jQuery plugin for full-screen scrolling windows|
|jquery-chosen|https://github.com/harvesthq/chosen|Chosen is a library for making long, unwieldy select boxes more friendly.|
|superscrollorama|https://github.com/johnpolacek/superscrollorama|The jQuery plugin for supercool scroll animation|
|jquery-color|https://github.com/jquery/jquery-color|jQuery plugin for color manipulation and animation support.|
|jquery-smooth-scroll|https://github.com/kswedberg/jquery-smooth-scroll|Automatically make same-page links scroll smoothly

#### Solution
|Name     |Link                                     |Problem|
|:------- | :---------------------------------------|:--------  |
|offline.js|https://github.com/hubspot/offline  |network connection/disconnection detector|
|offline.js simulator|https://github.com/craigshoemaker/offlinejs-simulate-ui  |simulate the connection up/down with a radio|

#### Tutorials
[git] switch from https to SSH: https://github.com/waylau/github-help/blob/master/Changing%20a%20remote's%20URL%20%E4%BF%AE%E6%94%B9%E8%BF%9C%E7%A8%8B%20URL.md

#### Just cool
* An open-source status dashboard running on App Engine: https://github.com/twilio/stashboard

#### Code Snippets
The code for detecting online/offline with indicators:
```Javascript
//Object that will be exposed to the outside world. (Revealing Module Pattern)
  var OfflineDetector = {};

  //Flag indicating current network status.
  var isOffline = false;

  //Configuration Options for Offline.js
  Offline.options = {
      checks: {
          xhr: {
              //By default Offline.js queries favicon.ico.
              // MAKE SURE the favicon.ico is listed in the part of NETWORK in chessboard.appcache
              url: 'favicon.ico'
          }
      },
      checkOnLoad: true,
      interceptRequests: true,
      reconnect: true,
      requests: true,
      game: false
  };

  //Offline.js raises the 'up' event when it is able to reach
  //the server indicating that connection is up.
  Offline.on('up', function () {
      isOffline = false;
  });

  //Offline.js raises the 'down' event when it is unable to reach
  //the server indicating that connection is down.
  Offline.on('down', function () {
      isOffline = true;
  });

  //Expose Offline.js instance for outside world!
  OfflineDetector.Offline = Offline;

  //OfflineDetector.isOffline() method returns the current status.
  OfflineDetector.isOffline = function () {
      return isOffline
  };

  //start() method contains functionality to repeatedly
  //invoke check() method of Offline.js.
  //This repeated call helps in detecting the status.
  OfflineDetector.start = function () {
      var checkOfflineStatus = function () {
          Offline.check();
      };
      setInterval(checkOfflineStatus, 3000);
  };

  //Start OfflineDetector
  OfflineDetector.start();
```
