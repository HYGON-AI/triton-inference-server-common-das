<!--
# Copyright (c) 2020, NVIDIA CORPORATION. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#  * Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
#  * Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in the
#    documentation and/or other materials provided with the distribution.
#  * Neither the name of NVIDIA CORPORATION nor the names of its
#    contributors may be used to endorse or promote products derived
#    from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
# EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
# PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
# CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
# EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
# PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
# PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
# OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
-->

[![License](https://img.shields.io/badge/License-BSD3-lightgrey.svg)](https://opensource.org/licenses/BSD-3-Clause)

# Triton Inference Server Common

Common source, scripts and utilities shared across all Triton
repositories.

This repo is not typically built directly but is instead included in
the build of other repos. To build directly first install the required
dependencies.

```
$ apt-get install rapidjson-dev
```

Use cmake 3.17 or later to build and install in a local directory.

```
$ mkdir build
$ cd build
$ cmake -DCMAKE_INSTALL_PREFIX:PATH=`pwd`/install ..
$ make install
```

## Upstream

This repository is based on the Triton Inference Server Common repository.
The upstream baseline is fixed as follows:

- Upstream repository: https://github.com/triton-inference-server/common
- Upstream branch: `r26.09`
- Fixed upstream commit: `cc0e85d0cb0ec1e8abe1f57424eeb81ff1e1e671`
- Upstream license: BSD-3-Clause, see [LICENSE](LICENSE). The LICENSE text and
  all upstream copyright notices are carried unchanged.

Modified by Hygon Information Technology Co., Ltd.

## Third-Party Notices

This repository does not vendor any third-party source tree. If any third-party
component is introduced later, its provenance, license, and local path must be
registered in `THIRD_PARTY_NOTICES.md` in the repository root.

## HYGON Contributions

HYGON adapted `include/triton/common/nvtx.h` for HCU devices:

- Added HCU/HIP tracing support: when the code is built with `USE_HCU`, NVTX
  range push/pop and markers are implemented with the HIP roctx (roctracer)
  API (`roctxRangePush`, `roctxRangePop`, `roctxMark`) instead of the NVIDIA
  NVTX API.
- Preserved the original external interface of the file unchanged: `NvtxRange`
  construction/destruction semantics, the `NVTX_RANGE`, `NVTX_MARKER`, and
  `NVTX_INITIALIZE` macros, and the color constants keep the same behavior,
  so callers work unchanged on both NVIDIA and HCU platforms.
- Kept the non-HCU path unchanged: the original NVIDIA NVTX implementation,
  including the `detail::NvtxTritonDomain` helper and
  `nvtxDomainRangePushEx`/`nvtxDomainRangePop` calls, is untouched and all
  HCU changes are gated behind the `USE_HCU` compile-time option.

The original NVIDIA copyright notice in the modified file is preserved.

