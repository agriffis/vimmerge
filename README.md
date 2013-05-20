vimmerge
========

This script can run in one of five modes:

  1. 2-way merge, this is the same as vimdiff
  2. 2-way merge with output to a new file
  3. 3-way merge with output to "mine"
  4. 3-way merge with output to a new file
  5. 2-way merge with all input coming from one file, 
     which uses markup similar to the output of RCS merge.
     In this case, only a single input file is given on the cmdline.

By default it assumes the same file ordering as diff3, but unlike diff3 it
will save the results in "mine" by default:

    vimmerge mine older yours

To write to a new file, use -w:

    vimmerge -w merged mine older yours

To completely change the order of the incoming parameters, either specify them
explicitly or use --order:

    vimmerge -o older -y yours -m mine -w merged
    vimmerge --order oymw older yours mine merged

To use mode 5, where the input file is the result of running RCS merge,
simply specify a single input file, optionally specifying an alternate output
file.

    vimmerge conflicted
    vimmerge -w merged conflicted
