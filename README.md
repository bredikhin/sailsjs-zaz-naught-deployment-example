# Deploying a Sails.js project with Zaz and Naught

## Installation

Clone, `npm install`.

Also, install [Zaz](https://github.com/bredikhin/zaz): `sudo npm install -g zaz`.

## Configuration

Configure your [Zaz](https://github.com/bredikhin/zaz) deployment stages in `zaz.json`.

Configure your [Naught](https://github.com/andrewrk/naught) commands in `package.json`,
`scripts` section like this:

    ...
    "scripts": {
      "start": "naught start  --worker-count 3 --ipc-file tmp/naught.ipc --log log/naught.log --stdout log/stdout.log --stderr log/stderr.log app.js",
      "stop": "naught stop tmp/naught.ipc",
      "status": "naught status tmp/naught.ipc",
      "deploy": "naught deploy --timeout 15 tmp/naught.ipc"
    },
    ...

In general, if starting with a fresh Sails.js project, all you have to modify is `app.js`
according to the [Naught's requirements](https://github.com/andrewrk/naught#usage).

Specifically, in this example we added the following to the `app.js`:

```javascript
// Deploying with Naught: send the `online` message
sails.on('lifted', function() {
  process.send('online');
});

// Deploying with Naught: graceful shutdown
process.on('message', function(message) {
  if (message === 'shutdown') {
    sails.lower();
    process.exit(0);
  }
});
```

## Usage

`zaz <stage name>`, e.g. `zaz production`.

## Contributions

* are welcome;
* should be tested;
* should follow the same coding style.

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2014 [Ruslan Bredikhin](http://www.ruslanbredikhin.com/)
