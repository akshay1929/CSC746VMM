# vmmul instructional test harness

This directory contains a benchmark harness for testing different implementations of
vector-matrix multiply (VMM) for varying problem sizes.

The main code is benchmark.cpp, which sets up the problem, iterates over problem
sizes, sets up the vector and matrix, executes the vmmul call, and tests the
result for accuracy by comparing your result against a reference implementation (CBLAS).

Note that cmake needs to be able to find the CBLAS package. For CSC 746 Fall 2022,
this condition is true on Cori@NERSC and on the class VM. It is also true for some
other platforms, but you are on your own if using a platform other than Cori@NERSC
or the class VM.

# Build instructions - general

After downloading, cd into the main source directly, then:

% mkdir build  
% cd build  
% cmake ../  

Before these commands actually work, you may need to make some adjustments to your environment.
Below is information for Cori@NERSC, and for MacOS systems.

# Build peculiarities on Cori@NERSC

When building on Cori, make sure you are on a KNL node when doing the compilation. The
Cori login nodes are *not* KNL nodes, the Cori login nodes have Intel Xeon E5-2698
processors, not the Intel Xeon Phi 7250 (KNL) processors.  The simplest way to do this is
grab an interactive KNL node:  
salloc --nodes 1 --qos interactive --time 01:00:00 --constraint knl --account m3930

Also on Cori:

- cmake version: you need to use cmake/3.14 or higher. By default, Cori's cmake is cmake/3.10.2. 
Please type "module load cmake" from the command line, and the modules infrastructure will make
available to you cmake/3.14.4.

- Programming environment. You need to use the Cray-Gnu compilers for this assignment. To access
them, please type "module swap PrgEnv-intel PrgEnv-gnu" on the command line.

# Build peculiarities for MacOSX platforms:

On Prof. Bethel's laptop, which is an intel-based Macbook Pro running Big Sur, and
with Xcode installed, cmake (version 3.20.1) can find the BLAS package, but then the build fails with
an error about not being able to find cblas.h.

The workaround is to tell cmake where cblas.h lives by using an environment variable:
export CXXFLAGS="-I /Library/Developer/CommandLineTools/SDKs/MacOSX10.15.sdk/System/Library/Frameworks/Accelerate.framework/Versions/A/Frameworks/vecLib.framework/Versions/A/Headers/"
then clean your build directory (rm -rf * inside build) and run cmake again. 

Note you will need to "locate cblas.h" on your machine and replace the path to cblas.h
in the CXXFLAGS line above with the path on your specific machine.

# Adding your code

For timing:

You will need to modify the benchmark.cpp code to add timing instrumentation, to 
report FLOPs executed, and so forth.


For vector-matrix multiplication:

There are stub routines inside dgemv-basic.cpp and dgemv-openmp.cpp where you can
add your code for doing basic and OpenMP-parallel vector-matrix multiply, respectively.

For the OpenMP parallel code, note that you specify concurrency at runtime using
the OMP_NUM_THREADS environment variable. While it is possible to set the number of
concurrent OpenMP threads at compile time, it is generally considered better practice to
specify the number of OpenMP threads via the OMP_NUM_THREADS environment variable.


# Running the codes

Some sample job scripts are provided as part of the harness. In principle, you should be able to use
them to launch batch jobs that run your code. You will probably need to make some adjustments
to these scripts for your particular testing workflow.

These sample job scripts have some reference values and tips for managing OpenMP-related
environment variables that are relevant to HW3 and Cori@NERSC.


#eof

# Results

Basic
# Static scheduling

T = 1
Description:	Basic implementation of matrix-vector multiply.

Working on problem size N=1024 
Elapsed time is : 0.00450403 
Working on problem size N=2048 
Elapsed time is : 0.0179831 
Working on problem size N=4096 
Elapsed time is : 0.0720668 
Working on problem size N=8192 
Elapsed time is : 0.287617 
Working on problem size N=16384 
Elapsed time is : 1.15616 
Description:	Basic implementation of matrix-vector multiply.

