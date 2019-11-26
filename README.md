Quran.com Project - Building Docker Images
==========================================

Each folder have Docker files and other specific parameter files for build Images and Containers for use with Quran.com project development.

## First Steps

### Clonning 

For start to work with this images, you need clone this project on you workspace.

Using Docker Build with the URL in GIT format is not indicated, as you need to enter other sensitive and sensitive parameters before building, folders and files like those used by SSH (RSync) are required for deploying in addition to updating and revising the target project sub-module.

The basic command to clone the repository is:

```
$ git clone --recursive git@github.com:islamic-works/guran.com-docker-images.git
```

After that make the change to the context branch you want to work with, then update the git submodule to its context, you can use the command as follows if you will work with the "frontend-v1" context.

```
$ git checkout frontend-v1
$ git submodule update --init --recursive
```

If you want to work on the submodule in any specific branch, you can enter the context folder, the directory where the submodule was cloned and finally checkout to the specific source project branch, then return the root of the docker image project repository and run the docker build command as shown in the next sections.

## Context

### Frontend

Context for Quran.com Frontend project development, testing, and other activities.

For build a image using this context you can use a command like:

```
$ docker build -t quran.com-frontend:docker-new-approach-v2 frontend/
```

### Using the image

The following command allows you to execute the image created above, and adjust its content and behavior, at this point it becomes a running container, when exiting with the exit command, it remains running and is not deleted. It can later be converted into a final image for reuse or even sending to other colleagues.

Considering that you are inside the directory that is the clone of this repository, just use the command below, see that the path used as source is referenced in the command with the help of the pwd variable, in case of windows you should take precautions, the directory then. The question is where is the cloned repository, in my case `~/workspace/quran.com/quran.com-docker` as a submodule of this repository.

```
$ docker run -it --name quran.com --mount "type=bind,source=$(pwd)/frontend/git-repository,target=/tmp/workspace"  quran.com-frontend:docker-new-approach-v2 bash
```

Note that when executing the above command, the supervisor will not be executed, it is the one that keeps the node running answering the requests on port 8000.

### API

Context for Quran.com development, testing, and other API project activities.

For build a image using this context you can use a command like:

```
$ docker build -t quran.com-api:docker-new-approach-v3 api/
```

## Branchs and Merge

For each context there must be a working branch, and it must be unified with the branch that acts with the contexts composed by the docker-composer files.

You should never take a branch that has more than one context and merge with one of simple context.

For example, there are the following branchs which are simple context "api-v3", "frontend-v1", "frontend-v2", each of these branches have a unique context and will be where to test and develop the parameters. ideal for related project.

Next, we have the following composite branches: "frontend-v1_plus_api-v3" and "frontend-v2_plus_api-v3", in these branches we will have the composition of contexts through the "docker-composer" files that will make the composition of containers according to each one of the contexts, and so it should keep up to date by merging the simple branch into its composite branch,

The composite branch should not be changed except in very specific cases and **SHOULD NEVER** merge into a simple branch, as doing so will take garbage from other contexts.
