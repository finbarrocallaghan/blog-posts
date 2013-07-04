Title: compiling tmux locally
Date: 2013-06-25 21:30
Tags: tmux
Category:
Slug: compiling_tmux_locally
Author: Finbarr O'Callaghan
Summary: build tmux, without root
Modified:

[tmux][1] is super handy, even if you don't own the box.. so, you might be
motivated to compile it locally if you need to get some stuff done

it's not too difficult, but requires a non-trivial amount of RTFM

anyway, let's say we want to do the build in *~/dev*
<p align="center">`git clone https://github.com/ThomasAdam/tmux.git ~/dev/tmux_git`</p>
is your first step.. in the *~/dev/tmux_git* dir you can do *./autogen.sh*
which should get you a *configure* script.. now, if all the libraries and
stuff are already on the box then you could just do *./configure && make*
and have a functioning tmux, generally though, there'll be a few missing
dependencies, [libevent][2] seems to be the most commonly missing one though.. 

this means we also have to build libevent and maybe [ncurses][3] locally,
which, if you made it to here is probably where you're stuck.. download
the *.tar.gz* files, build them locally, doing something like: 
<p align="center">`./configure --prefix=$(echo ~/dev/)`</p>
followed by *make && make install*

you should hopefully by now have all the include files and stuff required
for building tmux.. (double check you have *~/dev/include* and *~/dev/lib*
containing stuff like *event.h* and all the *.so* files..

finally to compile tmux.. if 
<p align="center">`CFLAGS="-I$(echo ~/dev/include)" LDFLAGS="-L$(echo ~/dev/lib)" ./configure --enable-static`</p>
and *make* work without any complaints.. you should now have a tmux executable in
*~/dev/tmux_git/tmux*, which you can create a soft link to in *~/.local/bin*
or somewhere else in your $PATH

[1]: http://tmux.sourceforge.net/
[2]: http://libevent.org/
[3]: http://www.gnu.org/software/ncurses/
