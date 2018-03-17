Literate Commits
================

This is a simplistic toolchain designed to allow for literate modifications of
existing non-literate codebases.

Existing tools are written with the assumption that the codebase is maintained
as a literate program, and are difficult to use on a project that is primarily
developed by others in a non-literate fashion.  The primary goal of this
literate toolchain is to allow for literate commits or patches to be created,
without requiring any extensive changes to the codebase to do so, and where the
literate source will be thrown away once the commit has been pushed and merged.


### User flow

0. alias vim=litvim
0. (lit)vim files to open them as a literate program
0. Edit document as normal
0. `:w` automatically invokes `unlit -l` so that the non-literate source file
   always matches the literate source file.
0. Compile, run, and edit as otherwise normal.
0. When ready to publish, run `extractlit` to generate the document, and
   `unlit` without `-l` to generate the source to add and commit.

It is wise to add `*lit` to your .gitignore to avoid accidentally adding literate files.


### Writing

All literate files are markdown.  Documentation is expected to be written in
markdown syntax.  There is no support (yet) for rearranging code or
documentation blocks when extracting to code or documentation.

Literate files within this toolchain are markdown files with all code
blockquoted.  A literate file might look like:

    > A line of code
    > A second line of code
    > The third line of code

Editing this to look like

    > A line of code

    Prefixing a block results in it being included for documentation extraction.

    > A second line of code

    A suffix to a block requires an empty subsequent code block that will be removed.

    >

    > The third line of code.

Extracting for documentation will generate a file containing

    Prefixing a block results in it being included for documentation extraction.

        A second line of code

    A suffix to a block requires an empty subsequent code block that will be removed.


### Compiling

Extracting to code only retains lines that begins with a `>` character, and
strips the leading `>SPACE` from the line.

When developing and debugging, an option (`-l`) exists to retain only newline
characters from removed content.  Most languages ignore vertical whitespace,
and thus this can be abused to trivially make line numbers from a compilation
error in an extracted file to match the literate source file, even though a
majority of the content has been removed.

