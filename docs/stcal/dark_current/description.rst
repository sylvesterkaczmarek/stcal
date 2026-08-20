Overview
========

The ``stcal.dark_current`` package provides dark-current subtraction for ramp data.
The correction adapts a dark reference to the science exposure's group structure
before subtraction when the reference and science data use different ``nframes``
or ``groupgap`` settings.

The main entry point is :func:`stcal.dark_current.dark_sub.do_correction`, which
accepts science and dark-reference data models and converts them to the package's
internal data classes.  Code that already uses those classes can call
:func:`stcal.dark_current.dark_sub.do_correction_data` directly.

Correction behavior
-------------------

When the science and dark-reference group structures match, the reference is
subtracted directly.  Otherwise, dark frames are averaged to match the science
exposure.  Three-dimensional dark references use
:func:`stcal.dark_current.dark_sub.average_dark_frames_3d`; four-dimensional
integration-dependent references use
:func:`stcal.dark_current.dark_sub.average_dark_frames_4d`.

If the science exposure extends beyond the available dark reference, the dark is
extrapolated to cover the required groups.  If the dark reference uses a larger
``nframes`` or ``groupgap`` value than the science exposure, the correction is
skipped and the input science data are returned without dark subtraction.

The correction returns the dark-subtracted science data together with the
averaged dark reference when one was generated.  A requested ``dark_output`` is
recorded on the returned dark object for the calling pipeline to save.
