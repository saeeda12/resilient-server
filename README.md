# resilient-server [![Build Status](https://api.travis-ci.org/h2non/resilient-server.svg?branch=master)][travis] [![Dependency Status](https://gemnasium.com/h2non/resilient-server.svg)][gemnasium] [![NPM version](https://badge.fury.io/js/resilient-server.svg)][npm]

Dummy HTTP server fully compatible with the [Resilient](http://resilient-http.github.io) [specification](https://github.com/resilient-http/spec) discovery protocol.

It provides a simple CRUD-like HTTP API to publish and store service servers URLs, with multiple application version based on [semantic versioning](http://semver.org).
The stored servers are persisted as volatile data (in-memory). After a server restart is required data re-registering

It was designed to be used for testing, demo and development proposals with resilient based applications. 
For production environments, see the [Hydra](http://innotech.github.io/hydra) project

Additionally it implements full CORS support for direct browser consumption

## Installation

### Standalone

Download the executable server as standalone binary for your target OS

- **[Linux](http://dl.bintray.com/h2non/resilient/resilient-server-0.1.1-linux-x64.nar)** (x64)
- **[OSX](http://dl.bintray.com/h2non/resilient/resilient-server-0.1.1-darwin-x64.nar)** (x64)

Then simply run it from your prefered shell

```bash
$ chmod +x resilient-server-0.1.1-linux-x64.nar
```

```bash
$ ./resilient-server-0.1.1-linux-x64.nar
```

### Node.js

Install the package
```bash
$ npm install -g resilient-server
```

Start the server
```bash
$ resilient-server -p 8080 -h 0.0.0.0 --api-key awesome
```

Show the help
```bash
$ resilient-server --help
```

## HTTP API

#### GET /:appName

Get a list of servers for the given application service, optionally specifiying the app semantic version

##### Request

```
curl -i http://localhost:8080/my-app-api?version=1.0.0
```

##### Response

Valid response
```json
HTTP/1.1 200 OK
Content-Type: application/json
Server: resilient-server 0.1.0

[
  "http://api1.server.me",
  "http://api2.server.me",
  "http://api3.server.me"
]
```

Missing app name
```
HTTP/1.1 404 Not Found
```

#### GET /all

Get the complete list of registered apps and servers URIs

##### Request

```
curl -i http://localhost:8080/all
```

##### Response

Valid response
```json
HTTP/1.1 200 OK
Content-Type: application/json
Server: resilient-server 0.1.0

[
  {
    "name": "app",
    "versions": {
      "version": "1.0.0",
      "updated": 1415144314038,
      "servers": [
        "http://api2.server.me",
        "http://api2.server.me",
        "http://api3.server.me"
      ]
    }
  }
]
```

Missing app name
```
HTTP/1.1 404 Not Found
```

#### POST|PUT /:appName

Update the servers for the given application service, optionally specifiying the app semantic version

**Note**: this service could require an API key token, if it's was defined via `--api-token` flag

##### Request

```
curl -i -H "Accept: application/json" \
  -H "API-Token: awesome" \
  -H "Version: 1.0.0" \
  -X POST -d '["http://newapi.server.com"]' \
  http://localhost:8080/my-app-api
```

##### Response

Valid response
```
HTTP/1.1 204 No Content
```

Invalid response
```
HTTP/1.1 400 Bad Request
```

#### DELETE /:appName

Removes the servers of a given app from the registry, optionally specifiying the app semantic version

##### Request

```
curl -i -H "Accept: application/json" \
  -H "API-Token: awesome" \
  -H "Version: 1.0.0" \
  -X DELETE  \
  http://localhost:8080/my-app-api
```

##### Response

Valid response
```
HTTP/1.1 204 No Content
```

Invalid response
```
HTTP/1.1 404 Not Found
```

## Development

You must have installed [node.js](http://nodejs.org) >= `0.10`

Clone this repository

```bash
git clone https://github.com/h2non/resilient-server.git && cd resilient-server
```

Install runtime and developmennt dependencies

```bash
npm install
```

Run tests

```bash
grunt test
```

Run the server

```
./bin/resilient-server --port 8080 --debug
```

Show help

```
./bin/resilient-server --help
```

Run as a service (using [forever](https://github.com/nodejitsu/forever))

```bash
forever -m 5 ./bin/resilient-server
```

## License

MIT - Tomas Aparicio

[travis]: http://travis-ci.org/h2non/resilient-server
[gemnasium]: https://gemnasium.com/h2non/resilient-server
[npm]: http://npmjs.org/package/resilient-server
