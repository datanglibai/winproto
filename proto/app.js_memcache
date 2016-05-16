'use strict';

var express = require('express');
var Memcached = require('memcached');

var app = express();

// The environment variables are automatically set by App Engine when running
// on GAE. When running locally, you should have a local instance of the
// memcached daemon running.
var memcachedAddr = process.env.MEMCACHE_PORT_11211_TCP_ADDR || 'localhost';
var memcachedPort = process.env.MEMCACHE_PORT_11211_TCP_PORT || '11211';
var memcached = new Memcached(memcachedAddr + ':' + memcachedPort);

app.get('/', function (req, res, next) {
  memcached.get('foo', function (err, value) {
    if (err) { return next(err); }
    if (value) {
      return res.status(200).send('Value: ' + value);
    }

    var result = Math.random();
    memcached.set('foo', result, 60, function (err) {
      if (err) { return next(err); }
      return res.status(200).send('Value: ' + result);
    });
  });
});

app.get('/api',function (req,res) {
  res.status(200).json({key:'value'});
})

app.use(express.static('view'));
app.use(express.static('content'));


// Start the server
var server = app.listen(process.env.PORT || '8080', '0.0.0.0', function () {
  console.log('App listening at http://%s:%s', server.address().address,
    server.address().port);
  console.log('Press Ctrl+C to quit.');
});