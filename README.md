T-Rext
======

T-Rext is a command-line filter that attempts to clean up spacing,
punctuation, and capitalization in a text file.  Its purpose is so that,
when you are writing a text generator, such as a Markov processor, you
need not worry too much about its output format; just toss its output
through T-Rext when you're done to make it more presentable.

The current version of T-Rext is 0.2.  Version 0.2 requires Python 3.x;
Python 2.x is no longer supported.  Docker images based on the required
version of Python for each version, are [available on Docker Hub][].

Usage
-----

### Usage from the Command Line

    bin/t-rext raw_output.txt > cleaned_output.txt

This will take lines that look like this:

    " Well , " said the king , , " no . "

and reformat them to look like this:

    “Well,” said the king, “no.”

To use T-Rext from any working directory, add the `bin` directory in this
repository to your `PATH`.  For example, you might add this line to your
`.bashrc`:

    export PATH=/path/to/this/repo/bin:$PATH

An easy way to accomplish the above is to install [shelf][], then
dock T-Rext using

    shelf_dockgh catseye/T-Rext

### Usage from Python

T-Rext is built on an over-engineered library of pipeline processors, which
you can use directly (note, its interface is not stable and liable to change.)
To use the T-Rext Python modules in other Python programs, make sure the
`src` directory of this repository is on your `PYTHONPATH`.  For example,
you might add this line to your `.bashrc`:

    export PYTHONPATH=/path/to/this/repo/src:$PYTHONPATH

Then you can add imports like this to the top of your script:

    from t_rext.processors import TrailingWhitespaceProcessor

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

Single spaces after opening quotes and before closing quotes are elided.

    | " Yes , " he said.
    = “Yes,” he said.

But not the other way 'round.

    | Muttering "Yes," he turned around.
    = Muttering “Yes,” he turned around.

Multiple spaces after opening quotes and before closing quotes are elided.

    | "   Yes ,   " he said.
    = “Yes,” he said.

But not the other way 'round.

    | Muttering   "Yes,"    he turned around.
    = Muttering   “Yes,”    he turned around.

Quotes do not match across paragraphs.

    | Turbid "Waters" that "leak.
    | 
    | You "don't" have a clue.
    = Turbid “Waters” that “leak.
    = 
    = You “don't” have a clue.

Single spaces before apostrophes are elided in some situations.

    | It wasn 't Arthur 's car.
    = It wasn't Arthur's car.

Punctuation at the beginning of a line is elided in some cases.

    | , where he said so.
    = Where he said so.

Capitalization is applied at the beginning of a line, and the
beginning of a sentence.

    | , where. he said so.
    = Where. He said so.

    | Really?    that was... so
    = Really?    That was... so

Two full stops becomes an ellipsis.  Full stop then comma becomes
just a comma.

    | It was.. the nice., thing.
    = It was... the nice, thing.

[Falderal]:                https://catseye.tc/node/Falderal
[shelf]:                   https://catseye.tc/node/shelf
[available on Docker Hub]: https://hub.docker.com/r/catseye/t-rext
