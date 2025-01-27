# 阿菌•未霜 Clang/LLVM Toolchain with Binutils
这是一个预构建的工具链，构建始终来自最新的[LLVM](https://llvm.org/ "LLVM")和[Binutils](https://www.gnu.org/software/binutils/ "Binutils")源而不是稳定版本，因此无法保证完全的稳定性。它是用Full LTO、PGO和BOLT构建的，以尽可能减少编译时间。

## 性能测试

### 测试环境

#### 系统配置
- CPU: Intel(R) Core(TM) i9-14900K (Pcores 5.8GHz, Ecores 4.6GHz)
- RAM: DDR5 6400MHz x2
- SYSTEM: Ubuntu 22.04 (VMware Workstation Pro 17.5.1) 

#### 测试对象
- Linux Kernel 6.9.0-rc4, defconfig

### Ubuntu clang

#### 版本信息

    Ubuntu clang version 14.0.0-1ubuntu1.1
    Target: x86_64-pc-linux-gnu
    Thread model: posix
    InstalledDir: /usr/bin
    Found candidate GCC installation: /usr/bin/../lib/gcc/x86_64-linux-gnu/11
    Found candidate GCC installation: /usr/bin/../lib/gcc/x86_64-linux-gnu/12
    Selected GCC installation: /usr/bin/../lib/gcc/x86_64-linux-gnu/12
    Candidate multilib: .;@m64
    Selected multilib: .;@m64

#### 测试结果

    $ time make -C /develop/toolchain/clang/linux -skj40 ARCH=x86_64 HOSTCFLAGS=-w KCFLAGS=-w KCPPFLAGS=-w LLVM=1 O=/develop/toolchain/clang/out defconfig all
    
    ________________________________________________________
    Executed in   71.15 secs    fish           external
       usr time   27.53 mins    0.00 micros   27.53 mins
       sys time    3.11 mins  396.00 micros    3.11 mins

### 阿菌•未霜 clang（amd64-kernel-arm）

#### 版本信息

    阿菌•未霜 clang version 19.0.0git (https://github.com/llvm/llvm-project.git e32c4dfefcd1d54eb8f353f6fa08ef6f06d0fcc4)
    Target: x86_64-pc-linux-gnu
    Thread model: posix
    InstalledDir: /develop/storage/nfs/toolchain/clang/amd64-kernel-arm/bin
    Build config: +assertions
    Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/11
    Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/12
    Selected GCC installation: /usr/lib/gcc/x86_64-linux-gnu/12
    Candidate multilib: .;@m64
    Selected multilib: .;@m64

#### 测试结果

    $ time make -C /develop/toolchain/clang/linux -skj40 ARCH=x86_64 HOSTCFLAGS=-w KCFLAGS=-w KCPPFLAGS=-w LLVM=/develop/storage/nfs/toolchain/clang/amd64-kernel-arm/bin/ O=/develop/toolchain/clang/out defconfig all
    
    ________________________________________________________
    Executed in   54.38 secs    fish           external
       usr time   19.58 mins  145.00 micros   19.58 mins
       sys time    2.54 mins  289.00 micros    2.54 mins

### 阿菌•未霜 clang（amd64-kernel-arm_static）

#### 版本信息

    阿菌•未霜 clang version 19.0.0git (https://github.com/llvm/llvm-project.git 0e44ffe817ae0f544199be70f468975fcc3ab5c5)
    Target: x86_64-pc-linux-gnu
    Thread model: posix
    InstalledDir: /develop/storage/nfs/toolchain/clang/amd64-kernel-arm_static/bin
    Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/11
    Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/12
    Selected GCC installation: /usr/lib/gcc/x86_64-linux-gnu/12
    Candidate multilib: .;@m64
    Selected multilib: .;@m64

#### 测试结果

    time make -C /develop/toolchain/clang/linux -skj40 ARCH=x86_64 HOSTCFLAGS=-w KCFLAGS=-w KCPPFLAGS=-w LLVM=/develop/storage/nfs/toolchain/clang/amd64-kernel-arm_static/bin/ O=/develop/toolchain/clang/out defconfig all
    
    ________________________________________________________
    Executed in   45.04 secs    fish           external
       usr time   15.60 mins    0.00 micros   15.60 mins
       sys time    2.31 mins  508.00 micros    2.31 mins

### 阿菌•未霜 clang（amd64-full-toolchain）

#### 版本信息

    阿菌•未霜 clang version 19.0.0git (https://github.com/llvm/llvm-project.git 342aa3eb23992ad742d22ef0e0d8821a9b29de3e)
    Target: x86_64-pc-linux-gnu
    Thread model: posix
    InstalledDir: /develop/storage/nfs/toolchain/clang/amd64-full-toolchain/bin
    Build config: +assertions
    Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/11
    Found candidate GCC installation: /usr/lib/gcc/x86_64-linux-gnu/12
    Selected GCC installation: /usr/lib/gcc/x86_64-linux-gnu/12
    Candidate multilib: .;@m64
    Selected multilib: .;@m64

#### 测试结果

    time make -C /develop/toolchain/clang/linux -skj40 ARCH=x86_64 HOSTCFLAGS=-w KCFLAGS=-w KCPPFLAGS=-w LLVM=/develop/storage/nfs/toolchain/clang/amd64-full-toolchain/bin/ O=/develop/toolchain/clang/out defconfig all
    
    ________________________________________________________
    Executed in   55.16 secs    fish           external
       usr time   20.03 mins    0.00 millis   20.03 mins
       sys time    2.66 mins   12.25 millis    2.66 mins

## 分支说明

### amd64-kernel-arm
- 此分支的工具链针对AArch32、AArch64 和x86架构，运行于x86-64架构的主机，主要用于内核编译。
- 此分支的工具链不适用于裸机开发以外的任何东西，它在构建时并未考虑到对任何libc或用户空间开发的支持。
- 此分支的工具链使用基于glibc 2.35的Ubuntu 22.04 LTS构建，无法保证比该版本更旧的发行版的兼容性。不支持其他 libc 实现（例如 musl）。

#### 支持的目标架构
    aarch64    - AArch64 (little endian)
    aarch64_32 - AArch64 (little endian ILP32)
    aarch64_be - AArch64 (big endian)
    arm        - ARM
    arm64      - ARM64 (little endian)
    arm64_32   - ARM64 (little endian ILP32)
    armeb      - ARM (big endian)
    thumb      - Thumb
    thumbeb    - Thumb (big endian)
    x86        - 32-bit X86: Pentium-Pro and above
    x86-64     - 64-bit X86: EM64T and AMD64

#### 目录结构
	amd64-kernel-arm/
		aarch64-linux-gnu/
		arm-linux-gnueabi/
		bin/
		include/
		lib/
		share/
		x86_64-pc-linux-gnu/

### amd64-kernel-arm_static
- 此分支的工具链针对AArch32、AArch64 和x86架构，运行于x86-64架构的主机，主要用于内核编译。
- 此分支的工具链不适用于裸机开发以外的任何东西，它在构建时并未考虑到对任何libc或用户空间开发的支持。
- 此分支的工具链使用基于glibc 2.35的Ubuntu 22.04 LTS构建，二进制文件采用静态链接。

#### 支持的目标架构
    aarch64    - AArch64 (little endian)
    aarch64_32 - AArch64 (little endian ILP32)
    aarch64_be - AArch64 (big endian)
    arm        - ARM
    arm64      - ARM64 (little endian)
    arm64_32   - ARM64 (little endian ILP32)
    armeb      - ARM (big endian)
    thumb      - Thumb
    thumbeb    - Thumb (big endian)
    x86        - 32-bit X86: Pentium-Pro and above
    x86-64     - 64-bit X86: EM64T and AMD64

#### 目录结构
	amd64-kernel-arm/
		aarch64-linux-gnu/
		arm-linux-gnueabi/
		bin/
		include/
		lib/
		share/
		x86_64-pc-linux-gnu/

### amd64-full-toolchain
- 此分支的工具链是一个完整LLVM项目的预构建，运行于x86-64架构的主机。
- 此分支的工具链使用基于glibc 2.35的Ubuntu 22.04 LTS构建，无法保证比该版本更旧的发行版的兼容性。不支持其他 libc 实现（例如 musl）。

#### 支持的目标架构
    aarch64    - AArch64 (little endian)
    aarch64_32 - AArch64 (little endian ILP32)
    aarch64_be - AArch64 (big endian)
    amdgcn     - AMD GCN GPUs
    arm        - ARM
    arm64      - ARM64 (little endian)
    arm64_32   - ARM64 (little endian ILP32)
    armeb      - ARM (big endian)
    avr        - Atmel AVR Microcontroller
    bpf        - BPF (host endian)
    bpfeb      - BPF (big endian)
    bpfel      - BPF (little endian)
    hexagon    - Hexagon
    lanai      - Lanai
    mips       - MIPS (32-bit big endian)
    mips64     - MIPS (64-bit big endian)
    mips64el   - MIPS (64-bit little endian)
    mipsel     - MIPS (32-bit little endian)
    msp430     - MSP430 [experimental]
    nvptx      - NVIDIA PTX 32-bit
    nvptx64    - NVIDIA PTX 64-bit
    ppc32      - PowerPC 32
    ppc32le    - PowerPC 32 LE
    ppc64      - PowerPC 64
    ppc64le    - PowerPC 64 LE
    r600       - AMD GPUs HD2XXX-HD6XXX
    riscv32    - 32-bit RISC-V
    riscv64    - 64-bit RISC-V
    sparc      - Sparc
    sparcel    - Sparc LE
    sparcv9    - Sparc V9
    systemz    - SystemZ
    thumb      - Thumb
    thumbeb    - Thumb (big endian)
    ve         - VE
    wasm32     - WebAssembly 32-bit
    wasm64     - WebAssembly 64-bit
    x86        - 32-bit X86: Pentium-Pro and above
    x86-64     - 64-bit X86: EM64T and AMD64
    xcore      - XCore

#### 目录结构
	amd64-full-toolchain/
		aarch64-linux-gnu/
		arm-linux-gnueabi/
		bin/
		etc/
		include/
		lib/
		libexec/
		powerpc64le-linux-gnu/
		powerpc64-linux-gnu/
		powerpc-linux-gnu/
		s390x-linux-gnu/
		share/
		x86_64-pc-linux-gnu/

