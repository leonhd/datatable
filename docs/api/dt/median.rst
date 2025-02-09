
.. xfunction:: datatable.median
    :src: src/core/expr/head_reduce_unary.cc compute_median
    :tests: tests/test-reduce.py
    :cvar: doc_dt_median
    :signature: median(cols)

    Calculate the median value for each column from `cols`.


    Parameters
    ----------
    cols: Expr
        Input columns.

    return: Expr
        f-expression having one row, and the same names, stypes and
        number of columns as in `cols`.

    except: TypeError
        The exception is raised when one of the columns from `cols`
        has a non-numeric type.


    Examples
    --------
    .. code-block:: python

        >>> from datatable import dt, f, by
        >>>
        >>> df = dt.Frame({'A': [1, 1, 2, 1, 2],
        ...                'B': [None, 2, 3, 4, 5],
        ...                'C': [1, 2, 1, 1, 2]})
        >>>
        >>> df
           |     A      B      C
           | int32  int32  int32
        -- + -----  -----  -----
         0 |     1     NA      1
         1 |     1      2      2
         2 |     2      3      1
         3 |     1      4      1
         4 |     2      5      2
        [5 rows x 3 columns]

    Get the median from column A::

        >>> df[:, dt.median(f.A)]
           |       A
           | float64
        -- + -------
         0 |       1
        [1 row x 1 column]

    Get the median of multiple columns::

        >>> df[:, dt.median([f.A, f.B])]
           |       A        B
           | float64  float64
        -- + -------  -------
         0 |       1      3.5
        [1 row x 2 columns]

    Same as above, but more convenient::

        >>> df[:, dt.median(f[:2])]
           |       A        B
           | float64  float64
        -- + -------  -------
         0 |       1      3.5
        [1 row x 2 columns]

    You can pass in a dictionary with new column names::

        >>> df[:, dt.median({"A_median": f.A, "C_mid": f.C})]
           | A_median    C_mid
           |  float64  float64
        -- + --------  -------
         0 |        1        1
        [1 row x 2 columns]

    In the presence of :func:`by()`, it returns the median of each column
    per group::

        >>> df[:, dt.median({"A_median": f.A, "B_median": f.B}), by("C")]
           |     C  A_median  B_median
           | int32   float64   float64
        -- + -----  --------  --------
         0 |     1       1         3.5
         1 |     2       1.5       3.5
        [2 rows x 3 columns]


    See Also
    --------

    - :func:`mean()` -- function to calculate mean values.
    - :func:`sd()` -- function to calculate standard deviation.
