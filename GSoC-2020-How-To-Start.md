This page intends to answer the most common question currently on our Gitter channel - "How should I start working on a particular idea?". Hopefully we will provide entry points and context for the most current most popular ones.

## Table of Contents
| Name | Skills needed | Difficulty |
|---|---|---| 
| [Update outdated Website](update-outdated-website) | RST, Sphinx, Jekyll | Medium |

## Update Outdated Website

### Description

PCL's website is composed of 5 elements: main page, tutorials (basic and advanced), API reference docs and a blog with post from prior GSoC initiatives.  The website is and looks old, it has been hacked more than once ([#2864](https://github.com/PointCloudLibrary/pcl/issues/2864)) and it is not versioned i.e., code changes over time and some tutorials need to be updated accordingly, but our users have no easy way to access tutorials matched to a specific version of the library.

The majority of the website's content comes from here https://github.com/PointCloudLibrary/pcl/tree/master/doc (basic and advanced tutorials, and the API reference) here https://github.com/PointCloudLibrary/blog (blog)

* Create a static website on Github pages or similar
* Provide a basic replacement including:
  * documentation for past releases
  * prevent invalidation of links as much as possible, while updating the content
* Make it the new home for `pointclouds.org` with HTTPS support
* Integrate versioned (documentation+tutorials) to make it the go-to resource for PCL
* (Stretch goal) Migrate all content from existing website, including 
  * ability to create blog posts (using Jekyll or similar)
  * lost content (from internet archives)


Author: @SergioRAgostinho