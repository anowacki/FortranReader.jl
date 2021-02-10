# FortranReader

## Introduction

FortranReader.jl provides basic commands to read the 'unformatted' files written
by Fortran programs using [Julia](https://julialang.org).

Although not technically portable, most Fortran programs write these files in
a predictable way, in 'records'.  Each record contains either one or several
variables, marked before and after with the length of the variable(s) in bytes.
This marker is written as a 4-byte integer.

This package is a simple module to fulfil a narrow purpose and **comes without
tests or any guarantee of future updates**.  See the section on
[alternatives](#Alternatives) below for better options for new projects.


## Installation

```julia
julia> import Pkg; Pkg.add(url="https://github.com/anowacki/FortranReader.jl")
```


## Using the package

Three functions are exported:
 - `read_record`
 - `skip_record`
 - `write_record`

Each is passed an `IO` object and will read, skip or write a single Fortran
record.  The user must pass the type of the record and the dimensions of the
array being read/written if not a scalar.


## Example

Read a rank-2 array of `real`s in the fourth record of a Gfortran unformatted
Fortran binary file into `data`.
The first and second records contains the dimensions of the array and
are read as a scalars, passing no dimensions.  We do not need the third
record and so skip over it.

```julia
data = open("file") do io
    m = read_record(io, Int)
    n = read_record(io, Int)
    skip_record(io)
    read_record(io, Float32, m, n)
end
```


## Alternatives

- [FortranFiles.jl](https://github.com/traktofon/FortranFiles.jl) is
  maintained and tested, comes with documentation, and should be used
  instead of this package for new projects.


## Contributions

Bug reports are welcome via GitHub issues but no guarantee is given
that they will be fixed, though if accompanied with a pull request fixing
a bug, I will do my best to merge it in.

Feature requests will not be acted upon.
