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

### Connect to GitHub

Works automatically for public repos. See "git" resource documentation for auth methods for private repos.

## Interact

```
fly -t tutorial set-pipeline -p hello-world -c ./sample-pipelines/hello-world.yml
# pipelines are paused when first created
fly -t tutorial unpause-pipeline -p hello-world
# trigger the job and watch it run to completion
fly -t tutorial trigger-job --job hello-world/hello-world-job --watch
```

This is wild - `fly intercept`, plug in to a build container to investigate it. 
`./fly -t tutorial intercept -u http://localhost:8080/teams/main/pipelines/hello-world/resources/repo`


## Open Questions

### How can I share a resource with many pipelines?

Looks like v5.0.0 introduced "Global Resources", https://concourse-ci.org/global-resources.html . Global resources deduplicate "check" commands based on `resources.type` and `resources.source`. 

For my use case, I'd like to share one private SSH key with many repositories, so that anyone who creates a build is able to clone private repos. It doesn't seem like this is really supported - if `resources.source` has to be identical, I'd need to provide the private SSH key in every config.

https://concourse-ci.org/creds.html might have more of what I'm looking for. 

### How do I add new folders to the UI? There's no "main" in the yml or the commands, I guess there's a default folder. I think this might be called "teams"?

### How do you develop on a Concourse repo? Can I apply a configuration just to one branch, and then merge it? 
	How can I apply new configs on `git push`, without requiring a manual step?
	https://concourse-ci.org/multi-branch-workflows.html probably has some details on this

### How do I clone a repo? I have a "git" resource set up to trigger new builds, can I also use that to get access to that commit's code?
I know we have the 'git' resource. Does this clone the full repo every time? Do we have any caching here?

### How do I securely store an SSH key for polling private GitHub repos?
	I think I'd want some way to provide one resource to many build plans.
	https://concourse-ci.org/global-resources.html

## Answered Questions

### How long has Concourse been around?

Earliest release is 27 Jan 2015: https://github.com/concourse/concourse/releases?page=20 . Based on releases, it looks like the last release by vito was 16 February 2021, maybe the project changed ownership then? 






