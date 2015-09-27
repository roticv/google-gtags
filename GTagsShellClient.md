## Utility:  GTags on the command-line ##

Sitaram Iyer wrote a command-line script interface to gtags.  This is
generally useful for browsing code with less on the command line, and
wanting to look up a tag (definition or invocation) as you go along.
Convenient to alias "t" to
`gtags.h`
Usage and examples follow.

```
  Command-line interface to gtags.

  Usage: gtags.sh [flags] tag ...
  flags:
    -L|--lang c++|java|python|szl # language of source files to look in
    -w|--word                     # match whole word only (same as ^tag$)
    -i|--invocations              # match invocations *and* definitions of tag
    -v|--vi  |  -l|--less         # emit vi or less commands instead of file:line
    -r|--restrict <dir>           # restrict matches to <dir>/
    --color | --nocolor           # override "use ansi colors if isatty(stdout)"

  (tip: may be convenient to alias 't' to gtags.sh)

  Example usage:                   gtags.sh -w -r mapreduce Output
  which is equivalent to:          gtags.sh --color ^Output$ | grep \ mapreduce/

  For invocations and definitions: gtags.sh -i -w -r mapreduce Output
  Emit vi cmds for cut-and-paste:  gtags.sh -v -w -r mapreduce Output
```