Data frame parameters
---------------------

TODO: not practical to store all data in JSON, external files useful

Supported formats
=================

Support for the following data formats is available via the `Pandas <http://pandas.pydata.org/pandas-docs/stable/io.html>`_ module.

    * Comma separated values (.csv)
    * Excel (.xls, .xlsx)
    * HDF5 (.h5)

**Warning:** When reading Excel documents formulae are supported but not re-evaluated and links are not updated; the value read is the value present when the document was last saved.

Timeseries
==========

.. csv-table:: timeseries1.csv
   :header: "Timestamp", "Rainfall", "Flow"
   :widths: 15, 10, 10

   "1910-01-01", 0.0, 23.920
   "1910-01-02", 1.8, 22.140
   "1910-01-03", 5.2, 22.570

.. code-block:: javascript

    "flow": {
        "type": "dataframe",
        "url": "timeseries1.csv",
        "index_col": "Timestamp"
        "parse_dates": true,
        "column": "Flow"
    }

TODO: comment WRT automatic resampling

Constants
=========

.. csv-table:: demands.csv
   :header: "City", "Population", "Demand"
   :widths: 15, 10, 10

   "Oxford", "30,294", 20.3
   "Cambridge", "28,403", 19.4
   "London", "790,930", 520.9

.. code-block:: javascript

   "max_flow": {
       "type": "constant",
       "url": "demands.csv",
       "index_col": "City"
       "index": "Oxford"
       "column": "Demand"
   }

Monthly profiles
================

.. csv-table:: demands_monthly.csv
   :header: "City", "Jan", "Feb", "Mar", "...", "Dec"
   :widths: 15, 10, 10, 10, 10, 10

   "Oxford", 23.43, 25.32, 24.24, "...", 21.24
   "Cambridge", 11.23, 14.34, 13.23, "...", 12.23

.. code-block:: javascript

    "max_flow": {
        "type": "monthlyprofile",
        "url": "demands_monthly.csv",
        "index_col": "City",
        "index": "Oxford"
    }

Multi-index
===========

.. csv-table:: multiindex_data.csv
    :header: "level", "node", "max_flow", "cost"
    :widths: 10, 15, 10, 10

    0,"demand1",10,-10
    0,"demand2",20,-20
    1,"demand1",100,-100
    1,"demand2",200,-200

.. code-block:: javascript

    {
        "name": "DC1",
        "type": "output",
        "max_flow": {
            "type": "constant",
            "url": "multiindex_data.csv",
            "column": "max_flow",
            "index": [0, "demand1"],
            "index_col": ["level", "node"]
        },
        "cost": {
            "type": "constant",
            "url": "multiindex_data.csv",
            "column": "cost",
            "index": [1, "demand1"],
            "index_col": ["level", "node"]
        }
    }

In the example above, *max_flow* evaluates to 10 and *cost* evaluates to -100.

Optimisation
============

TODO: each time "url" used, file is parsed - inefficient! - instead use "tables" section and "table": "___"

.. code-block:: javascript

    "parameters": {
        "oxford_demand": {
            "type": "constant",
            "table": "simple_data",
            "column": "Demand",
            "index": "Oxford"
        }
    },
    "tables": {
        "simple_data": {
            "url": "demands.csv",
            "index_col": "City"
        }
    }
