#How to Run Gtags under Solaris

FYI, Here are the changes I made to get gtags running on Solaris:

1)
```
google-gtags/SConstruct : added the LIBS.
 env.Program("gtags", ["gtags.cc", objects], LIBS=['nsl',
'socket','stdc++'])
```

2)
```
 configure
```
> - changed default 'tar' to GNU tar

3)  I used a pre-installed gcc-3.2.3 installation on my Solaris box.
```
   google-gtags/scons/scons-local-0.96.1/SCons/Tool/sunc++.py

  def get_cppc(env):
   cppcPath =  'gcc-v3.2.3/bin'
  ...
   return (cppcPath, 'gcc', 'gcc', cppcVersion)
```

4)
```
 chmod +w *.py
  perl -pi -e 's/\/usr\/bin\/python2\.(2|4)/<path to my python
installation>/g' *.py
  perl -pi -e 's/\/usr\/bin\/env\W+python/\/grid\/common\/pkgs\/python
\/v2\.4\.2\/bin\/python/g' scons/scons.py
```

5) To run in Xemacs (since Xemacs is supported by Sun Workshop)

> In gtags.el:
> a) add "/"  to google-source-root
> > `(defvar google-source-root "/"`

> b) ` (if (google-xemacs)`
> > `(load "emacs/21.3/lisp/progmodes/etags.el"))`

> -> point to the etags.el file that comes with your Emacs
installation.

