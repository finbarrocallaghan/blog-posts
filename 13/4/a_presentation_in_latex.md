Title: a presentation in LaTeX
Date: 2013-04-13 16:50
Tags: LaTeX, linux, beamer
Category:
Slug: a_presentation_in_latex
Author: Finbarr O'Callaghan
Summary: building a presentation in LaTeX, with beamer
Modified: 2013-05-10 20:50

typesetting in LaTeX has various advantages for writing scientific
articles or even a thesis... but building a presentation is a bit
trickier...  so, a possible workflow, to avoid resorting to powerpoint..

##making a "masterslide"

the theme of your presentation.. beamer has a few of these built in, but
to be honest, most of them are a bit ugly (at least imo).. and also, you
can spot the default ones a mile off, which I guess isn't really a good
thing..

###an example header:

    :::latex
    \documentclass[xcolor=x11names,compress,professionalfont]{beamer}
    \usepackage{tikz,amsmath,siunitx}
    \usepackage[light,condensed]{kurier}%a nicer font
    \usepackage[T1]{fontenc}

    %\usepackage[absolute,showboxes,overlay]{textpos}
    \usepackage[absolute,overlay]{textpos}

    %define some new colors
    \definecolor{c1}{RGB}{0.0,204.0,255.0}%{{{
    \definecolor{b1}{RGB}{31,65,136}
    \definecolor{g1}{RGB}{0,100,0}

    %set colors for the headings & items
    \setbeamercolor*{normal text}{fg=black,bg=white}
    \setbeamercolor*{alerted text}{fg=red}
    \setbeamercolor*{example text}{fg=black}
    \setbeamercolor*{structure}{fg=b1}
    \setbeamertemplate{navigation symbols}{} %get rid of the navigation symbols
    %}}}

    \setbeamertemplate{footline}{ %{{{
    \begin{tikzpicture}[remember picture,overlay]
      \node[xshift=-1.9cm] at (current page.south east)
      {
        \begin{tikzpicture}[remember picture, overlay]
          \includegraphics[width=1.75cm]{figs/files/tyndall_logo};
        \end{tikzpicture}
      };
      \node at (current page.south west)
      {
        \begin{tikzpicture}[remember picture, overlay]
          \draw[color=b1,fill=b1] (0.845\paperwidth,0) -- ++(-0.1627cm ,+0.15cm)
                                                -- (0,0.15cm) -- (0,0) -- cycle;
        \end{tikzpicture}
      };
    \end{tikzpicture}
    }%}}}


<br/>


###a few comments on the header..
    ::::latex
    \documentclass[xcolor=x11names,compress,professionalfont]{beamer}
<br/>
as we're using a sans-serif font for the presentation, LaTeX will attempt to
render mathematics in this font, which we don't really want (it looks a bit
strange)..  so, including `professioinalfont` here sorts it

    ::::latex
    %\usepackage[absolute,showboxes,overlay]{textpos}
    \usepackage[absolute,overlay]{textpos}
<br/>
we'll be using the `textpos` package to place text on the screen, possibly not
the best approach, but it works..  using `showboxes` makes the placement of
text a lot easier when you're working on a draft, also, using `absolute`
positioning makes things a lot less confusing, it's `relative` by default afaict

    ::::latex
    \setbeamertemplate{navigation symbols}{} %get rid of the navigation symbols
<br/>
these seem to serve no purpose other than making the audience aware you've used
beamer to make your presentation, which i guess isn't a bad thing, but they do
look a bit silly

anyway, using the header above should produce something looking like:
[<p align="center"><img src="/static/images/template_thumb.png" alt="template example"/></p>][2]

##creating content
there seems to be a few different approaches to this, i'm pretty confident my approach isn't the best, but as i mentioned already, it works..
firstly, splitting your content into multiple `.tex` files makes things much easier, mostly because we can do something like:

    ::::latex
    %\includeonly{titles}
    \includeonly{intro}
    %\includeonly{fp_dynamics}
    %\includeonly{fp_statics}
    %\includeonly{fp_static_abs}
    %\includeonly{two_mode}

    \begin{document}
    \include{titles}
    \include{intro}
    \include{fp_dynamics}
    \include{fp_statics}
    \include{fp_static_abs}
    \include{two_mode}
    %\include{postscript}
    \end{document}
<br/>

in our main `.tex` file.. in this instance our latex compilation only pulls in
the `intro` section, which speeds things up a lot, particularly if your
presentation contains lots of images and stuff

the nature of LaTeX means creating composite slides is very easy, especially
for keeping stuff in the same place in consecutive slides..

    ::::latex
    \begin{frame}{first topic}


    \only<1>{
    \begin{tikzpicture}[remember picture,overlay]%{{{
      \node [ xshift = 0.75cm ] at (current page.west)
      {
        \begin{tikzpicture}[remember picture, overlay]
          \includegraphics[width=6.0cm]{figs/first_picture};
        \end{tikzpicture}
      };
    \end{tikzpicture}%}}}
    }

    \only<1->{
    \begin{textblock*}{5.5cm}(6.5cm,1.25cm)
    \begin{itemize}
    \item first item
    \item second item
    \end{itemize}
    \end{textblock*}
    }

    \only<2>{
    \begin{tikzpicture}[remember picture,overlay]%{{{
      \node [ xshift = 0.75cm ] at (current page.west)
      {
        \begin{tikzpicture}[remember picture, overlay]
          \includegraphics[width=6.0cm]{figs/second_picture};
        \end{tikzpicture}
      };
    \end{tikzpicture}%}}}
    }

    \end{frame}
<br/>
now, this should be more or less self explanatory, but here, we're showing a
picture and a few itemized pieces of text on the first slide, and then, on the
second slide, we keep the text the same, but show a different image.. there's
really not a whole pile more too it..

##compiling ALL the things

    ::::text
    latexmk -pvc -pdf tex_file_containing_all_the_include_and_includeonly_bits
<br/>
should compile and open your document, and the `-pvc` flag makes sure your
`pdf` gets updated every time you make a changes in your `.tex` files.. the
`-pdf` flag is assuming your graphics are pdfs, obviously if you're using `eps`
or `ps` graphics you'd use `-ps` instead

###acrobat reader is a bit of a mess
whilst this isn't really news to anyone.. given that the usual pdf reader found
on windows machines you'll likely be using to give your presentation, it might
be worthwhile running

    :::text
    gs -dSAFER -dBATCH -dNOPAUSE  -sDEVICE=pdfwrite -sOutputFile=output.pdf input.pdf
<br/>
as suggested [here][1], due to a bug in acrobat reader

##the difficult part
creating interesting content

[1]: http://tex.stackexchange.com/q/64448
[2]: /static/images/template.png
