# A Dockerized Selenium Grid with Nightwatch

I built this project to quickly provision a dockerized environment for running
UI tests against a dockerized app. It employs a dockerized [Selenium Grid](https://github.com/SeleniumHQ/selenium/wiki/Grid2),
which yields a far more cost-effective CI solution compared to purchasing and maintaining *n* dedicated machines.

This project is geared toward a Node.js audience, meaning I've included `npm`
scripts as wrappers for the `docker-compose` commands. Hopefully, once you've
completed the initial setup, you won't have to recall any docker commands. :smiley:

Nightwatch serves as the testrunner. It is automatically provisioned in the
`nightwatch` docker image, which you can easily customize in the included `nightwatch.json` file.

## Dependencies (OSX)

1. a copy of this repo on your machine
2. [homebrew package manager](http://brew.sh/)
3. docker, docker-machine, and docker-compose: `$ brew install docker docker-machine docker-compose`
4. optional extras to make your life easier: [dinghy](https://github.com/codekitchen/dinghy)
   
## Dependencies (Linux)

1. a copy of this repo on your machine
2. [docker and docker-compose](https://docs.docker.com/engine/installation/linux/)
3. optional extras to make your life easier: [dory](https://github.com/FreedomBen/dory)

### A note on dinghy and dory

`dinghy` and `dory` are excellent Docker utilities for MacOSX and Linux, respectively. They simplify your dockerized
development workflow in multiple ways, perhaps the most convenient of which is this: instead of viewing your dockerized 
web app in your browser with `http://$(docker-machine ip):<port>`, you can simply go to `http://myapp.docker`.

_Both `dinghy` and `dory` are optional dependencies, and one may certainly use the bare-bones Docker ecosystem 
(and [docker-grid-nightwatch](https://github.com/mycargus/docker-grid-nightwatch)) without them._

## Setup

By default this project will use [a bare-bones Sinatra web app](https://github.com/mycargus/hello_docker_world) as the 
system under test (SUT). 

If you'd like to see this project in action before adding your app, go ahead and skip to the 
["How do I execute the tests?"](https://github.com/mycargus/docker-grid-nightwatch/blob/master/README.md#how-do-i-execute-the-tests) section.

## Where do I add my app?

Add the docker image of the SUT to the `docker-compose.yml` file under the `web` service container. 

If you're using `dinghy` or `dory`, be sure to define the SUT's virtual URL (a default is provided). For example:

```
web:
  image: my-app-under-test:latest
  environment:
    VIRTUAL_HOST: myapp.docker
```

That was easy!

If you're not sure how to create or pull a docker image, I recommend working through the official Docker tutorial located on
their website.

## How do I execute the tests?

Start the Selenium hub, the SUT, and the Selenium browser nodes:

```sh
$ npm start
```

Execute the tests with Nightwatch:

```sh
$ npm test
```

When you're done, stop and remove the docker containers:

```sh
$ npm stop
```

Alternatively, if you don't want to install Node on your native machine, you may use the included `bin/` scripts. For example:

```sh
bin/start
bin/test
bin/stop
```

## I want to see the app under test. How can I do that?

Open your browser and go to http://hello.docker. Easy!

If you changed the value of `VIRTUAL_HOST` for the web service in your
docker-compose.yml config, then you'll want to open that URL instead.

## Can I view the Selenium grid console?

Yep! After having started the Selenium hub and nodes (`$ npm start`), open a
browser and go to [http://selenium.hub.docker](http://selenium.hub.docker), then click the 'console' link.

## A test is failing. How do I debug it?

Start the Selenium hub, the app under test, and the Selenium *debug* browser nodes:

```sh
$ npm run debug_start
```

View the chrome debug node via VNC (password: `secret`):

```sh
$ open vnc://node.chrome.debug.docker
```

View the firefox debug node via VNC (password: `secret`):

```sh
$ open vnc://node.firefox.debug.docker
```

Next execute the Nightwatch tests against the debug nodes:

```sh
$ npm run debug_test
```

Again, if you don't want to install Node on your native machine, you may use the included `bin/` scripts:

```sh
bin/debug_start
bin/debug_test
```
