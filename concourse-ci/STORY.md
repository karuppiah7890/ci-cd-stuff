# Story

I'm checking out Concourse CI a CI/CD tool

I have noticed it in multiple places before

I noticed it today over here

https://tanzu.vmware.com/concourse

https://www.youtube.com/watch?v=0bi_EWzhPvs

https://github.com/concourse/concourse

https://concourse-ci.org/

I got the concourse server

https://github.com/concourse/concourse/releases/download/v7.1.0/concourse-7.1.0-darwin-amd64.tgz

and the fly cli too, a client I guess?

https://github.com/concourse/concourse/releases/download/v7.1.0/fly-7.1.0-darwin-amd64.tgz

It has something called as resources, and also tasks and jobs

https://concourse-ci.org/resources.html

Seems like resource is kind of like the input / output. Git can be an input to
a CI pipeline, so that the pipeline can pull the git repo source code and then
do stuff like build, test etc

Resources also have a type. So, some "core" types are git and s3 it seems.

https://concourse-ci.org/resource-types.html

The full list of community maintained resource types are here it seems

https://resource-types.concourse-ci.org/ (Resource Types Catalog)

I'm going to follow the quick start first! :)

https://concourse-ci.org/quick-start.html

```bash
$ wget https://concourse-ci.org/docker-compose.yml

--2021-04-09 08:17:12--  https://concourse-ci.org/docker-compose.yml
Resolving concourse-ci.org (concourse-ci.org)... 185.199.109.153, 185.199.108.153, 185.199.110.153, ...
Connecting to concourse-ci.org (concourse-ci.org)|185.199.109.153|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 855 [text/yaml]
Saving to: ‘docker-compose.yml’

docker-compose.yml              100%[=======================================================>]     855  --.-KB/s    in 0s      

2021-04-09 08:17:13 (27.2 MB/s) - ‘docker-compose.yml’ saved [855/855]
```

```bash
$ docker-compose up -d
Creating network "concourse-ci_default" with the default driver
Pulling concourse-db (postgres:)...
latest: Pulling from library/postgres
75646c2fb410: Pull complete
2355d0ffeb55: Pull complete
7e98825f6d67: Pull complete
cfd3ce06be45: Pull complete
c7b7bb83e8f7: Pull complete
c67869305108: Pull complete
19614baa7ddd: Pull complete
af508737d813: Pull complete
b60c3437a436: Pull complete
424da1ff3ea9: Pull complete
076f107f1898: Pull complete
7b398ea488bf: Pull complete
e0fcc114ae29: Pull complete
67d927dd9b8a: Pull complete
Digest: sha256:b25265ac1dfa19224fd47dd9f5744aa177248fd64e89f407446559cc7dbc7a23
Status: Downloaded newer image for postgres:latest
Pulling concourse (concourse/concourse:)...
latest: Pulling from concourse/concourse
92dc2a97ff99: Pull complete
be13a9d27eb8: Pull complete
c8299583700a: Pull complete
f2b7569ce3d2: Pull complete
a1124fe8176e: Pull complete
25c84d6eaab1: Pull complete
Digest: sha256:9adc59ea1ccdb2d0262451d30ff0298dc92139ba7cfb8bfd99b1a469441594e0
Status: Downloaded newer image for concourse/concourse:latest
Creating concourse-ci_concourse-db_1 ... done
Creating concourse-ci_concourse_1    ... done
```

That was a really big image of concourse, hmm

```bash
$ docker-compose ps 
           Name                          Command               State           Ports         
---------------------------------------------------------------------------------------------
concourse-ci_concourse-db_1   docker-entrypoint.sh postgres    Up      5432/tcp              
concourse-ci_concourse_1      dumb-init /usr/local/bin/e ...   Up      0.0.0.0:8080->8080/tcp
```

I'm downloading the fly CLI from the web UI

http://localhost:8080/api/v1/cli?arch=amd64&platform=darwin

```bash
$ mv ~/Downloads/fly /usr/local/bin/fly
$ chmod +x /usr/local/bin/fly
```

```bash
$ fly
Killed: 9

$ sudo xattr -d com.apple.quarantine /usr/local/bin/fly
Password:
```

