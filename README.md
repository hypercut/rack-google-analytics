# Important!!!

Version 1.0.0 is in the making and will not be backwards compatible... but will bring
all the shinyness from analytics.js to your middleware stack.

You can checkout the current state [here](https://github.com/kangguru/rack-google-analytics/tree/analytics-js)
and there's also a [pre release](http://rubygems.org/gems/rack-google-analytics/versions/1.0.0.pre1) on rubygems.org

# Rack google Analytics

[![Build Status](https://travis-ci.org/kangguru/rack-google-analytics.png?branch=master)](https://travis-ci.org/kangguru/rack-google-analytics)

Simple Rack middleware to help injecting the Google Analytics tracking code in your website.

This middleware injects either the synchronous or asynchronous Google Analytics tracking code into the correct place of any request only when the response's `Content-Type` header contains `html` (therefore `text/html` and similar).

## Usage

#### Gemfile

```ruby
gem 'rack-google-analytics'
```

#### Sinatra

```ruby
## app.rb
use Rack::GoogleAnalytics, :tracker => 'UA-xxxxxx-x'
```

#### Padrino

```ruby
## app/app.rb
use Rack::GoogleAnalytics, :tracker => 'UA-xxxxxx-x'
```

#### Rails 2.X

```ruby
## environment.rb:
config.gem 'rack-google-analytics', :lib => 'rack/google-analytics'
config.middleware.use Rack::GoogleAnalytics, :tracker => 'UA-xxxxxx-x'
```

#### Rails 3.X and 4.X

```ruby
## application.rb:
config.middleware.use Rack::GoogleAnalytics, :tracker => 'UA-xxxxxx-x'
```

### Options

* `:async`      -  sets to use asynchronous tracker
* `:multiple`   -  sets track for multiple subdomains. (must also set :domain)
* `:domain`     -  sets the domain name for the GATC cookies. Defaults to `auto`. (must also set :multiple)
* `:top_level`  -  sets tracker for multiple top-level domains. (must also set :domain)
* `:anonymize_ip` -  sets the tracker to remove the last octet from all IP addresses, see https://developers.google.com/analytics/devguides/collection/gajs/methods/gaJSApi_gat?hl=de#_gat._anonymizeIp for details.
* `:site_speed_sample_rate` - Defines a new sample set size for Site Speed data collection, see https://developers.google.com/analytics/devguides/collection/gajs/methods/gaJSApiBasicConfiguration?hl=de#_gat.GA_Tracker_._setSiteSpeedSampleRate
* `:adjusted_bounce_rate_timeouts` - An array of times in seconds that the tracker will use to set
timeouts for adjusted bounce rate tracking. See http://analytics.blogspot.ca/2012/07/tracking-adjusted-bounce-rate-in-google.html for details.

Note: since 0.2.0 this will use the asynchronous Google Analytics tracking code, for the traditional behaviour please use:

```ruby
use Rack::GoogleAnalytics, :tracker => 'UA-xxxxxx-x', :async => false
```

If you are not sure what's best, go with the defaults, and read here if you should opt-out.

## Custom Variable Tracking

In your application controller, you may track a custom variable. For example:

```ruby
set_ga_custom_var(1, "LoggedIn", value, GoogleAnalytics::CustomVar::SESSION_LEVEL)
```

See https://developers.google.com/analytics/devguides/collection/gajs/gaTrackingCustomVariables for details.

## Event Tracking

In your application controller, you may track an event. For example:

```ruby
track_ga_event("Users", "Login", "Standard")
```

See https://developers.google.com/analytics/devguides/collection/gajs/eventTrackerGuide

## Custom Push

In your application controller, you may push arbritrary data. For example:

```ruby
ga_push("_addItem", "ID", "SKU")
```

## Dynamic Tracking Code

You may instead define your tracking code as a lambda taking the Rack environment, so that you may set the tracking code
dynamically based upon information in the Rack environment. For example:

```ruby
config.middleware.use Rack::GoogleAnalytics, :tracker => lambda { |env|
        return env[:site_ga].tracker if env[:site_ga]
}
```


## Thread Safety

This middleware *should* be thread safe. Although my experience in such areas is limited, having taken the advice of those with more experience; I defer the call to a shallow copy of the environment, if this is of consequence to you please review the implementation.

## Note on Patches/Pull Requests

* Fork the project.
* Make your feature addition or bug fix.
* Add tests for it. This is important so I don't break it in a
  future version unintentionally.
* Commit, do not mess with rakefile, version, or history.
  (if you want to have your own version, that is fine but bump version in a commit by itself I can ignore when I pull)
* Send me a pull request. Bonus points for topic branches.

## Copyright

Copyright (c) 2009-2012 Lee Hambley. See LICENSE for details.
With thanks to [Ralph von der Heyden](https://github.com/ralph) and [Simon Schoeters](https://github.com/cimm) - And the biggest hand to [Arthur Chiu](https://github.com/achiu) for the huge work that went into the massive 0.9 re-factor.
