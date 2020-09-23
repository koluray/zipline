.. image:: https://media.quantopian.com/logos/open_source/zipline-logo-03_.png
    :target: https://www.zipline.io
    :width: 212px
    :align: center
    :alt: Zipline

=============

|Gitter|
|version status|
|travis status|
|appveyor status|
|Coverage Status|

Zipline is a Pythonic algorithmic trading library. It is an event-driven
system for backtesting. Zipline is currently used in production as the backtesting and live-trading
engine powering `Quantopian <https://www.quantopian.com>`_ -- a free,
community-centered, hosted platform for building and executing trading
strategies. Quantopian also offers a `fully managed service for professionals <https://factset.quantopian.com>`_
that includes Zipline, Alphalens, Pyfolio, FactSet data, and more.

- `Join our Community! <https://groups.google.com/forum/#!forum/zipline>`_
- `Documentation <https://www.zipline.io>`_
- Want to Contribute? See our `Development Guidelines <https://www.zipline.io/development-guidelines>`_

Features
========

- **Ease of Use:** Zipline tries to get out of your way so that you can
  focus on algorithm development. See below for a code example.
- **"Batteries Included":** many common statistics like
  moving average and linear regression can be readily accessed from
  within a user-written algorithm.
- **PyData Integration:** Input of historical data and output of performance statistics are
  based on Pandas DataFrames to integrate nicely into the existing
  PyData ecosystem.
- **Statistics and Machine Learning Libraries:** You can use libraries like matplotlib, scipy,
  statsmodels, and sklearn to support development, analysis, and
  visualization of state-of-the-art trading systems.

Installation
============

Zipline currently supports Python 2.7 and Python 3.5, and may be installed via
either pip or conda.

**Note:** Installing Zipline is slightly more involved than the average Python
package. See the full `Zipline Install Documentation`_ for detailed
instructions.

For a development installation (used to develop Zipline itself), create and
activate a virtualenv, then run the ``etc/dev-install`` script.

Quickstart
==========

See our `getting started tutorial <https://www.zipline.io/beginner-tutorial>`_.

The following code implements a simple dual moving average algorithm.

.. code:: python

    from zipline.api import order_target, record, symbol

    def initialize(context):
        context.i = 0
        context.asset = symbol('AAPL')


    def handle_data(context, data):
        # Skip first 300 days to get full windows
        context.i += 1
        if context.i < 300:
            return

        # Compute averages
        # data.history() has to be called with the same params
        # from above and returns a pandas dataframe.
        short_mavg = data.history(context.asset, 'price', bar_count=100, frequency="1d").mean()
        long_mavg = data.history(context.asset, 'price', bar_count=300, frequency="1d").mean()

        # Trading logic
        if short_mavg > long_mavg:
            # order_target orders as many shares as needed to
            # achieve the desired number of shares.
            order_target(context.asset, 100)
        elif short_mavg < long_mavg:
            order_target(context.asset, 0)

        # Save values for later inspection
        record(AAPL=data.current(context.asset, 'price'),
               short_mavg=short_mavg,
               long_mavg=long_mavg)


You can then run this algorithm using the Zipline CLI.
First, you must download some sample pricing and asset data:

.. code:: bash

    $ zipline ingest
    $ zipline run -f dual_moving_average.py --start 2014-1-1 --end 2018-1-1 -o dma.pickle --no-benchmark

This will download asset pricing data data sourced from Quandl, and stream it through the algorithm over the specified time range.
Then, the resulting performance DataFrame is saved in ``dma.pickle``, which you can load and analyze from within Python.

You can find other examples in the ``zipline/examples`` directory.

Questions?
==========

If you find a bug, feel free to `open an issue <https://github.com/quantopian/zipline/issues/new>`_ and fill out the issue template.

Contributing
============

All contributions, bug reports, bug fixes, documentation improvements, enhancements, and ideas are welcome. Details on how to set up a development environment can be found in our `development guidelines <https://www.zipline.io/development-guidelines>`_.

If you are looking to start working with the Zipline codebase, navigate to the GitHub `issues` tab and start looking through interesting issues. Sometimes there are issues labeled as `Beginner Friendly <https://github.com/quantopian/zipline/issues?q=is%3Aissue+is%3Aopen+label%3A%22Beginner+Friendly%22>`_ or `Help Wanted <https://github.com/quantopian/zipline/issues?q=is%3Aissue+is%3Aopen+label%3A%22Help+Wanted%22>`_.

Feel free to ask questions on the `mailing list <https://groups.google.com/forum/#!forum/zipline>`_ or on `Gitter <https://gitter.im/quantopian/zipline>`_.

.. note::

   Please note that Zipline is not a community-led project. Zipline is
   maintained by the Quantopian engineering team, and we are quite small and
   often busy.

   Because of this, we want to warn you that we may not attend to your pull
   request, issue, or direct mention in months, or even years. We hope you
   understand, and we hope that this note might help reduce any frustration or
   wasted time.


.. |Gitter| image:: https://badges.gitter.im/Join%20Chat.svg
   :target: https://gitter.im/quantopian/zipline?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge
.. |version status| image:: https://img.shields.io/pypi/pyversions/zipline.svg
   :target: https://pypi.python.org/pypi/zipline
.. |travis status| image:: https://travis-ci.org/quantopian/zipline.png?branch=master
   :target: https://travis-ci.org/quantopian/zipline
.. |appveyor status| image:: https://ci.appveyor.com/api/projects/status/3dg18e6227dvstw6/branch/master?svg=true
   :target: https://ci.appveyor.com/project/quantopian/zipline/branch/master
.. |Coverage Status| image:: https://coveralls.io/repos/quantopian/zipline/badge.png
   :target: https://coveralls.io/r/quantopian/zipline

