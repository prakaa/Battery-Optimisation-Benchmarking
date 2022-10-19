# Battery-Optimisation-Benchmarking
Benchmarking MIP solutions across packages in Python and Julia

## Julia
- [JuMP](https://github.com/UNSW-CEEM/Battery-Optimisation-Benchmarking/blob/master/battery_optimisation_benchmarking/julia/jump.ipynb)

## Python
- [linopy](https://github.com/UNSW-CEEM/Battery-Optimisation-Benchmarking/blob/master/battery_optimisation_benchmarking/python/linopy.ipynb)
  - Solution found but appears to be wrong based on what I think is a correct formulation
- [python-mip](https://github.com/UNSW-CEEM/Battery-Optimisation-Benchmarking/blob/master/battery_optimisation_benchmarking/python/mip.ipynb)
- [pyomo](https://github.com/prakaa/Battery-Optimisation-Benchmarking/blob/master/battery_optimisation_benchmarking/python/pyomo.ipynb)

## Bechmark Results

**Note 1**: Julia is just-in-time compiled. The first run of a function will involve compilation, so the first run will generally always be slower. The table below reports times for first and subsequent runs as (first, subsequent)

**Note 2**: Python packages are installed via `poetry`, which presumably uses `pip`. Using the `Pypy` versions of packages (where available) may improve times.

**Note 3**: Solution is wrong for linopy. Not sure what I'm doing wrong with linopy, or some issues with the package (still in alpha).

| Package (Language) | Solution?  | Cbc Solver Time (s) | Gurobi Solver Time (s) | HiGHS Solver Time (s) |
|--------------------|------------|---------------------|------------------------|-----------------------|
| pyomo (Python      | Yes        | ~6.7                | ~3.6                   | N/A                   |
| mip (Python)       | Yes        | ~9.8                | ~1.5                   | N/A                   |
| linopy (Python)    | Incorrect? | -                   | -                      | -                     |
| JuMP (Julia)       | Yes        | (~11.6, ~5.0)       | (~10.4, ~2.4)          | (~10.7, ~8.1)         |

## Qualitative Comparison

- JuMP:
  - Pluses
    - Has the best syntax and is the easiest framework to write models. Retrieving solutions is fine, but could be simpler
    - Decent performance and access to broad range of solvers
    - Most featured, including callbacks and more problem types beyond MIP (e.g. NLP, QP, SOCP)
    - Very very good documentation that is also a good intro to optimisation
    - Extensions of varying maturity (e.g. stochastic programming)
    - Passing solver level args is possible
  - Downsides
    - Has the downside of needing to learn Julia and some specific Julia syntax
      - One option is to solve models in Julia then do everything else in Python. There is also PyCall.jl
    - As Julia is just-in-time compiled, "time-to-first-[plot,solve]" can be long, but this can be reduced by things like PackageCompiler.jl

- python-mip:
  - Pluses
    - Relatively nice syntax for model creation
    - Very fast and could be even faster if using Pypy
    - Advanced MIP features (lazy constraints)
    - Passing solver level args is possible
  - Downsides
    - Locked to LP or MIP
    - Locked to Cbc or Gurobi
    - Missing some features such as indicator constraints
    - Retrieving solutions is awkward
    
- linopy
  - Pluses
    - Vectorised model creation that can use numpy, pandas or xarray data structures
    - Solution retrieval is very easy
    - Multiple solvers (though the python interfaces may be needed)
  - Downsides
    - Locked to LP or binary MIP as of Oct 22
    - In alpha and some bugs/issues
      - Couldn't solve this model properly'
    - Stricter model definition (min only, all variables on LHS)
    - Intertemporal constraints are awkard to construct
    
- pyomo
  - Pluses
    - Very mature and large user community
    - Can formulate many types of problems and has multiple extensions (e.g. stochastic programming)
    - Syntax is relatively compact
    - Broad solver range (though HiGHS interface in development)
    - Passing solver level args is possible
    - Handling model components as attributes has several benefits, e.g. method call to retrieve solution results
  - Downsides
    - Syntax is rather arcane
    - Docs could be better. Understanding how to build models often requires looking at examples.   
