**WARNING:** this project is highly experimental, use at your own risk

# openresty-decorator-buildpack

A helper buildpack for Pivotal CloudFoundry. This buildpack augments Nginx + OpenResty application
by adding additional opm modules to the application.

This buildpack must be used along with with the
Nginx buildpack and OpenResty.

## Usage

Deploy an app using the Nginx buildpack and OpenResty. See [here](https://github.com/cloudfoundry/nginx-buildpack/tree/master/fixtures/openresty) for a sample application (the important part is the
`buildpack.yml` file).

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
