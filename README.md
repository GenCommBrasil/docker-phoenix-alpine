# Docker Phoenix Alpine

## About this Repo

Based on the elixir alpine 1.6 official docker image (available on [official repo](https://github.com/c0b/docker-elixir)).

This Dockerfile adds a Phoenix layer as well as the necessary dependencies to run on Wercker CI.

## Additional Instructions to use with Wercker

Wercker CI supports Alpine images, although it is necessary to explicitly specify the usage of `sh` instead of `bash`. This can be achieved by specifying the `cmd` property in the box section of your `wercker.yml`.

```
box:
	id: rakutenbrasil/phoenix-alpine
	cmd: /bin/sh
```

Further reference can be found at [wercker faq](http://devcenter.wercker.com/docs/faq/alpine-faq).
