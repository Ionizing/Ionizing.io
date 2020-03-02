+++
title = "在 macOS 上编译 VASP 5.4.4"
author = ["Smith Peter"]
date = 2019-05-21
lastmod = 2020-03-02T20:58:46+08:00
tags = ["VASP", "macOS"]
categories = ["VASP"]
draft = false
+++

因组里的服务器放假期间关闭，又适逢疫情导致假期延长好久不能使用 VASP ，故考虑在笔记本上编译一个 VASP ，跑一些小的体系试验用。


## 准备工作 {#准备工作}


### 本机环境 {#本机环境}

本辣鸡使用的机器拥有 16G RAM 和 4 核 CPU ，跑十几个原子的体系应该还是没问题的。系统是 macOS 10.15.1 ，机器上已经安装了 Homebrew 。


### 安装依赖库 {#安装依赖库}


#### GCC 全家桶 {#gcc-全家桶}

由于 macOS 默认使用 LLVM + Clang 编译器，并且将系统的 `gcc` 设置为了 `clang` 的软链，因此直接使用 VASP 提供的 Makefile 编译时会出现各种问题，这里需要使用革努的编译器全家桶 `GCC` ，主要用到了其中的 `gcc` 、 `gfortran` 。

使用 `brew` 安装 `gcc`

```nil
$ brew install gcc@9
==> Installing dependencies for gcc: mpfr and libmpc
==> Installing gcc dependency: mpfr
==> Downloading https://mirrors.ustc.edu.cn/homebrew-bottles/bottles/mpfr-4.0.2.catalina.bottle.tar.gz
######################################################################## 100.0%
==> Pouring mpfr-4.0.2.catalina.bottle.tar.gz
🍺  /usr/local/Cellar/mpfr/4.0.2: 28 files, 4.7MB
==> Installing gcc dependency: libmpc
==> Downloading https://mirrors.ustc.edu.cn/homebrew-bottles/bottles/libmpc-1.1.0.catalina.bottle.tar.gz
######################################################################## 100.0%
==> Pouring libmpc-1.1.0.catalina.bottle.tar.gz
🍺  /usr/local/Cellar/libmpc/1.1.0: 12 files, 361.9KB
==> Installing gcc
==> Downloading https://mirrors.ustc.edu.cn/homebrew-bottles/bottles/gcc-9.2.0_1.catalina.bottle.tar.gz
######################################################################## 100.0%
==> Pouring gcc-9.2.0_1.catalina.bottle.tar.gz
🍺  /usr/local/Cellar/gcc/9.2.0_1: 1,462 files, 287.9MB
```

注意：此时运行 `gcc` 调用的仍然是系统的 `clang` ，要真正使用 GNU 的编译器，应该运行 `gcc-9` 。


#### OpenBLAS {#openblas}

VASP 依赖 BLAS 库，如果有 Intel 编译器全家桶，这些库，包括后面要用到的 FFTW 库等都是已经安装好了的，然而由于经济方面原因，用不起 Intel 的全家桶，并且牙膏厂的编译器时不时会抽风，尤其是在语法上 `icc` 与 `gfortran` 有着各种恩恩怨怨，本着不向资本主义妥协的原则，这里使用 OpenBLAS 来满足 VASP 的依赖，只要使用方法正确，
OpenBLAS 在性能上丝毫不亚于 MKL ，而且没有各种 CVE 。

```nil
$ brew install openblas
==> Downloading https://mirrors.ustc.edu.cn/homebrew-bottles/bottles/openblas-0.3.7.catalina.bottle.tar.gz
######################################################################## 100.0%
==> Pouring openblas-0.3.7.catalina.bottle.tar.gz
==> Caveats
openblas is keg-only, which means it was not symlinked into /usr/local,
because macOS provides BLAS and LAPACK in the Accelerate framework.

For compilers to find openblas you may need to set:
  export LDFLAGS="-L/usr/local/opt/openblas/lib"
  export CPPFLAGS="-I/usr/local/opt/openblas/include"

==> Summary
🍺  /usr/local/Cellar/openblas/0.3.7: 22 files, 120.2MB
```


