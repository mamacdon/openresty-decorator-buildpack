**WARNING:** this project is highly experimental, use at your own risk

# openresty-decorator-buildpack

A helper buildpack for Pivotal CloudFoundry. This buildpack allows you to add additional opm modules
to an application that uses the Nginx buildpack and OpenResty.

## Requirements

- A CloudFoundry application based on the Nginx buildpack that uses OpenResty

## Usage

Deploy an app using the Nginx buildpack and OpenResty. See [here](https://github.com/cloudfoundry/nginx-buildpack/tree/master/fixtures/openresty) for a sample application (the important part is the`buildpack.yml` file).

When you've got that working, edit your app's `manifest.yml` file. Add `openresty-decorator-buildpack`
to the list of buildpacks, making sure it appears **after** the nginx buildpack.

```yml
buildpacks:
 - https://github.com/cloudfoundry/nginx-buildpack.git
 - https://github.com/mamacdon/openresty-decorator-buildpack
```

Create a `opm.deps` file containing the list of opm packages you wish to install, one per line.

```text
cdbattags/lua-resty-jwt
zmartzone/lua-resty-openidc
```

Push your application.

When it starts, the opm packages will be available and can be loaded
using `require()` like any Lua module:

```lua
require('resty.openidc').authenticate({ … })
```

## Configuration

The environment variables below can be set at staging time to override the buildpack's default behavior.

- `ODB_SKIP_LUA_PATHS`: when `true`, prevents the buildpack from adding `lua_package_path`
  and `lua_package_cpath` to your `nginx.conf`. Set this if you prefer to configure these paths
  yourself.

- `ODB_SKIP_LUA_SSL`: when `true`, prevents the buildpack from adding a `lua_ssl_trusted_certificate`
  directive to your `nginx.conf`. By default the buildpack adds this directive, trusting all the
  default root CAs present in `/etc/ssl/certs/`. Set this variable if you wish to manage the
  CA list yourself.
