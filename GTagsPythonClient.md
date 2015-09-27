## GTags Python API ##

`gtags.py` contains Python bindings that talk to the
GTags server exactly the same way Emacs does. You can use this to augment any
Python-capable editor to provide the same functionality. (The Vi client is
built using this library.)

Sample Python code:

```
import gtags

results = gtags.find_matching_tags_exact("c++", "HandleRead", 0)
                                       # lang   tagname       show callers? (0,1)

for match in results:
  print match.filename_, match.lineno_, match.tag_
```