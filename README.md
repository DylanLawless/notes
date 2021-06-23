# Zettelkasten notes
This is my method for creating literature notes and writing.

1. Tags written as comments in .md.
* < !-- [[tag1]] -- >
* < !-- [[tag2]] -- >
2. Citations are written as @key, which will be recognised by latex_engine: xelatex and reference to my master bibliography.
3. There are three types of filenames: timestamp-notetype-filename.md.
	- 202106231101-fleet-one_note.md
	- 202106231102-lit-one_paper.md
	- 202106231103-perm-complex_topic.md

## Fleeting notes
* Single source or idea from single day.
* Name or url of source at top.
* No biblio unless it is going to literature note.

## Literature
* Literature notes only have one connection, to the book they came from.
* Citation using .bib.

## Permanent 
* Permanent notes can have many connections (to individual notes, as part of multiple topics etc).

# Set up
## bash_profile
The bash profile sets alias so that either fleeting note, or literature note can be created for any current directory.
The correct path is set, timestamp, and user input filename. 

``` bash

# Start a fleeting note with timestamp and filename - see vimrc for details
alias note=f
f() {
        vim ~/web_local/notes/$(date "+%Y%m%d%H%M")-fleet-"$1".md
}

alias notelit=g
g() {
        vim ~/web_local/notes/$(date "+%Y%m%d%H%M")-lit-"$1".md
}

# Master bibliography
alias bib='vim  /Users/username/Library/Mobile\ Documents/com~apple~CloudDocs/bib/master_bibliography.bib'
```

## Vimrc
Within the current file, start a new file with
"leader (spacebar) nz" 

Vimrc outputs this command
":NewZettel filename"

Make a md link to it with CtrlP (vim pluing requirement),
then add the link with ctrlX.

"ctrl+p" select the file
"ctrl+x" to paste the markdown link.

Here is the ~/.vim/vimrc commands. I am sure it could be written more neatly.

``` bash

" {{{ zettelkasten
let g:zettelkastenf = "~/web_local/notes/"
let g:zettelkastenl = "~/web_local/notes/"
let g:zettelkastenp = "~/web_local/notes/"

command! -nargs=1 NewZettelf :execute ":tabnew" zettelkastenf . strftime("%Y%m%d%H%M") . "-fleet" . "-<args>.md"
command! -nargs=1 NewZettell :execute ":tabnew" zettelkastenl . strftime("%Y%m%d%H%M") . "-lit" . "-<args>.md"
command! -nargs=1 NewZettelp :execute ":tabnew" zettelkastenp . strftime("%Y%m%d%H%M") . "-perm" . "-<args>.md"
" You can read about this by typing :help strftime and otherwise this is a good resource https://vim.fandom.com/wiki/Insert_current_date_or_time.

" New Zettle
nnoremap <leader>nzf :NewZettelf 
nnoremap <leader>nzl :NewZettell 
nnoremap <leader>nzp :NewZettelp 

" CtrlP function for inserting a markdown link with Ctrl-X
function! CtrlPOpenFunc(action, line)
   if a:action =~ '^h$'
      " Get the filename
      let filename = fnameescape(fnamemodify(a:line, ':t'))
	  let filename_wo_timestamp = fnameescape(fnamemodify(a:line, ':t:s/\d\+-//'))

      " Close CtrlP
      call ctrlp#exit()
      call ctrlp#mrufiles#add(filename)

      " Insert the markdown link to the file in the current buffer
	  let mdlink = "[ ".filename_wo_timestamp." ]( ".filename." )"
      put=mdlink
  else
      " Use CtrlP's default file opening function
      call call('ctrlp#acceptfile', [a:action, a:line])
   endif
endfunction

let g:ctrlp_open_func = {
         \ 'files': 'CtrlPOpenFunc',
         \ 'mru files': 'CtrlPOpenFunc'
         \ }

" }}}
```

