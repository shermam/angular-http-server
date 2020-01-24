# Single Page App dev-server

This is a fork from simonh1000/angular-http-server (https://github.com/simonh1000/angular-http-server)
This server adds a simple gzip capability to angular-http-server.
Files with the extensions [".js", ".html", ".css", ".txt", ".json"] that are in the root folder are gzipped and kept in memory to be served to requests.
I did this exclusively to test a --prod build of an angular app with lighthouse.

A simple dev-server designed for Single Page App (SPA) developers. **`spa-server-gzip` is not, and makes no claims to be, a production server.**

It returns a file if it exists (ex. your-icon.png, index.html), routes all other requests to index.html (rather than giving a 404 error) so that you SPA's routing can take over. The only time it will error out is if it can't locate the index.html file.

Originally designed for my Angular work, this dev-server will work with any Single Page App (SPA) framework that uses URL routing (React, Vue, Elm, ...).

## To use:

```sh
npm install -g spa-server-gzip
cd /path/to/site
spa-server-gzip
```

And browse to `localhost:8080`.

## Options

Specify a port using `-p <port number>`

```sh
spa-server-gzip -p 9000
```

Open in a default browser automatically by using `--open` alias `-o`

```sh
spa-server-gzip --open
```

HTTPS can be enabled (using a generated self-signed certificate) with `--https` or `--ssl`

```sh
spa-server-gzip --https
```

You may manually specify the paths to your self-signed certificate using the `--key` and `--cert` flags

```sh
spa-server-gzip --https --key ./secret/key.pem --cert ./secret/cert.pem
```

[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) can be enabled with the --cors flag

```sh
spa-server-gzip --cors
```

Add cache headers

```sh
spa-server-gzip --cache
```

Specify a path to serve from

```sh
spa-server-gzip --path example
```

Specify the base href of the application

```sh
spa-server-gzip --baseHref myapp
```

Disable logging

```sh
spa-server-gzip --silent
```

All options can be specified by a config file, optionally read via `--config` flag.
CLI options take precedence over any options read from the config file.

```sh
spa-server-gzip --config configs/spa-server-gzip.config.js
```

Feedback via: https://github.com/shermam/spa-server-gzip

## Config File

The config file can either export an object of parameters, or a function that will be passed in the parsed `argv` from minimalist.

Simple example:

```js
module.exports = {
    p: 8081,
    cors: true,
    silent: true
};
```

Complicated example:

```js
module.exports = argv => {
    const config = {
        cors: true
    };

    if (argv.p === 443) {
        config.ssl = true;
    }

    return config;
};
```

## Self-Signed HTTPS Use

#### Production

The `--https` or `--ssl` flags are intended for development and/or testing purposes only. Self-signed certificates do not properly verify the identity of the web app and they will cause an end-users web browser to display an error. Only use `spa-server-gzip` with a self-signed certificate for development and/or testing. This can be accomplished by using the self-signed certificate generated when you pass the `--https`/`--ssl` flag. An example of when you should use this feature is with end-to-end testing suites such as [Protractor](http://www.protractortest.org/). or other suites which require the SPA application to be actively served.

## Changelog

-   1.9.0 - adds --baseHref (thanks bertbaron)
-   1.8.0 - rewrite of path resolution (thanks dpraul)
-   1.7.0 - add option to include own ssl certificate (thanks dpraul)
-   1.6.0 - add --config option (thanks dpraul)
-   1.5.0 - add --open option (thanks tluanga34)
-   1.4.0 - add --path option (thanks nick-bogdanov)

## Contributing

Contributions are welcome, but do create an issue first to discuss.

Use prettier for formatting

## Testing

Run unit tests with

```sh
$ yarn run test
```

Testing - try:

```sh
node angular-http-server.js --path example --ssl -p 9000
```
