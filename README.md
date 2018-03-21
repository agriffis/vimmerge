vimmerge
========

Wrapper for merging with vimdiff. Handles 2- and 3-way merges, optionally
writing output to a new file. Also handles RCS merge markup as git
generates for merge conflicts.

git mergetool
-------------

You can use vimmerge with git mergetool like this:

    git config --global merge.tool vimmerge
    git config --global mergetool.vimmerge.cmd 'vimmerge \"$MERGED\"'

You might also want this to avoid `.orig` files littering your working
tree:

    git config --global mergetool.keepBackup false

Now when you merge with conflicts, it's like this:

    git merge branch  # conflicts!
    git mergetool     # runs vimmerge in sequence on conflicting files

At this point you have two windows in vim, just as you would have with
vimdiff. One window is the conflicts from the merge, the other window is
your file. You can use [window movement keys](http://vimdoc.sourceforge.net/htmldoc/windows.html#window-move-cursor)
and [diff commands](http://vimdoc.sourceforge.net/htmldoc/diff.html#copy-diffs) to
resolve the conflicts, then write out your file when you're done.

If you `:qa!` (quit without writing) then nothing is modified on disk.

git without mergetool
---------------------

You can also use vimmerge without git's mergetool config. Without
mergetool, it's like this:

    git merge branch  # conflicts!
    git status        # find a file that's in conflict
    vimmerge filename

Just like using git mergetool, you now have something that looks like
vimdiff, and at this point it converges with the mergetool instructions
above.

non-git modes
-------------

This script is pretty flexible and varies depending on the command-line
arguments. Two-way merges look like this:

    vimmerge one two           # same as vimdiff
    vimmerge one two -w three  # like vimdiff, with output to new file

Three-way merges are where it gets interesting, because you have a base
that's a common ancestor of the conflicting files. By default the file
ordering is the same as diff3, but unlike diff3 it saves the results in
"mine" by default:

    vimmerge mine older yours

To write to a new file, use -w:

    vimmerge -w merged mine older yours

You can also use explicit arguments for the files, rather than relying on
order:

    vimmerge -o older -y yours -m mine -w merged

Some calling programs insist on blindly passing the files in a certain
order. You can use `--order` to work around those:

    vimmerge --order oymw older yours mine merged