#### OpenMPI {#openmpi}

虽然本机是笔记本，谈不上什么大规模并行，但 4 核跑总比 1 核跑要快很多，因此
OpenMPI 还是要装的。至于为什么装 OpenMPI 而不是 MPICH ，MPICH 的风评似乎比 OpenMPI
要好一些，但 FFTW 只依赖于 OpenMPI ，不知这是不是维护这个仓库的作者没有写好依赖的缘故。

```nil
$ brew install open-mpi
==> Downloading https://mirrors.ustc.edu.cn/homebrew-bottles/bottles/open-mpi-4.0.2.catalina.bottle.tar.gz
Already downloaded: ...
==> Pouring open-mpi-4.0.2.catalina.bottle.tar.gz
🍺  /usr/local/Cellar/open-mpi/4.0.2: 752 files, 11.2MB
```


#### FFTW {#fftw}

要注意， FFTW 库依赖于 OpenMPI 库，如果先安装 FFTW ，则它默认安装 OpenMPI

```nil
brew install fftw
==> Downloading https://mirrors.ustc.edu.cn/homebrew-bottles/bottles/fftw-3.3.8_1.catalina.bottle.tar.gz
######################################################################## 100.0%
==> Pouring fftw-3.3.8_1.catalina.bottle.tar.gz
🍺  /usr/local/Cellar/fftw/3.3.8_1: 73 files, 14.8MB
```


#### LAPACK {#lapack}

```nil
brew install lapack
==> Downloading https://mirrors.ustc.edu.cn/homebrew-bottles/bottles/lapack-3.8.0_2.catalina.bottle.tar.gz
######################################################################## 100.0%
==> Pouring lapack-3.8.0_2.catalina.bottle.tar.gz
==> Caveats
lapack is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

For compilers to find lapack you may need to set:
  export LDFLAGS="-L/usr/local/opt/lapack/lib"
  export CPPFLAGS="-I/usr/local/opt/lapack/include"

==> Summary
🍺  /usr/local/Cellar/lapack/3.8.0_2: 28 files, 9.9MB
```


#### ScaLAPACK {#scalapack}

```nil
brew install scalapack
==> Downloading https://mirrors.ustc.edu.cn/homebrew-bottles/bottles/scalapack-2.0.2_16.catalina.bottle.tar.gz
######################################################################## 100.0%
==> Pouring scalapack-2.0.2_16.catalina.bottle.tar.gz
🍺  /usr/local/Cellar/scalapack/2.0.2_16: 25 files, 5.6MB
```


## 编译 VASP {#编译-vasp}


### 编译 `vasp.5.lib` {#编译-vasp-dot-5-dot-lib}

在 `vasp.5.lib/` 目录下，可以发现有很多文件，包括一大串以 `makefile` 开头的文件

```nil
$ ls
README.lapack              itmtv.s                    makefile.linux_abs         makefile.t3e
crayerrf.F                 lapack_atlas.f             makefile.linux_alpha       makefile.vpp
dclock.c                   lapack_double.f            makefile.linux_efc_itanium preclib.F
dclock_.c                  lapack_double_1.1.f        makefile.linux_gfortran    ptimers.f
dclock_ds20.c              lapack_single.f            makefile.linux_ifc_P4      ptimers.h
dclock_simple.c            lapack_single_1.1.f        makefile.linux_pg          sclock_cray.F
derrf.c                    linpack_double.f           makefile.linux_pgi_opt     sclock_nec.F
derrf_.c                   linpack_single.f           makefile.nec               sclock_t3d.F
derrf_t3d.c                makefile.cray              makefile.rs6000            serrf.c
diolib.F                   makefile.dec               makefile.rs6000_p1         stripnr
dlexlib.F                  makefile.fujitsu           makefile.sgi               timing.c
drdatab.F                  makefile.hp                makefile.sp2               timing.fujitsu.F
errf.F                     makefile.hpux_itanium      makefile.sun               timing_.c
fujitsu.F                  makefile.linux             makefile.t3d               timing_ds20.c
```

