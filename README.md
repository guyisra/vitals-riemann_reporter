# Vitals::RiemannReporter

The Riemann Reporter allows [Vitals](https://github.com/jondot/vitals) to send metrics to Riemann instead of StatsD.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'vitals-riemann_reporter'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install vitals-riemann_reporter

## Usage

Initialise Vitals with a RiemannReporter. You may also like to use the RiemannFormat so that your metric names do not include things that are already passed to Riemann as fields(ie. host, facility and environment):

```
require 'vitals'
Vitals.configure! do |c|
  c.format = Vitals::Formats:RiemannFormat
  c.facility = 'facility'
  c.environment = 'environment'
  c.reporter = Vitals::Reporters::RiemannReporter.new(host: 'riemann-host', port: 5555, facility: 'facility', environment: 'environment')
end
```

The Riemann client does not open a connection until a metric is sent, so you can speed up your first request by sending a connected event in your initializer:

```
Vitals.inc('riemann_connected')
```

In Riemann, events will be tagged as 'vitals' and either 'counter', 'gauge' or 'timer' depending on the type of event. They will have their host, facility and environment set as fields.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/Ben-M/vitals-riemann_reporter.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

