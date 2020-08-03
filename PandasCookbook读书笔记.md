## PandasCookbook
1. Foundations
    * main components: index, columns, data
    * dtypes: boolean, integer, float, complex, object, datetime, timedelta, categotical
    * DataFrame <-> Series
    * Set options for displaying
    * Renaming index, rows, columns
    * Creating and deleting columns
2. Operations
    * Selecting multiple columns by list
    * Selecting columns by dtype, by `filter`
    * Ordering columns
    * Descriptions of entire DataFrame
    * Broadcast while using operators on the entire DataFrame
    * Dealing with missing values 
    * Operations on the columns rather than rows by setting axis=1
3. Data Analysis
    * routine: 
        1. read in the dataset, view by `head()`
        2. get dimensions of the DataFrame with `shape()`
        3. list dtypes,  missing value numbers, and memory usage with `info()`
        4. get summary stats for the numerical cols with `describe(include=[np.number)` (using transpose `.T` for more readable output)
        5. get summary stats for the non-numerical cols with `describe(include=[np.object, pd.Categorical])` (using transpose `.T` for more readable output)
        6. above all are called metadata, data about data: like dimensions, col names, dtypes, source, date, acceptable values
    * reducing memory by changing dtypes
        1. those no need int64 --> int8 (like 0/1 or [0-10] or [0-100)
        2. those no need object --> categorical (like F/M or RED/BLUE/YELLOW)
        3. Int64Index costs lots more than RangeIndex
    * selecting the extreme values using nlargest/nsmallest
    * sorting before `groupby.agg("first"/"last")`
    * cummax/cummin/cumsum for getting cumulative values
4. Subsets
    * celect Series from DataFrame with `.loc` and `.iloc`
    * `Series.tolist()` is much faster than `list(Series)`
    * use `get_loc()` to find the integer position of the desired cols
    * `.at` and `.iat` are faster but only accept scalar values
    * Lazy selecting `df[item]`: 
        * string: return the column as a Series ==> `df.loc[:, item]`
        * list of strings: return a DataFrame with just the columns in the list ==> `df.loc[:, item]`
        * slice object: return all rows selected by the slice ==> `df.loc[item, :]` if index slice elif integer slice `df.iloc[item, :]`
        * list/Series of booleans: return only the rows with True in the same location (must be equal length) ==> `df.loc[item, :]`
    * Specially, if the index are sorted by `sort_index()`, slice notation can be used in `.loc()` to select all rows bewteen two str
5. Boolean Indexing
    * remember to drop nan before boolean indexing
    * multiple boolean conditions: &, | and ~
    * operator precedence: `[&, |, ~]` > `[>, ==, <]` > `[and, or, not]`
    * peformance: boolean indexing is much slower than index selection, and the unique & sorted indexes is the fastest (since binary search is applied when sorted) -- sort before select!
    * visualization: `DataFrane.plot(color, figsize, style, ms)`; `matplotlib.pyplot.fill_between()`
    * more SQL like operator: use `S.isin(["str1", "str2"])` rather than `(S == "str1") | (S == "str2")`; use `S.between(num1, num2)` rather than `(S >= num1) | (S <= num2)`
    * Z-Score: ![formula](https://bkimg.cdn.bcebos.com/pic/d058ccbf6c81800aaff74971b13533fa828b47bb?x-bce-process=image/resize,m_lfit,w_268,limit_1), std is 1, mean is 0.
    * query method: using `@` for variable, more readable, supports `in` and `not in`, slightly faster
    * where method: preserve True and replace False (default NaN)
    * clip method: assigns values outside boundary to boundary values
    * mask method: exact opposite of `where`
    * `assert_frame_equal` method: similiar to `equals`, but without checking the equality of data types when setting `check_like=False` (by default)
    * performance comparison: boolean indexing is faster than mask/where/clip (about ten times)
6. Index Alighment
    * Index object support the __set__ operations, __ndarrays__ slicings, and __Series__ methods, but not mutable
    * `reindex()` is for sorting (existing index), to reassign the index by the new vals, use `.index = []`
    * silent index alignment (which creates a Cartesian product unless exactly the same) happens when any operation between two DataFrames begins
    * use `DataFrame.add()` with `fill_value = xx` can avoid missing values/indexes in either of the DataFrames
    * __USEFUL__: use `DataFrame.style.highlight_null()` and more to highlight the corresponding data in the output
    * automatic index alighment will fail when one of the two DataFrames has not exact same index and repeats its indexes
    * `DataFrame.eq(Series)` will align the cols of the DataFrame with the labels of the passed Series
    * `Series[lambda x: x]` will give a Series where Series is True
    * `Series.idxmax(axis="columns")` can be useful for finding the max cols for values of index
    * `Series.value_counts(normalize=True)` can be used to get the distribution of values
7. Grouping
    * groupby aggregation functions: min, max, mean, median, sum, count, size (size vs count:  size includes NaN, while count does not), std, var, describe, nunique, idxmin. idxmax
    * while groupby aggregation has multiple functions, the columns will become MultiIndex
    * remove MultiIndex in cols using `.columns = .columns.get_level_values(0) + "_" + .columns.get_level_values(1)`; remove MultiIndex in rows using `reset_index()`
    * groupby object is iterable and has many useful methods like `nth` -- selects given rows from each group
    * split-apply-combine is the core process of groupby; in the apply step, there are 3 main operations:
        1. Aggregation: compute a summary statistic (or statistics) for each group.
        2. Transformation: perform some group-specific computations and return a like-indexed object.
        3. Filtration: discard some groups, according to a group-wise computation that evaluates True or False
    * `np.where()` is a vectorized if-then-else function which can map a Series or array of booleans to other values
    * `pd.cut()` can bin values into discrete intervals, which can be used for groupby continuous variables
    * `DataFrame.apply(sorted, axis=1)` is much slower than `np.sort(axis=1)`
8. Restructuring
    * Tidy Data Definition:
        * Each variable forms a column
        * Each observation forms a row
        * Each type of observational unit forms a table
    * Terminology of the transformation:
        * from horizontal column names into vertical column values: melting = stacking = unpivoting
        * from horizontal column names into vertical column values: unstacking = pivoting
    * Adapt to the different conditions:
        1. var in col: `.stack().rename_axis().reset_index()` or `.stack().reset_index().rename()` or just `.melt(id_vars, value_vars, var_name, value_name)` (**melt will drop index silently**)
        2. multi var in col: `wide_to_long(DataFrame, stubnames, i, j, sub)` with stubnames [‘A’, ‘B’] for cols like A-suffix1, A-suffix2,…, B-suffix1, B-suffix2,… (- is sep)
        3. matrixize (tidy to dirty): `unstack()` or `pivot(index, columns, values)`
        4. `DataFrame.stack().unstack(0)` is actually `DtatFrame.T` or `DtatFrame.transpose()`
        5. `DataFrame.swaplevel` to switch the placement of levels; `rename_axis(None)` or  `droplevel` to dispose the level values
        6. `pivot` vs `pivot_table`: `pivot` raises error when duplicated rows exist, while `pivot_table` accepts a `aggfuc` to deal this situation and supports multi index
        7. var in val or same cell: use `.str.extract` or `.str.split` with `(expand=True)` first and then use concat or create a new DataFrame
        8. var in col and val: `melt` + `pivot` or `stack` + `unstack`, with `rename_axis` and `reset_index`
9. Combining
    * Appending rows:
        1. use `.loc` to append a list or dict or a Series(index is the key) 
        2. use `.append` to append a Series (with a name or set ignore_index=True), DataFrame, dict or a **list** of them
    * Concatenating DataFrames:
        1. set `axis` parameter to 0 for vertical concat, while 1 for horizontal concat (default vertical)
        2. set `keys` parameter to label the sub DataFrames concated (default None)
        3. set `join` parameter to get inner or outer concat (default outer)
        4. `append` method is actually calling `concat` with `axis=0`
    * `concat`, `join` and `merge`:
        1. `concat`: pandas func, >=2 tables, vertical & horizontal, by index, error for duplicate
        2. `join`: DataFrame method, >=2 tables, horizontal, by cols or index, suffix for duplicate
        3. `merge`: DataFrame method, ==2 tables, horizontal, by cols or index, suffix for duplicate
    * Connecting to databases:
        1. pymysql, pymssql needs SQL query
        2. sqlalchemy does not need SQL query
10. Time Series
    * Date objects comparision:
        1. datetime module: date, time, datetime, timedelta
        2. pandas module: Timestamp (derived from NumPy's datetime64), can be initialized by `pd.Timestamp` and `pd.to_timedelta`, Timedelta (derived from NumPy's timedelta64), DateOffset
    * Slicing date index partially:
        1. `.loc[year|month|day]` can be used to select all the datetimes of the year|month|day
        2. `.loc[date : date]` can work for selecting range of data
        3. `between_time` can be used to select all dates that between two given time 
        4. `at_time` can be used to select all dates at a specific time
        5. `.first(DateOffset objects)` can be used to select the first n segments of time: using DateOffset objects:
            * 'M': pd.offsets.MonthBegin; 'MS': pd.offsets.MonthEnd
            * '5D': 5 days; '5B': 5 business days; '7W': 7 weeks, with weeks ending on Sunday; '3QS': 3rd quarter start; 'A': one year end
            * Anchored offsets: offsets with an anchoring suffix, e.g., 'W-THU': every week ending on Thursday
            * see more on [Pandas Doc: DateOffset objects](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects)
    * Resampling:
        1. groupby for time series, can be replicated by `groupby(pd.Grouper(freq=))` setting freq to corresponding Timedelta
        2. unable to group by anything other than periods of time, so a groupby operation on multi cols can only be achieved by groupby (even a `cut` can be included)
    * `xs`: select cross-section from the Series/DataFrame
    * `merge_asof`:  similar to a left-join except that we match on nearest key rather than equal keys
11. Visualization
    * matplotlib:
        1. stateful interface: all of its calls directly with the pyplot module
        2. object-oriented interface: init `fig, ax = plt.subplots(figsize=(15,3))` then operate on fig and ax
    * pandas:
        1. easy to use just by calling `df.plot`
        2. common-use parameters: `color`(same length of data), `ax`(where to draw), `title`
    * seaborn:
        1. even more user-friendly than pandas: like `countplot` does not even need a `value_counts()`