由于 VASP 并不太过依赖 Linux 系统的组件（主要目的是进行科学计算而不是搞 CS 嘛），因此这里可以套用作者为 Linux 的编写的 Makefile 模板（这里强调一些 macOS 和 Linux
并不是同一类操作系统，尽管它们都提供了某种程度上相似的 shell 和系统文件目录结构）。

```sh
cp makefile.linux_gfortran Makefile
```

编辑 `Makefile` ，将所有提到 `gcc` 的地方全部替换为 `gcc-9` （应该只有一处），然后 `make`

```nil
$ make
gcc-9 -E -P -C preclib.F >preclib.f
gfortran -O1 -ffree-form  -c preclib.f
cc -O -c timing_.c
cc -O -c derrf_.c
cc -O -c dclock_.c
gcc-9 -E -P -C diolib.F >diolib.f
gfortran -O1 -ffree-form  -c diolib.f
gcc-9 -E -P -C dlexlib.F >dlexlib.f
gfortran -O1 -ffree-form  -c dlexlib.f
gcc-9 -E -P -C drdatab.F >drdatab.f
gfortran -O1 -ffree-form  -c drdatab.f
gfortran -O1  -c lapack_double.f
gfortran -O1  -c linpack_double.f
gfortran -O1  -c lapack_atlas.f
rm libdmy.a
rm: libdmy.a: No such file or directory
make: [libdmy.a] Error 1 (ignored)
ar vq libdmy.a preclib.o timing_.o derrf_.o dclock_.o  diolib.o dlexlib.o drdatab.o
ar: creating archive libdmy.a
q - preclib.o
q - timing_.o
q - derrf_.o
q - dclock_.o
q - diolib.o
q - dlexlib.o
q - drdatab.o
/Library/Developer/CommandLineTools/usr/bin/ranlib: file: libdmy.a(preclib.o) has no symbols
/Library/Developer/CommandLineTools/usr/bin/ranlib: file: libdmy.a(diolib.o) has no symbols
/Library/Developer/CommandLineTools/usr/bin/ranlib: file: libdmy.a(dlexlib.o) has no symbols
/Library/Developer/CommandLineTools/usr/bin/ranlib: file: libdmy.a(drdatab.o) has no symbols
```

再次查看当前目录，发现已经生成了 `libdmy.a` ，表示编译成功。

这里忽略编译命令输出的那几个 `has no symbols` 错误，因为其对应的 `preclib.F` 等文件根本没写任何代码，生成的目标文件没有符号理所当然。


### 编译 `vasp.5.4.4` {#编译-vasp-dot-5-dot-4-dot-4}

现在开始编译 VASP 的主体。

首先从 `arch/` 文件夹复制出对应的 `makefile.include`

```sh
cp arch/makefile.include.linux_gnu ./makefile.include
```

编辑 `makefile.include` ，将之前装好的库路径填到里面：

