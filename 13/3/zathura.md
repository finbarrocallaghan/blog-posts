Title: zathura
Date: 2013-03-21 22:50
Tags: linux, productivity
Category:
Slug: zathura
Author: Finbarr O'Callaghan
Summary: a minimalist, customizable pdf reader
Modified:

spending a lot of time looking at pdfs in evince/okular/acroread can get
old pretty quickly... [zathura][1] is 

>...a highly customizable and functional document viewer. It
>provides a minimalistic and space saving interface as well as an easy
>usage that mainly focuses on keyboard interaction.

which, it pretty much is.. using a *zathurarc* you can define a
colorscheme to make reading pdf's etc a bit easier on the eye, for example, setting

    :::text
    set recolor_darkcolor #999
    set recolor_lightcolor #333
    set recolor


<br />
in `$XDG_CONFIG_DIR/zathura/zathurarc` makes your {pdf,djvu,ps}s look like: 

<p align="center"> <img src="/static/images/zathura.png" alt="zathura"/></p>
which is nice, and having vim bindings in more places can never be a bad
thing

[1]: http://pwmt.org/projects/zathura/
