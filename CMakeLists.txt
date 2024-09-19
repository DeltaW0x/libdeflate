#
# SPDX-FileCopyrightText: Copyright (c) 2020, 2021, 2022 NVIDIA CORPORATION & AFFILIATES. All rights reserved.
# SPDX-FileCopyrightText: Copyright (c) Microsoft Corporation. All rights reserved.
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#


set(SOURCES
  lib/adler32.c
  lib/crc32.c
  lib/deflate_compress.c
  lib/deflate_decompress.c
  lib/gdeflate_compress.c
  lib/gdeflate_decompress.c
  lib/gzip_compress.c
  lib/gzip_decompress.c
  lib/utils.c
  lib/zlib_compress.c
  lib/zlib_decompress.c
  lib/x86/cpu_features.c
  lib_gdeflate/GDeflateCompress.cpp
  lib_gdeflate/GDeflateDecompress.cpp
)

set(HEADERS
  lib/adler32_vec_template.h
  lib/bt_matchfinder.h
  lib/crc32_table.h
  lib/crc32_vec_template.h
  lib/decompress_template.h
  lib/gdeflate_decompress_template.h
  lib/deflate_compress.h
  lib/deflate_constants.h
  lib/gzip_constants.h
  lib/hc_matchfinder.h
  lib/lib_common.h
  lib/matchfinder_common.h
  lib/unaligned.h
  lib/zlib_constants.h
  lib/x86/adler32_impl.h
  lib/x86/cpu_features.h
  lib/x86/crc32_impl.h
  lib/x86/crc32_pclmul_template.h
  lib/x86/decompress_impl.h
  lib/x86/matchfinder_impl.h
  lib_gdeflate/config.h 
  lib_gdeflate/TileStream.h 
  libdeflate/lib_gdeflate/Utils.h
)

set(PUBLIC_HEADERS
    libdeflate.h
    GDeflate.h
)

if (WIN32)
# The follow warnings have been disabled to be able to compile this library without making modifications
# to the original source.
#  C4244 'return': conversion from 'uint64_t' to 'unsigned int', possible loss of data - compiler_msc.h 68
#  C4127 conditional expression is constant - common_defs.h 290 
#  C4267 'function': conversion from 'size_t' to 'uint32_t', possible loss of data - common_defs.h  291 
#  C4100 'c': unreferenced formal parameter - deflate_compress.c    2428    
#  C4245 '=': conversion from 'int' to 'unsigned int', signed/unsigned mismatch  - deflate_decompress.c 752 
#  C4456 declaration of 'len' hides previous local declaration - deflate_compress.c 1118    
#  C4018 '>=': signed/unsigned mismatch - decompress_template.h 297 
#  C4146 unary minus operator applied to unsigned type, result still unsigned - bt_matchfinder.h    219 
#  C4310 cast truncates constant value - bt_matchfinder.h   219 
add_compile_options(/wd4244 /wd4127 /wd4267 /wd4100 /wd4245 /wd4456 /wd4018 /wd4146 /wd4310)
endif (WIN32)

include_directories(.)
add_library(libdeflate_static STATIC ${SOURCES} ${HEADERS} ${PUBLIC_HEADERS})
target_include_directories(libdeflate_static PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(libdeflate_static PROPERTIES
  OUTPUT_NAME deflate
  PUBLIC_HEADER libdeflate.h GDeflate.h)