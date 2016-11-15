# git-pisect - Parallel regression finder

# Install

    $ wget https://raw.githubusercontent.com/hoelzro/git-pisect/master/git-pisect
    $ cpanm List:MoreUtils
    $ cpanm Sys::Info # optional, used for CPU detection
    
# Usage

    $ git bisect start
    $ git bisect bad 1234abc
    $ git bisect good xyz0987
    $ git pisect command [args]

`git-pisect` attempts to detect the number of CPUs you have and runs that many tests
at once - you can specify the number of jobs to run with the `PISECT_JOBS` environment
variable.

# Dependencies

Requires the [List::MoreUtils](https://metacpan.org/pod/List::MoreUtils) Perl module, and optionally the [Sys::Info](https://metacpan.org/pod/Sys::Info)
module if you want to automatically detect the number of jobs to run on non-Linux systems.

# Caveats

This is kind of proof-of-concept and is experimental, so the following are things to keep in mind while playing with `git-pisect`:

  * I have no idea how it'll handle a flapping test (eg. multiple good/bad boundaries); it should converge on a boundary of some kind, though.
  * It won't work if the test suite needs input from STDIN.
  * It won't work if the test suite isn't safe to run in a parallel environment.
  * The output of tests is discarded - it would be preferable to log it somewhere.
  * Skipping commits isn't implemented.
  * The "number of steps" metric isn't accurate, since it currently uses the existing `git-bisect` machinery to print the progress report.