T = 2
Working on problem size N=1024 
Elapsed time is : 0.00450148 
Working on problem size N=2048 
Elapsed time is : 0.0180571 
Working on problem size N=4096 
Elapsed time is : 0.0720329 
Working on problem size N=8192 
Elapsed time is : 0.287371 
Working on problem size N=16384 
Elapsed time is : 1.15448 
srun: Job 63060579 step creation temporarily disabled, retrying (Requested nodes are busy)
srun: Step created for job 63060579
Description:	Basic implementation of matrix-vector multiply.

T = 4
Working on problem size N=1024 
Elapsed time is : 0.00455817 
Working on problem size N=2048 
Elapsed time is : 0.0179758 
Working on problem size N=4096 
Elapsed time is : 0.0719247 
Working on problem size N=8192 
Elapsed time is : 0.287542 
Working on problem size N=16384 
Elapsed time is : 1.1546 
Description:	Basic implementation of matrix-vector multiply.

T = 8
Working on problem size N=1024 
Elapsed time is : 0.00449933 
Working on problem size N=2048 
Elapsed time is : 0.018009 
Working on problem size N=4096 
Elapsed time is : 0.0718916 
Working on problem size N=8192 
Elapsed time is : 0.287459 
Working on problem size N=16384 
Elapsed time is : 1.15445 
Description:	Basic implementation of matrix-vector multiply.

T = 16
Working on problem size N=1024 
Elapsed time is : 0.00449684 
Working on problem size N=2048 
Elapsed time is : 0.0179946 
Working on problem size N=4096 
Elapsed time is : 0.0719722 
Working on problem size N=8192 
Elapsed time is : 0.287605 
Working on problem size N=16384 
Elapsed time is : 1.15459 
Description:	Basic implementation of matrix-vector multiply.

T = 32
Working on problem size N=1024 
Elapsed time is : 0.0044984 
Working on problem size N=2048 
Elapsed time is : 0.018015 
Working on problem size N=4096 
Elapsed time is : 0.0722035 
Working on problem size N=8192 
Elapsed time is : 0.287722 
Working on problem size N=16384 
Elapsed time is : 1.15404 
Description:	Basic implementation of matrix-vector multiply.

T = 64
Working on problem size N=1024 
Elapsed time is : 0.00453551 
Working on problem size N=2048 
Elapsed time is : 0.0180168 
Working on problem size N=4096 
Elapsed time is : 0.071889 
Working on problem size N=8192 
Elapsed time is : 0.287655 
Working on problem size N=16384 
Elapsed time is : 1.15399

# Dynamic scheduling

T = 1
Description:	Basic implementation of matrix-vector multiply.

Working on problem size N=1024 
Elapsed time is : 0.00449906 
Working on problem size N=2048 
Elapsed time is : 0.018021 
Working on problem size N=4096 
Elapsed time is : 0.0719803 
Working on problem size N=8192 
Elapsed time is : 0.28759 
Working on problem size N=16384 
Elapsed time is : 1.15473 
Description:	Basic implementation of matrix-vector multiply.

T = 2
Working on problem size N=1024 
Elapsed time is : 0.00449807 
Working on problem size N=2048 
Elapsed time is : 0.0180097 
Working on problem size N=4096 
Elapsed time is : 0.0719326 
Working on problem size N=8192 
Elapsed time is : 0.287562 
Working on problem size N=16384 
Elapsed time is : 1.15516 
Description:	Basic implementation of matrix-vector multiply.

T = 4
Working on problem size N=1024 
Elapsed time is : 0.00449877 
Working on problem size N=2048 
Elapsed time is : 0.0180008 
Working on problem size N=4096 
Elapsed time is : 0.071998 
Working on problem size N=8192 
Elapsed time is : 0.287535 
Working on problem size N=16384 
Elapsed time is : 1.15467 
Description:	Basic implementation of matrix-vector multiply.

T = 8
Working on problem size N=1024 
Elapsed time is : 0.00450812 
Working on problem size N=2048 
Elapsed time is : 0.0180334 
Working on problem size N=4096 
Elapsed time is : 0.0722375 
Working on problem size N=8192 
Elapsed time is : 0.288646 
Working on problem size N=16384 
Elapsed time is : 1.15596 
Description:	Basic implementation of matrix-vector multiply.

