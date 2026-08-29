cmake_minimum_required(VERSION 3.16)
project(Lufia___The_Legend_Returns__USA_ C CXX)

include(CheckIPOSupported)

# Set C standard
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(GBRECOMP_ENABLE_PERFORMANCE_COUNTERS "Compile generated/runtime performance counters" OFF)

set(GBRECOMP_GENERATED_COMPILE_JOBS "8" CACHE STRING "Maximum concurrent generated-source compiles under Ninja; 0 disables the pool")

# Migrate stale generated-project cache entries when the default profile changes.
set(GBRECOMP_PROFILE_CACHE_VERSION "2")
set(GBRECOMP_APPLIED_PROFILE_CACHE_VERSION "0" CACHE STRING "Internal generated-project profile version")
if(NOT GBRECOMP_APPLIED_PROFILE_CACHE_VERSION STREQUAL GBRECOMP_PROFILE_CACHE_VERSION)
    if(NOT DEFINED GBRECOMP_ENABLE_IPO)
        set(GBRECOMP_ENABLE_IPO OFF CACHE BOOL "Enable interprocedural optimization/LTO for non-Debug builds" FORCE)
    endif()
    if(NOT DEFINED GBRECOMP_ENABLE_STRIP)
        set(GBRECOMP_ENABLE_STRIP ON CACHE BOOL "Strip symbols from generated executables in non-Debug builds" FORCE)
    endif()
    if(NOT DEFINED GBRECOMP_GENERATED_OPT_LEVEL)
        set(GBRECOMP_GENERATED_OPT_LEVEL "1" CACHE STRING "Optimization level for the generated ROM source files" FORCE)
    endif()
    set(GBRECOMP_APPLIED_PROFILE_CACHE_VERSION "${GBRECOMP_PROFILE_CACHE_VERSION}" CACHE STRING "Internal generated-project profile version" FORCE)
endif()

