## The GTags Protocol, v2 ##

This page describes the protocol used by the GoogleTags (GTags) server and client to talk to each other.

A client opens a new connection to a server for each request. A server listening on a particular port responds to requests for one or more TAGS files which can be distinguished on the basis of (1) language and (2) whether they contain all lines with function calls, or only those with tag definitions.

After opening a connection, the client sends requests as s-expressions of this form:

```
(command (attribute value ...) ...)
```

Since no requests from previous GTags clients ("v1") begin with "(", we can maintain backwards compatibility with old clients.

**command** specifies an action and each attribute (associated with one or more values) specifies options to send to the server. The order in which the attributes are sent does not matter.

For each request the server sends back an s-expression of this form:

```
((server-start-time (*start-time-high* *start-time-low*))
 (sequence-number *seq-num*)
 (value *return-value*))
```

**start-time-high** and **start-time-low** are integers representing the high- and low-order 16 bits of the server start time. **seq-num** is an integer of the server's choosing, identifying the current request. The combination of these three integers is guaranteed to be unique across all requests to that particular server. The meaning of **return-value** is described below for each request type.

## Required attributes ##

These attributes, denoted **required-attributes**, are required on all commands.

  * The `client-type` attribute contains a string describing the client type, for example:
    * `(client-type "gnu-emacs")`
    * `(client-type "xemacs")`
    * `(client-type "python")`
    * `(client-type "vi")`
    * `(client-type "shell")`
  * The `client-version` attribute contains an integer specifying the client version, for example:
    * `(client-version 41)`
  * The `protocol-version` attribute contains the version of the protocol being used. For the version of the protocol described here:
    * `(protocol-version 2)`

## Commands ##

### Searching ###

```
(lookup-tag-exact *required-attributes* (tag *tag-name*) [(language *language*)] [(callers *callers*)] [(current-file *current-file*)])
```

Returns tags which exactly match the string `tag-name`.

```
(lookup-tag-prefix-regexp *required-attributes* (tag *tag-regexp*) [(language *language*)] [(callers *callers*)] [(current-file *current-file*)])
```
Returns tags which begin with the regexp _tag-regexp_.

```
(lookup-tag-snippet-regexp *required-attributes* (tag *tag-regexp*) [(language *language*)] [(callers *callers*)] [(current-file *current-file*)])
```
Returns tags which appear on the same line as snippets matching the regexp _tag-regexp_.

```
(lookup-tags-in-file *required-attributes* (file *file*) [(language *language*)] [(callers *callers*)])
```
Returns all tags found in _file_.

The `return-value` is a list of matches of this form:
```
(((tag *tag*)
  (snippet *snippet*)
  (filename *filename*)
  (lineno *line-number*)
  (offset *file-offset*)
  [(directory-distance *dist*)])
 ...)
```

For each match:

  * **tag** (string) contains the matched tag.
  * **snippet** (string) is the line containing the match.
  * **filename** (string) is the path of the matching file, relative to google3.
  * **line-number** (integer) is the line number of the match.
  * **file-offset** (integer) is the byte offset of the beginning of the tag (relative to the beginning of the file).

(If you're comparing this to the Etags format, that format defines an additional _size_ attribute which is in fact not the filesize of the matching file, but the space needed to hold the Etags internal data structure for a given file. In recent versions of Emacs, this attribute is ignored.)

The _language_ attribute (one of `"c++"`, `"java"`, or `"python"`) specifies the language of the TAGS file to search. If omitted, the server may guess the file type on the basis of _current-file_ (if that is provided), or simply choose one TAGS file from among the ones it has loaded.

If _callers_ is `nil`, then only lines containing tag definitions are searched; otherwise, all lines containing calls are searched.

The server may (for any of these commands) optionally return `error-tagsfile-not-loaded` if it determines that it has not loaded the tags file corresponding to `language` and `callers`.

## Miscellaneous ##

```
(ping *required-attributes*)
```
Can be used to determine server latency. `return-value` is unspecified.

```
(reload-tags-file *required-attributes* (file *tags-file-path*))
```
Reloads the tags file from _tags-file-path_. _return-value_ is _t_ if the file is loaded successfully.

```
(log *required-attributes* (message *expr* ...))
```
Writes any number of s-expressions (the _expr_ values) to the server log, with unspecified _return-value_.

```
(get-server-version _required-attributes_)
```
Retrieves the server version number. The _return-value_ is an integer specifying the server version.

```
(get-supported-protocol-versions *required-attributes*)
```
_return-value_ is a list of the protocol versions that the server can support, e.g. _(1 2)_.








