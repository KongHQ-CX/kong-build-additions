# Kong build Additions

Various build scripts and patches for Kong.

## Index

### Add Second rate-limiting Plugin

> Directory Link: [add-second-rate-limiting-plugin](./add-second-rate-limiting-plugin)

This build helper adds a second [`rate-limiting`](https://docs.konghq.com/hub/kong-inc/rate-limiting/) plugin, called "rate-limiting-2", to your Kong container image.

#### Building

Firstly, **adjust your target Kong version in the first line of the [Dockerfile](./add-second-rate-limiting-plugin/Dockerfile)**:

```Dockerfile
ARG KONG_VERSION="3.3.1.0"
ARG PLUGIN_VERSION_TAG="3.3.1"
```

⚠️ **The `PLUGIN_VERSION_TAG` should match the major.minor.patch version as the `KONG_VERSION`** ⚠️

Alternatively, you can set the build arguments with e.g. `--build-arg=KONG_VERSION=3.4.0.0 --build-arg PLUGIN_VERSION_TAG=3.4.0` during your image build.

Now when you execute the Docker build, it will create you a second copy of the open source "rate-limiting" plugin!

#### Running

##### Konnect

You need to upload the ["schema.lua" from the plugin source code](https://github.com/Kong/kong/blob/master/kong/plugins/rate-limiting/schema.lua), from the [Git tag](https://github.com/Kong/kong/tags) that corresponds to the version of Kong that you built above, as a [Konnect Custom Plugin](https://docs.konghq.com/konnect/runtime-manager/plugins/#custom-plugins).

**You must enable the new plugin in your Kong deployment(s) with:**

```sh
export KONG_PLUGINS="bundled,rate-limiting-2"
```

##### On-Prem

Simply reference the second instance of the plugin by name `rate-limiting-2` - all config options will be the same. You will also see it in the **Kong Manager** plugins menu, if enabled.

**You must enable the new plugin in your Kong deployment(s) with:**

```sh
export KONG_PLUGINS="bundled,rate-limiting-2"
```