```
$ fly
Usage:
  fly [OPTIONS] <command>

Application Options:
  -t, --target=              Concourse target name
  -v, --version              Print the version of Fly and exit
      --verbose              Print API requests and responses
      --print-table-headers  Print table headers even for redirected output

Help Options:
  -h, --help                 Show this help message

Available commands:
  abort-build               Abort a build (aliases: ab)
  active-users              List the active users since a date or for the past 2 months (aliases: au)
  archive-pipeline          Archive a pipeline (aliases: ap)
  builds                    List builds data (aliases: bs)
  check-resource            Check a resource (aliases: cr)
  check-resource-type       Check a resource-type (aliases: crt)
  checklist                 Print a Checkfile of the given pipeline (aliases: cl)
  clear-task-cache          Clears cache from a task container (aliases: ctc)
  completion                generate shell completion code
  containers                Print the active containers (aliases: cs)
  curl                      curl the api (aliases: c)
  delete-target             Delete target (aliases: dtg)
  destroy-pipeline          Destroy a pipeline (aliases: dp)
  destroy-team              Destroy a team and delete all of its data (aliases: dt)
  disable-resource-version  Disable a version of a resource (aliases: drv)
  edit-target               Edit a target (aliases: etg)
  enable-resource-version   Enable a version of a resource (aliases: erv)
  execute                   Execute a one-off build using local bits (aliases: e)
  expose-pipeline           Make a pipeline publicly viewable (aliases: ep)
  format-pipeline           Format a pipeline config (aliases: fp)
  get-pipeline              Get a pipeline's current configuration (aliases: gp)
  get-team                  Show team configuration (aliases: gt)
  help                      Print this help message
  hide-pipeline             Hide a pipeline from the public (aliases: hp)
  hijack                    Execute a command in a container (aliases: intercept, i)
  jobs                      List the jobs in the pipelines (aliases: js)
  land-worker               Land a worker (aliases: lw)
  login                     Authenticate with the target (aliases: l)
  logout                    Release authentication with the target (aliases: o)
  order-pipelines           Orders pipelines (aliases: op)
  pause-job                 Pause a job (aliases: pj)
  pause-pipeline            Pause a pipeline (aliases: pp)
  pin-resource              Pin a version to a resource (aliases: pr)
  pipelines                 List the configured pipelines (aliases: ps)
  prune-worker              Prune a stalled, landing, landed, or retiring worker (aliases: pw)
  rename-pipeline           Rename a pipeline (aliases: rp)
  rename-team               Rename a team (aliases: rt)
  rerun-build               Rerun a build (aliases: rb)
  resource-versions         List the versions of a resource (aliases: rvs)
  resources                 List the resources in the pipeline (aliases: rs)
  schedule-job              Request the scheduler to run for a job. Introduced as a recovery command for the v6.0 scheduler. (aliases: sj)
  set-pipeline              Create or update a pipeline's configuration (aliases: sp)
  set-team                  Create or modify a team to have the given credentials (aliases: st)
  status                    Login status
  sync                      Download and replace the current fly from the target (aliases: s)
  targets                   List saved targets (aliases: ts)
  teams                     List the configured teams (aliases: t)
  trigger-job               Start a job in a pipeline (aliases: tj)
  unpause-job               Unpause a job (aliases: uj)
  unpause-pipeline          Un-pause a pipeline (aliases: up)
  unpin-resource            Unpin a resource (aliases: ur)
  userinfo                  User information
  validate-pipeline         Validate a pipeline config (aliases: vp)
  volumes                   List the active volumes (aliases: vs)
  watch                     Stream a build's output (aliases: w)
  workers                   List the registered workers (aliases: ws)
```

I logged into the Web UI using `test` username and `test` password.

I did the same in the cli client

```bash
$ fly -t tutorial login -c http://localhost:8080 -u test -p test
logging in to team 'main'


target saved
```

Next I'm here

https://concoursetutorial.com/basics/task-hello-world/

```bash
$ git clone https://github.com/starkandwayne/concourse-tutorial.git
Cloning into 'concourse-tutorial'...
remote: Enumerating objects: 3834, done.
remote: Total 3834 (delta 0), reused 0 (delta 0), pack-reused 3834
Receiving objects: 100% (3834/3834), 11.19 MiB | 9.40 MiB/s, done.
Resolving deltas: 100% (2297/2297), done.
$ ls
STORY.md		concourse-tutorial	docker-compose.yml
```

