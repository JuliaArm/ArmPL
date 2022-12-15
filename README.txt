# About:

    Julia bindings to Arm Performance Libraries (BLAS). 

    This package is using <libblastrampoline> to link <libarmpl.so> which is
    usually symlinked to <libarmpl_ilp64_mp.so> [_ilp64 means: 64-bit Ints (i),
    longs (l) and pointers (p) and _mp implies the OpenMP threaded version].
    
    Please be advised that such components of Arm Performance Libraries as
    <libamath.so> (faster transcendental math functions (sin, cos, pow)) and
    <libastring.so> (faster string processing operations) are currently not
    supported.


# Release info:

    Version: 0.1. Date: July 2022. This is a very preliminary (testing and 
    working) release.


# Requirements:

    1) Unix, 64 bit operating system;

    2) Arm Performance Libraries present on the system: 
    
    For details about installation procedure of Arm Performance Libraries 
    please see: https://developer.arm.com/downloads/-/arm-performance-libraries;
    
    After system installation of Arm Performance Libraries, usual location of
    <libarmpl_ilp64_mp.so> is i.e. </opt/arm/armpl_22.0.2_gcc-11.2/lib>.

    3) <LD_LIBRARY_PATH> environmental variable set to a directory containing
    <libarmpl_ilp64_mp.so> i.e.:

    <LD_LIBRARY_PATH=/opt/arm/armpl_22.0.2_gcc-11.2/lib julia>


# Install:

    julia> import Pkg; Pkg.add(url="https://github.com/JuliaArm/ArmPL.jl")
    
    or
    
    julia> ] 
    (@v1.7) pkg> add https://github.com/JuliaArm/ArmPL.jl


# Usage:

    julia> using ArmPL


# Additional usage information:

    By default ArmPL (<libarmpl_ilp64_mp.so>) automatically sets number of BLAS 
    threads to maximum based on default values of operating system 
    <OMP_NUM_THREADS> environmental variable. 
    
    However, <OMP_NUM_THREADS> environmental variable can also be controlled 
    manually and desired number of BLAS threads can be provided i.e.
    at the time of Julia start. 
    
    Three such scenarios are discussed below with advisory notes. The examples
    assume a CPU with 80 physical cores:

    1) In case processed Julia code is parallel, the advise is to restrict ArmPL
    and Julia threads to 1 and add desired number of processes e.g. start julia 
    with: 
    
    <OMP_NUM_THREADS=1 LD_LIBRARY_PATH=/opt/arm/armpl_22.0.2_gcc-11.2/lib julia
    -p 80 -t 1>
    
    and then to use Julia Distributed.jl package:
    
    julia>  using Distributed
            @everywhere begin
              using ArmPL 
              <your code>
            end

    2) In case of single threaded Julia code calling expensive BLAS operations,
    the advise is to restrict Julia to 1 thread and let ArmPL handle maximum 
    number of BLAS threads automatically i.e.

    <LD_LIBRARY_PATH=/opt/arm/armpl_22.0.2_gcc-11.2/lib julia -t 1>
    
    or set it manually i.e. 
    
    <OMP_NUM_THREADS=80 LD_LIBRARY_PATH=/opt/arm/armpl_22.0.2_gcc-11.2/lib julia
    -t 1>

    3) In case of calling ArmPL from different threads in parallel, the advise is
    to restrict ArmPL to 1 thread and start julia with number of threads that equal
    the number of CPU physical cores i.e.

    <OMP_NUM_THREADS=1 LD_LIBRARY_PATH=/opt/arm/armpl_22.0.2_gcc-11.2/lib julia
    -t 80>


# Testing environment:

    The package was tested on Neoverse N1, Oracle Linux 8.5 / Ubuntu 20.04.4 LTS
    (Focal Fossa), Julia 1.7.3.


# Licence:

    ArmPL is released under the terms of the MIT "Expat" License.


# Legal disclaimers:

    This account is a private GitHub account. Package naming tries to follow best 
    Julia Language Community practices. All registered trademarks, copyrights and 
    intellectual property rights belong to their respected owners.


# Contact:

    [juliaarm at proton dot me] Please be informed that the mailbox is checked 
    every few days and not all messages may be answered immediately. Please be 
    advised that any potential questions explicitly related to the usage of this 
    Julia package should be addressed by Julia Discourse and any potential issues 
    by GitHub.


# Special thanks:

    OP, CB, CE, MF, CG, JL, JH, KJ, DS
