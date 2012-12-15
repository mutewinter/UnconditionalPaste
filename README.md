# Description

_Note: this is a clone of http://www.vim.org/scripts/script.php?script_id=3355
since it hasn't made it to GitHub yet._

If you're like me, you occasionally do a linewise yank, and then want to
insert that yanked text in the middle of some other line (or vice versa).
The mappings defined by this plugin will allow you to do a character-, line-,
or block-wise paste no matter how you yanked the text, both from normal and
insert mode.

Often, the register contents aren't quite in the form you need them. Maybe you
need to convert yanked lines to comma-separated arguments, maybe join the
lines with another separator, maybe the reverse: un-joining a single line on
some pattern to yield multiple lines. Though you can do the manipulation after
pasting, this plugin offers shortcut mappings for these actions, which are
especially helpful when you need to repeat the paste multiple times.

# Source

Based on vimtip #1199 by cory,
    http://vim.wikia.com/wiki/Unconditional_linewise_or_characterwise_paste

# Related works

- whitespaste.vim (vimscript #4351) automatically removes blank lines around
  linewise contents, and condenses inner lines to a single one. By default, it
  remaps p / P, but this can be changed.

# Usage

```
["x]gcp, ["x]gcP        Paste characterwise (newline characters and indent are
                        flattened to spaces) [count] times.

["x]glp, ["x]glP        Paste linewise (even if yanked text is not a complete
                        line) [count] times.

["x]gbp, ["x]gbP        Paste blockwise (inserting multiple lines in-place,
                        pushing existing text further to the right) [count]
                        times.

["x]g,p, ["x]g,P        Paste characterwise, with each line delimited by ", "
                        instead of the newline (and indent).

["x]gqp, ["x]gqP        Query for a separator string, then paste
                        characterwise, with each line delimited by it.

["x]gQp, ["x]gQP        Paste characterwise, with each line delimited by the
                        previously queried (gqp) separator string.

["x]gup, ["x]guP        Query for a separator pattern, un-join the register
                        contents, then paste linewise.

["x]gUp, ["x]gUP        Un-join the register contents on the previously
                        queried (gup) separator pattern, then paste
                        linewise.

CTRL-R CTRL-C {0-9a-z"%#*+/:.-}
                        Insert the contents of a register characterwise
                        (newline characters and indent are flattened to
                        spaces).
                        If you have options like 'textwidth', 'formatoptions',
                        or 'autoindent' set, this will influence what will be
                        inserted.
CTRL-R , {0-9a-z"%#*+/:.-}
                        Insert the contents of a register characterwise, with
                        each line delimited by ", " instead of the newline
                        (and indent).
CTRL-R CTRL-Q {0-9a-z"%#*+/:.-}
                        Query for a separator string, then insert the contents
                        of a register characterwise, with each line delimited
                        by it.
CTRL-R CTRL-Q CTRL-Q {0-9a-z"%#*+/:.-}
                        Insert the contents of a register characterwise, with
                        each line delimited by the previously queried (gqp,
                        i_CTRL-R_CTRL-Q) separator string.
CTRL-R CTRL-U {0-9a-z"%#*+/:.-}
                        Query for a separator pattern, un-join the contents of
                        a register, then insert it linewise.
CTRL-R CTRL-U CTRL-U {0-9a-z"%#*+/:.-}
                        Un-join the contents of
                        a register on the previously queried (gup,
                        i_CTRL_R_CTRL-U) pattern, then insert it linewise.
```

# Installation

This script is packaged as a vimball. If you have the "gunzip" decompressor
in your PATH, simply edit the *.vmb.gz package in Vim; otherwise, decompress
the archive first, e.g. using WinZip. Inside Vim, install by sourcing the
vimball or via the :UseVimball command.
    vim UnconditionalPaste*.vmb.gz
    :so %
To uninstall, use the :RmVimball command.

# Dependencies

- Requires Vim 7.0 or higher.
- repeat.vim (vimscript #2136) plugin (optional)

# Configuration

for a permanent configuration, put the following commands into your vimrc:

The default separator string for the gQp and i_CTRL-R_CTRL-Q_CTRL-Q
mappings is a <Tab> character; to preset another one (it will be overridden by
gqp and i_CTRL-R_CTRL-Q), use:
    let g:UnconditionalPaste_JoinSeparator = 'text'

The default separator pattern for the gUp and i_CTRL-R_CTRL-U_CTRL-U
mappings matches any whitespace and newlines (i.e. it will get rid of empty
lines); to preset another one (it will be overridden by gup and
i_CTRL-R_CTRL-U), use:
    let g:UnconditionalPaste_UnjoinSeparatorPattern = '-'

If you want to use different mappings (e.g. starting with <Leader>), map your
keys to the <Plug>UnconditionalPaste... mapping targets _before_ sourcing this
script (e.g. in your vimrc):

```vim
    nmap <Leader>Pc <Plug>UnconditionalPasteCharBefore
    nmap <Leader>pc <Plug>UnconditionalPasteCharAfter
    nmap <Leader>Pl <Plug>UnconditionalPasteLineBefore
    nmap <Leader>pl <Plug>UnconditionalPasteLineAfter
    nmap <Leader>Pb <Plug>UnconditionalPasteBlockBefore
    nmap <Leader>pb <Plug>UnconditionalPasteBlockAfter
    nmap <Leader>P, <Plug>UnconditionalPasteCommaBefore
    nmap <Leader>p, <Plug>UnconditionalPasteCommaAfter
    nmap <Leader>Pq <Plug>UnconditionalPasteQueriedBefore
    nmap <Leader>pq <Plug>UnconditionalPasteQueriedAfter
    nmap <Leader>PQ <Plug>UnconditionalPasteRecallQueriedBefore
    nmap <Leader>pQ <Plug>UnconditionalPasteRecallQueriedAfter
    nmap <Leader>Pu <Plug>UnconditionalPasteUnjoinBefore
    nmap <Leader>pu <Plug>UnconditionalPasteUnjoinAfter
    nmap <Leader>PU <Plug>UnconditionalPasteRecallUnjoinBefore
    nmap <Leader>pU <Plug>UnconditionalPasteRecallUnjoinAfter

    imap <C-G>c <Plug>UnconditionalPasteChar
    imap <C-G>, <Plug>UnconditionalPasteComma
    imap <C-G>q <Plug>UnconditionalPasteQueried
    imap <C-G>Q <Plug>UnconditionalPasteRecallQueried
    imap <C-G>u <Plug>UnconditionalPasteUnjoin
    imap <C-G>U <Plug>UnconditionalPasteRecallUnjoin
```
