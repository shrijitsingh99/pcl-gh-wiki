This page intends to answer the most common question currently on our Gitter channel - "How should I start working on a particular idea?". Hopefully we will provide entry points and context for the most current most popular ones.

## Table of Contents
| Name | Skills needed | Difficulty |
|---|---|---| 
| [Update outdated Website](#update-outdated-website) | RST, Sphinx, Doxygen, Python | Medium |
| [Walk through documentation](#walk-through-documentation) | Technical writing, Doxygen | Medium |

## Update Outdated Website

### Context

PCL's website is composed of 5 elements: main page, tutorials (basic and advanced), API reference docs and a blog with posts from prior GSoC initiatives.  The website is and looks old, it has been hacked more than once ([#2864](https://github.com/PointCloudLibrary/pcl/issues/2864)) and it is not versioned i.e., code changes over time and some tutorials need to be updated accordingly, but our users have no easy way to access tutorials matched to a specific version of the library.

The majority of the website's content comes from [here](https://github.com/PointCloudLibrary/pcl/tree/master/doc) (basic and advanced tutorials, and the API reference) and [here](https://github.com/PointCloudLibrary/blog) (blog) and there are still some pages which are only accessible on the server at pointclouds.org and need to be manually edited by a restricted set of users.

The current maintainer group identified that we need a solution that is as maintenance free as possible and that allows the developer community to be fully focused on maintaining its content, in a streamlined and easy way. Solutions of this nature are provided by third-party services and a number of our users pointed us to https://readthedocs.org/ (RTD), as a viable mechanism for what we want to achieve.

Please note that this migration and update process is more of a DevOps type of task than a Web Development one, at least at this stage. While having a custom style and identity for the new website is desired by all, it's not really something we are concerned at this point. The only focus now is to get essential the functionalities up and running.

### Goals

* Create a static website on Github pages or similar - *my suggestion here is to go straight for RTD.*
* Provide a basic replacement including:
  * documentation for past releases - *RTD provides this functionality*
  * prevent invalidation of links as much as possible, while updating the content - *use some scripting language (Python preferred) to crawl for urls and test them*.
* Make it the new home for `pointclouds.org` with HTTPS support - *HTTPS support requires acquiring certificates, so I'm not sure how realistic this is at this point*.
* Integrate versioned (documentation+tutorials) to make it the go-to resource for PCL - *RTD*
* (Stretch goal) Migrate all content from existing website, including 
  * ability to create blog posts (using Jekyll or similar)
  * lost content (from internet archives)

### How to Start

1. Get acquainted with RTD and start mapping the features that they offer to our goals and requirements.
2. Clone PCL and build the docs. We expose three targets for that: `doc`, `tutorials` and `advanced`. Check the CMake source files inside our [docs folder](https://github.com/PointCloudLibrary/pcl/tree/master/doc) and have a look at the [script](https://github.com/PointCloudLibrary/pcl/blob/master/.ci/azure-pipelines/documentation.yml) in which we update the documentation on the website.
3. As a first step ignore versioning. Generate a page which lists all tutorials and generate pages for all of them.
4. Get the API reference on your prototype website.

At this point you're already in a pretty good spot :)

## Walk-through Documentation

### Context

Every piece of code evolves over time. Documentation is written at a particular point in time and as the underlying code evolves, sooner or later some documentation will become outdated and no longer be applicable or simply not represent the current "best practices". Keeping documentation up-to-date ensures a positive experience for the library's users and good quality documentation frees contributors to spend time on improving on the actual code itself. As a new user, the first 

This task presents a three-fold challenge

On one hand it requires some prior knowledge of how the library evolved and what are the current considered best practices e.g.: using PCL in consumer projects. The mentors will need to actively intervene here to ensure the current best practices are followed.

### Goals
[Summer of Docs focus] Includes Tutorials, Guides and Examples
  * Update to reflect forward movement in PCL
  * Detect and increase coverage

### How To Start

Place yourself in the role of a potential new user of the library. What are the questions a new user will ask?

1. 

Author: [@SergioRAgostinho](https://github.com/SergioRAgostinho)