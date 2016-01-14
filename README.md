# chuckmove

``Because when Chuck moves, Python bends''

A tool to help reorganize Python trees by rewriting import statements.

Lets say you want to relocate a Python module to a different location in the tree, and also rename it. So for instance, x is to become y.z. The syntax you use is:

> chuckmove -o x -n y.z <root directory>

Invoking this command makes the tool recursively descend into the root directory, make a backup copy of each file (adding the prefix '.orig') and rewrite the imports in it, so that "import x" gets turned into "import y.z"

It's not a blind search/replace - it recognizes Python grammar, so it works with all of these forms:

> from x import a
> from x.b import c 
> import x.d.e.f as foo # Comments are also handled

Nits:

1. Lines that don't parse because of syntax or grammatical errors are left as is.
2. Code references are currently not fixed. I.e. if import x.y is rewritten into import w.x.y, then you must manually update x.y.f() to w.x.y.f().

To generate a patch after running chuckmove:

> gendiff <dir> .orig
