# Battery-Optimisation-Benchmarking
Benchmarking MIP solutions across packages in Python and Julia

## Julia
- [JuMP](https://github.com/UNSW-CEEM/Battery-Optimisation-Benchmarking/blob/master/battery_optimisation_benchmarking/julia/jump.ipynb)

## Python
- [linopy](https://github.com/UNSW-CEEM/Battery-Optimisation-Benchmarking/blob/master/battery_optimisation_benchmarking/python/linopy.ipynb)
  - Solution found but appears to be wrong based on what I think is a correct formulation
- [python-mip](https://github.com/UNSW-CEEM/Battery-Optimisation-Benchmarking/blob/master/battery_optimisation_benchmarking/python/mip.ipynb)

## Summary
- Python-mip + Gurobi is fastest (even without using [Pypy](https://docs.python-mip.com/en/latest/install.html#pypy-installation-optional))
- Gurobi outperforms open-source, and HiGHS appears to outperform Cbc (the latter compared using Julia)
- Solution is wrong for linopy
  - Not sure what I'm doing wrong with linopy, or some issues with the package (still in alpha)

- JuMP:
  - Pluses
    - Has the best syntax and is the easiest framework to write models and retrieve solutions
    - Decent performance and access to broad range of solvers
    - Most featured, including callbacks and more problem types beyond MIP (e.g. NLP, QP, SOCP)
    - Very very good documentation that is also a good intro to optimisation
- Downsides
  - Has the downside of needing to learn Julia and some specific Julia syntax
    - One option is to solve models in Julia then do everything else in Python. There is also PyCall.jl
  - As Julia is just-in-time compiled, "time-to-first-[plot,solve]" can be long, but this can be reduced by things like PackageCompiler.jl
