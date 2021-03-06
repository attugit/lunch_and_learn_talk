## Lunch & Learn
### Quick project setup with use of FOSS

#### Karol Wozniak, 2015

----

### *Less Code == More Software*

----

### FOSS
#### Free Open Source Software

[OpenHub](https://www.openhub.net/)

====

### Outline
 - repository hosting
 - continuous integration
 - project documentation
 - programming blogs

----

### repository hosting
 - [github](https://github.com/)
 - [gitolite](https://github.com/sitaramc/gitolite)

----

### continuous integration

 - [travis-ci](https://travis-ci.org/)

----

### project documentation

 - [gh-pages](https://pages.github.com/)
 - [sphinx](http://sphinx-doc.org)

----

### programming blogs

 - [jekyll](http://jekyllrb.com/)

----

### bonus: C++ build tool

 - [premake](http://premake.github.io)

====

### GitHub

https://github.com

![](./assets/GitHub-Mark-Light-120px-plus.png)

### no limits

 - free public repos
 - collaborators
 - organization members

----

### organizations

 - [Google](https://github.com/google)
 - [Facebook](https://github.com/facebook)
 - [Microsoft](https://github.com/Microsoft)
 - [Amazon](https://github.com/awslabs)
 - [Mozilla](https://github.com/mozilla)
 - [Electronic Arts](https://github.com/electronicarts)
 - [Adobe](https://github.com/adobe)
 - [Nokia Networks](https://github.com/nokia-wroclaw)
 - many others...

----

### projects & mirros

 - [bootstrap](https://github.com/twbs/bootstrap)
 - [Ruby on Rails](https://github.com/rails/rails)
 - [Node.js](https://github.com/joyent/node)
 - [jQuery](https://github.com/jquery/jquery)
 - [impress.js](https://github.com/bartaz/impress.js)
 - [Homebrew](https://github.com/Homebrew/homebrew)
 - [reveal.js](https://github.com/hakimel/reveal.js)
 - [.NET](https://github.com/Microsoft/dotnet)
 - [mono](https://github.com/mono/mono)

----

### more...

 - [linux](https://github.com/torvalds/linux)
 - [git](https://github.com/git/git)
 - [llvm](https://github.com/llvm-mirror/llvm)
 - [gcc](https://github.com/gcc-mirror/gcc)
 - [clang](https://github.com/llvm-mirror/clang)
 - [boost](https://github.com/boostorg)
 - [ISO C++](https://github.com/cplusplus)
 - [CppCon](https://github.com/CppCon)

----

### what else...?

 - French [Civil Code](https://github.com/steeve/france.code-civil)
 - Polish [2014 electoral system source](https://github.com/wybory2014/Kalkulator1)

----
### Luxoft at GitHub

https://github.com/Luxoft

![](./assets/luxoft_logo.png)

----

### Twister

http://www.twistertesting.com

https://github.com/Luxoft/Twister

----

### me on GitHub

https://github.com/attugit

[This talk](http://attugit.github.io/lunch_and_learn_talk)
/ [src](https://github.com/attugit/lunch_and_learn_talk)

----

### creating repository

![](./assets/github_new_repo.png)

----

### creating repository

New
```
$ git clone git@github.com:attugit/newrepo.git
$ cd lunch_and_learn_talk/
```

Add existing
```
$ cd local_repo/
$ git remote add origin git@github.com:attugit/newrepo.git
$ git push -u origin master
```

----

### advantages

 - free
 - community
 - ssh key authorization
 - integration
   - issues
   - pull requests
   - gh-pages
   - more...

====

### Gitolite

http://gitolite.com

simple alternative

----

### file structure

```
$ tree gitolite-admin/
  gitolite-admin/
  |-- conf/
  |   `-- gitolite.conf
  `-- keydir/
      |-- admin.pub
      |-- dilbert.pub
      |-- alice.pub
      `-- buildbot.pub

  2 directories, 5 files
```

----

### basic configuration

```
$ cat conf/gitolite.conf
  @staff              =   dilbert alice

  repo gitolite-admin
      RW+             =   admin

  repo libby
      RW+             =   @staff
      R               =   buildbot
```

----

### creating new repo

```
$ git diff -U0
  diff --git a/conf/gitolite.conf b/conf/gitolite.conf
  index 2aa8c94..4eb0f12 100644
  --- a/conf/gitolite.conf
  +++ b/conf/gitolite.conf
  @@ -31,0 +32,3 @@ repo libby
  +
  +repo newrepo
  +    RW+     =   @staff
$ git commit -a -m "create newrepo"
$ git push -u gitolite master
```

```
$ cd ~/workspace/
$ git clone gitolite@hostname:newrepo.git
```

----

### advantages

 - free
 - simple configuration
 - ssh key authorization

----

### other

 - [Bitbucket](https://bitbucket.org/)
 - [GitLab](https://about.gitlab.com/)
 - [Bluemix](https://hub.jazz.net/)

====

### travis-ci

https://travis-ci.org

https://github.com/travis-ci

![](./assets/travis-mascot-200px.png)

----

### single configuration file

```
$ file .travis.yml
  .travis.yml: ASCII text
```

----

### enviorment

```
language: cpp
os: linux
env:
  global:
  - GCC_VERSION="4.9"
  - CLANG_VERSION="3.5"

```

----

### build matrix

```
matrix:
  include:
    - compiler: gcc
      env: MODE="Release"
    - compiler: gcc
      env: MODE="Debug"
    - compiler: clang
      env: MODE="Release"
  allow_failures:
    - compiler: clang
      env: MODE="Debug"
```

----

### toolchain

```
before_install:
  - sudo add-apt-repository --yes ppa:ubuntu-toolchain-r/test;
  - sudo apt-get update -qq;

install:
  - sudo apt-get install -qq g++-${GCC_VERSION};
  - export CXX="g++-${GCC_VERSION}" CC="gcc-${GCC_VERSION}";
  - sudo apt-get -qq install valgrind;
```

----

### build

```
script:
  - ./configure --testcmd="valgrind -q --error-exitcode=1";
  - make config=${MODE} -C build -f Makefile;
```

----

### other example

```
language: ruby
cache: bundler
sudo: false
rvm:
  - 2.1
install:
  - bundle install
script: make build
branches:
  only:
    - master
    - post_dev
env:
  global:
    - NOKOGIRI_USE_SYSTEM_LIBRARIES=true
```

----

### build history

![](./assets/travis_build_history.png)

----

### build history

![](./assets/travis_current_build.png)

----

### live

 - my [library](https://travis-ci.org/attugit/archie/builds)
 - Google [protobuf](https://travis-ci.org/google/protobuf)
 - MS [TypeScript](https://travis-ci.org/Microsoft/TypeScript)

----

### advantages

 - free
 - community
 - fast
 - reliable
 - simple configuration
 - configuration in repo

====

### gh-pages

https://pages.github.com/

Project website hosted by GitHub

----

### account website

1. create repo called `username.github.io`
2. push something
    ```
    $ git clone git@github.com:\
      >  ${USERNAME}/${USERNAME}.github.io.git
    $ cd ${USERNAME}.github.io
    $ echo "hello world" > index.html
    $ git commit -a -m "User website"
    $ git push -u origin master
    ```
3. open `http://username.github.io/`

----

### repo website

1. create branch called `gh-pages`
    ```
    $ git branch gh-pages
    $ git checkout gh-pages
    ```
2. write something
    ```
    $ echo "hello world" > index.html
    ```
3. push it to GitHub
    ```
    $ git commit -a -m "Repo website"
    $ git push -u origin gh-pages
    ```

----

### examples

 - [this talk](http://attugit.github.io/lunch_and_learn_talk/)
 - Google [protobuf](https://google.github.io/protobuf/)
 - Facebook [react](http://facebook.github.io/react/)
 - Facebook [rocksdb](https://facebook.github.io/rocksdb/)
 - [Microsoft](http://microsoft.github.io/) on GitHub

----

### advantages

 - free
 - automated
 - flexible
 - under VC

====

### Sphinx

![](./assets/sphinx-header.png)

----

### examples

 - [python](https://docs.python.org/3/)
 - [pip](https://pip.pypa.io/en/latest/)
 - [cython](http://docs.cython.org/)
 - [numpy](http://docs.scipy.org/doc/numpy/reference/)
 - [pyre](http://docs.danse.us/pyre/sphinx/)
 - [OpenCV](http://docs.opencv.org/)
 - [jinja](http://jinja.pocoo.org/docs/dev/)
 - [Spring Python](http://docs.spring.io/spring-python/1.2.x/sphinx/html/)
 - [django](https://docs.djangoproject.com/en/1.8/)

----

### setup

```
$ sphinx-quickstart ./doc
$ tree doc
  doc/
  |-- _build/
  |-- _static/
  |-- _templates/
  |-- conf.py
  |-- index.rst
  |-- make.bat
  `-- Makefile

  3 directories, 4 files
```

----

### minimal configuration

```
$ cat doc/conf.py
  import sys
  import os

  templates_path = ['_templates']
  source_suffix = '.rst'
  master_doc = 'index'
  project = u'libby'
  copyright = u'2015, Dilbert'
  version = '1.0'
  release = '1'
  exclude_patterns = ['_build']
  pygments_style = 'sphinx'
  html_theme = 'default'
  html_static_path = ['_static']
  htmlhelp_basename = 'libbydoc'
```

----

### minimal content

```
$ cat doc/index.rst
  Welcome to libbys's documentation!
  ##################################

  Contents:

  .. toctree::
     :maxdepth: 2

  Indices and tables
  ##################

  * :ref:`genindex`
  * :ref:`modindex`
  * :ref:`search`
```

----

### supported formats

```
$ make
  changes     devhelp     doctest     gettext
  html        info        latex       latexpdfja
  man         pseudoxml   singlehtml  text
  clean       dirhtml     epub        help
  htmlhelp    json        latexpdf    linkcheck
  pickle      qthelp      texinfo     xml
```

----

### source

![](./assets/sphinx_src.png)

----

### html

![](./assets/sphinx_html.png)

----

### pdf

![](./assets/sphinx_pdf.png)

----

### advantages

 - free
 - simple configuration
 - automated
 - plain text

====

### Jekyll

http://jekyllrb.com

![](./assets/jekyll-logo-dark-transparent.png)

----

### content

Plain text: markdown, css.

![](./assets/codig-session-src.png)

----

### static site

![](./assets/codig-session-html.png)

[Coding Session](http://attugit.github.io/)

----

### themes

http://jekyllthemes.org/

----

### localhost

```
$ jekyll build --watch &
$ jekyll serve
  Server address: http://0.0.0.0:4000/
  Server running... press ctrl-c to stop.
```

----

### configuration

```
$ cat _config.yml
  name: Coding Session
  description: Simple blog about programming
  url: http://attugit.github.io
  baseurl: ''
  github: 'attugit/archie'
  gaaccount: 'XX-11111111-1'
  disqus: ''
  comments: true
  permalink: /:year/:month/:title
  paginate: 3
  highlighter: pygments
  markdown: redcarpet
```

----

### advantages

 - free
 - plain text
 - simple
 - themes
 - features
 - disqus
 - RSS
 - deploy with gh-pages

====

### other

https://github.com/ripienaar/free-for-dev

====

# Q&A

====

### bonus: premake4

[industriousone](http://industriousone.com/premake-quick-start)

![](./assets/premake-logo.png)

----

### project layout

```
$ tree project/
  project/
  |-- premake4
  |-- premake4.lua
  |-- inc/
  |   `-- include_me.h
  `-- src/
      |-- bin/
      |   `-- main.cpp
      `-- lib/
          |-- build_me.cpp
          |-- build_me_too.cpp
          `-- internal.h

  4 directories, 7 files
```

----

### premake binary

```
$ stat --format="%A %s %n" project/premake4
  -rwxr-xr-x 339952 project/premake4
$ ldd project/premake4
  linux-vdso.so.1 (0x00007ffe1bfa4000)
  libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f779c23b000)
  libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f779c037000)
  libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f779bc8d000)
  /lib64/ld-linux-x86-64.so.2 (0x00007f779c55c000)
```

----

### configuration

```
$ cat project/premake4.lua
  _ACTION = _ACTION or "gmake"

  local workspace = solution "project"
    location ( "build" )
    targetdir ( "build" )
    includedirs { "inc" }
    configurations { "debug", "release" }
    language "C++"
    flags { "Cpp11", "ExtraWarnings", "FatalWarnings"}
    configuration "release"
      flags { "OptimizeSpeed" }
    configuration "debug"
      flags { "Symbols" }
```

---


```
  local lib = project "libby"
    kind "SharedLib"
    includedirs { "src" }
    files { "src/lib/*.cpp" }

  local app = project "app"
    kind "ConsoleApp"
    files { "src/bin/*.cpp" }
    links { "libby" }
```

----

### build

```
$ cd project/
$ ./premake4
  Building configurations...
  Running action 'gmake'...
  Generating build/Makefile...
  Generating build/libby.make...
  Generating build/app.make...
  Done.
```

---

```
$ make config=release -C build/ -f Makefile
```

----

### advantages

 - free
 - simple scripting
 - small
 - no dependecies
 - embedded lua
