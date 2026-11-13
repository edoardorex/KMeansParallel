# Parallel K-Means with OpenMP

An OpenMP version of the
[sequential k-means project](https://github.com/edoardorex/KMeansSequential),
exploring parallel point assignment with **Array of Structures (AoS)** and
**Structure of Arrays (SoA)** layouts.

## How it works

The program generates two-dimensional random points and cluster centroids,
assigns points to the nearest cluster, and repeatedly updates centroids.
OpenMP sections parallelize initialization, parallel loops distribute
assignment work, and atomic operations protect cluster accumulation.

It prints initialization and clustering timings and invokes Gnuplot to display
cluster-colored scatter plots.

## Configuration

The current settings in [main.cpp](main.cpp) are:

```cpp
double Range = 100000;
const int nPoint = 5000;
int nCluster = 5;
int itr = 20;
```

`AoS` and `SoA` in `main()` are both enabled. Change them to run one layout at a
time, and rebuild after changing source settings. The stopping rule is centroid
stability or the iteration limit.

The OpenMP runtime can be configured through its standard environment variables,
such as `OMP_NUM_THREADS`; the program has no thread-count CLI option.

## Build and run

Use a C++11 toolchain supporting GNU-style `-fopenmp`, an OpenMP runtime,
and the Windows `windef.h` header. The checked-in configuration is
Windows/GNU-oriented; other platforms need adaptation.
Install [Gnuplot](http://www.gnuplot.info/) and make `gnuplot` available on `PATH`.

With the compiler configured and CMake 3.13 or newer:

```sh
cmake -S . -B build
cmake --build build --target KMeansOpenMPParallel
```

Run the generated `KMeansOpenMPParallel` executable in a graphical environment.
Plotting creates and removes `dataPoint.txt` and `dataPointArray.txt` in the
working directory.

![Historical clustering visualization](gnuplot.PNG)

## Repository guide

| File | Purpose |
| --- | --- |
| [main.cpp](main.cpp) | AoS/SoA data, OpenMP regions, assignment, timing, and plotting |
| [Cluster.h](Cluster.h) | Centroid accumulation with OpenMP atomics |
| [CMakeLists.txt](CMakeLists.txt) | Build target and compiler flags |

This is an educational performance experiment. Speedup depends on workload,
thread count, random initialization, and hardware; the included screenshot is
not a reproducible benchmark result.

## Attribution

The original README credits **Edoardo Re, 2020**.
This repository is a fork of
[edoardore/KMeansParallel](https://github.com/edoardore/KMeansParallel).
Original source and history are preserved; no license is added or changed.

Discuss substantial changes in an issue and include appropriate tests.
