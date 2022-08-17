Building xSDK and xSDK-examples using Spack and running tests.

Notes:

The following was run in base directory of */scratch/shudson* on CELS GCE node compute-07.cels.anl.gov.

## Step 1: Setup

git cloning spack and xsdk-examples:

    git clone -c feature.manyFiles=true https://github.com/spack/spack.git

Set spack root:

    export SPACK_ROOT=/scratch/shudson/spack
    export PATH=$SPACK_ROOT/bin:$PATH


## Step 2: Build and test xSDK-examples

Install xSDK and examples.

    spack install --keep-stage --fail-fast -j 16 --test=root xsdk-examples%gcc@9.4.0

Find location of build directory

    spack location -b xsdk-examples

Go to to output of above command and run tests.

    cd /tmp/shudson/spack-stage/spack-stage-xsdk-examples-0.3.0-4aloneyesylhqdhfke3o25n3wo2cmr7q/spack-build-4aloney
    ctest .


## Output

shudson@compute-386-07:spack-build-4aloney$ ctest .
Test project /tmp/shudson/spack-stage/spack-stage-xsdk-examples-0.3.0-4aloneyesylhqdhfke3o25n3wo2cmr7q/spack-build-4aloney
      Start  1: HYPRE-ij_laplacian
 1/15 Test  #1: HYPRE-ij_laplacian .......................   Passed    0.12 sec
      Start  2: MFEM-mfem_ex1_gko
 2/15 Test  #2: MFEM-mfem_ex1_gko ........................   Passed    7.59 sec
      Start  3: MFEM-magnetic-diffusion--cpu
 3/15 Test  #3: MFEM-magnetic-diffusion--cpu .............   Passed    0.18 sec
      Start  4: MFEM-convdiff--hypre-boomeramg
 4/15 Test  #4: MFEM-convdiff--hypre-boomeramg ...........   Passed    0.18 sec
      Start  5: MFEM-convdiff--superlu
 5/15 Test  #5: MFEM-convdiff--superlu ...................   Passed    0.91 sec
      Start  6: MFEM-obstacle
 6/15 Test  #6: MFEM-obstacle ............................   Passed   10.90 sec
      Start  7: MFEM-transient-heat
 7/15 Test  #7: MFEM-transient-heat ......................   Passed    0.70 sec
      Start  8: MFEM-advection--cpu
 8/15 Test  #8: MFEM-advection--cpu ......................   Passed    0.49 sec
      Start  9: PETSc-ex19_1
 9/15 Test  #9: PETSc-ex19_1 .............................   Passed    0.18 sec
      Start 10: PETSc-ex19_hypre
10/15 Test #10: PETSc-ex19_hypre .........................   Passed    0.12 sec
      Start 11: PETSc-ex19_superlu_dist
11/15 Test #11: PETSc-ex19_superlu_dist ..................   Passed    0.14 sec
      Start 12: PLASMA-ex1solve
12/15 Test #12: PLASMA-ex1solve ..........................   Passed    0.06 sec
      Start 13: SUNDIALS-cv_petsc_ex7_1
13/15 Test #13: SUNDIALS-cv_petsc_ex7_1 ..................   Passed    0.14 sec
      Start 14: SUNDIALS-cv_petsc_ex7_2
14/15 Test #14: SUNDIALS-cv_petsc_ex7_2 ..................   Passed    0.10 sec
      Start 15: SUNDIALS-ark_brusselator1D_FEM_sludist
15/15 Test #15: SUNDIALS-ark_brusselator1D_FEM_sludist ...   Passed    0.40 sec
100% tests passed, 0 tests failed out of 15
Total Test time (real) =  22.22 sec