```diff
--- makefile.include.bak    2020-02-09 15:54:30.000000000 +0800
+++ makefile.include    2020-02-09 16:33:56.000000000 +0800
@@ -9,7 +9,7 @@
              -Dtbdyn \
              -Duse_shmem

-CPP        = gcc -E -P -C -w $*$(FUFFIX) >$*$(SUFFIX) $(CPP_OPTIONS)
+CPP        = gcc-9 -E -P -C -w $*$(FUFFIX) >$*$(SUFFIX) $(CPP_OPTIONS)

 FC         = mpif90
 FCL        = mpif90
@@ -21,15 +21,15 @@
 OFLAG_IN   = $(OFLAG)
 DEBUG      = -O0

-LIBDIR     = /opt/gfortran/libs/
-BLAS       = -L$(LIBDIR) -lrefblas
-LAPACK     = -L$(LIBDIR) -ltmglib -llapack
+LIBDIR     = /usr/local/opt/
+BLAS       = -L$(LIBDIR)/openblas/lib -lopenblas
+LAPACK     = -L$(LIBDIR)/lapack/lib -llapack #-ltmglib
 BLACS      =
-SCALAPACK  = -L$(LIBDIR) -lscalapack $(BLACS)
+SCALAPACK  = -L$(LIBDIR)/../lib/ -lscalapack $(BLACS)

 LLIBS      = $(SCALAPACK) $(LAPACK) $(BLAS)

-FFTW       ?= /opt/gfortran/fftw-3.3.4-GCC-5.4.1
+FFTW       ?= /usr/local
 LLIBS      += -L$(FFTW)/lib -lfftw3
 INCS       = -I$(FFTW)/include

@@ -41,7 +41,7 @@
 # For what used to be vasp.5.lib
 CPP_LIB    = $(CPP)
 FC_LIB     = $(FC)
-CC_LIB     = gcc
+CC_LIB     = gcc-9
 CFLAGS_LIB = -O
 FFLAGS_LIB = -O1
 FREE_LIB   = $(FREE)
@@ -49,7 +49,7 @@
 OBJECTS_LIB= linpack_double.o getshmem.o

 # For the parser library
-CXX_PARS   = g++
+CXX_PARS   = g++-9

 LIBS       += parser
 LLIBS      += -Lparser -lparser -lstdc++
@@ -65,8 +65,8 @@

 OBJECTS_GPU= fftmpiw.o fftmpi_map.o fft3dlib.o fftw3d_gpu.o fftmpiw_gpu.o

-CC         = gcc
-CXX        = g++
+CC         = gcc-9
+CXX        = g++-9
 CFLAGS     = -fPIC -DADD_ -openmp -DMAGMA_WITH_MKL -DMAGMA_SETAFFINITY -DGPUSHMEM=300 -DHAVE_CUBLAS

 CUDA_ROOT  ?= /usr/local/cuda
@@ -77,4 +77,4 @@
                    -gencode=arch=compute_35,code=\"sm_35,compute_35\" \
                    -gencode=arch=compute_60,code=\"sm_60,compute_60\"

-MPI_INC    = /opt/gfortran/openmpi-1.10.2/install/ompi-1.10.2-GFORTRAN-5.4.1/include
+MPI_INC    = /usr/local/include
```

然后在 `vasp.5.4.4/` 下运行

```sh
make
```

此时编译可能会出现这个错误：

```nil
gcc-9 -O -c -o getshmem.o getshmem.c
getshmem.c: In function 'getshmem_C':
getshmem.c:23:42: error: 'SHM_NORESERVE' undeclared (first use in this function)
   23 |   shmflg = IPC_CREAT | IPC_EXCL | 0600 | SHM_NORESERVE ;
      |                                          ^~~~~~~~~~~~~
getshmem.c:23:42: note: each undeclared identifier is reported only once for each function it appears in
getshmem.c: In function 'getshmem_error_C':
getshmem.c:40:42: error: 'SHM_NORESERVE' undeclared (first use in this function)
   40 |   shmflg = IPC_CREAT | IPC_EXCL | 0600 | SHM_NORESERVE ;
      |                                          ^~~~~~~~~~~~~
```

此时需要对 `src/lib/getshmem.c` 进行修改，在文件头部加上 `#define SHM_NORESERVE 0` 即可。

然后再执行 `make` ，滚去看会沙雕视频，回来应该就编译好了，此时 bin/ 下面应该有编译好的三个可执行文件

```nil
$ ls bin/
vasp_gam vasp_ncl vasp_std
```