T = 16
Working on problem size N=1024 
Elapsed time is : 0.00450375 
Working on problem size N=2048 
Elapsed time is : 0.018022 
Working on problem size N=4096 
Elapsed time is : 0.0721642 
Working on problem size N=8192 
Elapsed time is : 0.28842 
Working on problem size N=16384 
Elapsed time is : 1.15853 
srun: Job 63060579 step creation temporarily disabled, retrying (Requested nodes are busy)
srun: Step created for job 63060579
Description:	Basic implementation of matrix-vector multiply.

T = 32
Working on problem size N=1024 
Elapsed time is : 0.00449979 
Working on problem size N=2048 
Elapsed time is : 0.018005 
Working on problem size N=4096 
Elapsed time is : 0.071931 
Working on problem size N=8192 
Elapsed time is : 0.287678 
Working on problem size N=16384 
Elapsed time is : 1.15409 
Description:	Basic implementation of matrix-vector multiply.

T = 64
Working on problem size N=1024 
Elapsed time is : 0.00450622 
Working on problem size N=2048 
Elapsed time is : 0.0180225 
Working on problem size N=4096 
Elapsed time is : 0.0720989 
Working on problem size N=8192 
Elapsed time is : 0.288028 
Working on problem size N=16384 
Elapsed time is : 1.15638

# OPENMP
# Static scheduling 

T = 1
Description:	OpenMP dgemv.

Working on problem size N=1024 
Elapsed time is : 0.00841679 
Working on problem size N=2048 
Elapsed time is : 0.0262574 
Working on problem size N=4096 
Elapsed time is : 0.0888509 
Working on problem size N=8192 
Elapsed time is : 0.321739 
Working on problem size N=16384 
Elapsed time is : 1.22185 
Description:	OpenMP dgemv.

T = 2
Working on problem size N=1024 
Elapsed time is : 0.00500996 
Working on problem size N=2048 
Elapsed time is : 0.0135415 
Working on problem size N=4096 
Elapsed time is : 0.0453409 
Working on problem size N=8192 
Elapsed time is : 0.162649 
Working on problem size N=16384 
Elapsed time is : 0.61446 
srun: Job 63060579 step creation temporarily disabled, retrying (Requested nodes are busy)
srun: Step created for job 63060579
Description:	OpenMP dgemv.

T = 4
Working on problem size N=1024 
Elapsed time is : 0.00381039 
Working on problem size N=2048 
Elapsed time is : 0.00684593 
Working on problem size N=4096 
Elapsed time is : 0.0228507 
Working on problem size N=8192 
Elapsed time is : 0.0814856 
Working on problem size N=16384 
Elapsed time is : 0.307734 
Description:	OpenMP dgemv.

T = 8
Working on problem size N=1024 
Elapsed time is : 0.00365074 
Working on problem size N=2048 
Elapsed time is : 0.00354096 
Working on problem size N=4096 
Elapsed time is : 0.0115295 
Working on problem size N=8192 
Elapsed time is : 0.0409531 
Working on problem size N=16384 
Elapsed time is : 0.154084 
Description:	OpenMP dgemv.

T = 16
Working on problem size N=1024 
Elapsed time is : 0.00502103 
Working on problem size N=2048 
Elapsed time is : 0.00185128 
Working on problem size N=4096 
Elapsed time is : 0.00594808 
Working on problem size N=8192 
Elapsed time is : 0.0206692 
Working on problem size N=16384 
Elapsed time is : 0.0771928 
Description:	OpenMP dgemv.

T = 32
Working on problem size N=1024 
Elapsed time is : 0.00805395 
Working on problem size N=2048 
Elapsed time is : 0.00448211 
Working on problem size N=4096 
Elapsed time is : 0.00373542 
Working on problem size N=8192 
Elapsed time is : 0.0106389 
Working on problem size N=16384 
Elapsed time is : 0.0395299 
Description:	OpenMP dgemv.

T = 64
Working on problem size N=1024 
Elapsed time is : 0.0143869 
Working on problem size N=2048 
Elapsed time is : 0.00612243 
Working on problem size N=4096 
Elapsed time is : 0.0116256 
Working on problem size N=8192 
Elapsed time is : 0.0217286 
Working on problem size N=16384 
Elapsed time is : 0.0201688


# Dynamic sceduling

T = 1
Description:	OpenMP dgemv.

