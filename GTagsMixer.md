# GTags Mixer #

GTags mixer is a new local component of GTags that acts as a proxy between the clients (emacs, vi, etc.) and multiple GTags servers. For each query received from the client, the mixer queries the servers and the locally watched folders, mixes the results, and send the mixed results back to the client.

## Config File ##

The GTags mixer configuration is located in the `gtagsmixer_socket_config`. It is in an SExpression format, as in the following example:

```
(gtags-corpuses "corpus1" "corpus2")

(gtags-languages "c++" "java" "python")

(gtags-language-has-callgraph ("c++" . t)
			      ("java" . t)
			      ("python" . t))

(gtags-language-hostnames
 ("c++" . "gtags.example.com")
 ("java" . "gtags.example.com")
 ("python" . "gtags.example.com"))

(gtags-callgraph-hostnames
 ("c++" . "gtags.example.com")
 ("java" . "gtags.example.com")
 ("python" . "gtags.example.com"))

(gtags-language-ports ("c++" . 2223)
		      ("java" . 2224)
		      ("python" . 2225))

(gtags-callgraph-ports ("c++" . 2233)
		       ("java" . 2234)
		       ("python" . 2235))
```

You should configure `gtags-language-hostnames` and `gtags-language-ports` to specify the addresses of the language definition servers.  The same should also be done for callgraphs (using `gtags-callgraph-hostnames` and `gtags-callgraph-ports`) for languages that have been specified to have a callgraph through the `gtags-languages-has-callgraph` option.

## Watching Folders ##

The mixer can also index local folders that it watches for changes.  To tell the mixer to add a watch folder, execute

```
gtagswatcher --add /path/to/watch1 --add /path/to/watch2
```

To tell the mixer to stop watching folders, execute

```
gtagswatcher --remove /path/to/watch2
```