.. _`Zipline Install Documentation` : https://www.zipline.io/install

官方版本依赖：
(env_zipline_dev) YanHes-MacBook-Pro:zipline yanhe$ conda install -c Quantopian zipline
Collecting package metadata (repodata.json): done
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 4.8.4
  latest version: 4.8.5

Please update conda by running

    $ conda update -n base -c defaults conda



## Package Plan ##

  environment location: /Users/yanhe/anaconda3/envs/env_zipline_dev

  added / updated specs:
    - zipline


The following NEW packages will be INSTALLED:

  alembic            Quantopian/osx-64::alembic-0.7.7-py35_0
  asn1crypto         pkgs/main/noarch::asn1crypto-1.4.0-py_0
  bcolz              Quantopian/osx-64::bcolz-0.12.1-np114py35_0
  blas               pkgs/main/osx-64::blas-1.0-mkl
  blosc              pkgs/main/osx-64::blosc-1.20.0-hab81aa3_0
  bottleneck         pkgs/main/osx-64::bottleneck-1.2.1-py35h1d22016_1
  bzip2              pkgs/main/osx-64::bzip2-1.0.8-h1de35cc_0
  cffi               pkgs/main/osx-64::cffi-1.11.5-py35h6174b99_1
  chardet            pkgs/main/osx-64::chardet-3.0.4-py35_1
  click              pkgs/main/noarch::click-7.1.2-py_0
  contextlib2        pkgs/main/noarch::contextlib2-0.6.0.post1-py_0
  cryptography       pkgs/main/osx-64::cryptography-2.3.1-py35hdbc3d79_0
  decorator          pkgs/main/noarch::decorator-4.4.2-py_0
  empyrical          Quantopian/osx-64::empyrical-0.5.3-py35_0
  h5py               pkgs/main/osx-64::h5py-2.8.0-py35h878fce3_3
  hdf5               pkgs/main/osx-64::hdf5-1.10.2-hfa1e0ec_1
  icu                pkgs/main/osx-64::icu-58.2-h0a44026_3
  idna               pkgs/main/osx-64::idna-2.7-py35_0
  intel-openmp       pkgs/main/osx-64::intel-openmp-2019.4-233
  intervaltree       Quantopian/osx-64::intervaltree-2.1.0-py35_0
  iso3166            Quantopian/osx-64::iso3166-0.9-py35_0
  iso4217            Quantopian/osx-64::iso4217-1.6.20180829-py35_0
  libgfortran        pkgs/main/osx-64::libgfortran-3.0.1-h93005f0_2
  libiconv           pkgs/main/osx-64::libiconv-1.16-h1de35cc_0
  libxml2            pkgs/main/osx-64::libxml2-2.9.10-h3b9e6c8_1
  libxslt            pkgs/main/osx-64::libxslt-1.1.34-h83b36ba_0
  logbook            Quantopian/osx-64::logbook-0.12.5-py35_0
  lru-dict           Quantopian/osx-64::lru-dict-1.1.4-py35_0
  lxml               pkgs/main/osx-64::lxml-4.2.5-py35hef8c89e_0
  lz4-c              pkgs/main/osx-64::lz4-c-1.9.2-hb1e8313_1
  lzo                pkgs/main/osx-64::lzo-2.10-haf1e3a3_2
  mako               pkgs/main/noarch::mako-1.1.3-py_0
  markupsafe         pkgs/main/osx-64::markupsafe-1.0-py35h1de35cc_1
  mkl                pkgs/main/osx-64::mkl-2018.0.3-1
  multipledispatch   pkgs/main/osx-64::multipledispatch-0.6.0-py35_0
  networkx           pkgs/main/osx-64::networkx-1.11-py35_1
  numexpr            Quantopian/osx-64::numexpr-2.6.1-np114py35_0
  numpy              pkgs/main/osx-64::numpy-1.14.2-py35ha9ae307_0
  pandas             pkgs/main/osx-64::pandas-0.22.0-py35h0a44026_0
  pandas-datareader  pkgs/main/noarch::pandas-datareader-0.8.1-py_0
  patsy              pkgs/main/osx-64::patsy-0.5.0-py35_0
  pycparser          pkgs/main/noarch::pycparser-2.20-py_2
  pyopenssl          pkgs/main/osx-64::pyopenssl-18.0.0-py35_0
  pysocks            pkgs/main/osx-64::pysocks-1.6.8-py35_0
  pytables           pkgs/main/osx-64::pytables-3.4.4-py35h13cba08_0
  python-dateutil    pkgs/main/noarch::python-dateutil-2.8.1-py_0
  python-interface   Quantopian/osx-64::python-interface-1.5.3-py35_0
  pytz               pkgs/main/noarch::pytz-2020.1-py_0
  requests           Quantopian/osx-64::requests-2.20.1-py35_0
  scipy              pkgs/main/osx-64::scipy-1.1.0-py35hcaad992_0
  six                pkgs/main/noarch::six-1.15.0-py_0
  snappy             pkgs/main/osx-64::snappy-1.1.8-hb1e8313_0
  sortedcontainers   pkgs/main/noarch::sortedcontainers-2.2.2-py_0
  sqlalchemy         pkgs/main/osx-64::sqlalchemy-1.2.11-py35h1de35cc_0
  statsmodels        pkgs/main/osx-64::statsmodels-0.9.0-py35h917ab60_0
  toolz              pkgs/main/noarch::toolz-0.10.0-py_0
  trading-calendars  Quantopian/osx-64::trading-calendars-1.11.2-py35_0
  urllib3            pkgs/main/osx-64::urllib3-1.23-py35_0
  zipline            Quantopian/osx-64::zipline-1.4.0-np114py35_0
  zstd               pkgs/main/osx-64::zstd-1.4.5-h41d2c2f_0
