GTags 2.0.0 is a major release that adds the GTags mixer.

The mixer is used as an intermediary for all communications between the client (e.g. emacs, vim) and the servers.  By specifying watch folders (through the watcher), the mixer can monitor local folders for changes.  GTags queries will automatically incorporate results from the servers as well as the locally watched folders.

Note: Due to filesystem-dependencies, this version of GTags is not compatible with Mac OS X.