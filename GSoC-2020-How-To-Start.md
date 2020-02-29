This page intends to answer the most common question currently on our Gitter channel - "How should I start working on a particular idea?". Hopefully we will provide entry points and context for the most current most popular ones.

## Table of Contents
| Name | Skills needed | Difficulty |
|---|---|---| 
| [Update outdated Website](update-outdated-website) | RST, Sphinx, Doxygen, Python | Medium |

## Update Outdated Website

### Context

PCL's website is composed of 5 elements: main page, tutorials (basic and advanced), API reference docs and a blog with post from prior GSoC initiatives.  The website is and looks old, it has been hacked more than once ([#2864](https://github.com/PointCloudLibrary/pcl/issues/2864)) and it is not versioned i.e., code changes over time and some tutorials need to be updated accordingly, but our users have no easy way to access tutorials matched to a specific version of the library.

The majority of the website's content comes from [here](https://github.com/PointCloudLibrary/pcl/tree/master/doc) (basic and advanced tutorials, and the API reference) and [here](https://github.com/PointCloudLibrary/blog) (blog) and there are still some pages which are only accessible on the server at pointclouds.org and need to be manually edited by a restricted set of users.

The current maintainer group identified that we need a solution that is as maintenance free as possible and that allows the developer community to be fully focused on maintaining its content, in a streamlined and easy way. Solutions of this nature are provided by third-party services and a number of our users pointed us to https://readthedocs.org/ (RTD), as a viable mechanism to what we want to achieve.

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

Author: [@SergioRAgostinho](https://github.com/SergioRAgostinho)