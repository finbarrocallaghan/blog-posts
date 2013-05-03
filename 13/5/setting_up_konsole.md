Title: setting up konsole
Date: 2013-05-03 17:10
Tags: linux, console, kde, konsole
Category:
Slug: setting_up_konsole
Author: Finbarr O'Callaghan
Summary: making konsole usable


[konsole][1] is the terminal emulator bundled with KDE.. it's one of the faster
emulators available and competes very well with urxvt and friends. however, as
is the case with many of these types of applications the default settings are
certifiably ridiculous. fortunately though, it can be made to look and behave
sensibly..  but it requires a bit of effort..

###a konsole without borders
by right clicking somewhere in the middle of your konsole window and choosing
`configure current profile` you can access most of the settings of interest. I
generally like to hide the tab/scroll/menu bars, they don't really offer much in
any case, just take up space! 

another setting i like use is to drop the kwin titlebar and frame.. to do this
you have to right click your konsole title bar.. bringing up the following
window.. when you're sorted it should look something like: 

<p align="center"><img src="/static/images/konsole_window_settings.png" alt="konsole window settings"/></p>

###a konsole 'kolorscheme'
the next step in de-uglifying  konsole is to sort a decent colorscheme.. there's
a few decent-ish looking ones, [Soloarized][2] being notable amongst them.. but
we can always do better! one of the more popular ways of setting colorschemes
for terminals like urxvt and aterm is in the `~/.Xdefaults`.. konsole on the
other hand has its own unique (afaik) colorscheme format, which makes converting
to and fro a bit awkward.. a thread on the [crunchbang][3] forums with some very
nice themes inspired me to make an effort at [converting them][4] into something
konsole could handle.. 

###the end result

<p align="center"><img src="/static/images/konsole_colorscheme.png" alt="example colorscheme"/></p>

[1]: http://konsole.kde.org/
[2]: http://ethanschoonover.com/solarized
[3]: http://crunchbang.org/forums/viewtopic.php?id=9935
[4]: https://github.com/finbarrocallaghan/konsole_kolors