## 测试编译结果 : {#测试编译结果}

下载测试用的 VASP 输入文件 [DiamondSirel.tgz](./DiamondSirel.tgz) 这是一个结构优化的任务。

解压并复制 `vasp_std` 到这个文件夹，然后 `mpirun -np 4 ./vasp_std`

```nil
 running on    4 total cores
 distrk:  each k-point on    4 cores,    1 groups
 distr:  one band on    1 cores,    4 groups
 using from now: INCAR
 vasp.5.4.4.18Apr17-6-g9f103f2a35 (build Feb  9 2020 16:40:51) complex
 POSCAR found :  1 types and       2 ions
 scaLAPACK will be used
 LDA part: xc-table for Pade appr. of Perdew
 POSCAR, INCAR and KPOINTS ok, starting setup
 FFT: planning ...
 WAVECAR not read
 entering main loop
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.179626582768E+01   -0.17963E+01   -0.20663E+03  6032   0.424E+02
DAV:   2    -0.108794948288E+02   -0.90832E+01   -0.87100E+01  9120   0.547E+01
DAV:   3    -0.109711659580E+02   -0.91671E-01   -0.91671E-01  7608   0.676E+00
DAV:   4    -0.109714243356E+02   -0.25838E-03   -0.25838E-03  9136   0.359E-01
DAV:   5    -0.109714245379E+02   -0.20237E-06   -0.20249E-06  7832   0.788E-03    0.290E+00
DAV:   6    -0.108628458314E+02    0.10858E+00   -0.69362E-02  6760   0.137E+00    0.173E+00
DAV:   7    -0.108120620014E+02    0.50784E-01   -0.13246E-01  7288   0.202E+00    0.145E-01
DAV:   8    -0.108133616708E+02   -0.12997E-02   -0.36487E-03  6352   0.441E-01    0.250E-02
DAV:   9    -0.108134635282E+02   -0.10186E-03   -0.10446E-04  9320   0.780E-02    0.143E-02
DAV:  10    -0.108134842312E+02   -0.20703E-04   -0.30583E-05  6400   0.416E-02
   1 F= -.10813484E+02 E0= -.10813484E+02  d E =-.108135E+02
 curvature:   0.00 expect dE= 0.000E+00 dE for cont linesearch  0.000E+00
 trial: gam= 0.00000 g(F)=  0.234E-01 g(S)=  0.000E+00 ort = 0.000E+00 (trialstep = 0.100E+01)
 search vector abs. value=  0.234E-01
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108216317427E+02   -0.81682E-02   -0.31840E-01  6816   0.358E+00    0.101E-01
DAV:   2    -0.108225552667E+02   -0.92352E-03   -0.92110E-03  8872   0.763E-01    0.722E-02
DAV:   3    -0.108225417087E+02    0.13558E-04   -0.91815E-05  7384   0.686E-02
   2 F= -.10822542E+02 E0= -.10822542E+02  d E =-.905748E-02
 trial-energy change:   -0.009057  1 .order   -0.009145   -0.023352    0.005061
 step:   0.8219(harm=  0.8219)  dis= 0.01378  next Energy=   -10.823080 (dE=-0.960E-02)
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108229856861E+02   -0.43042E-03   -0.10815E-02  6792   0.654E-01    0.195E-02
DAV:   2    -0.108230167158E+02   -0.31030E-04   -0.33280E-04  8704   0.146E-01
   3 F= -.10823017E+02 E0= -.10823017E+02  d E =-.953248E-02
 curvature:  -0.41 expect dE=-0.232E-04 dE for cont linesearch -0.254E-06
 ZBRENT: interpolating
 opt :   0.8177  next Energy=   -10.823017 (dE=-0.953E-02)
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108230141529E+02   -0.28467E-04   -0.52173E-06  5768   0.170E-02    0.345E-03
DAV:   2    -0.108230142014E+02   -0.48522E-07   -0.34846E-07  3152   0.491E-03
   4 F= -.10823014E+02 E0= -.10823014E+02  d E =-.952997E-02
 curvature:  -0.41 expect dE=-0.229E-04 dE for cont linesearch -0.309E-08
 ZBRENT: interpolating
 opt :   0.8181  next Energy=   -10.823014 (dE=-0.953E-02)
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108230142235E+02   -0.70550E-07   -0.11367E-07  2968   0.290E-03    0.113E-04
DAV:   2    -0.108230142253E+02   -0.18198E-08   -0.12941E-08  2928   0.947E-04
   5 F= -.10823014E+02 E0= -.10823014E+02  d E =-.952999E-02
 curvature:  -0.37 expect dE=-0.206E-04 dE for cont linesearch -0.691E-09
 trial: gam= 0.00211 g(F)=  0.559E-04 g(S)=  0.000E+00 ort = 0.662E-05 (trialstep = 0.964E+00)
 search vector abs. value=  0.560E-04
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108230342088E+02   -0.19985E-04   -0.70753E-04  6960   0.171E-01    0.474E-03
DAV:   2    -0.108230364157E+02   -0.22069E-05   -0.21824E-05  7808   0.366E-02
   6 F= -.10823036E+02 E0= -.10823036E+02  d E =-.221904E-04
 trial-energy change:   -0.000022  1 .order   -0.000022   -0.000054    0.000009
 step:   0.8219(harm=  0.8219)  dis= 0.00095  next Energy=   -10.823037 (dE=-0.230E-04)
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108230370205E+02   -0.28117E-05   -0.17356E-05  5272   0.271E-02    0.977E-04
DAV:   2    -0.108230371043E+02   -0.83840E-07   -0.86627E-07  2952   0.730E-03
   7 F= -.10823037E+02 E0= -.10823037E+02  d E =-.228790E-04
 curvature:  -0.41 expect dE=-0.416E-07 dE for cont linesearch -0.532E-09
 ZBRENT: interpolating
 opt :   0.8258  next Energy=   -10.823037 (dE=-0.229E-04)
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108230371129E+02   -0.92414E-07   -0.85747E-08  2928   0.264E-03    0.234E-04
DAV:   2    -0.108230371141E+02   -0.12301E-08   -0.99860E-09  2928   0.889E-04
   8 F= -.10823037E+02 E0= -.10823037E+02  d E =-.228888E-04
 curvature:  -0.40 expect dE=-0.411E-07 dE for cont linesearch -0.123E-08
 ZBRENT: interpolating
 opt :   0.8234  next Energy=   -10.823037 (dE=-0.229E-04)
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108230371153E+02   -0.23854E-08   -0.52331E-09  2928   0.542E-04    0.202E-05
DAV:   2    -0.108230371154E+02   -0.83929E-10   -0.82127E-10  2928   0.230E-04
   9 F= -.10823037E+02 E0= -.10823037E+02  d E =-.228901E-04
 curvature:  -0.07 expect dE=-0.673E-08 dE for cont linesearch -0.111E-09
 ZBRENT: interpolating
 opt :   0.8226  next Energy=   -10.823037 (dE=-0.229E-04)
 bond charge predicted
       N       E                     dE             d eps       ncg     rms          rms(c)
DAV:   1    -0.108230371156E+02   -0.32716E-09   -0.75907E-10  2928   0.189E-04    0.103E-05
DAV:   2    -0.108230371156E+02   -0.12164E-10   -0.11950E-10  2928   0.918E-05
  10 F= -.10823037E+02 E0= -.10823037E+02  d E =-.228903E-04
 curvature:  -0.06 expect dE=-0.576E-08 dE for cont linesearch -0.689E-10
 ZBRENT: interpolating
 opt :   0.8223  next Energy=   -10.823037 (dE=-0.229E-04)
 writing wavefunctions
```

看起来没什么问题，收工。
