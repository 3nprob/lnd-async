# lnd-async
This library simplifies connecting to Lightning Network Daemon via gRPC. It also wraps all callback functions in promises to make api calls easier to work with.

The default behavior assumes LND is running on `localhost` port `10009`. It also assumes that macaroons are enabled and the `tls.cert` and `admin.macaroon` are found in the OS specific default data paths.

To establish a connection to gRPC `localhost:10009` with macaroons:
```javascript
const lnd = require('lnd-async');

async function getInfo() {
  let client = await lnd.connect();
  return await client.getInfo({});
}
```

This call can be customized to meet your specific needs:
```javascript
const lnd = require('lnd-async');

async function getInfo() {
  let client = await lnd.connect({ 
    lndHost: '10.10.0.12', 
    lndPort: 10019, 
    certPath: __dirname + '/tls.cert',
    macaroonPath: __dirname + '/admin.macaroon',
  });
  return await client.getInfo({});
}
```

You can also initiate calls with no macaroons by passing the `noMacaroons` flag. When this option is supplied, the `macaroonPath` is ignored.
```javascript
const lnd = require('lnd-async');

async function getInfo() {
  let client = await lnd.connect({ noMacaroons: true });
  return await client.getInfo({});
}
```