```bash
$ cd concourse-tutorial/
$ ls
CODE_OF_CONDUCT.md	VERSION			docs			mkdocs.yml		tutorials
README.md		ci			manifest-develop.yml	test
Staticfile		docker-compose.yml	manifest-production.yml	theme-overrides
```

```bash
$ cd tutorials/basic/task-hello-world
```

```bash
$ fly -t tutorial execute -c task_hello_world.yml
executing build 1 at http://localhost:8080/builds/1
initializing
selected worker: b593714cd460

selected worker: b593714cd460
waiting for docker to come up...
Pulling busybox@sha256:1ccc0a0ca577e5fb5a0bdf2150a1a9f842f47c8865e861fa0062c5d343eb8cac...
docker.io/library/busybox@sha256:1ccc0a0ca577e5fb5a0bdf2150a1a9f842f47c8865e861fa0062c5d343eb8cac: Pulling from library/busybox
f531cdc67389: Pulling fs layer
f531cdc67389: Download complete
f531cdc67389: Pull complete
Digest: sha256:1ccc0a0ca577e5fb5a0bdf2150a1a9f842f47c8865e861fa0062c5d343eb8cac
Status: Downloaded newer image for busybox@sha256:1ccc0a0ca577e5fb5a0bdf2150a1a9f842f47c8865e861fa0062c5d343eb8cac
docker.io/library/busybox@sha256:1ccc0a0ca577e5fb5a0bdf2150a1a9f842f47c8865e861fa0062c5d343eb8cac

Successfully pulled busybox@sha256:1ccc0a0ca577e5fb5a0bdf2150a1a9f842f47c8865e861fa0062c5d343eb8cac.

selected worker: b593714cd460
running echo hello world
hello world
succeeded
```

The `b593714cd460` is the concourse server docker container I think

```bash
$ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                    NAMES
b593714cd460   concourse/concourse   "dumb-init /usr/loca…"   10 minutes ago   Up 10 minutes   0.0.0.0:8080->8080/tcp   concourse-ci_concourse_1
d703675118ec   postgres              "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes   5432/tcp                 concourse-ci_concourse-db_1
```

Yup, notice the `b593714cd460` in the `docker ps` output for container ID of the
concourse server container!

Hmm

```bash
$ cat task_hello_world.yml 
---
platform: linux

image_resource:
  type: docker-image
  source: {repository: busybox}

run:
  path: echo
  args: [hello world]
```

```bash
$ fly -t tutorial execute -c task_ubuntu_uname.yml
executing build 2 at http://localhost:8080/builds/2
initializing
selected worker: b593714cd460
selected worker: b593714cd460
waiting for docker to come up...
Pulling ubuntu@sha256:5403064f94b617f7975a19ba4d1a1299fd584397f6ee4393d0e16744ed11aab1...
docker.io/library/ubuntu@sha256:5403064f94b617f7975a19ba4d1a1299fd584397f6ee4393d0e16744ed11aab1: Pulling from library/ubuntu
a70d879fa598: Pulling fs layer
c4394a92d1f8: Pulling fs layer
10e6159c56c0: Pulling fs layer
c4394a92d1f8: Download complete
10e6159c56c0: Verifying Checksum
10e6159c56c0: Download complete
a70d879fa598: Verifying Checksum
a70d879fa598: Download complete
a70d879fa598: Pull complete
c4394a92d1f8: Pull complete
10e6159c56c0: Pull complete
Digest: sha256:5403064f94b617f7975a19ba4d1a1299fd584397f6ee4393d0e16744ed11aab1
Status: Downloaded newer image for ubuntu@sha256:5403064f94b617f7975a19ba4d1a1299fd584397f6ee4393d0e16744ed11aab1
docker.io/library/ubuntu@sha256:5403064f94b617f7975a19ba4d1a1299fd584397f6ee4393d0e16744ed11aab1

Successfully pulled ubuntu@sha256:5403064f94b617f7975a19ba4d1a1299fd584397f6ee4393d0e16744ed11aab1.

selected worker: b593714cd460
running uname -a
Linux 3e3a7174-4f67-4c2a-6b6f-13903dcdb64c 4.19.121-linuxkit #1 SMP Thu Jan 21 15:36:34 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
succeeded
```

```bash
$ cat task_ubuntu_uname.yml 
---
platform: linux

image_resource:
  type: docker-image
  source: {repository: ubuntu}

run:
  path: uname
  args: [-a]
```
