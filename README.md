# JoliCode's Docker starter kit

## Before using the stack (remove this chapter once done)

After copy/pasting this starter kit to your project and before first launch
you will need to (in this order):

 * edit `env.project_name` in [fabfile.py](fabfile.py) to match your project name
 * find and replace all mentions of `app.test` with the domain of your choice

Example CLI commands make your project locally available on https://local.toto.com:

```bash
grep -lri app.test | xargs sed -i 's/app.test/local.toto.com/g'
```

*Note*: The project name will be used as a prefix for docker container name,
and for some other small things.

Generate the SSL certificate to use in the local stack:

```bash
infrastructure/docker/services/router/generate-ssl.sh
```

You are ready to go!

*Note*: Some Fabric tasks have been added for DX purposes. Checkout and adapt
the tasks `install`, `migrate` and `cache_clear` to your project

## Running the application locally

### Requirements

A docker environment is provided and requires you to have these tools available:

 * docker
 * docker-compose
 * fabric

Install and run `pipenv` to install the required tools:

```bash
pipenv install
```

You can configure your current shell to be able to use fabric commands directly
(without having to prefix everything by `pipenv run`)

```bash
pipenv shell
```

### Domain configuration (first time only)

Before running the app for the first time, ensure the domain name `app.test`
point the IP of your Docker deamon by editing your `/etc/hosts` file.

This IP is probably 127.0.0.1 unless you run Docker in a special VM (docker-machine, dinghy, etc).

```
echo '127.0.0.1 app.test' | sudo tee -a /etc/hosts
```

Using dinghy? Run `dinghy ip` to get the IP of the VM.

### Starting the stack

Launch the stack by running this command:

```bash
fab start
```

> Note: the first start of the stack should take a few minutes.

The site is now accessible at [https://app.test](https://app.test)
(you may need to accept self-signed SSL certificate).

### Builder

Having some composer, yarn or another modifications to make on the project?
Start the builder which will give you access to a container with all these
tools available:

```bash
fab builder
```

### Other tasks

Checkout `fab -l` to have the list of available fabric tasks.
