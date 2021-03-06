.. :changelog:

Release History
---------------

5.0.0 (2019-05-03)
++++++++++++++++++
- Removed the ``executable`` argument to ``Oct2Py`` in favor of using
environment variables because we need to instantiate the ``octave``
convenience instance at startup.

4.3.0 (2019-05-02)
++++++++++++++++++
- Add a plot_backend kwarg for eval and clean up default backend selection.

4.2.0 (2019-04-28)
++++++++++++++++++
- Add support for Pandas DataFrames and Series (#125)
- Remove unwanted octave-workspace files at exit (#133)

4.1.0 (2019-04-23)
++++++++++++++++++
- Add a backend property to specify the graphics toolkit

4.0.5 (2017-04-08)
++++++++++++++++++
- Fixed a thread safety bug when writing matlab files.

4.0.0 (2017-03-07)
++++++++++++++++++
Features
--------
- Added an ``feval`` method, which can be used to call Octave functions without  creating a dynamic function.  The function also supports calling a function by path.
- The new ``feval`` method and the dynamic functions both now support a ``store_as`` argument, which saves the result of the call to the Octave workspace rather than returning it.
- Added ``get_pointer`` method that can be used to retrieve a pointer to a value in the Octave namespace, including Octave functions.  Pointers can be passed to ``feval`` or dynamic functions as function arguments.  A pointer passed as a nested value will be passed by value instead.
- Added an Oct2Py ``Cell`` ndarray subclass used for Octave cell arrays.
- Added an Oct2PY ``StructArray`` numpy ``recarray`` subclass used for Octave structure arrays.
- Added a ``stream_handler`` argument to ``eval`` and the new ``feval`` method that can be used to capture streaming output using a simple callback.

Breaking Changes
----------------
- Removed inferred ``nout`` for Octave function calls; it must be explicitly given if not ``1``.  The old behavior was too surprising and relied on internal logic of the CPython interpreter.
- Any code that received Cell or Struct Array values will need to be updated.
- Numpy booleans are now equivalent to Octave literals.  They were previously handled as uint8 when sending to Octave.
- Deprecated the use of keyword arguments to Octave function calls, use standard Octave calling conventions.
- Deprecated the ``log`` and `return_both` keyword arguments to ``eval()``. See docs on ``Oct2Py.eval()`` for more information.
- Oct2Py will no longer create dynamic functions for values that are not Octave functions - use `get_pointer` or ``pull`` instead.


3.9.0 (2017-01-28)
++++++++++++++++++
- Added support for Python 3.6 and Octave `input()` functions.


3.8.0 (2016-12-25)
++++++++++++++++++
- Added support for Octave class objects and clean up repr() and help()
  for dynamic Octave methods, (PR #104)


3.7.0 (2016-12-24)
++++++++++++++++++
- Fixed error that caused the session to crash on Windows when Octave
  encountered a syntax error.
- Added separate width and height specifiers to the `%%octave` magic so
  the image can be constrained in one dimension while maintaining its
  aspect ratio.
- Added an `extract_figures` method to the `Oct2Py` class which
  gives back a list of IPython Image or SVG objects for the created figures.
- Completely rewrote the internal communication to Octave on
  top of the `octave_kernel`, which enabled the Windows crash fix.
- Removed the internal `_make_figs.m` file, since that functionality
  is now in `octave_kernel`.


3.6.1 (2016-11-20)
++++++++++++++++++
- More plot creation cleanup - fault tolerance for svg files.


3.6.0 (2016-11-20)
++++++++++++++++++
- Cleanup of plot creation - separate _make_figs.m file


3.5.0 (2016-01-23)
++++++++++++++++++
- Disable --braindead Octave argument.


3.4.0 (2016-01-09)
++++++++++++++++++
- Improved handling of Octave executable

3.3.0 (2015-07-16)
++++++++++++++++++
- Support for Octave 4.0
- Fixes for corner cases on structs


3.2.0 (2015-06-18)
++++++++++++++++++
- Better handling of returned empty values
- Allow OCTAVE environment variable


3.1.0 (2015-02-13)
++++++++++++++++++
- Fix handling of temporary files for multiprocessing
- Clean up handling of plot settings


3.0.0 (2015-01-10)
++++++++++++++++++
- Add `convert_to_float` property that is True by default.
- Suppress output in dynamic function calls (using ';')


2.4.2 (2014-12-19)
++++++++++++++++++
- Add support for Octave 3.8 on Windows

2.4.1 (2014-10-22)
++++++++++++++++++
- Prevent zombie octave processes.

2.4.0 (2014-09-27)
++++++++++++++++++
- Make `eval` output match Octave session output.
  If verbose=True, print all Octave output.
  Return the last "ans" from Octave, if available.
  If you need the response, use `return_both` to get the
  `(resp, ans)` pair back
- As a result of the previous, Syntax Errors in Octave code
  will now result in a closed session on Windows.
- Fix sizing of plots when in inline mode.
- Numerous corner case bug fixes.


2.3.0 (2014-09-14)
++++++++++++++++++
- Allow library to install without meeting explicit dependencies
- Fix handling of cell magic with inline comments.


2.2.0 (2014-09-14)
++++++++++++++++++
- Fix IPython notebook support in Ubuntu 14.04
- Fix toggling of inline plotting


2.1.0 (2014-08-23)
++++++++++++++++++
- Allow keyword arguments in functions: `octave.plot([1,2,3], linewidth=2))`
  These are translated to ("prop", value) arguments to the function.
- Add option to show plotting gui with `-g` flag in OctaveMagic.
- Add ability to specify the Octave executable as a keyword argument to
  the Oct2Py object.
  - Add specifications for plot saving instead of displaying plots to `eval` and
    dynamic functions.


2.0.0 (2014-08-14)
++++++++++++++++++
- **Breaking changes**
 -- Removed methods: `run`, `call`, `lookfor`
 -- Renamed methods: `_eval` -> `eval`, `get` -> `pull`, `put` -> `push`,
    `close` -> `exit`
 -- Removed run and call in favor of using eval dynamic functions.
 -- Renamed methods to avoid overshadowing Octave builtins and for clarity.
 -- When a command results in "ans", the value of "ans" is returned
    instead of the printed string.
- Syntax Errors on Windows no longer crash the session.
- Added ability to interrupt commands with CTRL+C.
- Fixed Octavemagic not following current working directory.


1.6.0 (2014-07-26)
++++++++++++++++++
- Added 'temp_dir' argument to Oct2Py constructor (#50)
- Added 'kill_octave' convenience method to kill zombies (#46)
- Improved Octave shutdown handling (#45, #46)
- Added 'oned_as' argument to Oct2Py constructor (#49)


1.5.0 (2014-07-01)
++++++++++++++++++
- Removed optional pexpect dependency
- Brought back support for Python 2.6


1.4.0 (2014-06-28)
++++++++++++++++++
- Added support for Python 3.4 and Octave 3.8
- Support long_field names
- Dropped support for Python 3.2


1.3.0 (2014-01-20)
++++++++++++++++++
- Added support for Octave keyboard function (requires pexpect on Linux).
- Improved error messages when things go wrong in the Octave session
- (Linux) When pexpect is installed, Octave no longer closes session when
  a Syntax Error is encountered.
- Fixed: M-files with no docstrings are now supported.


1.2.0 (2013-12-14)
++++++++++++++++++
- OctaveMagic is now part of Oct2Py: ``%load_ext oct2py.ipython``
- Enhanced Struct behavior - supports REPL completion and pickling
- Fixed: Oct2Py will install on Python3 when using setup.py


1.1.1 (2013-11-14)
++++++++++++++++++
- Added support for wheels.
- Fixed: Put docs back in the manifest.
- Fixed: Oct2py will install when there is no Octave available.


1.1.0 (2013-10-27)
++++++++++++++++++

- Full support for plotting with no changes to user code
- Support for Nargout = 0
- Overhaul of front end documentation
- Improved test coverage and added badge.
- Supports Python 2 and 3 from a single code base.
- Fixed: Allow help(Oct2Py()) and tab completion on REPL
- Fixed: Allow tab completion for Oct2Py().<TAB> in REPL


1.0.0 (2013-10-4)
+++++++++++++++++

- Support for Python 3.3
- Added logging to Oct2Py class with optional logger keyword
- Added context manager
- Added support for unicode characters
- Improved support for cell array and sparse matrices
- Fixed: Changes to user .m files are now refreshed during a session
- Fixed: Remove popup console window on Windows


0.4.0 (2013-01-05)
++++++++++++++++++

- Singleton elements within a cell array treated as a singleton list
- Added testing on 64 bit architecture
- Fixed:  Incorrect Octave commands give a more sensible error message


0.3.6 (2012-10-08)
++++++++++++++++++

- Default Octave working directory set to same as OS working dir
- Fixed: Plot rending on older Octave versions


0.3.4 (2012-08-17)
++++++++++++++++++

- Improved speed for larger matrices, better handling of singleton dimensions


0.3.0 (2012-06-16)
++++++++++++++++++

- Added Python 3 support
- Added support for numpy object type


0.2.1 (2011-11-25)
++++++++++++++++++

- Added Sphinx documentation


0.1.4 (2011-11-15)
++++++++++++++++++

- Added support for pip


0.1.0 (2011-11-11)
++++++++++++++++++

- Initial Release
