Memo 
=====

.. _Introduction:

Memo everything

Change files' name to random strings with digits.
----------

From all files in the current working directory ./targetdata/ to random strings with digits

.. note::

   Uncomment 'os.renames(tdir+item, tdir+rname)'
   
.. code-block:: console

   import os
   import random
   import string

   if __name__ == '__main__':
       tdir = './targetdata/'
       tlist = os.listdir(tdir)

       numitem = len(tlist)
       iidx = 1
       for item in tlist:
           pw = "".join([random.choice(string.ascii_uppercase) for _ in range(15)])
           pw1 = "".join([random.choice(string.ascii_lowercase) for _ in range(15)])
           pw2 = "".join([random.choice(string.ascii_letters) for _ in range(15)])
           pw3 = "".join([random.choice(string.digits) for _ in range(15)])
           tlist = list(pw+pw1+pw2+pw3)
           random.shuffle(tlist)
           rname = "".join(tlist)

           tprefix = f"[{iidx:04d}/{numitem:04d}]"
           print(f"{tprefix} From : {tdir+item}")
           print(f"   To : {tdir+rname}".rjust(len(tprefix)+len(f"   To : {tdir+rname}"), ' '))
           iidx = iidx + 1

           # os.renames(tdir+item, tdir+rname)