# Default to a smaller, iteration-friendly profile for generated projects.
# Benchmark and release builds can still opt into Release/-O3/IPO explicitly.
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE MinSizeRel CACHE STRING "Build type" FORCE)
endif()
if(CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
    if(CMAKE_BUILD_TYPE STREQUAL "Debug")
        add_compile_options(-O0 -g)
    elseif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
        add_compile_options(-O2 -g)
    elseif(CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
        add_compile_options(-Os)
    else()
        add_compile_options(-O3)
    endif()
    add_compile_options(-ffunction-sections -fdata-sections)
endif()

# Self-contained runtime snapshot embedded by gbrecomp
set(GBRT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/runtime")

# Find SDL2
find_package(SDL2 REQUIRED)

# Create runtime library with PPU and platform support
add_library(gbrt STATIC
    ${GBRT_DIR}/src/gbrt.c
    ${GBRT_DIR}/src/gbrt_data_mod.c
    ${GBRT_DIR}/src/gbrt_hash.c
    ${GBRT_DIR}/src/gbrt_host_configuration.c
    ${GBRT_DIR}/src/gbrt_port.c
    ${GBRT_DIR}/src/gbrt_presentation.c
    ${GBRT_DIR}/src/gbrt_semantic.c
    ${GBRT_DIR}/src/differential.c
    ${GBRT_DIR}/src/ppu.c
    ${GBRT_DIR}/src/audio.c
    ${GBRT_DIR}/src/audio_stats.c
    ${GBRT_DIR}/src/interpreter.c
    ${GBRT_DIR}/src/platform_sdl.cpp
)

# Vendored Dear ImGui subset
target_sources(gbrt PRIVATE
    ${GBRT_DIR}/vendor/imgui/imgui.cpp
    ${GBRT_DIR}/vendor/imgui/imgui_draw.cpp
    ${GBRT_DIR}/vendor/imgui/imgui_tables.cpp
    ${GBRT_DIR}/vendor/imgui/imgui_widgets.cpp
    ${GBRT_DIR}/vendor/imgui/backends/imgui_impl_sdl2.cpp
    ${GBRT_DIR}/vendor/imgui/backends/imgui_impl_sdlrenderer2.cpp
)
target_include_directories(gbrt PUBLIC 
    ${GBRT_DIR}/include
    ${GBRT_DIR}/vendor/imgui
)
target_link_libraries(gbrt PUBLIC SDL2::SDL2)
target_compile_definitions(gbrt PUBLIC GB_HAS_SDL2)

if(GBRECOMP_ENABLE_PERFORMANCE_COUNTERS)
    target_compile_definitions(gbrt PUBLIC GBRT_ENABLE_PERFORMANCE_COUNTERS)
endif()

# Main executable
add_executable(Lufia___The_Legend_Returns__USA_
    Lufia___The_Legend_Returns__USA__main.c
    Lufia___The_Legend_Returns__USA_.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_0.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_1.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_2.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_3.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_4.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_5.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_6.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_7.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_8.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_9.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_10.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_11.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_12.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_13.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_14.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_15.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_16.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_17.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_18.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_19.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_20.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_21.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_22.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_23.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_24.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_25.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_26.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_27.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_28.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_29.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_30.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_31.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_32.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_33.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_34.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_35.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_36.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_37.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_38.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_39.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_40.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_41.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_42.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_43.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_44.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_45.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_46.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_47.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_48.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_49.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_50.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_51.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_52.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_53.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_54.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_55.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_56.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_57.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_58.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_59.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_60.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_61.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_62.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_63.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_64.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_65.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_66.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_67.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_68.c
    Lufia___The_Legend_Returns__USA__funcs_0.c
    Lufia___The_Legend_Returns__USA__funcs_1.c
    Lufia___The_Legend_Returns__USA__funcs_2.c
    Lufia___The_Legend_Returns__USA__funcs_3.c
    Lufia___The_Legend_Returns__USA__funcs_4.c
    Lufia___The_Legend_Returns__USA__funcs_5.c
    Lufia___The_Legend_Returns__USA__funcs_6.c
    Lufia___The_Legend_Returns__USA__funcs_7.c
    Lufia___The_Legend_Returns__USA__funcs_8.c
    Lufia___The_Legend_Returns__USA__funcs_9.c
    Lufia___The_Legend_Returns__USA__funcs_10.c
    Lufia___The_Legend_Returns__USA__funcs_11.c
    Lufia___The_Legend_Returns__USA__funcs_12.c
    Lufia___The_Legend_Returns__USA__funcs_13.c
    Lufia___The_Legend_Returns__USA__funcs_14.c
    Lufia___The_Legend_Returns__USA__funcs_15.c
    Lufia___The_Legend_Returns__USA__funcs_16.c
    Lufia___The_Legend_Returns__USA__funcs_17.c
    Lufia___The_Legend_Returns__USA__funcs_18.c
    Lufia___The_Legend_Returns__USA__funcs_19.c
    Lufia___The_Legend_Returns__USA__funcs_20.c
    Lufia___The_Legend_Returns__USA__funcs_21.c
    Lufia___The_Legend_Returns__USA__funcs_22.c
    Lufia___The_Legend_Returns__USA__funcs_23.c
    Lufia___The_Legend_Returns__USA__funcs_24.c
    Lufia___The_Legend_Returns__USA__funcs_25.c
    Lufia___The_Legend_Returns__USA__funcs_26.c
    Lufia___The_Legend_Returns__USA__funcs_27.c
    Lufia___The_Legend_Returns__USA__funcs_28.c
    Lufia___The_Legend_Returns__USA__funcs_29.c
    Lufia___The_Legend_Returns__USA__funcs_30.c
    Lufia___The_Legend_Returns__USA__funcs_31.c
    Lufia___The_Legend_Returns__USA__funcs_32.c
    Lufia___The_Legend_Returns__USA__funcs_33.c
    Lufia___The_Legend_Returns__USA__funcs_34.c
    Lufia___The_Legend_Returns__USA__funcs_35.c
    Lufia___The_Legend_Returns__USA__funcs_36.c
    Lufia___The_Legend_Returns__USA__funcs_37.c
    Lufia___The_Legend_Returns__USA__funcs_38.c
    Lufia___The_Legend_Returns__USA__funcs_39.c
    Lufia___The_Legend_Returns__USA__funcs_40.c
    Lufia___The_Legend_Returns__USA__funcs_41.c
    Lufia___The_Legend_Returns__USA__funcs_42.c
    Lufia___The_Legend_Returns__USA__funcs_43.c
    Lufia___The_Legend_Returns__USA__funcs_44.c
    Lufia___The_Legend_Returns__USA__funcs_45.c
    Lufia___The_Legend_Returns__USA__funcs_46.c
    Lufia___The_Legend_Returns__USA__funcs_47.c
    Lufia___The_Legend_Returns__USA__funcs_48.c
    Lufia___The_Legend_Returns__USA__funcs_49.c
    Lufia___The_Legend_Returns__USA__funcs_50.c
    Lufia___The_Legend_Returns__USA__funcs_51.c
    Lufia___The_Legend_Returns__USA__funcs_52.c
    Lufia___The_Legend_Returns__USA__funcs_53.c
    Lufia___The_Legend_Returns__USA__funcs_54.c
    Lufia___The_Legend_Returns__USA__funcs_55.c
    Lufia___The_Legend_Returns__USA__funcs_56.c
    Lufia___The_Legend_Returns__USA__funcs_57.c
    Lufia___The_Legend_Returns__USA__funcs_58.c
    Lufia___The_Legend_Returns__USA__funcs_59.c
    Lufia___The_Legend_Returns__USA__funcs_60.c
    Lufia___The_Legend_Returns__USA__funcs_61.c
    Lufia___The_Legend_Returns__USA__funcs_62.c
    Lufia___The_Legend_Returns__USA__funcs_63.c
    Lufia___The_Legend_Returns__USA__funcs_64.c
    Lufia___The_Legend_Returns__USA__funcs_65.c
    Lufia___The_Legend_Returns__USA__funcs_66.c
    Lufia___The_Legend_Returns__USA__funcs_67.c
    Lufia___The_Legend_Returns__USA__funcs_68.c
    Lufia___The_Legend_Returns__USA__funcs_69.c
    Lufia___The_Legend_Returns__USA__funcs_70.c
    Lufia___The_Legend_Returns__USA__funcs_71.c
    Lufia___The_Legend_Returns__USA__funcs_72.c
    Lufia___The_Legend_Returns__USA__funcs_73.c
    Lufia___The_Legend_Returns__USA__funcs_74.c
    Lufia___The_Legend_Returns__USA__funcs_75.c
    Lufia___The_Legend_Returns__USA__funcs_76.c
    Lufia___The_Legend_Returns__USA__funcs_77.c
    Lufia___The_Legend_Returns__USA__funcs_78.c
    Lufia___The_Legend_Returns__USA__funcs_79.c
    Lufia___The_Legend_Returns__USA__funcs_80.c
    Lufia___The_Legend_Returns__USA__funcs_81.c
    Lufia___The_Legend_Returns__USA__funcs_82.c
    Lufia___The_Legend_Returns__USA__funcs_83.c
    Lufia___The_Legend_Returns__USA__funcs_84.c
    Lufia___The_Legend_Returns__USA__funcs_85.c
    Lufia___The_Legend_Returns__USA__funcs_86.c
    Lufia___The_Legend_Returns__USA__funcs_87.c
    Lufia___The_Legend_Returns__USA__funcs_88.c
    Lufia___The_Legend_Returns__USA__funcs_89.c
    Lufia___The_Legend_Returns__USA__funcs_90.c
    Lufia___The_Legend_Returns__USA__funcs_91.c
    Lufia___The_Legend_Returns__USA__funcs_92.c
    Lufia___The_Legend_Returns__USA__funcs_93.c
    Lufia___The_Legend_Returns__USA__funcs_94.c
    Lufia___The_Legend_Returns__USA__funcs_95.c
    Lufia___The_Legend_Returns__USA__funcs_96.c
    Lufia___The_Legend_Returns__USA__funcs_97.c
    Lufia___The_Legend_Returns__USA__funcs_98.c
    Lufia___The_Legend_Returns__USA__funcs_99.c
    Lufia___The_Legend_Returns__USA__funcs_100.c
    Lufia___The_Legend_Returns__USA__funcs_101.c
    Lufia___The_Legend_Returns__USA__funcs_102.c
    Lufia___The_Legend_Returns__USA__funcs_103.c
    Lufia___The_Legend_Returns__USA__funcs_104.c
    Lufia___The_Legend_Returns__USA__funcs_105.c
    Lufia___The_Legend_Returns__USA__funcs_106.c
    Lufia___The_Legend_Returns__USA__funcs_107.c
    Lufia___The_Legend_Returns__USA__funcs_108.c
    Lufia___The_Legend_Returns__USA__funcs_109.c
    Lufia___The_Legend_Returns__USA__funcs_110.c
    Lufia___The_Legend_Returns__USA__funcs_111.c
    Lufia___The_Legend_Returns__USA__funcs_112.c
    Lufia___The_Legend_Returns__USA__funcs_113.c
    Lufia___The_Legend_Returns__USA__funcs_114.c
    Lufia___The_Legend_Returns__USA__funcs_115.c
    Lufia___The_Legend_Returns__USA__funcs_116.c
    Lufia___The_Legend_Returns__USA__funcs_117.c
    Lufia___The_Legend_Returns__USA__funcs_118.c
    Lufia___The_Legend_Returns__USA__funcs_119.c
    Lufia___The_Legend_Returns__USA__funcs_120.c
    Lufia___The_Legend_Returns__USA__funcs_121.c
    Lufia___The_Legend_Returns__USA__funcs_122.c
    Lufia___The_Legend_Returns__USA__funcs_123.c
    Lufia___The_Legend_Returns__USA__funcs_124.c
    Lufia___The_Legend_Returns__USA__funcs_125.c
    Lufia___The_Legend_Returns__USA__funcs_126.c
    Lufia___The_Legend_Returns__USA__funcs_127.c
    Lufia___The_Legend_Returns__USA__funcs_128.c
    Lufia___The_Legend_Returns__USA__funcs_129.c
    Lufia___The_Legend_Returns__USA__funcs_130.c
    Lufia___The_Legend_Returns__USA__funcs_131.c
    Lufia___The_Legend_Returns__USA__funcs_132.c
    Lufia___The_Legend_Returns__USA__funcs_133.c
    Lufia___The_Legend_Returns__USA__funcs_134.c
    Lufia___The_Legend_Returns__USA__funcs_135.c
    Lufia___The_Legend_Returns__USA__funcs_136.c
    Lufia___The_Legend_Returns__USA__funcs_137.c
    Lufia___The_Legend_Returns__USA__funcs_138.c
    Lufia___The_Legend_Returns__USA__funcs_139.c
    Lufia___The_Legend_Returns__USA__funcs_140.c
    Lufia___The_Legend_Returns__USA__funcs_141.c
    Lufia___The_Legend_Returns__USA__funcs_142.c
    Lufia___The_Legend_Returns__USA__funcs_143.c
    Lufia___The_Legend_Returns__USA__funcs_144.c
    Lufia___The_Legend_Returns__USA__funcs_145.c
    Lufia___The_Legend_Returns__USA__funcs_146.c
    Lufia___The_Legend_Returns__USA__funcs_147.c
    Lufia___The_Legend_Returns__USA__funcs_148.c
    Lufia___The_Legend_Returns__USA__funcs_149.c
    Lufia___The_Legend_Returns__USA__funcs_150.c
    Lufia___The_Legend_Returns__USA__funcs_151.c
    Lufia___The_Legend_Returns__USA__funcs_152.c
    Lufia___The_Legend_Returns__USA__funcs_153.c
    Lufia___The_Legend_Returns__USA__funcs_154.c
    Lufia___The_Legend_Returns__USA__funcs_155.c
    Lufia___The_Legend_Returns__USA__funcs_156.c
    Lufia___The_Legend_Returns__USA__funcs_157.c
    Lufia___The_Legend_Returns__USA__funcs_158.c
    Lufia___The_Legend_Returns__USA__funcs_159.c
    Lufia___The_Legend_Returns__USA__funcs_160.c
    Lufia___The_Legend_Returns__USA__funcs_161.c
    Lufia___The_Legend_Returns__USA__funcs_162.c
    Lufia___The_Legend_Returns__USA__funcs_163.c
    Lufia___The_Legend_Returns__USA__funcs_164.c
    Lufia___The_Legend_Returns__USA__funcs_165.c
    Lufia___The_Legend_Returns__USA__funcs_166.c
    Lufia___The_Legend_Returns__USA__funcs_167.c
    Lufia___The_Legend_Returns__USA__funcs_168.c
    Lufia___The_Legend_Returns__USA__funcs_169.c
    Lufia___The_Legend_Returns__USA__funcs_170.c
    Lufia___The_Legend_Returns__USA__funcs_171.c
    Lufia___The_Legend_Returns__USA__funcs_172.c
    Lufia___The_Legend_Returns__USA__funcs_173.c
    Lufia___The_Legend_Returns__USA__funcs_174.c
    Lufia___The_Legend_Returns__USA__funcs_175.c
    Lufia___The_Legend_Returns__USA__funcs_176.c
    Lufia___The_Legend_Returns__USA__funcs_177.c
    Lufia___The_Legend_Returns__USA__funcs_178.c
    Lufia___The_Legend_Returns__USA__funcs_179.c
    Lufia___The_Legend_Returns__USA__funcs_180.c
    Lufia___The_Legend_Returns__USA__funcs_181.c
    Lufia___The_Legend_Returns__USA__funcs_182.c
    Lufia___The_Legend_Returns__USA__funcs_183.c
    Lufia___The_Legend_Returns__USA__funcs_184.c
    Lufia___The_Legend_Returns__USA__funcs_185.c
    Lufia___The_Legend_Returns__USA__funcs_186.c
    Lufia___The_Legend_Returns__USA__funcs_187.c
    Lufia___The_Legend_Returns__USA__funcs_188.c
    Lufia___The_Legend_Returns__USA__funcs_189.c
    Lufia___The_Legend_Returns__USA__funcs_190.c
    Lufia___The_Legend_Returns__USA__funcs_191.c
    Lufia___The_Legend_Returns__USA__funcs_192.c
    Lufia___The_Legend_Returns__USA__funcs_193.c
    Lufia___The_Legend_Returns__USA__funcs_194.c
    Lufia___The_Legend_Returns__USA__funcs_195.c
    Lufia___The_Legend_Returns__USA__funcs_196.c
    Lufia___The_Legend_Returns__USA__funcs_197.c
    Lufia___The_Legend_Returns__USA__funcs_198.c
    Lufia___The_Legend_Returns__USA__funcs_199.c
    Lufia___The_Legend_Returns__USA__funcs_200.c
    Lufia___The_Legend_Returns__USA__funcs_201.c
    Lufia___The_Legend_Returns__USA__funcs_202.c
    Lufia___The_Legend_Returns__USA__funcs_203.c
    Lufia___The_Legend_Returns__USA__funcs_204.c
    Lufia___The_Legend_Returns__USA__funcs_205.c
    Lufia___The_Legend_Returns__USA__funcs_206.c
    Lufia___The_Legend_Returns__USA__funcs_207.c
    Lufia___The_Legend_Returns__USA__funcs_208.c
    Lufia___The_Legend_Returns__USA__funcs_209.c
    Lufia___The_Legend_Returns__USA__funcs_210.c
    Lufia___The_Legend_Returns__USA__funcs_211.c
    Lufia___The_Legend_Returns__USA__funcs_212.c
    Lufia___The_Legend_Returns__USA__funcs_213.c
    Lufia___The_Legend_Returns__USA__funcs_214.c
    Lufia___The_Legend_Returns__USA__funcs_215.c
    Lufia___The_Legend_Returns__USA__funcs_216.c
    Lufia___The_Legend_Returns__USA__funcs_217.c
    Lufia___The_Legend_Returns__USA__funcs_218.c
    Lufia___The_Legend_Returns__USA__funcs_219.c
    Lufia___The_Legend_Returns__USA__funcs_220.c
    Lufia___The_Legend_Returns__USA__funcs_221.c
    Lufia___The_Legend_Returns__USA__funcs_222.c
    Lufia___The_Legend_Returns__USA__funcs_223.c
    Lufia___The_Legend_Returns__USA__funcs_224.c
    Lufia___The_Legend_Returns__USA__funcs_225.c
    Lufia___The_Legend_Returns__USA__funcs_226.c
    Lufia___The_Legend_Returns__USA__funcs_227.c
    Lufia___The_Legend_Returns__USA__funcs_228.c
    Lufia___The_Legend_Returns__USA__funcs_229.c
    Lufia___The_Legend_Returns__USA__funcs_230.c
    Lufia___The_Legend_Returns__USA__funcs_231.c
    Lufia___The_Legend_Returns__USA__funcs_232.c
    Lufia___The_Legend_Returns__USA__funcs_233.c
    Lufia___The_Legend_Returns__USA__funcs_234.c
    Lufia___The_Legend_Returns__USA__funcs_235.c
    Lufia___The_Legend_Returns__USA__funcs_236.c
    Lufia___The_Legend_Returns__USA__funcs_237.c
    Lufia___The_Legend_Returns__USA__funcs_238.c
    Lufia___The_Legend_Returns__USA__funcs_239.c
    Lufia___The_Legend_Returns__USA__funcs_240.c
    Lufia___The_Legend_Returns__USA__funcs_241.c
    Lufia___The_Legend_Returns__USA__funcs_242.c
    Lufia___The_Legend_Returns__USA__funcs_243.c
    Lufia___The_Legend_Returns__USA__funcs_244.c
    Lufia___The_Legend_Returns__USA__funcs_245.c
    Lufia___The_Legend_Returns__USA__funcs_246.c
    Lufia___The_Legend_Returns__USA__funcs_247.c
    Lufia___The_Legend_Returns__USA__funcs_248.c
    Lufia___The_Legend_Returns__USA__funcs_249.c
    Lufia___The_Legend_Returns__USA__funcs_250.c
    Lufia___The_Legend_Returns__USA__funcs_251.c
    Lufia___The_Legend_Returns__USA__funcs_252.c
    Lufia___The_Legend_Returns__USA__funcs_253.c
    Lufia___The_Legend_Returns__USA__funcs_254.c
    Lufia___The_Legend_Returns__USA__funcs_255.c
    Lufia___The_Legend_Returns__USA__funcs_256.c
    Lufia___The_Legend_Returns__USA__funcs_257.c
    Lufia___The_Legend_Returns__USA__funcs_258.c
    Lufia___The_Legend_Returns__USA__funcs_259.c
    Lufia___The_Legend_Returns__USA__funcs_260.c
    Lufia___The_Legend_Returns__USA__funcs_261.c
    Lufia___The_Legend_Returns__USA__funcs_262.c
    Lufia___The_Legend_Returns__USA__funcs_263.c
    Lufia___The_Legend_Returns__USA__funcs_264.c
    Lufia___The_Legend_Returns__USA__funcs_265.c
    Lufia___The_Legend_Returns__USA__funcs_266.c
    Lufia___The_Legend_Returns__USA__funcs_267.c
    Lufia___The_Legend_Returns__USA__funcs_268.c
    Lufia___The_Legend_Returns__USA__funcs_269.c
    Lufia___The_Legend_Returns__USA__funcs_270.c
    Lufia___The_Legend_Returns__USA__funcs_271.c
    Lufia___The_Legend_Returns__USA__funcs_272.c
    Lufia___The_Legend_Returns__USA__funcs_273.c
    Lufia___The_Legend_Returns__USA__funcs_274.c
    Lufia___The_Legend_Returns__USA__funcs_275.c
    Lufia___The_Legend_Returns__USA__funcs_276.c
    Lufia___The_Legend_Returns__USA__funcs_277.c
    Lufia___The_Legend_Returns__USA__funcs_278.c
    Lufia___The_Legend_Returns__USA__funcs_279.c
    Lufia___The_Legend_Returns__USA__funcs_280.c
    Lufia___The_Legend_Returns__USA__funcs_281.c
    Lufia___The_Legend_Returns__USA__funcs_282.c
    Lufia___The_Legend_Returns__USA__funcs_283.c
    Lufia___The_Legend_Returns__USA__funcs_284.c
    Lufia___The_Legend_Returns__USA__funcs_285.c
    Lufia___The_Legend_Returns__USA__funcs_286.c
    Lufia___The_Legend_Returns__USA__funcs_287.c
    Lufia___The_Legend_Returns__USA__funcs_288.c
    Lufia___The_Legend_Returns__USA__funcs_289.c
    Lufia___The_Legend_Returns__USA__funcs_290.c
    Lufia___The_Legend_Returns__USA__funcs_291.c
    Lufia___The_Legend_Returns__USA__funcs_292.c
    Lufia___The_Legend_Returns__USA__funcs_293.c
    Lufia___The_Legend_Returns__USA__funcs_294.c
    Lufia___The_Legend_Returns__USA__funcs_295.c
    Lufia___The_Legend_Returns__USA__funcs_296.c
    Lufia___The_Legend_Returns__USA__funcs_297.c
    Lufia___The_Legend_Returns__USA__funcs_298.c
    Lufia___The_Legend_Returns__USA__funcs_299.c
    Lufia___The_Legend_Returns__USA__funcs_300.c
    Lufia___The_Legend_Returns__USA__funcs_301.c
    Lufia___The_Legend_Returns__USA__funcs_302.c
    Lufia___The_Legend_Returns__USA__funcs_303.c
    Lufia___The_Legend_Returns__USA__funcs_304.c
    Lufia___The_Legend_Returns__USA__funcs_305.c
    Lufia___The_Legend_Returns__USA__funcs_306.c
    Lufia___The_Legend_Returns__USA__funcs_307.c
    Lufia___The_Legend_Returns__USA__funcs_308.c
    Lufia___The_Legend_Returns__USA__funcs_309.c
    Lufia___The_Legend_Returns__USA__funcs_310.c
    Lufia___The_Legend_Returns__USA__funcs_311.c
    Lufia___The_Legend_Returns__USA__funcs_312.c
    Lufia___The_Legend_Returns__USA__funcs_313.c
    Lufia___The_Legend_Returns__USA__funcs_314.c
    Lufia___The_Legend_Returns__USA__funcs_315.c
    Lufia___The_Legend_Returns__USA__funcs_316.c
    Lufia___The_Legend_Returns__USA__funcs_317.c
    Lufia___The_Legend_Returns__USA__funcs_318.c
    Lufia___The_Legend_Returns__USA__funcs_319.c
    Lufia___The_Legend_Returns__USA__funcs_320.c
    Lufia___The_Legend_Returns__USA__funcs_321.c
    Lufia___The_Legend_Returns__USA__funcs_322.c
    Lufia___The_Legend_Returns__USA__funcs_323.c
    Lufia___The_Legend_Returns__USA__funcs_324.c
    Lufia___The_Legend_Returns__USA__funcs_325.c
    Lufia___The_Legend_Returns__USA__funcs_326.c
    Lufia___The_Legend_Returns__USA__funcs_327.c
    Lufia___The_Legend_Returns__USA__funcs_328.c
    Lufia___The_Legend_Returns__USA__funcs_329.c
    Lufia___The_Legend_Returns__USA__funcs_330.c
    Lufia___The_Legend_Returns__USA__funcs_331.c
    Lufia___The_Legend_Returns__USA__funcs_332.c
    Lufia___The_Legend_Returns__USA__funcs_333.c
    Lufia___The_Legend_Returns__USA__funcs_334.c
    Lufia___The_Legend_Returns__USA__funcs_335.c
)

# Large generated translation units are memory-intensive. Keep runtime/UI
# compilation parallel while bounding only the generated executable target.
if(CMAKE_GENERATOR MATCHES "Ninja" AND GBRECOMP_GENERATED_COMPILE_JOBS MATCHES "^[1-9][0-9]*$")
    set_property(GLOBAL APPEND PROPERTY JOB_POOLS "gbrecomp_generated_compile_pool=${GBRECOMP_GENERATED_COMPILE_JOBS}")
    set_property(TARGET Lufia___The_Legend_Returns__USA_ PROPERTY JOB_POOL_COMPILE gbrecomp_generated_compile_pool)
endif()

# Generated ROM code defaults to a smaller manual-testing profile.
# Raise the optimization level or enable IPO/LTO explicitly for benchmark/release builds.
set(GBRECOMP_GENERATED_OPT_LEVEL "1" CACHE STRING "Optimization level for the generated ROM source files")
set_property(CACHE GBRECOMP_GENERATED_OPT_LEVEL PROPERTY STRINGS 0 1 2 3)
option(GBRECOMP_ENABLE_IPO "Enable interprocedural optimization/LTO for non-Debug builds" OFF)
option(GBRECOMP_ENABLE_STRIP "Strip symbols from generated executables in non-Debug builds" ON)
set(GBRECOMP_GENERATED_SOURCES
    Lufia___The_Legend_Returns__USA_.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_0.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_1.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_2.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_3.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_4.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_5.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_6.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_7.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_8.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_9.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_10.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_11.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_12.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_13.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_14.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_15.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_16.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_17.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_18.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_19.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_20.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_21.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_22.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_23.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_24.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_25.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_26.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_27.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_28.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_29.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_30.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_31.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_32.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_33.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_34.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_35.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_36.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_37.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_38.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_39.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_40.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_41.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_42.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_43.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_44.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_45.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_46.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_47.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_48.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_49.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_50.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_51.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_52.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_53.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_54.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_55.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_56.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_57.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_58.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_59.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_60.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_61.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_62.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_63.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_64.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_65.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_66.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_67.c
    Lufia___The_Legend_Returns__USA__dispatch_chunk_68.c
    Lufia___The_Legend_Returns__USA__funcs_0.c
    Lufia___The_Legend_Returns__USA__funcs_1.c
    Lufia___The_Legend_Returns__USA__funcs_2.c
    Lufia___The_Legend_Returns__USA__funcs_3.c
    Lufia___The_Legend_Returns__USA__funcs_4.c
    Lufia___The_Legend_Returns__USA__funcs_5.c
    Lufia___The_Legend_Returns__USA__funcs_6.c
    Lufia___The_Legend_Returns__USA__funcs_7.c
    Lufia___The_Legend_Returns__USA__funcs_8.c
    Lufia___The_Legend_Returns__USA__funcs_9.c
    Lufia___The_Legend_Returns__USA__funcs_10.c
    Lufia___The_Legend_Returns__USA__funcs_11.c
    Lufia___The_Legend_Returns__USA__funcs_12.c
    Lufia___The_Legend_Returns__USA__funcs_13.c
    Lufia___The_Legend_Returns__USA__funcs_14.c
    Lufia___The_Legend_Returns__USA__funcs_15.c
    Lufia___The_Legend_Returns__USA__funcs_16.c
    Lufia___The_Legend_Returns__USA__funcs_17.c
    Lufia___The_Legend_Returns__USA__funcs_18.c
    Lufia___The_Legend_Returns__USA__funcs_19.c
    Lufia___The_Legend_Returns__USA__funcs_20.c
    Lufia___The_Legend_Returns__USA__funcs_21.c
    Lufia___The_Legend_Returns__USA__funcs_22.c
    Lufia___The_Legend_Returns__USA__funcs_23.c
    Lufia___The_Legend_Returns__USA__funcs_24.c
    Lufia___The_Legend_Returns__USA__funcs_25.c
    Lufia___The_Legend_Returns__USA__funcs_26.c
    Lufia___The_Legend_Returns__USA__funcs_27.c
    Lufia___The_Legend_Returns__USA__funcs_28.c
    Lufia___The_Legend_Returns__USA__funcs_29.c
    Lufia___The_Legend_Returns__USA__funcs_30.c
    Lufia___The_Legend_Returns__USA__funcs_31.c
    Lufia___The_Legend_Returns__USA__funcs_32.c
    Lufia___The_Legend_Returns__USA__funcs_33.c
    Lufia___The_Legend_Returns__USA__funcs_34.c
    Lufia___The_Legend_Returns__USA__funcs_35.c
    Lufia___The_Legend_Returns__USA__funcs_36.c
    Lufia___The_Legend_Returns__USA__funcs_37.c
    Lufia___The_Legend_Returns__USA__funcs_38.c
    Lufia___The_Legend_Returns__USA__funcs_39.c
    Lufia___The_Legend_Returns__USA__funcs_40.c
    Lufia___The_Legend_Returns__USA__funcs_41.c
    Lufia___The_Legend_Returns__USA__funcs_42.c
    Lufia___The_Legend_Returns__USA__funcs_43.c
    Lufia___The_Legend_Returns__USA__funcs_44.c
    Lufia___The_Legend_Returns__USA__funcs_45.c
    Lufia___The_Legend_Returns__USA__funcs_46.c
    Lufia___The_Legend_Returns__USA__funcs_47.c
    Lufia___The_Legend_Returns__USA__funcs_48.c
    Lufia___The_Legend_Returns__USA__funcs_49.c
    Lufia___The_Legend_Returns__USA__funcs_50.c
    Lufia___The_Legend_Returns__USA__funcs_51.c
    Lufia___The_Legend_Returns__USA__funcs_52.c
    Lufia___The_Legend_Returns__USA__funcs_53.c
    Lufia___The_Legend_Returns__USA__funcs_54.c
    Lufia___The_Legend_Returns__USA__funcs_55.c
    Lufia___The_Legend_Returns__USA__funcs_56.c
    Lufia___The_Legend_Returns__USA__funcs_57.c
    Lufia___The_Legend_Returns__USA__funcs_58.c
    Lufia___The_Legend_Returns__USA__funcs_59.c
    Lufia___The_Legend_Returns__USA__funcs_60.c
    Lufia___The_Legend_Returns__USA__funcs_61.c
    Lufia___The_Legend_Returns__USA__funcs_62.c
    Lufia___The_Legend_Returns__USA__funcs_63.c
    Lufia___The_Legend_Returns__USA__funcs_64.c
    Lufia___The_Legend_Returns__USA__funcs_65.c
    Lufia___The_Legend_Returns__USA__funcs_66.c
    Lufia___The_Legend_Returns__USA__funcs_67.c
    Lufia___The_Legend_Returns__USA__funcs_68.c
    Lufia___The_Legend_Returns__USA__funcs_69.c
    Lufia___The_Legend_Returns__USA__funcs_70.c
    Lufia___The_Legend_Returns__USA__funcs_71.c
    Lufia___The_Legend_Returns__USA__funcs_72.c
    Lufia___The_Legend_Returns__USA__funcs_73.c
    Lufia___The_Legend_Returns__USA__funcs_74.c
    Lufia___The_Legend_Returns__USA__funcs_75.c
    Lufia___The_Legend_Returns__USA__funcs_76.c
    Lufia___The_Legend_Returns__USA__funcs_77.c
    Lufia___The_Legend_Returns__USA__funcs_78.c
    Lufia___The_Legend_Returns__USA__funcs_79.c
    Lufia___The_Legend_Returns__USA__funcs_80.c
    Lufia___The_Legend_Returns__USA__funcs_81.c
    Lufia___The_Legend_Returns__USA__funcs_82.c
    Lufia___The_Legend_Returns__USA__funcs_83.c
    Lufia___The_Legend_Returns__USA__funcs_84.c
    Lufia___The_Legend_Returns__USA__funcs_85.c
    Lufia___The_Legend_Returns__USA__funcs_86.c
    Lufia___The_Legend_Returns__USA__funcs_87.c
    Lufia___The_Legend_Returns__USA__funcs_88.c
    Lufia___The_Legend_Returns__USA__funcs_89.c
    Lufia___The_Legend_Returns__USA__funcs_90.c
    Lufia___The_Legend_Returns__USA__funcs_91.c
    Lufia___The_Legend_Returns__USA__funcs_92.c
    Lufia___The_Legend_Returns__USA__funcs_93.c
    Lufia___The_Legend_Returns__USA__funcs_94.c
    Lufia___The_Legend_Returns__USA__funcs_95.c
    Lufia___The_Legend_Returns__USA__funcs_96.c
    Lufia___The_Legend_Returns__USA__funcs_97.c
    Lufia___The_Legend_Returns__USA__funcs_98.c
    Lufia___The_Legend_Returns__USA__funcs_99.c
    Lufia___The_Legend_Returns__USA__funcs_100.c
    Lufia___The_Legend_Returns__USA__funcs_101.c
    Lufia___The_Legend_Returns__USA__funcs_102.c
    Lufia___The_Legend_Returns__USA__funcs_103.c
    Lufia___The_Legend_Returns__USA__funcs_104.c
    Lufia___The_Legend_Returns__USA__funcs_105.c
    Lufia___The_Legend_Returns__USA__funcs_106.c
    Lufia___The_Legend_Returns__USA__funcs_107.c
    Lufia___The_Legend_Returns__USA__funcs_108.c
    Lufia___The_Legend_Returns__USA__funcs_109.c
    Lufia___The_Legend_Returns__USA__funcs_110.c
    Lufia___The_Legend_Returns__USA__funcs_111.c
    Lufia___The_Legend_Returns__USA__funcs_112.c
    Lufia___The_Legend_Returns__USA__funcs_113.c
    Lufia___The_Legend_Returns__USA__funcs_114.c
    Lufia___The_Legend_Returns__USA__funcs_115.c
    Lufia___The_Legend_Returns__USA__funcs_116.c
    Lufia___The_Legend_Returns__USA__funcs_117.c
    Lufia___The_Legend_Returns__USA__funcs_118.c
    Lufia___The_Legend_Returns__USA__funcs_119.c
    Lufia___The_Legend_Returns__USA__funcs_120.c
    Lufia___The_Legend_Returns__USA__funcs_121.c
    Lufia___The_Legend_Returns__USA__funcs_122.c
    Lufia___The_Legend_Returns__USA__funcs_123.c
    Lufia___The_Legend_Returns__USA__funcs_124.c
    Lufia___The_Legend_Returns__USA__funcs_125.c
    Lufia___The_Legend_Returns__USA__funcs_126.c
    Lufia___The_Legend_Returns__USA__funcs_127.c
    Lufia___The_Legend_Returns__USA__funcs_128.c
    Lufia___The_Legend_Returns__USA__funcs_129.c
    Lufia___The_Legend_Returns__USA__funcs_130.c
    Lufia___The_Legend_Returns__USA__funcs_131.c
    Lufia___The_Legend_Returns__USA__funcs_132.c
    Lufia___The_Legend_Returns__USA__funcs_133.c
    Lufia___The_Legend_Returns__USA__funcs_134.c
    Lufia___The_Legend_Returns__USA__funcs_135.c
    Lufia___The_Legend_Returns__USA__funcs_136.c
    Lufia___The_Legend_Returns__USA__funcs_137.c
    Lufia___The_Legend_Returns__USA__funcs_138.c
    Lufia___The_Legend_Returns__USA__funcs_139.c
    Lufia___The_Legend_Returns__USA__funcs_140.c
    Lufia___The_Legend_Returns__USA__funcs_141.c
    Lufia___The_Legend_Returns__USA__funcs_142.c
    Lufia___The_Legend_Returns__USA__funcs_143.c
    Lufia___The_Legend_Returns__USA__funcs_144.c
    Lufia___The_Legend_Returns__USA__funcs_145.c
    Lufia___The_Legend_Returns__USA__funcs_146.c
    Lufia___The_Legend_Returns__USA__funcs_147.c
    Lufia___The_Legend_Returns__USA__funcs_148.c
    Lufia___The_Legend_Returns__USA__funcs_149.c
    Lufia___The_Legend_Returns__USA__funcs_150.c
    Lufia___The_Legend_Returns__USA__funcs_151.c
    Lufia___The_Legend_Returns__USA__funcs_152.c
    Lufia___The_Legend_Returns__USA__funcs_153.c
    Lufia___The_Legend_Returns__USA__funcs_154.c
    Lufia___The_Legend_Returns__USA__funcs_155.c
    Lufia___The_Legend_Returns__USA__funcs_156.c
    Lufia___The_Legend_Returns__USA__funcs_157.c
    Lufia___The_Legend_Returns__USA__funcs_158.c
    Lufia___The_Legend_Returns__USA__funcs_159.c
    Lufia___The_Legend_Returns__USA__funcs_160.c
    Lufia___The_Legend_Returns__USA__funcs_161.c
    Lufia___The_Legend_Returns__USA__funcs_162.c
    Lufia___The_Legend_Returns__USA__funcs_163.c
    Lufia___The_Legend_Returns__USA__funcs_164.c
    Lufia___The_Legend_Returns__USA__funcs_165.c
    Lufia___The_Legend_Returns__USA__funcs_166.c
    Lufia___The_Legend_Returns__USA__funcs_167.c
    Lufia___The_Legend_Returns__USA__funcs_168.c
    Lufia___The_Legend_Returns__USA__funcs_169.c
    Lufia___The_Legend_Returns__USA__funcs_170.c
    Lufia___The_Legend_Returns__USA__funcs_171.c
    Lufia___The_Legend_Returns__USA__funcs_172.c
    Lufia___The_Legend_Returns__USA__funcs_173.c
    Lufia___The_Legend_Returns__USA__funcs_174.c
    Lufia___The_Legend_Returns__USA__funcs_175.c
    Lufia___The_Legend_Returns__USA__funcs_176.c
    Lufia___The_Legend_Returns__USA__funcs_177.c
    Lufia___The_Legend_Returns__USA__funcs_178.c
    Lufia___The_Legend_Returns__USA__funcs_179.c
    Lufia___The_Legend_Returns__USA__funcs_180.c
    Lufia___The_Legend_Returns__USA__funcs_181.c
    Lufia___The_Legend_Returns__USA__funcs_182.c
    Lufia___The_Legend_Returns__USA__funcs_183.c
    Lufia___The_Legend_Returns__USA__funcs_184.c
    Lufia___The_Legend_Returns__USA__funcs_185.c
    Lufia___The_Legend_Returns__USA__funcs_186.c
    Lufia___The_Legend_Returns__USA__funcs_187.c
    Lufia___The_Legend_Returns__USA__funcs_188.c
    Lufia___The_Legend_Returns__USA__funcs_189.c
    Lufia___The_Legend_Returns__USA__funcs_190.c
    Lufia___The_Legend_Returns__USA__funcs_191.c
    Lufia___The_Legend_Returns__USA__funcs_192.c
    Lufia___The_Legend_Returns__USA__funcs_193.c
    Lufia___The_Legend_Returns__USA__funcs_194.c
    Lufia___The_Legend_Returns__USA__funcs_195.c
    Lufia___The_Legend_Returns__USA__funcs_196.c
    Lufia___The_Legend_Returns__USA__funcs_197.c
    Lufia___The_Legend_Returns__USA__funcs_198.c
    Lufia___The_Legend_Returns__USA__funcs_199.c
    Lufia___The_Legend_Returns__USA__funcs_200.c
    Lufia___The_Legend_Returns__USA__funcs_201.c
    Lufia___The_Legend_Returns__USA__funcs_202.c
    Lufia___The_Legend_Returns__USA__funcs_203.c
    Lufia___The_Legend_Returns__USA__funcs_204.c
    Lufia___The_Legend_Returns__USA__funcs_205.c
    Lufia___The_Legend_Returns__USA__funcs_206.c
    Lufia___The_Legend_Returns__USA__funcs_207.c
    Lufia___The_Legend_Returns__USA__funcs_208.c
    Lufia___The_Legend_Returns__USA__funcs_209.c
    Lufia___The_Legend_Returns__USA__funcs_210.c
    Lufia___The_Legend_Returns__USA__funcs_211.c
    Lufia___The_Legend_Returns__USA__funcs_212.c
    Lufia___The_Legend_Returns__USA__funcs_213.c
    Lufia___The_Legend_Returns__USA__funcs_214.c
    Lufia___The_Legend_Returns__USA__funcs_215.c
    Lufia___The_Legend_Returns__USA__funcs_216.c
    Lufia___The_Legend_Returns__USA__funcs_217.c
    Lufia___The_Legend_Returns__USA__funcs_218.c
    Lufia___The_Legend_Returns__USA__funcs_219.c
    Lufia___The_Legend_Returns__USA__funcs_220.c
    Lufia___The_Legend_Returns__USA__funcs_221.c
    Lufia___The_Legend_Returns__USA__funcs_222.c
    Lufia___The_Legend_Returns__USA__funcs_223.c
    Lufia___The_Legend_Returns__USA__funcs_224.c
    Lufia___The_Legend_Returns__USA__funcs_225.c
    Lufia___The_Legend_Returns__USA__funcs_226.c
    Lufia___The_Legend_Returns__USA__funcs_227.c
    Lufia___The_Legend_Returns__USA__funcs_228.c
    Lufia___The_Legend_Returns__USA__funcs_229.c
    Lufia___The_Legend_Returns__USA__funcs_230.c
    Lufia___The_Legend_Returns__USA__funcs_231.c
    Lufia___The_Legend_Returns__USA__funcs_232.c
    Lufia___The_Legend_Returns__USA__funcs_233.c
    Lufia___The_Legend_Returns__USA__funcs_234.c
    Lufia___The_Legend_Returns__USA__funcs_235.c
    Lufia___The_Legend_Returns__USA__funcs_236.c
    Lufia___The_Legend_Returns__USA__funcs_237.c
    Lufia___The_Legend_Returns__USA__funcs_238.c
    Lufia___The_Legend_Returns__USA__funcs_239.c
    Lufia___The_Legend_Returns__USA__funcs_240.c
    Lufia___The_Legend_Returns__USA__funcs_241.c
    Lufia___The_Legend_Returns__USA__funcs_242.c
    Lufia___The_Legend_Returns__USA__funcs_243.c
    Lufia___The_Legend_Returns__USA__funcs_244.c
    Lufia___The_Legend_Returns__USA__funcs_245.c
    Lufia___The_Legend_Returns__USA__funcs_246.c
    Lufia___The_Legend_Returns__USA__funcs_247.c
    Lufia___The_Legend_Returns__USA__funcs_248.c
    Lufia___The_Legend_Returns__USA__funcs_249.c
    Lufia___The_Legend_Returns__USA__funcs_250.c
    Lufia___The_Legend_Returns__USA__funcs_251.c
    Lufia___The_Legend_Returns__USA__funcs_252.c
    Lufia___The_Legend_Returns__USA__funcs_253.c
    Lufia___The_Legend_Returns__USA__funcs_254.c
    Lufia___The_Legend_Returns__USA__funcs_255.c
    Lufia___The_Legend_Returns__USA__funcs_256.c
    Lufia___The_Legend_Returns__USA__funcs_257.c
    Lufia___The_Legend_Returns__USA__funcs_258.c
    Lufia___The_Legend_Returns__USA__funcs_259.c
    Lufia___The_Legend_Returns__USA__funcs_260.c
    Lufia___The_Legend_Returns__USA__funcs_261.c
    Lufia___The_Legend_Returns__USA__funcs_262.c
    Lufia___The_Legend_Returns__USA__funcs_263.c
    Lufia___The_Legend_Returns__USA__funcs_264.c
    Lufia___The_Legend_Returns__USA__funcs_265.c
    Lufia___The_Legend_Returns__USA__funcs_266.c
    Lufia___The_Legend_Returns__USA__funcs_267.c
    Lufia___The_Legend_Returns__USA__funcs_268.c
    Lufia___The_Legend_Returns__USA__funcs_269.c
    Lufia___The_Legend_Returns__USA__funcs_270.c
    Lufia___The_Legend_Returns__USA__funcs_271.c
    Lufia___The_Legend_Returns__USA__funcs_272.c
    Lufia___The_Legend_Returns__USA__funcs_273.c
    Lufia___The_Legend_Returns__USA__funcs_274.c
    Lufia___The_Legend_Returns__USA__funcs_275.c
    Lufia___The_Legend_Returns__USA__funcs_276.c
    Lufia___The_Legend_Returns__USA__funcs_277.c
    Lufia___The_Legend_Returns__USA__funcs_278.c
    Lufia___The_Legend_Returns__USA__funcs_279.c
    Lufia___The_Legend_Returns__USA__funcs_280.c
    Lufia___The_Legend_Returns__USA__funcs_281.c
    Lufia___The_Legend_Returns__USA__funcs_282.c
    Lufia___The_Legend_Returns__USA__funcs_283.c
    Lufia___The_Legend_Returns__USA__funcs_284.c
    Lufia___The_Legend_Returns__USA__funcs_285.c
    Lufia___The_Legend_Returns__USA__funcs_286.c
    Lufia___The_Legend_Returns__USA__funcs_287.c
    Lufia___The_Legend_Returns__USA__funcs_288.c
    Lufia___The_Legend_Returns__USA__funcs_289.c
    Lufia___The_Legend_Returns__USA__funcs_290.c
    Lufia___The_Legend_Returns__USA__funcs_291.c
    Lufia___The_Legend_Returns__USA__funcs_292.c
    Lufia___The_Legend_Returns__USA__funcs_293.c
    Lufia___The_Legend_Returns__USA__funcs_294.c
    Lufia___The_Legend_Returns__USA__funcs_295.c
    Lufia___The_Legend_Returns__USA__funcs_296.c
    Lufia___The_Legend_Returns__USA__funcs_297.c
    Lufia___The_Legend_Returns__USA__funcs_298.c
    Lufia___The_Legend_Returns__USA__funcs_299.c
    Lufia___The_Legend_Returns__USA__funcs_300.c
    Lufia___The_Legend_Returns__USA__funcs_301.c
    Lufia___The_Legend_Returns__USA__funcs_302.c
    Lufia___The_Legend_Returns__USA__funcs_303.c
    Lufia___The_Legend_Returns__USA__funcs_304.c
    Lufia___The_Legend_Returns__USA__funcs_305.c
    Lufia___The_Legend_Returns__USA__funcs_306.c
    Lufia___The_Legend_Returns__USA__funcs_307.c
    Lufia___The_Legend_Returns__USA__funcs_308.c
    Lufia___The_Legend_Returns__USA__funcs_309.c
    Lufia___The_Legend_Returns__USA__funcs_310.c
    Lufia___The_Legend_Returns__USA__funcs_311.c
    Lufia___The_Legend_Returns__USA__funcs_312.c
    Lufia___The_Legend_Returns__USA__funcs_313.c
    Lufia___The_Legend_Returns__USA__funcs_314.c
    Lufia___The_Legend_Returns__USA__funcs_315.c
    Lufia___The_Legend_Returns__USA__funcs_316.c
    Lufia___The_Legend_Returns__USA__funcs_317.c
    Lufia___The_Legend_Returns__USA__funcs_318.c
    Lufia___The_Legend_Returns__USA__funcs_319.c
    Lufia___The_Legend_Returns__USA__funcs_320.c
    Lufia___The_Legend_Returns__USA__funcs_321.c
    Lufia___The_Legend_Returns__USA__funcs_322.c
    Lufia___The_Legend_Returns__USA__funcs_323.c
    Lufia___The_Legend_Returns__USA__funcs_324.c
    Lufia___The_Legend_Returns__USA__funcs_325.c
    Lufia___The_Legend_Returns__USA__funcs_326.c
    Lufia___The_Legend_Returns__USA__funcs_327.c
    Lufia___The_Legend_Returns__USA__funcs_328.c
    Lufia___The_Legend_Returns__USA__funcs_329.c
    Lufia___The_Legend_Returns__USA__funcs_330.c
    Lufia___The_Legend_Returns__USA__funcs_331.c
    Lufia___The_Legend_Returns__USA__funcs_332.c
    Lufia___The_Legend_Returns__USA__funcs_333.c
    Lufia___The_Legend_Returns__USA__funcs_334.c
    Lufia___The_Legend_Returns__USA__funcs_335.c
)
if(NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(MSVC)
        if(GBRECOMP_GENERATED_OPT_LEVEL STREQUAL "0")
            set(GBRECOMP_GENERATED_OPT_FLAG "/Od")
        elseif(GBRECOMP_GENERATED_OPT_LEVEL STREQUAL "1")
            set(GBRECOMP_GENERATED_OPT_FLAG "/O1")
        else()
            set(GBRECOMP_GENERATED_OPT_FLAG "/O2")
        endif()
        set_source_files_properties(${GBRECOMP_GENERATED_SOURCES} PROPERTIES COMPILE_OPTIONS "${GBRECOMP_GENERATED_OPT_FLAG}")
    else()
        set_source_files_properties(${GBRECOMP_GENERATED_SOURCES} PROPERTIES COMPILE_OPTIONS "-O${GBRECOMP_GENERATED_OPT_LEVEL}")
    endif()
endif()

if(NOT MSVC AND NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(APPLE)
        target_link_options(gbrt PRIVATE -Wl,-dead_strip)
        target_link_options(Lufia___The_Legend_Returns__USA_ PRIVATE -Wl,-dead_strip)
    else()
        target_link_options(gbrt PRIVATE -Wl,--gc-sections)
        target_link_options(Lufia___The_Legend_Returns__USA_ PRIVATE -Wl,--gc-sections)
    endif()
endif()

if(GBRECOMP_ENABLE_IPO AND NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
    check_ipo_supported(RESULT GBRECOMP_IPO_SUPPORTED OUTPUT GBRECOMP_IPO_ERROR)
    if(GBRECOMP_IPO_SUPPORTED)
        set_property(TARGET gbrt PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
        set_property(TARGET Lufia___The_Legend_Returns__USA_ PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
    else()
        message(STATUS "GB Recompiled: IPO/LTO not enabled (${GBRECOMP_IPO_ERROR})")
    endif()
endif()

target_link_libraries(Lufia___The_Legend_Returns__USA_ gbrt)
if(TARGET SDL2::SDL2main)
    target_link_libraries(Lufia___The_Legend_Returns__USA_ SDL2::SDL2main)
elseif(TARGET SDL2main)
    target_link_libraries(Lufia___The_Legend_Returns__USA_ SDL2main)
endif()
if(GBRECOMP_ENABLE_STRIP AND NOT MSVC AND NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(APPLE)
        add_custom_command(TARGET Lufia___The_Legend_Returns__USA_ POST_BUILD
            COMMAND ${CMAKE_STRIP} -Sx $<TARGET_FILE:Lufia___The_Legend_Returns__USA_>
            VERBATIM)
    elseif(CMAKE_STRIP)
        add_custom_command(TARGET Lufia___The_Legend_Returns__USA_ POST_BUILD
            COMMAND ${CMAKE_STRIP} --strip-debug $<TARGET_FILE:Lufia___The_Legend_Returns__USA_>
            VERBATIM)
    endif()
endif()
