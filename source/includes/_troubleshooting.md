# Troubleshooting

## No Data

Not seeing any data?

<table class="help">
    <tbody><tr>
      <td>
        <span class="step">1</span>
      </td>
      <td>
        <p>Is there a <code>log/scout_apm.log</code> file?
        </p>
        <p>
          If not, the gem was never initialized by the application. <a href="#step6">Jump to the last step</a>.<br>If there is a file, examine the file for any error messages:</p>
        <p></p>
<pre>
tail -n1000 log/scout_apm.log | grep "Starting monitoring" -A20
</pre>
          See something noteworthy? Proceed to <a href="#step6">to the last step</a>.
        </p>
        <p>
          If there is no log file, check that the <code>scout_apm</code> gem is not restricted to a certain <code>group</code> in your <code>Gemfile</code>. This would prevent <code>scout_apm</code> from loading in a <code>staging</code> environment:
          <p></p>
<pre>
group :production do
  gem 'unicorn'
  gem 'scout_apm'
end
</pre>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">2</span>
      </td>
      <td>
        <p>
          Using Unicorn?
        </p>
        <p>Add the <code>preload_app true</code> directive to your Unicorn config file. <a href="http://unicorn.bogomips.org/Unicorn/Configurator.html#method-i-preload_app">Read more</a> in the Unicorn docs.
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">3</span>
      </td>
      <td>
        <p>Was the <code><strong>scout_apm</strong></code> gem deployed with your application?
        </p>
<pre>
bundle list scout_apm
</pre>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">4</span>
      </td>
      <td>
        <p>Did you download the config file, placing it in <code><strong>config/scout_apm.yml</strong></code>?
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">5</span>
      </td>
      <td>
        <p>
          Did you restart the app?
        </p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">6</span>
      </td>
      <td>
        <p><a name="step6"></a>Are you sure the application has processed any requests?</p>
<pre>
tail -n1000 log/production.log | grep "Processing"
</pre>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">7</span>
      </td>
      <td>
        <p>Are your controllers inheriting from <code>ActionController::Metal</code> instead of <code>ActionController::Base</code>?</p>
        <p><a href="#actioncontroller-metal">Add instrumentation</a> to your <code>ActionController::Metal</code> controllers.</p>
      </td>
    </tr>
    <tr>
      <td>
        <span class="step">8</span>
      </td>
      <td>
        <p>
          Oops! Looks like messed up. <a href="mailto:support@scoutapp.com">Send us an email</a> with the following:
        </p>
        <ul>
          <li>The last 1000 lines of your <code>log/scout_apm.log</code> file: <code>tail -n1000 log/scout_apm.log</code>.
          </li>
          <li>Your application's gems <code>bundle list</code>.
          </li>
          <li>
            Rails version
          </li>
          <li>
            Application Server (examples: Passenger, Thin, etc.)
          </li>
          <li>
            Web Server (examples: Apache, Nginx, etc.)
          </li>
        </ul>
        <p>
          We typically respond within a couple of hours during the business day.
        </p>
      </td>
    </tr>
  </tbody></table>

## Missing memory metrics

Memory allocation metrics are available version 2.0 of our agent, which is in BETA. To add memory metrics, modify your Gemfile entry for Scout:

<code>gem 'scout_apm', '~> 2.0.x'</code>

Then run `bundle update scout_apm` and deploy.

## Service Status

We're transparent about our uptime and service issues. If you appear to be experiencing issues with our service:

* [Check out status site](http://status.scoutapp.com). You can subscribe to service incidents. 
* [Email us](mailto:support@scoutapp.com)

## Contacting Support

Don't hesitate to contact us at [support@scoutapp.com](mailto:support@scoutapp.com) any issues. We typically respond in a couple of hours during the business day.