# Gdb-vim
Simple cheat sheet for getting gdb working with vim in order to create a simple multi-screen development environment.

REQUIREMENTS
============

1. Upgrade vim > 8.1 
https://itsfoss.com/vim-8-release-install/

2. Check Gdb > 8.1 is installed and working
https://www.thegeekstuff.com/2010/03/debug-c-program-using-gdb/
upgrade Gdb trick
https://stackoverflow.com/questions/32773255/how-to-update-gdb-to-most-current-stable-version

3. vim/gdb Usage intro
https://www.dannyadam.com/blog/2019/05/debugging-in-vim/
https://vimhelp.org/terminal.txt.html

4. basic tutorial Gdb
https://www.howtoforge.com/tutorial/how-to-debug-c-programs-in-linux-using-gdb/
https://www.thegeekstuff.com/2010/03/debug-c-program-using-gdb/

Gdb Docs:
=========
TOC 			http://sourceware.org/gdb/current/onlinedocs/gdb/
Download pdf 		https://www.sourceware.org/gdb/documentation/



	==========================
	 A. Basic setup and usage
	========================== 

Invoke Termdebug
================

:packadd termdebug
:Termdebug [name of file - no .c]

Run a file with args
====================
GDB win : r(un) [args]

Vim status:
:Run [args]
:Stop,
:Over        execute the gdb "next" command (`:Next` is a Vim command)
:Continue

Close dbg
=========
CTR+d
q(uit)

Window Management
=================
Go to main vim window

:set number <-- set up numbering for debugging

CTR-W then L <-- cap L shifts vim win to vertical Right

:vertical res 80 <--still in main vim window - 80 chars wide

Move around windows
===================
CTR-W w (cycle windows)
Or,
CTR-W p (previous window)

Move from vim into GDB window
:Gdb 

:redraw! 	<-- get rid of artefacts after accidental gdb mouseover

GDB Window instructions
=======================
file [name of file - no .c] 
r(un) [args]
p(rint) 	<--  var - dump a var
b(reak) 	<--  set break at line number
d(elete)	<--  rm break
r[un] 		<--  Run to next breakpoint or to end
s[tep] 		<--  Single-step, descending into functions
n[ext] 		<--  Single-step without descending into functions
fin[ish] 	<--  Finish current function, loop, etc. (useful!)
c[ontinue] 	<--  Continue to next breakpoint or end
up	 	<--  Go up one context level on stack (to caller)
do[wn] 	 	<--  Go down one level (only possible after up) 

OR use Vim editor win
=====================
Click on Run / Step etc with mouse
:Next etc on cmd line
:Break and :Clear to set break points
:Over        execute the gdb "next" command (`:Next` is a Vim command)

Inspect a var
=============
p var
print var 

Vim status bar :
:Eval var
:Eval + highlight (mouseover) var
:Eval (current line in editor)
or,
mouseover var and pres K

Workflow
========

load vim with file to edit
evoke gdb
sort out place of windows
set number <-- turn on vim line numbering

load file into gdb 'file [filename sans .c]'
OR when evoking gdb
compile with :
!gcc -g -Wall [filename.c] -o [filename sans .c]  
		-g dbg on 
		-o output file 
		-Wall enables all compiler's warning

r(un) [args]

	======================
	B GDB commands complex
	======================

Logging gdb output
==================

set logging on [off] <-- default gdb.txt
set logging file [filename]
set logging overwrite [else appending]

create a dir and turn on logging for that file:
!mkdir dbg
set logging file dbg/myfile.txt
set logging on


Repeat a command e.g.
n(ext) 3 <--jump 3 lines, including this line
s(tep) 3
(blank line - return) <-- repeat last command

Args:
set args [args] <--set a default arg to use for all following invocations
show args 

Arrays:
set print elements 10 <-- max limit the no of array elements shown

Rewind/restart:
	(do some debug - get to a point to freeze)
checkpoint 		<-- returns this checkpoint-id eg 1
	(go on and do some further dbg)
restart 1 		<-- restart at previous checkpoint
delete checkpoint 1  	<-- optionally clean up

NB A checkpoint is an identical copy of a process. Therefore if you create a checkpoint at (eg.) the start of main, and simply return to that checkpoint instead of restarting the process, you can avoid the effects of address randomization and your symbols will all stay in the same place. 

frame : e.g. a function being called, 

fin(ish) drop out of current frame and output to screen any return value of function

