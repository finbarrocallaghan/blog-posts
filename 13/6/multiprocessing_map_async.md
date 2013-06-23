Title: multiprocessing's map_async()
Date: 2013-06-04 23:19
Tags: python
Category:
Slug: multiprocessing_map_async
Author: Finbarr O'Callaghan
Summary: using all your cores with python
Modified:

a working example

    :::python
    import subprocess as sp
    import multiprocessing as mp
    import numpy as np

    def run_job(d):
      sp.call(["echo","running var1: {} var2: {} var3: {} var4: {}".format(
        d['var1'],d['var2'],d['var3'],d['var4']) ])


    var1_vals = np.array([0.1,0.2])
    var2_vals = np.array([0.9,1.0,1.1,1.2])

    print np.size(var2_vals)
    print

    pd = []

    for var1 in var1_vals:
      for var2 in var2_vals:
        pd.append({
          'var1':var1,
          'var2':var2,
          'var3':1.1,
          'var4':1.0
        }) 
        

    po = mp.Pool()
    res = po.map_async(run_job, pd)
    po.close()
    po.join()
    print
<br />


produces something like the following output

    :::text
    sample/ $ python map_async_example.py            
    4

    running var1: 0.1 var2: 0.9 var3: 1.1 var4: 1.0
    running var1: 0.1 var2: 1.0 var3: 1.1 var4: 1.0
    running var1: 0.1 var2: 1.1 var3: 1.1 var4: 1.0
    running var1: 0.1 var2: 1.2 var3: 1.1 var4: 1.0
    running var1: 0.2 var2: 1.0 var3: 1.1 var4: 1.0
    running var1: 0.2 var2: 0.9 var3: 1.1 var4: 1.0
    running var1: 0.2 var2: 1.1 var3: 1.1 var4: 1.0
    running var1: 0.2 var2: 1.2 var3: 1.1 var4: 1.0
<br />

and is actually pretty useful.. particularly if you're trying to wrap some
legacy code you didn't write in some odd language... for arguments sake, let's
say we have a fortran executable you'd like to run for a whole load of
parameters, which accepts its arguments from STDIN... in this case, we'd
change the argument of `sp.call` to contain something like `./a.out var1 var2
var3 var4`.. now, in my case, we're using all four cores on the machine and
everything is a lot less tedious than trying to get fortran/C to do the same 
