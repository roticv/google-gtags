# GTags for Emacs #

To use GTags, put the cursor over the identifier in question and type `M-x gtags-show-tag-locations [RET] [RET]`. (The search defaults to the identifier under point, but you can change it.) If there's only one place where the identifier is defined, then Emacs immediately jumps there. If there are multiple matches, then GTAGS will pop up a new window showing all matches (and the filename, and a snippet). Click on any of the items (or press `[RET]` with the cursor on its line) to jump to the definition.

To see partial matches, use `M-x gtags-show-tag-locations-regexp`, which will let you enter a regular expression and match prefixes.

`M-x gtags-pop-tag` will return you to the context
from which you did gtags-show-tag-locations. If you only vaguely remember the name of the
function, `M-x gtags-show-matching-tags` lets you type in a regular expression,
and pops up a window showing all tags in the tags database that match.
The window that gtags-show-matching-tags open is a special GTAGS mode
that will automatically do a gtags-show-tag-locations on the tag that your cursor is one
when you hit ENTER (Thanks to Arthur for this functionality!).

Similarly, `M-x gtags-show-callers` will show you who all the
callers of a particular function are, and operates similarly.  A
variation of this call is `M-x gtags-show-call-tree` which
displays a tree of callers.  Using this function, you can see who
calls a method, who calls the callers of the method, who calls those
callers, etc.  Due to the way gtags works, the information presented
may not be accurate (a call to Foo.showSomething() may be confused
with a call to Bar.showSomething()).

For the truly lazy, you can use
`M-x gtags-show-tag-locations-under-point` and
`M-x gtags-show-callers-under-point`, which
will eliminate the prompt and just use whatever your cursor is currently pointed
at for the moment.

You can also search all the definition lines. For instance, if you
want to find all virtual functions that return Fprint, you can type
`M-x gtags-search-definition-snippets virtual Fprint`, and
gtags will show all
functions that return Fprint and are marked virtual. You can also use regular
expressions, so for instance, you can use ` Fprint .*::.*` to see all
methods that are defined outside the class declaration.

If you don't like having to type all that every time just to do a
little browsing, do what I do, and stick the following into your
.emacs:

```
(global-set-key [f7] 'gtags-show-tag-locations-regexp)
(global-set-key [f8] 'gtags-show-callers)
(global-set-key [f9] 'gtags-pop-tag)
(global-set-key [f10] 'gtags-show-matching-tags)
```

That'll let you use the function keys instead of having to type
`M-x gtags-show-tag-location`.


In Emacs, whenever you're asked for the name of the tag you want to
look up, you can press TAB for completion in the usual Emacs way.  In
other words, you can type
`M-x gtags-show-tag-locations RET EditBillingInf TAB` in a Java file and Emacs will complete this to EditBillingInformation.  If you press RET, Emacs will take you to the
definition of that class.

To see all the tags for a particular file in a concise, alphabetized
list, do `M-x gtags-list-tags`.  It defaults to the current file.

The Emacs code figures out the current language  through a combination of file extension and language mode of the current buffer. When you issue a tags-related command, Emacs issues a request to a server chosen based on the command type to port chosen by language.
The server returns the relevant tags to your Emacs, which then searches through the results in the standard fashion.

## Commands ##

gtags-first-tag: The equivalent of Emacs's built-in find-tag. We recommend gtags-show-tag-locations instead.

gtags-next-tag: Cycles between all tags. (To be used with gtags-first-tag)

gtags-show-tag-locations: Enter the name of the identifier you're searching
on. If there's only one place where the identifier is defined, then EMACS takes
you to that definition. If there are multiple matches, then GTAGS will pop up a
new window with the identifier, the path to the file (relative to the top of
source tree as provided by the variable gtags-source-root), and a snippet
showing the declaration. Click on any of the items (or hitting enter after
moving the cursor into the relevant region) will take you to the definition.

gtags-show-tag-locations-regexp: lets you enter a regular expression.

gtags-pop-tag will return you to the context from which you did
gtags-show-tag-locations.

gtags-show-matching-tags: pops up a window showing all tags in the tags
database that match. The window that gtags-show-matching-tags open is a special
GTAGS mode that will automatically do a gtags-show-tag-locations on the tag
that your cursor is one when you hit ENTER

gtags-show-callers: will show you who all the callers of a particular function
are.

gtags-show-tag-locations-under-point,gtags-show-callers-under-point: just like
the similarly named functinos, but eliminates the prompt and just use whatever
your cursor is currently pointed at for the moment.

gtags-search-definition-snippets: search all the definition lines. You can also
use regular expressions.

gtags-list-tags: see all the tags for a particular file in a concise,
alphabetized form. It defaults to the file opened by the current buffer.

In Emacs, whenever you're asked for the name of the tag you want to
look up, you can press TAB for completion in the usual Emacs way.

gtags-start-gtags-mixer: starts the GTags mixer.

gtags-stop-gtags-mixer: stops the GTags mixer.

gtags-restart-gtags-mixer: restarts the GTags mixer, replacing the old mixer.

### GDoc mode ###

M-x gdoc-mode turns on gdoc mode. gdoc-mode brings up two windows, one to show
an "implicit search" over whatever your cursor is hanging over, and the other to
show a "best guess" as to what you might be interested in as you browse. To turn
off gdoc-mode, type M-x gdoc-mode again. (We'll admit that gdoc-mode could use
some improvements, but it'll be interesting to see what others make of it)

## Advanced Users ##

To set the default language for Gtags, i.e. the language that it uses if it can't tell what language to use because the current buffer isn't in one of the modes for the languages it knows, add one of these to your .emacs:
```
(setq gtags-default-mode 'c++-mode)
(setq gtags-default-mode 'java-mode)
(setq gtags-default-mode 'python-mode)
```

To set the output format for Gtags, use one of these options:
```
(setq gtags-output-mode 'single-line)
(setq gtags-output-mode 'single-line-grouped)
(setq gtags-output-mode 'standard)
```