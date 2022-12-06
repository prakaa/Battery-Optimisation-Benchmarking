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

**Note 3**: Linopy is still in alpha (at time of writing), so a direct comparison is not appropriate.

**Note 3**: Times reflect rough average of 10 runs

| Package (Language) | Solution?  | Cbc Solver Time (s) | Gurobi Solver Time (s) | HiGHS Solver Time (s) |
|--------------------|------------|---------------------|------------------------|-----------------------|
| pyomo (Python      | Yes        | ~7.2                | ~3.8                   | N/A                   |
| mip (Python)       | Yes        | ~10.1               | ~1.6                   | N/A                   |
| linopy (Python)    | Yes        | ~6.2                | ~2.7                   | ~5.8                  |
| JuMP (Julia)       | Yes        | (~12.2, ~5.5)       | (~10.4, ~2.4)          | (~10.0, ~7.4)         |

## Qualitative Comparison

### JuMP:
  - Pluses
    - Has the best syntax and is the easiest framework to write models. 
    - Retrieving solutions requires understanding data structures, but is still simple
      - Improved with addition of [`Tables.jl` support](https://github.com/jump-dev/JuMP.jl/releases/tag/v1.4.0)
    - Decent performance and access to broad range of solvers
    - Most featured, including callbacks and more problem types beyond MIP (e.g. NLP, QP, SOCP)
    - Very very good documentation that is also a good intro to optimisation
    - Extensions of varying maturity (e.g. stochastic programming)
    - Passing solver level args is possible
  - Downsides
    - Has the downside of needing to learn Julia and some specific Julia syntax
      - One option is to solve models in Julia then do everything else in Python. There is also PyCall.jl
    - As Julia is just-in-time compiled, "time-to-first-[plot,solve]" can be long, but this can be reduced by things like PackageCompiler.jl. See [here](https://jump.dev/JuMP.jl/stable/tutorials/getting_started/performance_tips/)

### python-mip:
  - Pluses
    - Relatively nice syntax for model creation
    - Very fast and could be even faster if using Pypy
    - Advanced MIP features (lazy constraints)
  - Downsides
    - Locked to LP or MIP
    - Locked to Cbc or Gurobi
    - Missing some features such as indicator constraints
    - Retrieving solutions is awkward
    
### linopy
  - Pluses
    - Vectorised model creation that can use numpy, pandas or xarray data structures
    - Solution retrieval is very easy
    - Multiple solvers (though the python interfaces may be needed)
    - Passing solver level args is possible
  - Downsides
    - Locked to LP or binary MIP as of Oct 22
    - In alpha
    - Stricter model definition (min only, all variables on LHS, variable * constant only)
    - Constructing intertemporal constraints can require care (e.g. exclude t=0 intertemporal SoC constraint)
    
### pyomo
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
