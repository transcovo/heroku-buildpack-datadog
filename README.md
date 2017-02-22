heroku-buildpack-datadog
========================

A [Heroku Buildpack] to add [Datadog] [DogStatsD] relay to any Dyno.

## Usage

This buildpack is typically used in conjunction with other languages, so is
most useful with language-specific buildpacks - see [Heroku Language Buildpacks] for more.

Here are some setup commands to add this buildpack to your project, as well as
setting the required environment variables:

```shell
cd <root of my project>

heroku create # only if this is a new heroku project
heroku buildpacks:add heroku/nodejs # or other language-specific build page needed
heroku buildpacks:add --index 1 https://github.com/transcovo/heroku-buildpack-datadog.git
heroku config:set HEROKU_APP_NAME=$(heroku apps:info|grep ===|cut -d' ' -f2)
heroku config:add DATADOG_API_KEY=<your API key>

git push heroku master
```

You can create/retrieve the `DATADOG_API_KEY` from your account on [this page](https://app.datadoghq.com/account/settings#api).
API Key, not application key.

You can optionally set additional percentiles for your histogram metrics. By default
only 95th percentile will be generated. To generate additional percentiles, set *all*
persentiles, including default one, using env variable `DATADOG_HISTOGRAM_PERCENTILES`.
For example, if you want to generate 0.95 and 0.99 percentiles, you may use following
command:

```shell
heroku config:add DATADOG_HISTOGRAM_PERCENTILES="0.95, 0.99"
```

Documentaion about additional percentiles [here](https://help.datadoghq.com/hc/en-us/articles/204588979-How-to-graph-percentiles-in-Datadog).

Once complete, the Agent's dogstatsd binary will be started automatically with the Dyno startup.

Once started, provides a listening port on 8125 for statsd/dogstatsd metrics and events.

An example using Ruby is [here](https://github.com/miketheman/buildpack-example-ruby).

## Todo

Things that have not been tested, tried, figured out.

- see if we can bypass apt updates on every run
- determine how the compiled cache behaves with new releases of the
  datadog-agent package, as it stored the deb file
- tag release when stable, update docs on how to use a given release in
  `.buildpacks`, like "https://github.com/miketheman/heroku-buildpack-datadog.git#v1.0.0"

## Contributing

- Fork this repo
- Check out the code, create your own branch
- Make modifications, test heavily. Add tests if you can.
- Keep commits simple and clear. Show what, but also explain why.
- Submit a Pull Request from your feature branch to `master`

## Credits

This buildpack was heavily inspired by the heroku-buildpack-apt code, as well
as many others from Heroku and [@ddollar].
We leverage the same type of process runner that the Datadog Docker container
uses, with a couple of modifications.

Author: [@miketheman]

## License

MIT License, see `LICENSE` file for full text.

[Datadog]: http://www.datadog.com
[DogStatsD]: http://docs.datadoghq.com/guides/dogstatsd/
[Heroku Buildpack]: https://devcenter.heroku.com/articles/buildpacks
[Heroku Language Buildpacks]: https://devcenter.heroku.com/articles/buildpacks#default-buildpacks

[@ddollar]: https://github.com/ddollar
[@miketheman]: https://github.com/miketheman
