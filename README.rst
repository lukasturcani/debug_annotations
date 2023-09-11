This repo demonstrates a weird bug with ``.. autosummary::``.

Steps to reproduce:

#. Clone the repo
#. ``pip install -e .``
#. ``make -C docs doctest``

At this point you will see output like::

  **********************************************************************
  File "index.rst", line 35, in default
  Failed example:
      import debug_annotations

      raise RuntimeError(debug_annotations.B.__annotations__)
  Exception raised:
      Traceback (most recent call last):
        File "/home/lukas/projects/debug-annotations/.venv/lib/python3.11/doctest.py", line 1351, in __run
          exec(compile(example.source, filename, "single",
        File "<doctest default[0]>", line 3, in <module>
          raise RuntimeError(debug_annotations.B.__annotations__)
      RuntimeError: {'y': <class 'int'>, 'x': 'int'}
  **********************************************************************

So, why is this weird? Well, the annotations of the class ``B``
should be ``{'y': <class 'int'>}`` -- because the ``x`` attribute is
inherited from ``A``.

let run the code again!

#. ``make -C docs doctest``

now our output will be::

  **********************************************************************
  File "index.rst", line 35, in default
  Failed example:
      import debug_annotations

      raise RuntimeError(debug_annotations.B.__annotations__)
  Exception raised:
      Traceback (most recent call last):
        File "/home/lukas/projects/debug-annotations/.venv/lib/python3.11/doctest.py", line 1351, in __run
          exec(compile(example.source, filename, "single",
        File "<doctest default[0]>", line 3, in <module>
          raise RuntimeError(debug_annotations.B.__annotations__)
      RuntimeError: {'y': <class 'int'>}
  **********************************************************************

This is correct! In fact if we continue to re-run the ``doctest`` --
our annotations will be correct. However if we do

#. ``rm -r docs/source/_autosummary``
#. ``make -C docs doctest``

is this some kind of caching issue?
