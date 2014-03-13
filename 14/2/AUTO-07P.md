Title: AUTO-07P
Date: 2014-02-04 21:11
Tags: linux, python, bifurcations, continuation
Category:
Slug: AUTO-07P
Author: Finbarr O'Callaghan
Summary: continuation and bifurcation software for ordinary differential equations
Modified:

>AUTO is a software for continuation and bifurcation problems in ordinary
>differential equations

this is a relatively complex tool, with more or less excellent [documentation][1][pdf]..
but in places is a little bit rough around the edges.. that said, for such a
specialised area there's really not much to complain about.. and anyway, the
edges are pretty easy to tidy up

anyway, the main issues I came up against..

###enabling the ipython interface

if you've installed everything somewhat sensibly then `auto -i` should open up
AUTO with the ipython interface, which is very handy, tab-completion and all
the other good things ipython brings are a _must_ for working with AUTO

in the latest release however, it doesn't work with versions of ipython > 11.0,
and AUTO complains about ipython not having been installed..  fortunately this
has already been fixed in the dev version

replacing `~/auto/07p/python/interactiveBindings.py` with [this][2] should sort
it


###plotting in matplotlib

there's lots of dependencies required to make this work, and in fairness, most
of them are mentioned in the docs

however, the main culprit for this problem is the contents of
`~/auto/07p/pythongraphics/grapher.py`

    :::python
    #!/usr/bin/env python
    try:
        import Tkinter
        import tkSimpleDialog
        import tkFileDialog
    except ImportError:
        import tkinter as Tkinter # Python 3
        from tkinter import simpledialog as tkSimpleDialog
        from tkinter import filedialog as tkFileDialog
<br />
where the try/except buries any issues with importing the stuff required for
fancy plotting and drops you into the old school Tkinter interface.. (which is
awful)... commenting out the try/except and dealing with the errors arising
from the first 3 imports led me to realise I was missing the `tk-dev` package
(on ubuntu 12.04), which appears to be an unmentioned dependency

###max/min, plots

generally, when following limit cycles people tend to plot just the
maxima of the cycle.. if you also want to plot the minima (to overlay with
numerics for example) you can use the `IPLT` flag.. in the following run I'm
following a Hopf bifurcation and monitoring the max/min of U(1).. for max/min of
U(2) you would set `IPLT=-2` etc

    :::text
    AUTO In [3]: r3 = run(r2('HB1'),c='yam.2',UZR={-2:3},ISW=1,IPS=2,ILP=0,IPLT=-1)
    Starting yam ...

      BR    PT  TY  LAB    PAR(2)        MIN U(1)      MAX U(1)      MAX U(2)      MAX U(3)
      13   100       19   7.18230E+00   1.38549E-01   5.52980E+00   1.47324E-01   1.17757E-01
      13   200       20   6.33871E+00   1.93734E-02   7.18718E+00   1.92397E-01   1.41228E-01
      13   300       21   5.68379E+00   3.50677E-03   7.75285E+00   2.11506E-01   1.48706E-01
      13   400       22   5.18226E+00   7.23511E-04   7.88476E+00   2.20634E-01   1.51414E-01
      13   500       23   4.79096E+00   1.65134E-04   7.80820E+00   2.24777E-01   1.52480E-01
      13   600       24   4.47883E+00   4.13158E-05   7.61930E+00   2.26042E-01   1.52923E-01
      13   700       25   4.22482E+00   1.12902E-05   7.36622E+00   2.25495E-01   1.53115E-01
      13   800       26   4.01451E+00   3.36259E-06   7.07585E+00   2.23743E-01   1.53200E-01
<br />

now, you can use the in built matplotlib plotting tools if you're preparing a
figure for publication, but if you want to overlay results on numerical
simulations or do some type complex plot in plain matplotlib.. then your best
bet is to get the data out of AUTO and far away as fast as possible....

a crude regex that i've been using (for example)

    :::text
    AUTO In [4]: !tail -n +17 fort.7 | awk '{print $2","$5","$7","$8}' \
                                     | sed -e '1s/^/st,js,i1,i2\n/'    \
                                     | sed 's/\([0-9]\)-\([0-9]\)/\1E-\2/g' > hopf.sol
<br />
here you get out information about the stability of the equilibrium/limit cycle
from the sign of $2 (negative for stable, positive for unstable), and you can
then pick out whichever other columns are useful.. (note, this isn't something
you'll need to be doing regularly, but it's still useful to know how)

now, this bifurcation information is contained in the file hopf.sol..

my strategy for plotting data I've exported from AUTO then goes something like..

    :::python
    from pylab import *
    import pandas as pd

    hp = pd.read_csv('hopf.sol')

    stable_hp = hp[ hp['st'] < 0 ]
    unstable_hp = hp[ hp['st'] > 0 ]

    plot( stable_hp['js'], stable_hp['i1'], 'b-')
    plot( unstable_hp['js'], unstable_hp['i1'], 'b--')

    #plot(data_from_somewhere_else_etc)


<br />

obviously the plotting tools built in to AUTO are just wrappers of matplotlib
anyway, but if you're preparing something for publication or similar, it's
useful to be able to break out of the limitations it imposes


[1]: http://www.dam.brown.edu/people/sandsted/auto/auto07p.pdf
[2]: https://bitbucket.org/z2v/auto-07p/commits/cb686f32b7e3114c6c105cb2679b2af5 
