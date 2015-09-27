## The GTags TAGS-file Format, v2 ##

This document describes the file format read by the GTags server.

A TAGS file contains Lisp s-expressions which contain information about tag definitions. (The file may optionally be gzipped to save space.) Until GTags contains an integrated C++ parser, we'll provide ways to generate TAGS files from other sources of documentation such as Doxygen output, Etags/Ctags files, or cscope cache files.

A TAGS file begins with a header that looks like this:
```
(tags-format-version 2)
(tags-comment "comment string")
(timestamp 1155567890)
(tags-corpus-name "corpus-name")
(features (callers))
```
The `tags-format-version` must come first, but the other header declarations may be in any order.

The `tags-format-version` value identifies the version of the spec that the TAGS file conforms to, i.e. 2 for this spec. The `tags-comment` may contain a note about how the TAGS file was generated. The `timestamp` value is the UNIX timestamp corresponding to the time that the TAGS file was generated. The `tags-corpus-name` is a string specifying which directory/repository is given in the file.

Some intermediate tags formats (e.g. cscope or etags) don't contain all the information that GTags knows how to use. The `features` list in the header specifies the presence or absence of optional information. It contains a subset of the following symbols:

  * `callers` means each file contains an entry for all lines with a call to, or use of, another function/variable/type-- by default, we only store lines where a function/variable/type is defined. (Including this information is optional because it can be a big space hit.) It's important to distinguish between files which contain no caller lines and files which lack caller information; in the latter case, the server should send a warning back to clients instead of erroneously reporting that there are no callers.

The syntax for including the extra information required by these features is described separately in detail below.

Using the `features` list to specify the presence or absence of features on a per-TAGS-file basis is the exception rather than the rule. There are many additional optional features, in general, if a particular piece of information (say, a function's return type) is not available, then the corresponding attribute may simply be omitted from the description; GTags will attempt to do the right thing given the other information that is available.

Servers may use the timestamp or the features list to resolve conflicting definitions when multiple TAGS files are loaded.

After the header, the TAGS file contains an entry of this form for each source file represented:
```
(file (path "tools/tags/gtags.cc")
      (language {"c++"|"java"|"python"})
      (contents ((item  (line *lineno*)
                       [(offset *charno*)]
                        (descriptor *item-descriptor*)
                       [(snippet *snippet*)])
                 ...))
```

The `path` value contains a string with the path of the file described, relative to the base of the repository. The `language` gives the language of the file.

Each element in the `contents` list represents a single item of interest. For each of these, `lineno` gives the line number where it appears, `offset` gives the character offset from the beginning of the file to the beginning of the line, and `item-descriptor` gives an _item descriptor_ (described below). Optionally, `snippet` gives the text of that line.

## Cross-references ##

Some item descriptors contain attributes naming other types, functions, or variables (e.g. to give the return type of a function, or to indicate that a particular function is being called). These references are often ambiguous since types or identifiers in different contexts may share the same name.

We provide a way to resolve these ambiguities by assigning a unique identifier (which may be an arbitrary string) to each distinct entity. Any field below that requires a `reference` will require the name of the target and optionally an identifier, in this format:
```
(ref (name "TagsTable"))
(ref (name "TagsTable") (id "tools--tags--tagstable.h"))
```

The `id` value can then be linked to the `id` field of `type`, `function`, or `variable` declarations (described below).

## Item descriptors ##

The possible values for `item-descriptor` are listed below. Each is a list whose first element describes the type of item and whose subsequent elements are attribute/value pairs. The attributes may be reordered arbitrarily within each item descriptor.

```
(type  (tag *tag*)
      [(id *identifier*)]
      [(kind {struct|union|class|enum|interface})]
      [(scope {*reference*|top})]
      [(derived-from (*reference* ...))])
```
Indicates the definition of a type.
  * `*tag*` gives the name of the type without any qualifiers.
  * `*id*` gives a unique identifier for the type.
  * `*kind*` indicates whether the type is a struct, union, class, etc.
  * `*scope*` indicates the scope that contains the class: if the class is contained within another (e.g. SExpression::const\_iterator is contained within SExpression), then the `reference` should point to the containing class (in the example, SExpression). If the class is at the top-level scope, then =top= should be given instead.
  * `*derived-from*` gives a (possibly empty) list of references to supertypes of the current type.

```
(function  (tag *function-name*)
          [(id *identifier*)]
          [(specified-here ([definition] [declaration]))]
          [(scope {*reference*|top})]
          [(return-type {void|*reference*})]
          [(argument-types (*reference* ...))]
          [(signature *signature-string*)])
```
Indicates a function definition.
  * `*tag*` gives the name of the function without any qualifiers.
  * `*id*` gives a unique identifier for the function.
  * `*specified-here*` indicates whether the function definition, declaration, or both appear at the specified line. If omitted, =(definition)= is assumed.
  * `*scope*` indicates the scope that contains the function. See additional notes for =scope= under the =type= descriptor.
  * `*return-type*` gives a reference to the return type of the function, or =void= if the return type is void.
  * `*argument-types*` gives a list of references to the argument types.
  * `*signature-string*` gives the signature (return type, name, and argument list) of the function as a string, e.g. `Foo* Bar(List<Foo> lst)`

```
(variable  (tag *variable-name*)
          [(id *identifier*)]
          [(type *reference*)])
```
Indicates a variable definition (either global or class member).
  * `*tag*` gives the name of the variable without any qualifiers.
  * `*id*` gives a unique identifier for the variable.
  * `*type*` gives the type of the variable.

```
(generic-tag  (tag *tag*)
             [(id *identifier*)])
```
Indicates a tag which might be the definition of a type, a function, or a variable. Use this if the parser isn't sophisticated enough to figure out which.

```
(call (to *reference*))
```
Indicates that the line contains a call to a function, use of a variable, instantiation of a type, or forward declaration of a type. `*reference*` contains a reference to the function, variable, or type of interest.
