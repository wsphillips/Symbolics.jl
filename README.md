# Symbolics.jl

Symbolics.jl is a fast and modern Computer Algebra System (CAS) for a fast and modern
programming language (Julia). The goal is to have a high-performance and parallelized
symbolic algebra system that is directly extendable in the same language as the users.

## Documentation

For information on using the package, see the stable documentation. Use the in-development 
documentation for the version of the documentation which contains the unreleased features.

## Relationship to Other Packages

- [SymbolicUtils.jl](https://github.com/JuliaSymbolics/SymbolicUtils.jl): This is a 
  rule-rewriting system that is the core of Symbolics.jl. Symbolics.jl builds off of 
  SymbolicUtils.jl to extend it to a whole symbolic algebra system, complete with 
  support for differentation, solving symbolic systems of equations, etc. If you're
  looking for the barebones to build a new CAS for specific algebras, SymbolicUtils.jl
  is that foundation. Otherwise, Symbolics.jl is for you.
- [ModelingToolkit.jl](https://github.com/SciML/ModelingToolkit.jl): This is a
  symbolic-numeric modeling system for the SciML ecosystem. It heavily uses Symbolics.jl
  for its representation of symbolic equations along with tools like differentiation,
  and adds the representation of common modeling systems like ODEs, SDEs, and more.
  
## Example

```julia
@variables t x y
D = Differential(t)

z = t + t^2
D(z) # symbolic representation of derivative(t + t ^ 2, t)
expand_derivatives(D(z)) # 1 + 2t

ModelingToolkit.jacobian([x+x*y,x^2+y],[x,y])

#2×2 Array{Num,2}:
# 1 + y            x
#    2x  Constant(1)
      
B = simplify.([t^2+t+t^2  2t+4t
           x+y+y+2t   x^2 - x^2 + y^2])

#2×2 Array{Num,2}:
#  2 * t ^ 2 + t     6t
#  x + 2 * (t + y)  y ^ 2

simplify.(substitute.(B,[x=>y^2]))

#2×2 Array{Num,2}:
#       2 * t ^ 2 + t     6t
# y ^ 2 + 2 * (t + y)  y ^ 2

substitute.(B,([x=>2.0,y=>3.0,t=>4.0],))

#2×2 Array{ModelingToolkit.Constant,2}:
# Constant(36.0)  Constant(24.0)
# Constant(16.0)   Constant(9.0)
```