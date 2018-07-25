T-Rext
======

T-Rext is a command-line filter that attempts to clean up spaces and
punctuation in a text file.  Its purpose is so that, when you are writing
a text generator, such as a Markov processor, you need not worry too much
about its output format; just toss its output through T-Rext when you're
done to make it more presentable.

Usage
-----

    bin/t-rext raw_output.txt > cleaned_output.txt

This will take lines that look like this:

    " Well , " said the king , , " no . "

and reformat them to look like this:

    “Well,” said the king, “no.”

To use T-Rext from any working directory, add the `bin` directory in this
repository to your `PATH`.  For example, you might add this line to your
`.bashrc`:

    export PATH=/path/to/this/repo/bin:$PATH

T-Rext is built on an over-engineered library of pipeline processors, which
you can use directly (note, its interface is not stable and liable to change.)
To use the T-Rext Python modules in other Python programs, make sure the
`src` directory of this repository is on your `PYTHONPATH`.  For example,
you might add this line to your `.bashrc`:

    export PYTHONPATH=/path/to/this/repo/src:$PYTHONPATH

Then you can add imports like this to the top of your script:

    from t_rext.processors import TrailingWhitespaceProcessor

An easy way to accomplish the above two things is to dock T-Rext using
[shelf][]:

    cd ~/checkout && shelf_dockgh catseye/T-Rext

Tests
-----

This is a test suite, written in [Falderal][] format, for the `t-rext`
utility.  It also serves as documentation for said utility.

    -> Functionality "Clean up punctuation and spaces" is implemented by
    -> shell command "bin/t-rext %(test-body-file)"

    -> Tests for functionality "Clean up punctuation and spaces"

Spaces before commas and periods are elided.

    | Well , that is good .
    = Well, that is good.

Multiple commas are collapsed into a single comma.

    | Well , , that is good .
    = Well, that is good.

Multiple periods are not collapsed into a single period.

    | Well . . . that is good.
    = Well... that is good.

Quotes are oriented.

    | "Yes," he said.
    = “Yes,” he said.

Spaces after opening quotes and before closing quotes are elided.

    | " Yes , " he said.
    = “Yes,” he said.

But not the other way 'round.

    | Muttering "Yes," he turned around.
    = Muttering “Yes,” he turned around.

Quotes do not match across paragraphs.

    | Turbid "Waters" that "leak.
    | 
    | You "don't" have a clue.
    = Turbid “Waters” that “leak.
    = 
    = You “don't” have a clue.

[Falderal]:     http://catseye.tc/node/Falderal
[shelf]:        http://catseye.tc/node/shelf
