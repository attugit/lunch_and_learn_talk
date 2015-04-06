## Lunch & Learn
### Quick project setup with use of FOSS

#### Karol Wozniak, 2015

----

*Less Code == More Software*

====

### outline
 - repository hosting
 - continuous integration
 - project documentation
 - programming blogs

----

### tools
#### repository hosting
 - [github](https://github.com/)
 - [gitolite](https://github.com/sitaramc/gitolite)

----

### tools
#### continuous integration

 - [travis-ci](https://travis-ci.org/)

----

### tools
#### project documentation

 - [gh-pages](https://pages.github.com/)
 - [sphinx](http://sphinx-doc.org)
 - [readthedocs](https://readthedocs.org/)

----

### tools
#### programming blogs

 - [jekyll](http://jekyllrb.com/)
 - [disqus](dhttps://disqus.com/)

----

### tools
#### bonus: c++ build tools

 - [premake](http://premake.github.io)
 - [waf](https://github.com/waf-project/waf)

====

### FOSS
#### free open source software

[OpenHub](https://www.openhub.net/)

====

### github

https://github.com

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
 - [C++ standard](https://github.com/cplusplus)
 - [CppCon](https://github.com/CppCon)

----

### what else...?

 - French [Civil Code](https://github.com/steeve/france.code-civil)
 - Polish [2014 electoral system source](https://github.com/wybory2014/Kalkulator1)

----
### luxoft at github

https://github.com/Luxoft

----

### twister

http://www.twistertesting.com

https://github.com/Luxoft/Twister

----

### me on github

https://github.com/attugit

[This talk](http://attugit.github.io/lunch_and_learn_talk)
/[src](https://github.com/attugit/lunch_and_learn_talk)

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

### gitolite

http://gitolite.com

simple alternative

----

### file structure

```
$ tree gitolite-admin/
  gitolite-admin/
  |-- conf
  |   `-- gitolite.conf
  `-- keydir
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

====

### travis-ci

https://travis-ci.org

https://github.com/travis-ci

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

### live

- [build history](https://travis-ci.org/attugit/archie/builds)

---

- [protobuf](https://travis-ci.org/google/protobuf)
- [TypeScript](https://travis-ci.org/Microsoft/TypeScript)

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

====

## other

https://github.com/ripienaar/free-for-dev
