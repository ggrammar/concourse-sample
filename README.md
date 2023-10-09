# Concourse CI

This repository contains a tutorial implementation and experiments for Concourse CI.

I'm following the quickstart at https://concourse-ci.org/quick-start.html

## Installation

### Concourse API Server and Database

The tutorial provides a docker-compose.yml that we can use to run Concourse. To start it, `docker-compose up -d`.

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

TODO: Latest concourse-fly is 7.2.0, but server runs 7.10.0, and they're not compatible. Build our own docker image?
NOTE: Correct version of CLI can be downloaded from the server at http://localhost:8080/api/v1/cli?arch=amd64&platform=darwin

## Interact

```
fly -t tutorial set-pipeline -p hello-world -c ./sample-pipelines/hello-world.yml
# pipelines are paused when first created
fly -t tutorial unpause-pipeline -p hello-world
# trigger the job and watch it run to completion
fly -t tutorial trigger-job --job hello-world/hello-world-job --watch
```


## Open Questions

How long has Concourse been around? Earliest Docker image https://hub.docker.com/r/teliaoss/concourse-fly/tags?page=1&ordering=-last_updated is from April 2021, so 2.5 years. 
	https://github.com/concourse/concourse/releases?page=20 suggests Jan 27th 2015, 
	last release by vito was Feb 16 2021, maybe project changed ownership?

How do I add new folders to the UI? There's no "main" in the yml or the commands, I guess there's a default folder. 




