# 阿菌•未霜 Clang/LLVM Toolchain with Binutils
这是一个预构建的工具链，构建始终来自最新的[LLVM](https://llvm.org/ "LLVM")和[Binutils](https://www.gnu.org/software/binutils/ "Binutils")源而不是稳定版本，因此无法保证完全的稳定性。它是用Full LTO、PGO和BOLT构建的，以尽可能减少编译时间。

## 分支说明

### amd64-kernel-arm
- 此分支的工具链针对AArch32、AArch64 和x86架构，运行于x86-64架构的主机，主要用于内核编译。
- 此分支的工具链不适用于裸机开发以外的任何东西，它在构建时并未考虑到对任何libc或用户空间开发的支持。
- 此分支的工具链使用基于glibc 2.31的Ubuntu 20.04 LTS构建，无法保证比该版本更旧的发行版的兼容性。不支持其他 libc 实现（例如 musl）。

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

#### 目录结构
	amd64-full-toolchain/
		aarch64-linux-gnu/
		arm-linux-gnueabi/
		bin/
		include/
		lib/
		libexec/
		mipsel-linux-gnu/
		mips-linux-gnu/
		powerpc64le-linux-gnu/
		powerpc64-linux-gnu/
		powerpc-linux-gnu/
		riscv64-linux-gnu/
		s390x-linux-gnu/
		share/
		x86_64-pc-linux-gnu/

### arm64-full-toolchain
- 此分支的工具链是一个完整LLVM项目的预构建，运行于AArch64架构的主机。
- 此分支的工具链使用基于glibc 2.35的Ubuntu 22.04 LTS构建，无法保证比该版本更旧的发行版的兼容性。不支持其他 libc 实现（例如 musl）。

#### 目录结构
	amd64-full-toolchain/
		aarch64-unknown-linux-gnu/
		arm-linux-gnueabi/
		bin/
		include/
		lib/
		libexec/
		mipsel-linux-gnu/
		mips-linux-gnu/
		powerpc64le-linux-gnu/
		powerpc64-linux-gnu/
		powerpc-linux-gnu/
		riscv64-linux-gnu/
		s390x-linux-gnu/
		share/
		x86_64-linux-gnu/

## 仓库地址
[GitHub](https://github.com/Mandi-Sa/clang "GitHub")：仅用于发布预构建的压缩文件（\*.7z）

[Gitea](https://gitea.com/Mandi-Sa/clang "Gitea")：仅用于存储预构建的二进制文件（Current AR Archive、ELF 64-bit LSB shared object存储在LFS）
