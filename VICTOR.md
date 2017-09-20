TellForm 2.0.0
========


## Start

Before you start, make sure you have
1. [Redis](https://redis.io/) installed and running at 127.0.0.1:6379

```bash
 $ redis-server /usr/local/etc/redis.conf
```

2. [MongoDB](https://www.mongodb.com/) installed and running at 127.0.0.1:27017 (OR specify the host and port in config/env/all)

```bash
$ mongod
```

Setup environment.
```bash
$ grunt build
```

.env file example
```
GOOGLE_ANALYTICS_ID=yourGAID
PRERENDER_TOKEN=yourPrerender.ioToken
COVERALLS_REPO_TOKEN=yourCoveralls.ioToken
BASE_URL=localhost
DSN_KEY=yourPrivateRavenKey

# Mail config
MAILER_EMAIL_ID=user@domain.com
MAILER_PASSWORD=some-pass
MAILER_FROM=user@domain.com

# Use this for one of Nodemailer's pre-configured service providers
MAILER_SERVICE_PROVIDER=SendGrid

# Use these for a custom service provider 
# Note: MAILER_SMTP_HOST will override MAILER_SERVICE_PROVIDER
MAILER_SMTP_HOST=smtp.domain.com
MAILER_SMTP_PORT=465
MAILER_SMTP_SECURE=true

```

Side note: ___Currently we are using Raven and Sentry [https://www.getsentry.com](https://www.getsentry.com) for error logging. To use it you must provide a valid private DSN key in your .env file and a public DSN key in app/views/layout.index.html___

#### To run the development version:

Set ```NODE_ENV=development``` in .env file
```$ grunt```

#### To run the production version:

Set ```NODE_ENV=production``` in .env file
```$ grunt```

Your application should run on port 3000 or the port you specified in your .env file, so in your browser just go to [http://localhost:3000](http://localhost:3000)

## Deploying with Docker

To deploy with docker, first install docker [here](https://docs.docker.com/engine/installation/).

Then run these commands

```
$ docker run -p 27017:27017 -d --name some-mongo mongo
$ docker run -p 127.0.0.1:6379:6379 -d --name some-redis redis
$ docker run --rm -p 3000:3000 --link some-redis:redis-db --link some-mongo:db -e "SUBDOMAINS_DISABLED=TRUE" -e "DISABLE_CLUSTER_MODE=TRUE" -e "MAILER_EMAIL_ID=<YourEmailAPI_ID>" -e "MAILER_FROM=<noreply@yourdomain.com>" -e "MAILER_SERVICE_PROVIDER=<YourEmailAPIProvider>"  -e "MAILER_PASSWORD=<YourAPIKey>" -e "BASE_URL=localhost" -p 80:80 tellform/development
```


## Testing Your Application
You can run the full test suite included with TellForm with the test task:

```
$ grunt test
```

This will run both the server-side tests (located in the app/tests/ directory) and the client-side tests (located in the public/modules/*/tests/).

To execute only the server tests, run the test:server task:

```
$ grunt test:server
```

And to run only the client tests, run the test:client task:

```
$ grunt test:client
```

Currently the live example uses heroku github deployments. The Docker file is out of date and does not work. If someone wishes to get it working feel free to submit a pull request.

To calculate your total test coverage with Istanbul, run the coverage task
```bash
$ grunt coverage
```

To calculate your server-side test coverage with Istanbul, run the coverage task
```bash
$ grunt coverage:server
```

To calculate your client-side test coverage with Istanbul, run the coverage task
```bash
$ grunt coverage:client
```


## classic Setup

Also make sure to install [DNS Masq](http://www.thekelleys.org.uk/dnsmasq/doc.html) or equivalent if running it locally on your computer (look at dns_masq_setup_osx for instructions on OSX)

Install dependencies first.
```bash
$ npm install
$ bower install
```

Setup environment.
```bash
$ grunt build
```

Create your user account
```bash
$ node ./scripts/setup.js
```



## FORMA

#### Prerequisite:
Before you start, make sure you have
1. [Redis](https://redis.io/) installed and running at 127.0.0.1:6379
2. [MongoDB](https://www.mongodb.com/) installed and running at 127.0.0.1:27017 (OR specify the host and port in config/env/all)
3. [Docker](https://docs.docker.com/engine/installation/) installed and running

Also make sure to install [DNS Masq](http://www.thekelleys.org.uk/dnsmasq/doc.html) or equivalent if running it locally on your computer (look at dns_masq_setup_osx for instructions on OSX)


#### Install dependencies:
```
$ npm install
$ bower install
```

#### Prepare .env file:
Create .env file at project root folder. Fill in MAILER_EMAIL_ID and MAILER_PASSWORD, and either MAILER_SERVICE_PROVIDER using a [Nodemailer Well-known service](https://nodemailer.com/smtp/well-known/) or MAILER_SMTP_HOST, MAILER_SMTP_PORT, and MAILER_SMTP_SECURE for a custom SMTP server.
```
APP_NAME=forma
APP_DESC=
APP_KEYWORDS=
NODE_ENV=development
BASE_URL=localhost:5000
PORT=5000
username=forma_admin
SIGNUP_DISABLED=false
SUBDOMAINS_DISABLED=true
DISABLE_CLUSTER_MODE=true
GOOGLE_ANALYTICS_ID=
RAVEN_DSN=
PRERENDER_TOKEN=
COVERALLS_REPO_TOKEN=

# Mail config
MAILER_EMAIL_ID=forma@data.gov.sg
MAILER_PASSWORD=some-pass
MAILER_FROM=forma@data.gov.sg

# Use this for one of Nodemailer's pre-configured service providers
MAILER_SERVICE_PROVIDER=

# Use these for a custom service provider 
# Note: MAILER_SMTP_HOST will override MAILER_SERVICE_PROVIDER
MAILER_SMTP_HOST=
MAILER_SMTP_PORT=465
MAILER_SMTP_SECURE=true
```

**Note**: You can view the compatible types for MAILER_SERVICE_PROVIDER [here](https://nodemailer.com/smtp/well-known/)

#### Deploy with Docker:
Create and start mongo & redis docker container.
```
$ docker run -p 27017:27017 -d --name forma-mongo mongo
$ docker run -p 127.0.0.1:6379:6379 -d --name forma-redis redis
```

After code changes, build and start docker containers.
```
$ docker start forma-mongo && docker start forma-redis
$ docker build -t forma-tellform .
$ docker stop forma-tellform
$ docker run --rm -p 5000:5000 --link forma-redis:redis-db --link forma-mongo:db --name forma-tellform forma-tellform
```

Your application should run on port 5000 or the port you specified in your .env file, so in your browser just go to [http://localhost:5000](http://localhost:5000)


[Opensource.com](http://opensource.com/article/17/2/tools-online-surveys-polls)
