Title: ~/.tmux.conf
Date: 2013-05-09 01:09
Tags: tmux, linux
Category:
Slug: tmux_conf
Author: Finbarr O'Callaghan
Summary: a ~/.tmux.conf, with some justification
Modified:


[tmux][1]...
>is a terminal multiplexer

>it lets you switch easily between several programs in one terminal, detach them
>(they keep running in the background) and reattach them to a different
>terminal.

the benefits of using a multiplexer in the "cloud" and indeed on normal
computers are pretty much self evident, so i won't repeat them here..

[screen][2] is probably still the more popular multiplexer, but its development
has slowed to a crawl and its configuration is an absolute nightmare...
  
motivating a move from screen tmux is a bit of a tough one, the workflow is
somewhat different, the default bindings are different and the initial benefits
aren't immediately apparent.. however, if you've attempted to do any configuring
of your screen environment or to change some of the core settings then you'll
immediately appreciate the clearness and simplicity of a `~/.tmux.conf`


###tmux setup
current full setup is available [here][3], but i'll run through a few of the
highlights.. and some of the features which make switching from screen or in
fact using tmux in general worthwhile

####usage

    ::::text
    set -g default-shell /usr/bin/zsh
    set default-path "$PWD"
    set -sg escape-time 0

    unbind C-b
    set -g prefix C-a
    bind C-a send-prefix
    set -g default-terminal "screen-256color"
    
    set-window-option -g mode-keys vi
    set-window-option -g mode-mouse off
    
    set -g history-limit 100000
    set -g bell-action any
    set -g display-panes-time 500
    set -g set-titles on
    set -g aggressive-resize on
    set -g set-titles-string "tmux.#I.#W"
    set -g display-time 2000
<br/>
the first thing to note here is that we immediately rebind the prefix key to
`^a`, the tmux default is `^b` as it was initially developed within a screen
session.. of course, nesting tmux sessions is eminently doable, you just send
two `^a` 's... which is super handy if you ssh to a machine from within a
session and need to pick up from where you left off on that machine..

of course, if you go down a further tmux you'll need to send four `^a` 's, that
is, two `^a` 's to the second one down... it doesn't make much sense unless you
try it in fairness, but once you get the hang of it it's super useful.. if i get
around to it i'll post a gif or something..

anyway, here we've changed the prefix key and set the `default-path`, which
ensures any new panes/windows we open are opened in our current working
directory, rather than the directory the session is initially started in
    
####looks
    ::::text 
    set -g pane-active-border-fg blue
    
    setw -g utf8 on
    
    set -g status-utf8 on
    set -g status-interval 10
    
    set -g status-fg white
    set -g status-bg default                                           
    set -g status-left "#[fg=colour14]#(whoami)@#h #[fg=colour11]#S#[default]"
    set -g status-right "#[fg=white] #(date "+%-d/%-m/%y") #[fg=colour14]%H:%M:%S #[default]"
    
    set -g status-left-length 100                          
    set -g status-right-length 50                          
    set -g message-fg white
    set -g message-bg black
    set -g message-attr bright
    
    # default window title colors
    set-window-option -g window-status-fg white
    set-window-option -g window-status-bg default
    set-window-option -g window-status-attr dim
    
    # active window title colors
    set-window-option -g window-status-current-fg colour10
    set-window-option -g window-status-current-bg default
    set-window-option -g window-status-current-attr dim
<br/>
of course this part of the settings file isn't exactly crucial, but for me, the
[default][4] look of tmux is a bit ugly.. maybe i'm being fussy, but the luminous
green does nothing for me 

[<p align="center"><img src="/static/images/tmux_looks_thumb.png" alt="tmux looks"/></p>][5]
a nicer statusbar and less green in general makes things a bit easier on the
eye.. also, note here that the bottom right pane here contains another tmux session
on another machine.. 
    
####bindings
    ::::text
    bind-key S split-window
    bind-key | split-window -h
    
    bind -r C-h select-pane -L
    bind -r C-j select-pane -D
    bind -r C-k select-pane -U
    bind -r C-l select-pane -R
    
    #bind C-c run "tmux save-buffer - | xclip -i -selection clipboard"
    
    #copy tmux paste buffer to clipboard
    bind C-c run "tmux show-buffer | xclip -i -selection clipboard"
    #copy clipboard to tmux paste buffer and paste tmux paste buffer
    bind C-v run "tmux set-buffer --- \"$(xclip -o -selection clipboard)\"; tmux paste-buffer"
    
    bind-key 'a' last-window
    
    bind 'h' swap-window -t -
    bind 'l' swap-window -t +
    
    bind 'j' command-prompt -p "join pane from:"  "join-pane -s '%%'"
    bind 'k' command-prompt -p "send pane to:"  "join-pane -t '%%'"
    bind '@' command-prompt -p "send pane to:"  "join-pane -t ':%%'"
    
    bind 'n' next-layout
    
    bind -r J swap-pane -D
    bind -r K swap-pane -U
    
    bind Escape copy-mode
    bind -t vi-copy 'y' copy-selection  
    
    bind-key 'Space' next-window
    
    # meta left/right cycles windows
    bind-key -n M-right next
    bind-key -n M-left prev
    
    bind-key M-1 select-layout even-horizontal
    bind-key M-2 select-layout even-vertical
    bind-key M-3 select-layout main-horizontal
    bind-key M-4 select-layout main-vertical
    bind-key M-5 select-layout tiled
    bind-key M-6 select-layout 3a34,279x79,0,0[279x59,0,0,279x19,0,60]
    
    bind r source-file ~/.tmux.conf
<br/>
a few bindings to make tmux more "screen"-like..  the `bind -r` in particular is
useful, for repeatable commands, like moving around between panes.. or, as the
man page says

>the -r flag indicates this key may repeat

###an unassorted list of nice things about tmux
anyone familiar with screen will know the basic model is something like 
<p align="center">`session -> window`</p> 
so, you have a bunch of sessions, and within each session you can move around
windows pretty much however you want..  this separation means that an individual
screen session can crash without taking down everything, but has several notable
disadvantages.. individual sessions know nothing about each other, switching
between them is somewhat awkward..  for example copying/pasting stuff between sessions
requires you to use an external clipboard  

the tmux model however is pretty much
<p align="center">`sever -> session -> window -> pane`</p> 
which initially seems a bit complicated, and it certainly takes a bit of getting
used to, but it overcomes many of the annoying things about screen.. with tmux
you run your sessions under a single server, this means you can easily switch
sessions and send stuff around between them without having to detach and reattach

####:choose-tree (only in newer versions)
`^a s` brings up a prompt showing your list of sessions and what's going on in them 
<p align="center"><img src="/static/images/tmux_choosetree.png" alt="tmux"/></p>
here you can choose wherever you need to go

####buffers
as a consequence of all the tmux sessions running under a single server your
paste buffer is available everywhere, its functionality is very similar to vim
buffers, which is nice..

####send anything anywhere...
another consequence of the single server thing is that you can send any
individual pane pretty much wherever you want, with
<p align="center">`:join-pane -t session_name:window_number.pane_number`</p> 


[1]: http://tmux.sourceforge.net/
[2]: http://www.gnu.org/software/screen/
[3]: https://raw.github.com/finbarrocallaghan/dotfiles/master/tmux.conf
[4]: http://tmux.sourceforge.net/tmux3.png
[5]: /static/images/tmux_looks.png
