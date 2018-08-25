# Task list
- [ ] Go through all PR for the milestone
- [ ] Update Changelog
- [ ] Start a release branch
- [ ] (If needed) Revert whatever unintentional API/ABI breakage that might have occurred in the release branch
- [ ] Bump version to x.x.x in CMakeLists.txt
- [ ] Find replace all occurrences of x.x.x.99 CMake requirements in the tutorials and examples
- [ ] Try to build and test on all relevant platforms
- [ ] Finalize changelog and fix the release date
- [ ] Tag
- [ ] Remove the release branch
- [ ] Add binaries to the GitHub release page.
- [ ] Add link doc link to tutorials page
- [ ] Bump version to x.x.x-dev in CMakeLists.txt
- [ ] Send announcements to the mailing lists
- [ ] Throw a party!

# Creating the Change Log

Change log generation used to be a very tedious tasks which often delayed the release process. Upon preparing the 1.9.0 release, we identified the need to automate this procedure and the `change_log.py` tool was born. This is a tool compatible with Python 3.5+ which generates a markdown Change Log between two revisions.

The generated log lists the most important changes first, like new features and API changes among others, and then proceeds to provide an extensive change list for each individual module.

In order to generate a meaningful change log, we make use of Pull Requests' (PR) metadata such as id, title and labels. This cleared the need for generating a manual change log at the expense of a higher house keeping effort, since we now need to ensure that each PR has accurate labels. There are two important label groups with the following prefixes: `module: ` and `changes: `.

- `module: ` Module tags provide an organization of PRs in terms of the libraries modules. We also include some extra labels which are not exactly library modules per se, for instance things related to CI and CMake, but that are convenient to appear in their own section in the produced log.
- `changes: ` Changes tags are used to flag important API/ABI/behavior changes introduced by PRs.

It's also important to mention the `new feature` label, which arguably should belong to be also prefixed with `changes: ` but currently isn't. Regardless of that, the label is also parsed and new features are reported in the important changes summary.

There's currently no support for the `platform: ` prefix in labels, although it will be extended in the future if the need for it arises.

## Dependencies

The tool has a two 3rd party dependencies:
- `git` - it leverages `git log` functionalities to infer pull requests merges between both revisions
- `requests`- a Python library which simplifies handling authenticated requests to fetch PR data from GitHub's API. Available through PyPi. [Website](http://docs.python-requests.org/en/master/).

## Usage

### Simple

The most simple way of using the tool is simply invoking
```
$ python3 change_log.py pcl-1.8.1 pcl-1.9.0
```
This will generate a list with all PRs included in `pcl-1.9.0`, which were not part of `pcl-1.8.1`, effectively creating a change log between both revisions.

The last argument is actually optional. If no end revision is specified the tool will silently default to the repo's `HEAD`.

### Authenticated Request

`change_log.py` leverages the capabilities of the [GitHub REST API](https://developer.github.com/v3/) to fetch all necessary PR information. The API is free to use but imposes a hourly request rate limit for anonymous requests. This limit is too low to allow fetching the usual amount of data required when generating a change log between contiguous PCL releases. Performing user authenticated requests raises this limit but requires a GitHub account. Unless the amount of changes is small, it is likely that the authentication will always be required in order to fetch all necessary data in a single execution of the tool.

To execute `change_log.py` with GitHub authentication pass the additional argument `--username`
```
$ python3 change_log.py --username PointCloudLibrary pcl-1.8.1 pcl-1.9.0
Password for PointCloudLibrary: 
```
The script will securely prompt for your GitHub password in order to perform the requests. 

### Caching PR Data

Fetching PR data from GitHub is not exactly slow but also not exactly fast. In conjunction with the request rate restrictions, it made it worthwhile to cache the PR data fetched into a file. This is arguably only relevant for development purposes. Specifying the option `--cache` makes the script serialize all the data to a JSON file.

```
$ python3 change_log.py --cache pr_info.json  pcl-1.8.1 pcl-1.9.0
```
To later read from the cache file simply pass the following argument `--from-cache` and the location to the file.
```
$ python3 change_log.py --from-cache pr_info.json pcl-1.8.1
```
If the cache is used no request are made to GitHub. In theory you wouldn't need to specify `pcl-1.8.1` since the cache already holds the start and end revisions used to generate the cache. Anything you specify on that positional argument will be overridden by whatever was saved in the cache.  Unfortunately I'm yet to find a good way to overcome this limitation.


### Printing Usage

To have access to the full list of options simply invoke 

```
$ python3 change_log.py -h
usage: change_log.py [-h] [--username USERNAME] [--cache [FILE] | --from-cache
                     [FILE]]
                     start [end]

Generate a change log between two revisions. Check
https://github.com/PointCloudLibrary/pcl/wiki/Preparing-Releases#creating-the-
change-log for some additional examples on how to use the tool.

positional arguments:
  start                The start (exclusive) revision/commit/tag to generate
                       the log.
  end                  The end (inclusive) revision/commit/tag to generate the
                       log. (Defaults to HEAD)

optional arguments:
  -h, --help           show this help message and exit
  --username USERNAME  GitHub Account user name. If specified it will perform
                       requests with the provided credentials. This is often
                       required in order to overcome GitHub API's request
                       limits.
  --cache [FILE]       Caches the PR info extracted from GitHub into a JSON
                       file. (Defaults to 'pr_info.json')
  --from-cache [FILE]  Uses a previously generated PR info JSON cache file to
                       generate the change log. (Defaults to 'pr_info.json')```
```

# Shipping Binaries
## Mac OSX
Ship only modules enabled by default. Assume only the following optionals: existence of VTK and OpenGL support

* Go to CMakeLists.txt and comment out all section regarding RPATH setup (we should allow this to be specified with CMAKE).
* Configure
```shell
$ cmake .. -DCPACK_GENERATOR="TBZ2" \
           -DCPACK_PACKAGE_FILE_NAME="pcl-1.x.x-darwin"
           -DCMAKE_BUILD_TYPE=Release \
           -DPCL_ENABLE_SSE=OFF \
           -DWITH_CUDA=OFF \
           -DWITH_DAVIDSDK=OFF \
           -DWITH_DOCS=OFF \
           -DWITH_DSSDK=OFF \
           -DWITH_ENSENSO=OFF \
           -DWITH_FZAPI=OFF \
           -DWITH_LIBUSB=OFF \
           -DWITH_OPENGL=ON \
           -DWITH_OPENNI=OFF \
           -DWITH_OPENNI2=OFF \
           -DWITH_PCAP=OFF \
           -DWITH_PNG=OFF \
           -DWITH_QHULL=OFF \
           -DWITH_QT=OFF \
           -DWITH_RSSDK=OFF \
           -DWITH_VTK=ON
$ make -j package
# and pray your system doesn't run out of memory