Working on problem size N=1024 
Elapsed time is : 0.00838497 
Working on problem size N=2048 
Elapsed time is : 0.0260936 
Working on problem size N=4096 
Elapsed time is : 0.0887591 
Working on problem size N=8192 
Elapsed time is : 0.322132 
Working on problem size N=16384 
Elapsed time is : 1.22311 
Description:	OpenMP dgemv.

T = 2
Working on problem size N=1024 
Elapsed time is : 0.0049911 
Working on problem size N=2048 
Elapsed time is : 0.0135059 
Working on problem size N=4096 
Elapsed time is : 0.045245 
Working on problem size N=8192 
Elapsed time is : 0.162665 
Working on problem size N=16384 
Elapsed time is : 0.614129 
Description:	OpenMP dgemv.

T = 4
Working on problem size N=1024 
Elapsed time is : 0.00370692 
Working on problem size N=2048 
Elapsed time is : 0.00686166 
Working on problem size N=4096 
Elapsed time is : 0.0227937 
Working on problem size N=8192 
Elapsed time is : 0.0815658 
Working on problem size N=16384 
Elapsed time is : 0.308324 
Description:	OpenMP dgemv.

T = 8
Working on problem size N=1024 
Elapsed time is : 0.00372443 
Working on problem size N=2048 
Elapsed time is : 0.00357012 
Working on problem size N=4096 
Elapsed time is : 0.0115853 
Working on problem size N=8192 
Elapsed time is : 0.0409167 
Working on problem size N=16384 
Elapsed time is : 0.154902 
Description:	OpenMP dgemv.

T = 16
Working on problem size N=1024 
Elapsed time is : 0.00474674 
Working on problem size N=2048 
Elapsed time is : 0.00191326 
Working on problem size N=4096 
Elapsed time is : 0.00590795 
Working on problem size N=8192 
Elapsed time is : 0.0206569 
Working on problem size N=16384 
Elapsed time is : 0.0772755 
Description:	OpenMP dgemv.

T = 32
Working on problem size N=1024 
Elapsed time is : 0.00887679 
Working on problem size N=2048 
Elapsed time is : 0.00413728 
Working on problem size N=4096 
Elapsed time is : 0.00311322 
Working on problem size N=8192 
Elapsed time is : 0.0105606 
Working on problem size N=16384 
Elapsed time is : 0.0394107 
Description:	OpenMP dgemv.

T = 64
Working on problem size N=1024 
Elapsed time is : 0.0156667 
Working on problem size N=2048 
Elapsed time is : 0.00579625 
Working on problem size N=4096 
Elapsed time is : 0.0109808 
Working on problem size N=8192 
Elapsed time is : 0.0220112 
Working on problem size N=16384 
Elapsed time is : 0.0201208

# CBLAS
# Static scheduling

T = 1
Description:	Reference dgemv.

Working on problem size N=1024 
Elapsed time is : 0.00526056 
Working on problem size N=2048 
Elapsed time is : 0.00334003 
Working on problem size N=4096 
Elapsed time is : 0.0133663 
Working on problem size N=8192 
Elapsed time is : 0.0545445 
Working on problem size N=16384 
Elapsed time is : 0.217391 
Description:	Reference dgemv.

T = 2
Working on problem size N=1024 
Elapsed time is : 0.00518083 
Working on problem size N=2048 
Elapsed time is : 0.00330812 
Working on problem size N=4096 
Elapsed time is : 0.0133639 
Working on problem size N=8192 
Elapsed time is : 0.0545157 
Working on problem size N=16384 
Elapsed time is : 0.217061 
Description:	Reference dgemv.

T = 4
Working on problem size N=1024 
Elapsed time is : 0.00531837 
Working on problem size N=2048 
Elapsed time is : 0.00334688 
Working on problem size N=4096 
Elapsed time is : 0.0133809 
Working on problem size N=8192 
Elapsed time is : 0.0545143 
Working on problem size N=16384 
Elapsed time is : 0.216954 
Description:	Reference dgemv.

T = 8
Working on problem size N=1024 
Elapsed time is : 0.00523567 
Working on problem size N=2048 
Elapsed time is : 0.00331071 
Working on problem size N=4096 
Elapsed time is : 0.0134017 
Working on problem size N=8192 
Elapsed time is : 0.0548878 
Working on problem size N=16384 
Elapsed time is : 0.218738 
srun: Job 63060579 step creation temporarily disabled, retrying (Requested nodes are busy)
srun: Step created for job 63060579
Description:	Reference dgemv.

