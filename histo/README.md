Histo benchmark for the Lamellar Runtime
=================================================

Lamellar is an asynchronous tasking runtime for HPC systems developed in RUST

SUMMARY
-------

A initial implementation of the Histo benchmark (https://github.com/jdevinney/bale/tree/master/apps/histo_src) implemented in Rust for the Lamellar runtime

NEWS
----
Sept 2020: Update for Lamellar 0.2.1 release
July 2020: Second alpha release
Feb 2020: First alpha release

BUILD REQUIREMENTS
------------------
These benchmarks requires the following dependencies:

* [Lamellar](https://github.com/pnnl/lamellar-runtime) - now on [crates.io](https://crates.io/crates/lamellar)

* Crates listed in Cargo.toml

At the time of release, Lamellar has been tested with the following external packages:

| **GCC** | **CLANG** | **ROFI**  | **OFI**   | **IB VERBS**  | **MPI**       | **SLURM** | **LAMELLAR** |
|--------:|----------:|----------:|----------:|--------------:|--------------:|----------:|-------------:|
| 7.1.0   | 8.0.1     | 0.1.0     | 1.9.0     | 1.13          | mvapich2/2.3a | 17.02.7   | 0.2.1        |

The OFI_DIR environment variable must be specified with the location of the OFI installation.
The ROFI_DIR environment variable must be specified with the location of the ROFI installation.

BUILDING PACKAGE
----------------
In the following, assume a root directory ${ROOT}
0. download Benchmarks to ${ROOT}/Benchmarks 
    `cd ${ROOT} && git clone https://github.com/pnnl/lamellar-benchmarks`
0. download Lamellar to ${ROOT}/lamellar-runtime  -- or update Cargo.toml to point to the proper location
    `cd ${ROOT} && git clone https://github.com/pnnl/lamellar-runtime`

1. cd into registered-am or remote-closure directory and Compile benchmark 

`cargo build (--release)`

    executables located at ./target/debug(release)/<benchmark variant>

    where `<benchmark variant>` in {`histo_dma, histo_static, histo_buffered_updates_dma, histo_buffered_updates_static`}.


TESTING
-------
The benchmarks are designed to be run with on multiple compute nodes (1 node is valid). Here is a simple proceedure to run the tests that assume a compute cluster and [SLURM](https://slurm.schedmd.com) job manager. Please, refer to the job manager documentaiton for details on how to run command on different clusters. Lamellar grabs job information (size, distribution, etc.) from the jbo manager and runtime launcher (e.g., MPI, please refer to the BUILING REQUIREMENTS section for a list of tested software versions).

1. Allocates two compute nodes on the cluster:

`salloc -N 2 -p compute`

2. Run histo(_static) using `mpiexec` launcher.

`mpiexec -n 2 ./target/release/histo_(static) 10000000`  (argument = number of updates to perform)

3. Run histo_buffered_updates(_static)

`mpiexec -n 2 ./target/release/histo_buffered_updates(_static) 10000000 100`  (argument = number of updates to perform, number of updates to buffer)

Note that if using the "local" lamellae, simply execute the binary directly


HISTORY
-------
- version 0.2:
  - update to match Lamellar v0.2 api
  - implement registered AM version
- version 0.1:
  - initial implementations
  - use static array
  - use remote memory region
  
NOTES
-----

STATUS
------
Lamellar is still under development, thus not all intended features are yet
implemented. Benchmark will be updated to utilize new features.

CONTACTS
--------
Ryan Friese     - ryan.friese@pnnl.gov  
Roberto Gioiosa - roberto.gioiosa@pnnl.gov  
Mark Raugas     - mark.raugas@pnnl.gov  

## License

This project is licensed under the BSD License - see the [LICENSE.md](LICENSE.md) file for details.

## Acknowledgments

This work was supported by the High Performance Data Analytics (HPDA) Program at Pacific Northwest National Laboratory (PNNL),
a multi-program DOE laboratory operated by Battelle.
