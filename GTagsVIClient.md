## GTags Client for VI ##

You can use GoogleTags from VI/Vim by adding the following line to
your `.vimrc`:

```
source /path/to/gtags.vim
```

This adds three new commands to
Vim: `:Gtjump`, `:Gtselect`
and `:Gtag`. These are essentially analogues to
the `:tjump`, `:tselect` and `:tag`
commands.

You can also bind keys to these commands. For example, to
bind `ctrl-]` (the standard "jump to the definition of what's
under my cursor" key) you can add the following to
your `.vimrc`:

```
nmap <C-]> :call Gtag(expand('<cword>'))<CR>
```

<p>Note that these commands uses Vim's tag stack, so you can continue<br>
to use the standard Vim command <code>:pop</code> (or<br>
<code>ctrl-T</code>) to pop the tag stack.<br>
<br>
<code>gtags.vim</code> also adds a <code>:Gtgrep</code> command<br>
which searches over tag names and presents all occurrences in a<br>
<code>:grep</code> style vim window. Note that this isn't a full grep<br>
over the file contents. Rather, it only looks at the definitions of<br>
symbols (functions, classes, methods, etc.). Just like<br>
in <code>:grep</code>, the lines in this new window will include a<br>
"snippet" of the occurrence. You can jump to the actual occurrence by<br>
clicking or by pressing <code>enter</code> on a line.<br>
<br>
<code>gtags.vim</code> will attempt to use the file<br>
type of the current buffer to determine which one of these servers to<br>
communicate with. If nothing useful can be determined from the<br>
filetype, it will fall back to using the variable<br>
<code>g:google_tags_default_filetype</code>, which can be set to<br>
<code>'c++'</code>, <code>'java'</code>, or <code>'python'</code>.<br>
<code>'c++'</code> is the default value for this variable, but you can<br>
change it in your <code>.vimrc</code> like so:<br>
<br>
<pre><code>let g:google_tags_default_filetype='java'<br>
</code></pre>

<code>gtags.vim</code> will also use your own local<br>
<code>tags</code> file, so you can generate tags yourself (using the<br>
command line tool <code>ctags</code>) for files you have open for<br>
add/edit, and the tags from your local <code>tags</code> file will be<br>
combined with the results from the GTAGS server.<br>
<br>
As of 2.0.0, <code>gtags.vim</code> uses the GTags mixer. It should automatically start on your first GTags query. If for some reason, you wish to bypass the mixer, you can do so by executing:<br>
<br>
<pre><code>let g:google_tags_use_mixer=0<br>
</code></pre>

If you want to bypass the mixer altogether, you can add the previous line to your <code>.vimrc</code>.  To re-enable the mixer, execute<br>
<br>
<pre><code>let g:google_tags_user_mixer=1<br>
</code></pre>