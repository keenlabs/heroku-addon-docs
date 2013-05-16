The [Keen IO add-on](http://addons.heroku.com/keen) is a [Heroku add-on](http://addons.heroku.com) for doing custom app analytics in minutes instead of days.

Keen IO allows developers to build powerful analytics features directly into their apps with ease. It supports arbitrary event collection, analysis over API, and easy to use SDKs to collect, analyze, and visualize your data.

Keen IO is accessible via an API and has supported client libraries for Java, Ruby, Python, and Node.js.

## Provisioning the add-on

Keen IO can be attached to a Heroku application via the CLI:

```shell
$ heroku addons:add keen
-----> Adding keen to sharp-mountain-4005... done, v18 (free)
```

A list of all plans available can be found [here](http://addons.heroku.com/keen).
 
Once Keen IO has been added, a number of environment variables/settings will be available for your app configuration. They are:

+ KEEN_PROJECT_ID - The Project ID generated for your add-on.
+ KEEN_WRITE_KEY - The Write Key generated for your add-on.
+ KEEN_READ_KEY - The Read Key generated for your add-on.
+ KEEN_API_URL - The URL to make API requests to.

This can be confirmed using the `heroku config:get` command.

```shell
$ heroku config:get KEEN_API_URL
https://api.keen.io
```

After installing Keen IO the application should be configured to fully integrate with the add-on.

## Local setup

### Environment setup

After provisioning the add-on it’s necessary to locally replicate the config vars so your development environment can operate against the service.

Though less portable it’s also possible to set local environment variables using `export KEEN_API_URL=value`.

Use [Foreman](config-vars#local-setup) to configure, run and manage process types specified in your app’s [Procfile](procfile). Foreman reads configuration variables from an .env file. Use the following command to add the Keen IO config values retrieved from heroku config to `.env`.

```shell
$ heroku config -s | grep KEEN >> .env
$ more .env
```

Credentials and other sensitive configuration values should not be committed to source-control. In Git exclude the .env file with: `echo .env >> .gitignore`.

## Using with Ruby/Rails 3.x

Ruby on Rails applications will need to add the following entry into their `Gemfile` specifying the Keen IO client library.

```ruby
gem 'keen'
```

Update application dependencies with bundler.

```shell
$ bundle install
```    

Then send Keen IO an event from anywhere in your code:

```ruby
Keen.publish("sign_ups", { :username => "lloyd", :referred_by => "harry" })
```

This will publish an event to the `sign_ups` collection with the `username` and `referred_by` properties set.

## Using with Python/Django

Install with pip:

```shell
pip install keen
```

Then send Keen IO an event from anywhere in your code:

```python
from keen.client import KeenClient

project_id = "<YOUR_PROJECT_ID>"
write_key  = "<YOUR_WRITE_KEY>"
client = KeenClient(
    project_id, 
    write_key=write_key
)
client.add_event("sign_ups", {
    "username": "lloyd",
    "referred_by": "harry"
}
```

This will publish an event to the 'sign_ups' collection with the `username` and `referred_by` properties set.

## Using with Node

Install with npm:

```shell
npm install keen.io
```

Then send Keen IO an event from anywhere in your code:

```javascript
var keenIO = require('keen.io');

// Configure instance. Only projectId and writeKey are required to send data.
var keen = keenIO.configure({
    projectId: process.env['KEEN_PROJECT_ID'],
    writeKey: process.env['KEEN_WRITE_KEY']
});

keen.addEvent("sign_ups", {"username": "lloyd", "referred_by": "harry"}, function(err, res) {
    if (err) {
        console.log("Oh no, an error!");
    } else {
        console.log("Hooray, it worked!");
    }
});
Keen.publish("sign_ups", { :username => "lloyd", :referred_by => "harry" })
```

This will publish an event to the 'sign_ups' collection with the `username` and `referred_by` properties set.

## Using with Java

Download the jar from http://keen.io/static/code/KeenClient-Java.jar.

Add it to your other external libraries.

Then send Keen IO an event from anywhere in your code:

```java
protected void track() {
    // initialize the Keen Client with your Project ID.
    KeenClient.initialize("<YOUR_PROJECT_ID>", "<YOUR_WRITE_KEY>", "<YOUR_READ_KEY>");

    // create an event to upload to Keen
    Map<String, Object> event = new HashMap<String, Object>();
    event.put("username", "lloyd");
    event.put("referred_by", "harry");

    // add it to the "sign_ups" collection in your Keen Project
    try {
        KeenClient.client().addEvent("sign_ups", event);
    } catch (KeenException e) {
        // handle the exception in a way that makes sense to you
        e.printStackTrace();
    }

}
Keen.publish("sign_ups", { :username => "lloyd", :referred_by => "harry" })
```

This will publish an event to the 'sign_ups' collection with the `username` and `referred_by` properties set.

## Admin Panel

The Keen IO Admin Panel allows you to look at the events you've collected and their properties. It also has an API Workbench to make constructing queries super easy.

The dashboard can be accessed via the CLI:

```shell
$ heroku addons:open keen
Opening keen for sharp-mountain-4005…
```

or by visiting the [Heroku apps web interface](http://heroku.com/myapps) and selecting the application in question. Select Keen IO from the Add-ons menu.

## Troubleshooting

Jump on the [Keen IO Users Chat](http://users.keen.io) or e-mail team@keen.io!

## Migrating between plans

<div class="note" markdown="1">Application owners should carefully manage the migration timing to ensure proper application function during the migration process.</div>

Use the `heroku addons:upgrade` command to migrate to a new plan.

```shell
$ heroku addons:upgrade keen:startup
-----> Upgrading keen:startup to sharp-mountain-4005... done, v18 ($32/mo)
       Your plan has been updated to: keen:startup
```

## Removing the add-on

Keen IO can be removed via the  CLI.

This will make all associated data inaccessible. Contact support if you do this accidentally.

```shell
$ heroku addons:remove keen
-----> Removing keen from sharp-mountain-4005... done, v20 (free)
```

Before removing Keen IO a data export can be performed by doing an [Extraction](https://keen.io/docs/data-analysis/extractions/).

## Support

All Keen IO support and runtime issues should be submitted via one of the [Heroku Support channels](support-channels). Any non-support related issues or product feedback is welcome at team@keen.io.
