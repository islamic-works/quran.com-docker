Quran.com Project - Building Docker Images
==========================================

Each folder have Docker files and other specific parameter files for build Images and Containers for use with Quran.com project development.

## Frontend

## API


## Branchs and Merge

For each context there must be a working branch, and it must be unified with the branch that acts with the contexts composed by the docker-composer files.

You should never take a branch that has more than one context and merge with one of simple context.

For example, there are the following branchs which are simple context "api-v3", "frontend-v1", "frontend-v2", each of these branches have a unique context and will be where to test and develop the parameters. ideal for related project.

Next, we have the following composite branches: "frontend-v1_plus_api-v3" and "frontend-v2_plus_api-v3", in these branches we will have the composition of contexts through the "docker-composer" files that will make the composition of containers according to each one of the contexts, and so it should keep up to date by merging the simple branch into its composite branch,

The composite branch should not be changed except in very specific cases and **SHOULD NEVER** merge into a simple branch, as doing so will take garbage from other contexts.
