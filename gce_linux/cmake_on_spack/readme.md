Building xsdk using spack and then building and running xsdk-examples in cmake.

This is so I can work in xsdk-examples repo.

Notes:

I had to build spack xsdk with +trilinos. Trilinos did not build by default and was needed
at the cmake stage. The spack install line below uses --keep-stage and --test=root. This
was built previously for running examples via spack so these options may not be needed here.

Example, all taking place in base directory of */home/shudson*.
git cloning spack and xsdk-examples directly under that:

    git clone -c feature.manyFiles=true https://github.com/spack/spack.git
    git clone https://github.com/xsdk-project/xsdk-examples

Set spack root:

    export SPACK_ROOT=/home/shudson/spack
    export PATH=$SPACK_ROOT/bin:$PATH

Spack install (could create env first - does not matter):

    spack install --keep-stage --fail-fast --test=root xsdk-examples%gcc@9.4.0 + trilinos

Now create a spack environment (so can use a view for cmake line):

    . $SPACK_ROOT/share/spack/setup-env.sh
    spack env create myenv
    spack env activate -pv myenv  # -p puts env in prompt (-v dont know if need - to do with view)

Adding spack xsdk package to environment - I needed to add trilinos or it does not work.

    # spack add xsdk%gcc@9.4.0  # ideal line
    spack add xsdk%gcc@9.4.0 + trilinos  # Due to error
    spack concretize

The *spack.yaml* file will be at `$SPACK_ROOT/var/spack/environments/myenv/spack.yaml`
The active lines are:

    spack:
        specs: [xsdk%gcc@9.4.0+trilinos]
        view: true
        concretizer:
            unify: false

Basic line - for cmake did not work for me:

    cmake -DCMAKE_PREFIX_PATH=$SPACK_ROOT/var/spack/environments/myenv/.spack-env/view -DENABLE_CUDA=FALSE -DENABLE_HIP=FALSE -S xsdk-examples/ -B xsdk-examples/builddir

I had to put in .local on my system and I had to set `-DPETSc_DIR` for PETSc tests to work:

    cmake -DPETSc_DIR=/home/shudson/spack/var/spack/environments/myenv/.spack-env/view -DCMAKE_INSTALL_PREFIX=/home/shudson/.local -DCMAKE_PREFIX_PATH=/home/shudson/spack/var/spack/environments/myenv/.spack-env/view -DENABLE_CUDA=FALSE -DENABLE_HIP=FALSE -S xsdk-examples/ -B xsdk-examples/builddir

Now go to builddir and make and run tests:

    cd xsdk-examples/builddir/
    make
    make install
    ctest .

This worked:

    [myenv] shudson@compute-386-07:builddir$ ctest .
    Test project /home/shudson/xsdk-examples/builddir
        Start  1: HYPRE-ij_laplacian
    1/15 Test  #1: HYPRE-ij_laplacian .......................   Passed    0.16 sec
        Start  2: MFEM-mfem_ex1_gko
    2/15 Test  #2: MFEM-mfem_ex1_gko ........................   Passed    7.70 sec
        Start  3: MFEM-magnetic-diffusion--cpu
    3/15 Test  #3: MFEM-magnetic-diffusion--cpu .............   Passed    0.26 sec
        Start  4: MFEM-convdiff--hypre-boomeramg
    4/15 Test  #4: MFEM-convdiff--hypre-boomeramg ...........   Passed    0.21 sec
        Start  5: MFEM-convdiff--superlu
    5/15 Test  #5: MFEM-convdiff--superlu ...................   Passed    0.94 sec
        Start  6: MFEM-obstacle
    6/15 Test  #6: MFEM-obstacle ............................   Passed   12.95 sec
        Start  7: MFEM-transient-heat
    7/15 Test  #7: MFEM-transient-heat ......................   Passed    0.69 sec
        Start  8: MFEM-advection--cpu
    8/15 Test  #8: MFEM-advection--cpu ......................   Passed    0.55 sec
        Start  9: PETSc-ex19_1
    9/15 Test  #9: PETSc-ex19_1 .............................   Passed    0.21 sec
        Start 10: PETSc-ex19_hypre
    10/15 Test #10: PETSc-ex19_hypre .........................   Passed    0.17 sec
        Start 11: PETSc-ex19_superlu_dist
    11/15 Test #11: PETSc-ex19_superlu_dist ..................   Passed    0.18 sec
        Start 12: PLASMA-ex1solve
    12/15 Test #12: PLASMA-ex1solve ..........................   Passed    0.07 sec
        Start 13: SUNDIALS-cv_petsc_ex7_1
    13/15 Test #13: SUNDIALS-cv_petsc_ex7_1 ..................   Passed    0.14 sec
        Start 14: SUNDIALS-cv_petsc_ex7_2
    14/15 Test #14: SUNDIALS-cv_petsc_ex7_2 ..................   Passed    0.14 sec
        Start 15: SUNDIALS-ark_brusselator1D_FEM_sludist
    15/15 Test #15: SUNDIALS-ark_brusselator1D_FEM_sludist ...   Passed    0.50 sec

    100% tests passed, 0 tests failed out of 15

    Total Test time (real) =  24.93 sec
