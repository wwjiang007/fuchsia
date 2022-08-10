This is an experimental tool to generate markdown documentation from C++. It
consumes .yaml output from clang-doc and formats markdown for serving on
fuchsia.dev.

It is not yet ready for general use. It has many limitations and bugs and is not
yet hooked up to the build.

Instructions for running manually. It is currently very involved and will be
streamlined over time.

  * Create a .cc file in the target you want to document and include all of the
    headers you want to generate documentation for. It can be otherwise empty.

  * To do a build.

  * Run clang-doc on that output, substituting the name of the .cc file from the
    first step:

```
clang-doc --format=yaml -p=out/x64 --executor=all-TUs --filter=<NAME-OF-CC-DOC-FILE>
          --output=<SOME-TEMP-DIR> out/<BUILD-DIR>/compile_commands.json
```

  * Run cppdocgen on that output, re-listing the headers you want to include
    documentation for (relative to the build dir) and providing input (the temp
    directory generated by clang-doc) and output (for the markdown files)
    directories:

```
out/<BUILD-DIR>/host_x64/cppdocgen -i <TEMP-DIR-FROM-ABOVE> -o <OUT-DIR> -s 6
    -u "https://fuchsia.googlesource.com/fuchsia/+/refs/heads/main/"
    ../../sdk/lib/fdio/include/lib/fdio/directory.h
    ../../sdk/lib/fdio/include/lib/fdio/fd.h