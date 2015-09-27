# Google Tags (GTags) #

Google Tags is a project to speed up code browsing in large
source code base. Here are some scenarios where it can help:

  * While you're browsing source code, Gtags can immediately jump to the definition of a function or variable.
  * Gtags can grep over all definitions or function calls in your source code.
  * Gtags can tell you where all the callers of a particular function are.

We do this by extending the TAGS functionality in GNU Emacs and
XEmacs. A separate server-side component answers requests for particular tags
and returns a set of matching results for each.

We provide an [Emacs client](GTagsEmacsClient.md), a
[command-line client](GTagsShellClient.md), and a [VI client](GTagsVIClient.md).
We also provide a [Python API](GTagsPythonClient.md).

## More information about... ##
  * [How to install and deploy](GTagsInstallation.md)
  * Clients (including how to set up your client)
    * [GTagsEmacsClient](GTagsEmacsClient.md)
    * [GTagsShellClient](GTagsShellClient.md)
    * [GTagsVIClient](GTagsVIClient.md)
    * [GTagsPythonClient](GTagsPythonClient.md)
  * The [GTagsProtocol](GTagsProtocol.md): how clients send messages along the wire to the server.
  * The [GTagsTagsFormat](GTagsTagsFormat.md): how we represent tags information in files.

## Quick guide for Emacs ##

For more detail (or for non-Emacs users), follow one of the Clients links above.

First, load gtags.el.
  * `M-x google-show-tag-locations [RET] TAGNAME [RET]` searches for all tags definitions matching TAGNAME.
  * `M-x google-show-tag-locations-regexp [RET] PREFIX [RET]` searches for all tag definitions beginning with PREFIX (which may be a regular expression).
  * `M-x google-show-callers [RET] TAGNAME [RET]` searches for all tag calls matching TAGNAME.


## How It Works ##

The `gentags` script
(`gentags.py`) generate 6 tags files:`{cpp,java,python}{,.callers}.tags.gz`. We generate the files by running
`etags` followed by `etags_to_tags.py`.

The code for the server lives in `gtags.cc`.
To build it, run ./configure; make. To run the GTags
server:

`$ gtags --tags_file ./cpp.tags --tags_port 2222`

To save space and time, we support reading from gzipped tags files:

`$ gtags --tags_file ./cpp.tags.gz --tags_port 2222 --gunzip`

Different servers handle requests for different TAGS files;
different port numbers correspond to different languages. Clients
(like EMACS' `google-show-tag-locations`) must determine the
appropriate server/port to send requests to.

To prevent denial of service attacks, all commands return at most
1000 results, so that if you have too many results the server won't be
kept busy showing just results that are of interest to
you.

## List of Files ##

> `gtags.cc`: and friends: server (this is required)

> `etags.el`: GNU Emacs etags implementation (comes with GNU Emacs, required for Xemacs)

> `gtags.py`: python library to talk to the server

> `gtags.sh`: shell implementation to talk to server

> `gtags.vim`: vim client

> `gtags_vim.py`: support for vim client

> `rtags.py`: callgraph indexing for java, python, c/c++

> `gentags.py`: create indexes for java, python, c/c++

## GTags is brought to you by... ##
Ken Ashcraft,
Stephen Chen,
Arthur Gleckler,
Laurence Gonsalves,
Leandro Groisman,
Sitaram Iyer,
Piaw Na,
Arun Sharma,
Phil Sung