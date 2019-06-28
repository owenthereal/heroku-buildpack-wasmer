# heroku-buildpack-wasmer

This is a Heroku buildpack for [Wasmer](https://wasmer.io/).

## Example

The full example of running Nginx with Wasmer is available [here](https://github.com/jingweno/nginx-wasmer).

Set up project to use with [wapm](https://wapm.io/):

```
wapm init nginx-wasmer
```

Add a "dependencies" section to wapm.toml according to https://wapm.io/help/reference. For example:

```
$ cat wapm.toml
[package]
name = "nginx-wasm"
version = "0.1.0"
description = ""

[dependencies]
nginx = "0.1.1"
```

Add a `Procfile` that runs Nginx with Wasmer. The following script substitute Nginx port with the environment variable `$PORT`:

```
$ cat Procfile
web: sed -i -e "s/PORT/$PORT/g" nginx.conf && wapm run nginx -p . -c nginx.conf
```

Create a Heroku app, set the buildpack and deploy:

```
heroku create APP_NAME
heroku buildpacks:set https://github.com/jingweno/heroku-buildpack-wasmer
git push heroku master
curl https://APP_NAME.herokuapp.com
```
