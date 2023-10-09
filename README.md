# Concourse CI

This repository contains experiments for 

I'm following the quickstart at https://concourse-ci.org/quick-start.html

## Installation

### Concourse API Server and Database

The tutorial provides a docker-compose.yml that we can use to run Concourse. To start it:

```
docker-compose up -d
```

### fly CLI Tool

I'd prefer not to install a bunch of stuff to my machine, so I use the provided fly Docker image:

```
alias fly() { 
	docker run \
		--network concourse-sample_default \
		--volume $(pwd):/root \
		teliaoss/concourse-fly $@ 
}
```

Note the two Docker arguments that we're providing:
 - `--network`, to connect to the API server in the docker-compose network.
 - `--volume`, to persist the `.flyrc` that is created when we log in.