T = 16
Working on problem size N=1024 
Elapsed time is : 0.00522682 
Working on problem size N=2048 
Elapsed time is : 0.00330169 
Working on problem size N=4096 
Elapsed time is : 0.0133848 
Working on problem size N=8192 
Elapsed time is : 0.0545223 
Working on problem size N=16384 
Elapsed time is : 0.216929 
Description:	Reference dgemv.

T = 32
Working on problem size N=1024 
Elapsed time is : 0.00560222 
Working on problem size N=2048 
Elapsed time is : 0.0039289 
Working on problem size N=4096 
Elapsed time is : 0.0143898 
Working on problem size N=8192 
Elapsed time is : 0.0575724 
Working on problem size N=16384 
Elapsed time is : 0.232199 
Description:	Reference dgemv.

T = 64
Working on problem size N=1024 
Elapsed time is : 0.00532231 
Working on problem size N=2048 
Elapsed time is : 0.0033243 
Working on problem size N=4096 
Elapsed time is : 0.0133754 
Working on problem size N=8192 
Elapsed time is : 0.0544629 
Working on problem size N=16384 
Elapsed time is : 0.217202

# Dynamic scheduling 

T = 1
Description:	Reference dgemv.

Working on problem size N=1024 
Elapsed time is : 0.00519374 
Working on problem size N=2048 
Elapsed time is : 0.00331425 
Working on problem size N=4096 
Elapsed time is : 0.0133516 
Working on problem size N=8192 
Elapsed time is : 0.0544148 
Working on problem size N=16384 
Elapsed time is : 0.216801 
Description:	Reference dgemv.

T = 2
Working on problem size N=1024 
Elapsed time is : 0.00520792 
Working on problem size N=2048 
Elapsed time is : 0.00330206 
Working on problem size N=4096 
Elapsed time is : 0.0132843 
Working on problem size N=8192 
Elapsed time is : 0.0544896 
Working on problem size N=16384 
Elapsed time is : 0.216808 
Description:	Reference dgemv.

T = 4
Working on problem size N=1024 
Elapsed time is : 0.00527676 
Working on problem size N=2048 
Elapsed time is : 0.00332155 
Working on problem size N=4096 
Elapsed time is : 0.0144118 
Working on problem size N=8192 
Elapsed time is : 0.0572019 
Working on problem size N=16384 
Elapsed time is : 0.231093 
srun: Job 63060579 step creation temporarily disabled, retrying (Requested nodes are busy)
srun: Step created for job 63060579
Description:	Reference dgemv.

T = 8
Working on problem size N=1024 
Elapsed time is : 0.00518586 
Working on problem size N=2048 
Elapsed time is : 0.00329725 
Working on problem size N=4096 
Elapsed time is : 0.0133602 
Working on problem size N=8192 
Elapsed time is : 0.0544774 
Working on problem size N=16384 
Elapsed time is : 0.217253 
Description:	Reference dgemv.

T = 16
Working on problem size N=1024 
Elapsed time is : 0.0051737 
Working on problem size N=2048 
Elapsed time is : 0.00332274 
Working on problem size N=4096 
Elapsed time is : 0.013369 
Working on problem size N=8192 
Elapsed time is : 0.0544536 
Working on problem size N=16384 
Elapsed time is : 0.217004 
Description:	Reference dgemv.

T = 32
Working on problem size N=1024 
Elapsed time is : 0.00525887 
Working on problem size N=2048 
Elapsed time is : 0.00334633 
Working on problem size N=4096 
Elapsed time is : 0.0134315 
Working on problem size N=8192 
Elapsed time is : 0.0548707 
Working on problem size N=16384 
Elapsed time is : 0.218998 
Description:	Reference dgemv.

T = 64
Working on problem size N=1024 
Elapsed time is : 0.00522429 
Working on problem size N=2048 
Elapsed time is : 0.00331461 
Working on problem size N=4096 
Elapsed time is : 0.0132848 
Working on problem size N=8192 
Elapsed time is : 0.0544344 
Working on problem size N=16384 
Elapsed time is : 0.216718
