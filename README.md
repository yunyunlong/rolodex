*Status: DEV*

Rolodex for Open Peer
=====================

Having a distributed and secure communication system like [Open Peer](http://openpeer.org/) is quite useless if you have nobody to talk with.

This rolodex SDK supports the following:

  * Integration with [passport](http://passportjs.org/) for authentication with 120+ services.
  * Contact federation for any service with a *contacts* API (that we have a plugin for).
  * [Q](https://github.com/kriskowal/q) promise based [AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)-compatible client API.
  * [connect](https://github.com/senchalabs/connect) middleware to service client requests.
  * Contact information normalized to [hCard](http://microformats.org/wiki/hcard) properties.
  * Communication with services scheduled via [kickq](https://github.com/verbling/kickq) ( *currently disabled due to kickq [bug](https://github.com/verbling/kickq/issues/5)* )
  * Contacts and service status cached in [redis](http://redis.io/).
  * Easy horizontal round-robin scaling via shared-noting architecture (requires central redis).


Example
-------

    cd example
    make install
    # Configure services in `rolodex.config.json` (see 'Configuration' below)
    make run


Usage
-----

Install:

    npm install openpeer-rolodex
    # Provision a Redis database

When you first run your application, required dependencies for any configured
service will be automatically installed. You can also install these manually:

    cd ./lib/plugin/<service>
    npm install

Integrate:

  * Server-side - See `./example/server.js`.
  * Client-side - See `./example/ui/index.html` and `./example/ui/app.js`.


Configuration
-------------

For each service you want to integrate with you need to:

  1. Create an application on the service.
  2. Configure the service in `rolodex.config.json`.

The `rolodex.config.json` file must be structured as follows:

    {
        "allow": {
            # Optional for cross-domain access.
            "hosts": [
                "localhost"
            ]
        },
        "db": {
            "redis": {
                "host": "<redis host>",
                "port": <redis port>,
                # The following are defaults and may be omitted.
                "password": "",
                "prefix": "rolodex:"
            }
        },
        "routes": {
            # The following are defaults and may be omitted.
            client: "/.openpeer-rolodex/client",
            auth: "/.openpeer-rolodex/auth",
            authCallback: "/.openpeer-rolodex/callback",
            logout: "/.openpeer-rolodex/logout",
            refetch: "/.openpeer-rolodex/refetch",
            services: "/.openpeer-rolodex/services",
            contacts: "/.openpeer-rolodex/contacts"
        },
        "services": [
            // One or more of the service config objects below.
        ]
    }


### [GitHub](https://github.com/)

Create application here https://github.com/settings/applications
with callback URL `http://localhost:8080/.openpeer-rolodex/callback/github`.

    {
        "name": "github",
        "passport": {
            "clientID": "<Client ID>",
            "clientSecret": "<Client Secret>"
        }
    }

### [Twitter](https://twitter.com/)

Create application here https://dev.twitter.com/apps
with callback URL `http://127.0.0.1:8080/.openpeer-rolodex/callback/twitter`.

    {
        "name": "twitter",
        "passport": {
            "consumerKey": "<Consumer key>",
            "consumerSecret": "<Consumer secret>"
        }
    }

### [LinkedIn](http://linkedin.com/)

Create application here https://www.linkedin.com/secure/developer

    {
        "name": "linkedin",
        "passport": {
            "apiKey": "<API Key>",
            "secretKey": "<Secret Key>"
        }
    }

### [Facebook](http://facebook.com/)

Create application here https://developers.facebook.com/apps

    {
        "name": "facebook",
        "passport": {
            "appID": "<App ID>",
            "appSecret": "<App Secret>"
        }
    }


License
=======

[BSD-2-Clause](http://opensource.org/licenses/BSD-2-Clause)
