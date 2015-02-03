# awesome
The awesome collections for development

#### UI framework
* Semantic-UI: https://github.com/Semantic-Org/Semantic-UI
* magic - css animation: https://github.com/miniMAC/magic
* pure: https://github.com/yahoo/pure
* foundation: https://github.com/zurb/foundation
* react: https://github.com/facebook/react

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

#### Face Recognition
* headtrackr: https://github.com/auduno/headtrackr
* __examples - facetrackr__: http://auduno.github.io/headtrackr/examples/facetracking.html
* face++: http://faceplusplus.com.cn

#### NodeJS
|Name     |Link                                     |Description|
|:------- | :---------------------------------------|:--------  |
|grant    |https://github.com/simov/grant           |Authentication Middleware for Express and Koa -- 84 Supported Providers|
|sequelize|https://github.com/sequelize/sequelize|ORM: MySQL, MariaDB, SQLite, PostgreSQL and MSSQL|

#### jQuery
|Name     |Link                                     |Description|
|:------- | :---------------------------------------|:--------  |
|jquery-patterns|https://github.com/jquery-boilerplate/jquery-patterns|A variety of jQuery plugin patterns for jump starting your plugin development|

#### Solution
|Name     |Link                                     |Problem|
|:------- | :---------------------------------------|:--------  |
|offline.js|https://github.com/hubspot/offline  |network connection/disconnection detector|
|offline.js simulator|https://github.com/craigshoemaker/offlinejs-simulate-ui  |simulate the connection up/down with a radio|

#### Tutorials
[git] switch from https to SSH: https://github.com/waylau/github-help/blob/master/Changing%20a%20remote's%20URL%20%E4%BF%AE%E6%94%B9%E8%BF%9C%E7%A8%8B%20URL.md

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
