Title: pandas.read_csv() >>> numpy.loadtxt()!
Date: 2013-11-03 20:55
Tags: linux, pandas, python
Category:
Slug: pandas_read_csv
Author: Finbarr O'Callaghan
Summary: doing the same thing, but faster
Modified:

say we have a data file looking something like..

    :::text
    ~/ $ ls -sh data.dat
    4.8M data.dat
    ~/ $ head data.dat
    0.000000000000000000e+00 9.153280041166816927e-01
    1.000000000000000000e+00 6.518845981928764743e-01
    2.000000000000000000e+00 2.363067113212813375e-02
    3.000000000000000000e+00 8.937018454081635532e-02
    4.000000000000000000e+00 4.556273383228036655e-01
    5.000000000000000000e+00 7.174616388171529691e-01
    6.000000000000000000e+00 1.088523924174348290e-01
    7.000000000000000000e+00 9.872623379303430147e-01
    8.000000000000000000e+00 2.048647330182004067e-01
    9.000000000000000000e+00 2.264187483919821720e-02
    ~/ $
<br />

now, if the file happens to contain something particularly interesting, then
chances are we'll want to read the data and use it somehow.. my favourite way of
making data like this usable is to put it in a numpy array..

and so, the following two pieces of code do pretty much exactly the same thing,
but crucially, even for this relatively small file, one is almost a full order
of magnitude faster than the other...

####numpy_read_file.py
    :::python
    import numpy as np

    file_contents = np.loadtxt('data.dat')

    x = file_contents[:,0]
    y = file_contents[:,1]
<br />

####pandas_read_file.py
    :::python
    import pandas as pd

    file_contents = pd.read_csv('data.dat', sep=' ')

    x = file_contents.ix[:,0]
    y = file_contents.ix[:,1]
<br />

    :::text
    ~/ $ for i ({1..5}) time python numpy_read_file.py
    python numpy_read_file.py  1.10s user 0.07s system 99% cpu 1.188 total
    python numpy_read_file.py  1.11s user 0.10s system 99% cpu 1.210 total
    python numpy_read_file.py  1.13s user 0.05s system 98% cpu 1.198 total
    python numpy_read_file.py  1.10s user 0.08s system 99% cpu 1.183 total
    python numpy_read_file.py  1.17s user 0.07s system 99% cpu 1.242 total
    ~/ $ for i ({1..5}) time python pandas_read_file.py
    python pandas_read_file.py  0.28s user 0.03s system 96% cpu 0.322 total
    python pandas_read_file.py  0.25s user 0.04s system 97% cpu 0.303 total
    python pandas_read_file.py  0.26s user 0.04s system 97% cpu 0.304 total
    python pandas_read_file.py  0.28s user 0.02s system 96% cpu 0.310 total
    python pandas_read_file.py  0.26s user 0.06s system 98% cpu 0.328 total
    ~/ $
<br />
