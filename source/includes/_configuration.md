# Configuration

## Requirements

### Monitoring

Scout currently supports Ruby on Rails 2.2+ and Ruby 1.8.7+ and the following app servers/background job frameworks:

* Phusion Passenger
* Puma
* Rainbows
* Thin
* Unicorn
* WEBrick
* Sidekiq

[Memory Bloat detection](#memory-bloat-detection) and [ScoutProf](#scoutprof) require Ruby 2.1+.

### Git Integration - Optional

Viewing your application code within Scout requires that your source code is hosted at Github.com.

## Installation

Tailored instructions are provided within our user interface. General instructions:

<table class="help install">
  <tbody>
	  <tr>
      <td>
        <span class="step">1</span>
      </td>
      <td>
			  <p>Your Gemfile:</p>
<pre>
gem 'scout_apm'
</pre>

        <p><strong>Shell:</strong></p>

<pre>
bundle install
</pre>
      </td>
    </tr>
    <tr>
     	<td><span class="step">2</span></td>
     	<td><p>Download your customized config file, placing it at <code>config/scout_apm.yml</code>.</p></td>
    </tr>
    <tr>
     	<td><span class="step">3</span></td>
     	<td><p>Deploy.</p></td>
    </tr>
 	</tbody>
</table>


## Updating to the Newest Version

The latest version of `scout_apm` is <code><span id="latest-gem-version">SEE CHANGELOG</span></code>.

<table class="help install">
  <tbody>
    <tr>
      <td>
        <span class="step">1</span>
      </td>
      <td>
        <p>Ensure your Gemfile entry for Scout is: <code>gem 'scout_apm'</code> </p>
      </td>
    </tr>
    <tr>
      <td><span class="step">2</span></td>
      <td><p>Run <code> bundle update scout_apm</code></p></td>
    </tr>
      <tr>
      <td><span class="step">3</span></td>
      <td><p>Re-deploy your application.</p></td>
    </tr>
  </tbody>
</table>

The gem version changelog is [available here](https://github.com/scoutapp/scout_apm_ruby/blob/master/CHANGELOG.markdown).  

## Configuration Options

Scout APM can be configured via the `config/scout_apm.yml` Yaml file and/or environment variables. A config file with your organization key is available for download as part of the install instructions.

### ERB evaluation

ERB is evaluated when loading the config file. For example, you can set the app name based on the hostname:

```yaml
common: &defaults
  name: <%= "ProjectPlanner.io (#{Rails.env})" %>
```

### Configuration Reference

The following configuration settings are available:

<table class="lookup">
    <thead>
      <tr>
        <th>
          Setting&nbsp;Name
        </th>
        <th>
          Description
        </th>
        <th>
          Default
        </th>
        <th>
          Required
        </th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>
          name
        </th>
        <td>
          Name of the application (ex: 'Photos App').
        </td>
        <td>
          <code>Rails.application.class.to_s.
             sub(/::Application$/, '')</code>
        </td>
        <td>
          Yes
        </td>
      </tr>
      <tr>
        <th>
          key
        </th>
        <td>
          The organization API key.
        </td>
        <td></td>
        <td>
          Yes
        </td>
      </tr>
      <tr>
        <th>
          monitor
        </th>
        <td>
          Whether monitoring should be enabled. 
        </td>
        <td>
          <code>false</code>
        </td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>
          log_level
        </th>
        <td>
          The logging level of the agent. 
        </td>
        <td>
          <code>INFO</code>
        </td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>
          log_file_path
        </th>
        <td>The path to the <code>scout_apm.log</code> log file directory. Use <code>stdout</code> to log to STDOUT.
        </td>
        <td>
          <code>Environment#root+log/</code>&nbsp;or <code>STDOUT</code> if running on Heroku.
        </td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>
          hostname
        </th>
        <td>
          The hostname the metrics should be aggregrated under. 
        </td>
        <td>
          <code>Socket.gethostname</code>
        </td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>
          proxy
        </th>
        <td>Specify the proxy URL (ex: <code>https://proxy</code>) if a proxy is required. 
        </td>
        <td></td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>
          host
        </th>
        <td>
          The protocol + domain where the agent should report. 
        </td>
        <td>
          <code>https://apm.scoutapp.com</code>
        </td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>
          uri_reporting
        </th>
        <td>
          By default Scout reports the URL and query parameters with transaction traces. To exclude query params, use
          <code>path</code>.
        </td>
        <td>
          <code>full_path</code>
        </td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>
          disabled_instruments
        </th>
        <td>
          An Array of instruments that Scout should not install. Each Array element should should be a string-ified, case-sensitive class name (ex: <code>['Elasticsearch','HttpClient']</code>). The default installed instruments can be viewed in the <a href="https://github.com/scoutapp/scout_apm_ruby/tree/master/lib/scout_apm/instruments" target="_blank">agent source</a>.
        </td>
        <td>
          <code>[]</code>
        </td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>
          ignore
        </th>
        <td>
          An Array of web endpoints that Scout should not instrument. Routes that match the prefixed path (ex: <code>['/health', '/status']</code>) will be ignored by the agent.
        </td>
        <td>
          <code>[]</code>
        </td>
        <td>
          No
        </td>
      </tr>
      <tr>
        <th>dev_trace</th>
        <td>
          Indicates if DevTrace, the Scout development profiler, should be enabled. Note this setting only applies
          to the development environment.
        </td>
        <td>
          <code>false</code>
        </td>
        <td>No</td>
      </tr>
      <tr>
        <th>profile</th>
        <td>
          Indicates if ScoutProf, the Scout code profiler, should be enabled.
        </td>
        <td>
          <code>true</code>
        </td>
        <td>No</td>
      </tr>
    </tbody>
  </table>

### Environment Variables

You can also configure Scout APM via environment variables. _Environment variables override settings provided in_ `scout_apm.yml`.

Environment variables have the same names as those in the Yaml config file, but are prefixed with `SCOUT_`. For example, to set the organization key via environment variables:

```ruby
export SCOUT_KEY=YOURKEY
```

## Request Queuing

Scout can measure the time it takes a request to reach your Rails app from farther upstream (a load balancer or web server). This appears in Scout as "Request Queuing" and provides an indication of your application's capacity. Large request queuing time is an indication that your app needs more capacity.

<img src="images/request_queue_annotated.png" alt="request queuing" />

To see this metric within Scout, you need to configure your upstream software, adding an HTTP header that our agent reads. This is typically a one-line change.

### HTTP Header

The Scout agent depends on an HTTP request header set by an upstream load balancer (ex: HAProxy) or web server (ex: Apache, Ngnix). 

__Protip:__ We suggest adding the header as early as possible in your infrastructure. This ensures you won't miss performance issues that appear before the header is set.

The agent will read any of the following headers as the start time of the request:

```ruby
%w(X-Queue-Start X-Request-Start X-QUEUE-START X-REQUEST-START x-queue-start x-request-start)
```

Include a value in the format `t=MICROSECONDS_SINCE_EPOCH` where `MICROSECONDS_SINCE_EPOCH` is an integer value of the number of microseconds that have elapsed since the beginning of Unix epoch.

__Nearly any front-end HTTP server or load balancer can be configured to add this header.__ Some examples are below.

### Apache

Apache's __mod_headers__ module includes a `%t` variable that is formatted for Scout usage. To enable request queue reporting, add this code to your Apache config:

````
RequestHeader set X-Request-Start "%t"
````

### HAProxy

HAProxy 1.5+ supports timestamped headers and can be set in the frontend or backend section. We suggest putting this in the frontend to get a more accurate number:

````
http-request set-header X-Request-Start t=%Ts
````

### Nginx

Nginx 1.2.6+ supports the use of the `#{msec}` variable. This makes adding the request queuing header straightforward.

General Nginx usage:

````
proxy_set_header X-Request-Start "t=${msec}";
````

Passenger 5+:

````
passenger_set_header X_REQUEST_START "t=${msec}";
````

Older Passsenger versions:

````
passenger_set_cgi_param X_REQUEST_START "t=${msec}";
````

## ActionController::Metal

Scout instruments controllers that inherit from `ActionController::Base` automatically, but not those that inherit from `ActionController::Metal` (including `ActionController::API`). We instrument `ActionController::Base` as this allows us to instrument the full request cycle (which includes before/after filters). If we just instrumented `ActionController::Metal`, we'd miss this instrumentation.

However, adding instrumentation to controllers that inherit from `ActionController::Metal` is simple. Just include our instrumentation library in the controller:

```ruby
class FastController < ActionController::Metal
  include ScoutApm::Instruments::ActionControllerRails3Rails4Instruments
```

This won't interfere with our regular instrumentation. Your controller-action metrics will appear under the endpoints area of Scout, just like any other controller-action.

## Docker <img src="images/docker.png" style="float:right;width: 150px" />

Scout runs within Docker containers without any special configuration. 

It's common to configure Docker containers with environment variables. Scout can use [enviornment variables](#environment-variables) instead of the `scout_apm.yml` config file.

## Heroku <img src="images/heroku.png" style="float:right;width: 150px" />

Scout runs on Heroku without any special configuration. When Scout detects that an app is being served via Heroku:

* Logging is set to `STDOUT` vs. logging to a file. Log messages are prefixed with `[Scout]` for easy filtering.
* The dyno name (ex: `web.1`) is reported vs. the dyno hostname. Dyno hostnames are dynamically generated and don't have any meaningful information.

### Configuration

Scout can be configured via environment variables. This means you can use `heroku config:set` to configure the agent. For example, you can set the application name that appears in the Scout UI with:

```bash
heroku config:set SCOUT_NAME='My Heroku App'
```

See the configuration section for more information on the available config settings and environment variable functionality.

### Using the Scout Heroku Add-on

Scout is also available as a [Heroku Add-on](https://elements.heroku.com/addons/scout). The add-on automates setting the proper Heroku config variables during the provisioning process.

## Cloud Foundry <img src="images/cf_logo.png" style="float:right;width: 150px" />

Scout runs on Cloud Foundry without any special configuration. 

We suggest a few configuration changes in the `scout_apm.yml` file to best take advantage of Cloud Foundry:

1. Set `log_file_path: STDOUT` to send your the Scout APM log contents to the Loggregator. 
2. Use the application name configured via Cloud Foundry to identify the app.
3. Override the hostname reported to Scout. Cloud Foundry hostnames are dynamically generated and don't have any meaningful information. We suggest using a combination of the application name and the instance index.

A sample config for Cloud Foundry that implements the above suggestions:

```yaml
common: &defaults
  key: YOUR_KEY
  monitor: true
  # use the configured application name to identify the app.
  name: <%= ENV['VCAP_APPLICATION'] ? JSON.parse(ENV['VCAP_APPLICATION'])['application_name'] : "YOUR APP NAME (#{Rails.env})" %>
  # make logs available to the Loggregator
  log_file_path: STDOUT
  # reports w/a more identifiable instance name using the application name and instance index. ex: rails-sample.0
  hostname: <%= ENV['VCAP_APPLICATION'] ? "#{JSON.parse(ENV['VCAP_APPLICATION'])['application_name']}.#{ENV['CF_INSTANCE_INDEX']}"  : Socket.gethostname %>

production:
  <<: *defaults

development:
  <<: *defaults
  monitor: false

test:
  <<: *defaults
  monitor: false

staging:
  <<: *defaults
```


## Instrumented Libraries

The following libraries are currently instrumented:

* ActiveRecord
* ActionView
* ActionController
* ElasticSearch
* HTTPClient
* InfluxDB
* Mongoid
* Net::HTTP
* Moped
* Middleware
* Redis
* Sidekiq

Additionally, [Scout can also instrument request queuing time](#request-queuing).

## Adding Custom Context

[Context](#context) lets you see the forest from the trees. For example, you can add custom context to answer critical questions like:

* How many users are impacted by slow requests?
* How many trial customers are impacted by slow requests?
* How much of an impact are slow requests having on our highest paying customers?

It's simple to add [custom context](#context) to your app. There are two types of context:

### User Context

For context used to identify users (ex: email, name):

```ruby
ScoutApm::Context.add_user({})
```

Examples:

```ruby
ScoutApm::Context.add_user(email: @user.email)
ScoutApm::Context.add_user(email: @user.email, location: @user.location.to_s)
```

### General Context

```ruby
ScoutApm::Context.add({})
```

Examples:

```ruby
ScoutApm::Context.add(account: @account.name)
ScoutApm::Context.add(database_shard: @db.shard_id, monthly_spend: @account.monthly_spend)
```

### Default Context

Scout reports the Request URI and the user's remote IP Address by default.

### Context Types

Context values can be any of the following types:

* Numeric
* String
* Boolean
* Time
* Date

### Context Field Name Restrictions

Custom context names may contain alphanumeric characters, dashes, and underscores. Spaces are not allowed.

Attempts to add invalid context will be ignored.

### Example: adding the current user's email as context

Add the following to your `ApplicationController` class:

```ruby
before_filter :set_scout_context
```

Create the following method:

```ruby
def set_scout_context 
	ScoutApm::Context.add_user(email: current_user.email) if current_user.is_a?(User)
end
```

### Example: adding the monthly spend as context

Add the following line to the `ApplicationController#set_scout_context` method defined above:

```ruby
ScoutApm::Context.add(monthly_spend: current_org.monthly_spend) if current_org
```

## Environments

It typically makes sense to treat each environment (production, staging, etc) as a separate application within Scout and ignore the development and test environments.

Here's an example `scout_apm.yml` configuration to achieve this:

```yaml
common: &defaults
  name: <%= "YOUR_APP_NAME (#{Rails.env})" %>
  key: YOUR_KEY
  log_level: info
  monitor: true

production:
  <<: *defaults

development:
  <<: *defaults
  monitor: false
   
test:
  <<: *defaults
  monitor: false

staging:
  <<: *defaults
```

## Disabling a Node

To disable Scout APM on any node in your environment, just set `monitor: false` in your `scout_apm.yml` configuration file on that server, and restart your app server. Example:

```yaml
common: &defaults
  name: <%= "YOUR_APP_NAME (#{Rails.env})" %>
  key: YOUR_KEY
  log_level: info
  monitor: false

production:
  <<: *defaults
```

Since the YAML config file allows ERB evaluation, you can even programatically enable/disable nodes based on host name. This example enables Scout APM on web1 through web5:

```yaml
common: &defaults
  name: <%= "YOUR_APP_NAME (#{Rails.env})" %>
  key: YOUR_KEY
  log_level: info
  monitor: <%= Socket.gethostname.match(/web[1..5]/) %>

production:
  <<: *defaults
```

Aft you've disabled a node in your configuration file and restarted your app server, the node show up as inactive in the UI after 10 minutes.