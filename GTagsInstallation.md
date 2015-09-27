## Build Requirement ##
  * Gcc (tested on 4.2.1 only)
  * Python 2.4
  * tar, gunzip
  * [Boost Test Library (only for building and running unit tests)](http://www.boost.org/libs/test/doc/index.html)

## Build Instruction ##
To build GTags server, mixer, and watcher, do
```
$ cd google-gtags-2.0.0
$ ./configure
$ make
```

To build unit tests, do
```
$ make tests
```

To run unit tests, do
```
$ make runtests
```

## Deployment Requirement ##
  * Python 2.4
  * [Exuberant Ctags](http://ctags.sourceforge.net/)
  * You must run ctags executable as etags in order for it to generate tags file in etags format. To do this, simply create a symbolic link to ctags named etags.

## Recommended Deployment ##
  1. Checkout complete source tree nightly from your repository.
  1. Invoke gentags with: `gentags.py --etags=/path/to/etags --rtags=/path/to/rtags.py --etags_to_tags=/path/to/etags_to_tags.py`
  1. For each generated {cpp|java|python}[{.callers}].tags.gz for, start a instance of gtags server.
  1. Edit gtags.el, gtags.py and gtagsmixer\_socket\_config respectively so emacs and python clients and the mixer know where the servers are.
  1. Edit gtags.el and gtags.py respectively so emacs and python clients know the location of the mixer.
  1. Load gtags.el and gtags.vim in emacs and python respectively.
  1. Use the watcher to specify watch folders for the mixer.

We recommend you repeat step 1 and 2 nightly. If you have gtags servers running, you can instruct them to load the new tags file by sending them `reload-tags-file` command. See GTagsProtocol for more details.