# SYCL Academy

![SYCL Academy](sycl_academy.png "SYCL Academy")

This repository provides materials that can be used for teaching SYCL 1.2.1. The
materials are provided using the "Creative Commons Attribution Share Alike 4.0
International" license.

## What is SYCL?

If you're not familiar with SYCL or would like some further resources for
learning about SYCL below are a list of useful resources:

*  Read a description of SYCL on the [Khronos website SYCL page](https://www.khronos.org/sycl/).
*  Go to the Khronos website to find [a list of SYCL resources](https://www.khronos.org/sycl/resources).
*  Check out the [SYCL 1.2.1 reference guide](https://www.khronos.org/files/sycl/sycl-12-reference-card.pdf).
*  Browse SYCL news, blog posts, videos, projects and more on the [sycl.tech community website](https://sycl.tech/)
*  Get a list of the [available SYCL implementations](https://sycl.tech/#get-sycl)

### How to use the Materials

To use these materials simply clone this repository.

The lectures are written in reveal.js, and can be found in "Lesson_Materials",
in the sub-directory for each topic. To view them simply open the "index.html"
file in your browser. Your browser will have a "Full Screen" mode that can be
used to run the presentation, use the right and left cursors to move forward and
backward in the presentation.

The exercises can be found in "Code_Exercises" in the sub-directory for each
topic. Each exercise has a markdown document instructing what to do in the
exercise, a source file to start with and a solution file to provide an
example implementation to compare against.

## Contributing to SYCL Academy

Contributions to the materials are very gratefully received and this can be done
by submitting a Pull Request with any changes. Please limit the scope of each
Pull Request so that they can be reviewed and merged in a timely manner.

### List of Contributors

Codeplay Software Ltd., Heidelberg University.

## Supporting Organizations
Abertay University, Universidad de Concepcion, TU Dresden, University of Edinburgh, Federal University of Sao Carlos, University of Glasgow, Heriot Watt University, Universitat Innsbruck, Universidad de Málaga, University of Salerno and University of the West of Scotland.

## Lesson Curriculum

The SYCL Academy curriculum is divided up into a number of short lessons
consisting of slides for presenting the material and a more detailed write-up,
each accompanied by a tutorial for getting hands on experience with the subject
matter.

Each of the lessons are designed to be self contained modules in order to
support both academic and training style teaching environments.

## Building the Exercises

The exercises can be built for ComputeCpp CE, DPC++ and hipSYCL.

### Supported Platforms

Below is the supported platforms and devices for each SYCL implementations, see
this before deciding which SYCL implementation to use.

Make sure to also install the specified version to ensure that you can build
all of the exercises.

| Implementation | Supported Platforms | Supported Devices | Required Version |
|----------------|---------------------|-------------------|------------------|
| ComputeCpp | Windows 10 Visual Studio 2019 (64bit) <br> Ubtuntu 18.04 (64bit) | Intel CPU (OpenCL) <br> Intel GPU (OpenCL) | CE 2.2.0 |
| DPC++ | Intel DevCloud <br> Windows 10 Visual Studio 2019 (64bit) <br> Ubtuntu 18.04 (64bit) | Intel CPU (OpenCL) <br> Intel GPU (OpenCL) <br> Intel FPGA (OpenCL) <br> Nvidia GPU (CUDA) | 2021.1-beta05	|
| hipSYCL | Any Linux | CPU (OpenMP) <br> AMD GPU (ROCm)* <br> Nvidia GPU (CUDA) | Latest master |

\* See [here][rocm-gpus] for the official list of GPUs supported by AMD for
ROCm. We do not recommend using GPUs earlier than gfx9 (Vega 10 and Vega 20
chips).

### Install SYCL implementations

First you'll need to install your chosen SYCL implementation and any
dependencies they require.

#### Installing ComputeCpp

To set up ComputeCpp download the [ComputeCpp CE package][computecpp-download]
and follow the [getting started instructions][computecpp-getting-started].

#### Installing DPC++

To set up DPC++ follow the
[getting started instructions][dpcpp-getting-started].

If you are using the Intel DevCloud then the latest version of DPC++ will
already be installed and available in the path.

#### Installing hipSYCL

You will need a hipSYCL build from April 26th 2020 or newer. The easiest way to
install a recent distribution of hipSYCL is to use the
[daily package repositories][hipsycl-repositories-detail]. Binary packages are
provided for Ubuntu 18.04, CentOS 7 and Arch Linux. See
[here][hipsycl-repositories] for an explanation of the packages that you need.

If you do not need the ROCm backend, a recent distribution can also be installed
using the [spack][spack] package manager:
```
spack install hipsycl@master +cuda
```
If you do not need the CUDA backend, you can remove `+cuda`.

Of course, you can also build hipSYCL [from source manually][hipsycl-building].

### Pre-requisites

Before building the exercises you'll need:

* One of the platforms in the support matrix above, depending on which SYCL
implementation you are wishing to build for.
* A C++17 or above tool-chain.
* An appropriate build system for the platform you are targeting (CMake, Ninja,
Make, Visual Studio).

### Configuring using CMake (ComputeCpp CE and hipSYCL only)

Clone this repository, there are some additional dependencies configured as git
sub-modules so make sure to clone those as well. Then simply invoke CMake as
follows:

```
mkdir build

cd build

cmake ../ -G<cmake_generator> -A<cmake_arch> -D<sycl_implementation>=ON
```

For `<cmake_generator>` / `<cmake_arch>` we recommend:

* Visual Studio 16 2019 / x64 (Windows)
* Ninja / NA (Windows or Linux)
* Make / NA (Linux)

For `sycl_implementation` this can be one of:

* `SYCL_ACADEMY_USE_COMPUTECPP`
* `SYCL_ACADEMY_USE_HIPSYCL`

You can also specify the additional optional options:

-DSYCL_ACADEMY_INSTALL_ROOT=<path_to_sycl_impl_install_root>

For `<path_to_sycl_impl_install_root>` we recommend you specify the path to the
root directory of your SYCL implementation installation, though this may not
always be required.

-DSYCL_ACADEMY_ENABLE_SOLUTIONS=ON

This will enable building the solutions for each exercise as well as the source
files. This is disabled by default.

#### Additional cmake arguments for hipSYCL

When building with hipSYCL, cmake will additionally require you to specify the
target platform using `-DHIPSYCL_PLATFORM=cpu|rocm|cuda` and, when compiling for
GPU, the target architecture using `-DHIPSYCL_GPU_ARCH=arch`.
* For NVIDIA GPUs, `arch` is of the form `sm_XX`. For example, `sm_60` for
Pascal GPUs (GeForce GTX 1000 series). 
* When compiling for AMD GPUs, `arch` is of the form `gfxXXX`. For example,
`gfx900` for Vega 10 chips (Vega 56 and Vega 64) or `gfx906` (Radeon VII).

### Compiling directly (DPC++ only)

If you are using DPC++ there is no CMake integration, but it is very simple to
use the DPC++ compiler directly.

First you have to ensure that your environment is configured to use DPC++ (note
if you are using the Intel DevCloud then you don't need to do this step).

On Linux simply call the `setvars.sh` which when is available in
`/opt/intel/inteloneapi` when installed as root or sudo and
`~/intel/inteloneapi/` otherwise.

`source /opt/intel/inteloneapi/setvars.sh`

or

`source ~/intel/inteloneapi/setvars.sh`

On Windows the script is located in  `<dpc++_install_root>\setvars.bat`

Where `<dpc++_install_root>` is wherever the `inteloneapi` directory is
installed.

Once that's done you can invoke the DPC++ compiler as follows:

`dpcpp -I<syclacademy_root>/External/Catch2/single_include -o a.out source.cpp`

Where `<syclacademy_root>` is the path to the root directory of where you cloned
this repository.

## Online Interactive Tutorial

Hosted by tech.io, this [SYCL Introduction](https://tech.io/playgrounds/48226/introduction-to-sycl/introduction-to-sycl-2) tutorial introduces the concepts of SYCL. The website also provides the ability to compile and execute SYCL code from your web browser.

## Setting up Computers for SYCL

#### Machine Setup Instructions

ComputeCpp, a SYCL v1.2.1 conformant implementation by Codeplay Software provides setup instructions on [developer.codeplay.com](https://developer.codeplay.com). There is more detailed information about what hardware is supported by ComputeCpp on the [Platform Support](https://developer.codeplay.com/products/computecpp/ce/guides/platform-support) page.

Other SYCL implementations can be found on the SYCL community website [sycl.tech](https://sycl.tech).


SYCL and the SYCL logo are trademarks of the Khronos Group Inc.

[computecpp-download]: https://developer.codeplay.com
[computecpp-getting-started]: https://developer.codeplay.com/products/computecpp/ce/guides/getting-started?
[dpcpp-getting-started]: https://software.intel.com/en-us/articles/how-to-install-oneapi-products-and-run-data-parallel-cpp-code-samples

[hipsycl-repositories]: https://github.com/illuhad/hipSYCL#repositories
[hipsycl-repositories-detail]: https://github.com/illuhad/hipSYCL/blob/master/install/scripts/README.md#installing-from-repositories
[hipsycl-download]: https://github.com/illuhad/hipSYCL/blob/master/install/scripts/README.md#installing-from-repositories
[hipsycl-getting-started]: https://github.com/illuhad/hipSYCL#building-and-installing-hipsycl
[hipsycl-building]: https://github.com/illuhad/hipSYCL#manual-installation
[rocm-gpus]: https://github.com/RadeonOpenCompute/ROCm#supported-gpus
[spack]: https://github.com/spack/spack

[lesson-1-slides]: ./Lesson_Materials/Lesson-1-Introduction-to-SYCL/
[lesson-1-exercise]: ./Code_Exercises/Exercise_1_Getting_Started/doc.md
[lesson-1-source]: ./Code_Exercises/Exercise_1_Getting_Started/hello_world.cpp
[lesson-1-video]: https://youtu.be/1RqdVEDY5vg

[lesson-2-slides]: ./Lesson_Materials/Lesson-2-SYCL-Topology-Discovery-and-Queue-Creation/
[lesson-2-exercise]: ./Code_Exercises/Exercise_2_Configuring_a_Queue/doc.md
[lesson-2-source]: ./Code_Exercises/Exercise_2_Configuring_a_Queue/source.cpp
[lesson-2-solution]: ./Code_Exercises/Exercise_2_Configuring_a_Queue/solution.cpp
[lesson-2-video]: https://youtu.be/C443O1LhUQs

[lesson-3-slides]: ./Lesson_Materials/Lesson-3-SYCL-Kernel-Functions/
[lesson-3-exercise]: ./Code_Exercises/Exercise_3_Hello_World/doc.md
[lesson-3-source]: ./Code_Exercises/Exercise_3_Hello_World/source.cpp
[lesson-3-solution]: ./Code_Exercises/Exercise_3_Hello_World/solution.cpp
[lesson-3-video]: https://youtu.be/pLw_WXKDvj4

[lesson-4-slides]: ./Lesson_Materials/Lesson-4-Managing-Data-in-SYCL-Applications/
[lesson-4-exercise]: ./Code_Exercises/Exercise_4_Vector_Add/doc.md
[lesson-4-source]: ./Code_Exercises/Exercise_4_Vector_Add/source.cpp
[lesson-4-solution]: ./Code_Exercises/Exercise_4_Vector_Add/solution.cpp
[lesson-4-video]: https://youtu.be/UFNhgPOLNwI

[lesson-5-slides]: ./Lesson_Materials/Lesson-5-Data-Dependencies-in-SYCL/

[lesson-6-slides]: ./Lesson_Materials/Lesson-6-Handling-SYCL-Errors/
[lesson-6-exercise]: ./Code_Exercises/Exercise_8_Error_Handling/doc.md
[lesson-6-source]: ./Code_Exercises/Exercise_8_Error_Handling/source.cpp
[lesson-6-solution]: ./Code_Exercises/Exercise_8_Error_Handling/solution.cpp
[lesson-6-video]: https://youtu.be/Dwp9c_lNvSY

[additional-exercises-1]: ./Code_Exercises/Exercise_5_Image_Grayscale/doc.md
[additional-exercises-1-source]: ./Code_Exercises/Exercise_5_Image_Grayscale/source.cpp
[additional-exercises-1-solution]: ./Code_Exercises/Exercise_5_Image_Grayscale/solution.cpp

[additional-exercises-2]: ./Code_Exercises/Exercise_6_Matrix_Transpose/doc.md
[additional-exercises-2-source]: ./Code_Exercises/Exercise_6_Matrix_Transpose/source.cpp
[additional-exercises-2-solution]: ./Code_Exercises/Exercise_6_Matrix_Transpose/solution.cpp

[additional-exercises-3]: ./Code_Exercises/Exercise_7_Unified_Shared_Memory_Ext/doc.md
[additional-exercises-3-source]: ./Code_Exercises/Exercise_7_Unified_Shared_Memory_Ext/source.cpp
[additional-exercises-3-solution]: ./Code_Exercises/Exercise_7_Unified_Shared_Memory_Ext/solution.cpp

