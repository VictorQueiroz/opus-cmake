cmake_minimum_required(VERSION 3.1)
project(opus)

option(OPUS_USE_ALLOCA "USE_ALLOCA" ON)
option(OPUS_FIXED_POINT "FIXED_POINT" OFF)
option(OPUS_DISABLE_FLOAT_API "DISABLE_FLOAT_API" OFF)

set(OPUS_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/deps/opus)
set(OPUS_INCLUDE_DIR ${OPUS_SOURCE_DIR}/include)

set(SILK_SOURCES
    ${OPUS_SOURCE_DIR}/silk/CNG.c
    ${OPUS_SOURCE_DIR}/silk/code_signs.c
    ${OPUS_SOURCE_DIR}/silk/init_decoder.c
    ${OPUS_SOURCE_DIR}/silk/decode_core.c
    ${OPUS_SOURCE_DIR}/silk/decode_frame.c
    ${OPUS_SOURCE_DIR}/silk/decode_parameters.c
    ${OPUS_SOURCE_DIR}/silk/decode_indices.c
    ${OPUS_SOURCE_DIR}/silk/decode_pulses.c
    ${OPUS_SOURCE_DIR}/silk/decoder_set_fs.c
    ${OPUS_SOURCE_DIR}/silk/dec_API.c
    ${OPUS_SOURCE_DIR}/silk/enc_API.c
    ${OPUS_SOURCE_DIR}/silk/encode_indices.c
    ${OPUS_SOURCE_DIR}/silk/encode_pulses.c
    ${OPUS_SOURCE_DIR}/silk/gain_quant.c
    ${OPUS_SOURCE_DIR}/silk/interpolate.c
    ${OPUS_SOURCE_DIR}/silk/LP_variable_cutoff.c
    ${OPUS_SOURCE_DIR}/silk/NLSF_decode.c
    ${OPUS_SOURCE_DIR}/silk/NSQ.c
    ${OPUS_SOURCE_DIR}/silk/NSQ_del_dec.c
    ${OPUS_SOURCE_DIR}/silk/PLC.c
    ${OPUS_SOURCE_DIR}/silk/shell_coder.c
    ${OPUS_SOURCE_DIR}/silk/tables_gain.c
    ${OPUS_SOURCE_DIR}/silk/tables_LTP.c
    ${OPUS_SOURCE_DIR}/silk/tables_NLSF_CB_NB_MB.c
    ${OPUS_SOURCE_DIR}/silk/tables_NLSF_CB_WB.c
    ${OPUS_SOURCE_DIR}/silk/tables_other.c
    ${OPUS_SOURCE_DIR}/silk/tables_pitch_lag.c
    ${OPUS_SOURCE_DIR}/silk/tables_pulses_per_block.c
    ${OPUS_SOURCE_DIR}/silk/VAD.c
    ${OPUS_SOURCE_DIR}/silk/control_audio_bandwidth.c
    ${OPUS_SOURCE_DIR}/silk/quant_LTP_gains.c
    ${OPUS_SOURCE_DIR}/silk/VQ_WMat_EC.c
    ${OPUS_SOURCE_DIR}/silk/HP_variable_cutoff.c
    ${OPUS_SOURCE_DIR}/silk/NLSF_encode.c
    ${OPUS_SOURCE_DIR}/silk/NLSF_VQ.c
    ${OPUS_SOURCE_DIR}/silk/NLSF_unpack.c
    ${OPUS_SOURCE_DIR}/silk/NLSF_del_dec_quant.c
    ${OPUS_SOURCE_DIR}/silk/process_NLSFs.c
    ${OPUS_SOURCE_DIR}/silk/stereo_LR_to_MS.c
    ${OPUS_SOURCE_DIR}/silk/stereo_MS_to_LR.c
    ${OPUS_SOURCE_DIR}/silk/check_control_input.c
    ${OPUS_SOURCE_DIR}/silk/control_SNR.c
    ${OPUS_SOURCE_DIR}/silk/init_encoder.c
    ${OPUS_SOURCE_DIR}/silk/control_codec.c
    ${OPUS_SOURCE_DIR}/silk/A2NLSF.c
    ${OPUS_SOURCE_DIR}/silk/ana_filt_bank_1.c
    ${OPUS_SOURCE_DIR}/silk/biquad_alt.c
    ${OPUS_SOURCE_DIR}/silk/bwexpander_32.c
    ${OPUS_SOURCE_DIR}/silk/bwexpander.c
    ${OPUS_SOURCE_DIR}/silk/debug.c
    ${OPUS_SOURCE_DIR}/silk/decode_pitch.c
    ${OPUS_SOURCE_DIR}/silk/inner_prod_aligned.c
    ${OPUS_SOURCE_DIR}/silk/lin2log.c
    ${OPUS_SOURCE_DIR}/silk/log2lin.c
    ${OPUS_SOURCE_DIR}/silk/LPC_analysis_filter.c
    ${OPUS_SOURCE_DIR}/silk/LPC_inv_pred_gain.c
    ${OPUS_SOURCE_DIR}/silk/table_LSF_cos.c
    ${OPUS_SOURCE_DIR}/silk/NLSF2A.c
    ${OPUS_SOURCE_DIR}/silk/NLSF_stabilize.c
    ${OPUS_SOURCE_DIR}/silk/NLSF_VQ_weights_laroia.c
    ${OPUS_SOURCE_DIR}/silk/pitch_est_tables.c
    ${OPUS_SOURCE_DIR}/silk/resampler.c
    ${OPUS_SOURCE_DIR}/silk/resampler_down2_3.c
    ${OPUS_SOURCE_DIR}/silk/resampler_down2.c
    ${OPUS_SOURCE_DIR}/silk/resampler_private_AR2.c
    ${OPUS_SOURCE_DIR}/silk/resampler_private_down_FIR.c
    ${OPUS_SOURCE_DIR}/silk/resampler_private_IIR_FIR.c
    ${OPUS_SOURCE_DIR}/silk/resampler_private_up2_HQ.c
    ${OPUS_SOURCE_DIR}/silk/resampler_rom.c
    ${OPUS_SOURCE_DIR}/silk/sigm_Q15.c
    ${OPUS_SOURCE_DIR}/silk/sort.c
    ${OPUS_SOURCE_DIR}/silk/sum_sqr_shift.c
    ${OPUS_SOURCE_DIR}/silk/stereo_decode_pred.c
    ${OPUS_SOURCE_DIR}/silk/stereo_encode_pred.c
    ${OPUS_SOURCE_DIR}/silk/stereo_find_predictor.c
    ${OPUS_SOURCE_DIR}/silk/stereo_quant_pred.c
    ${OPUS_SOURCE_DIR}/silk/LPC_fit.c
)

set(SILK_SOURCES_SSE4_1
    ${OPUS_SOURCE_DIR}/silk/x86/NSQ_sse.c
    ${OPUS_SOURCE_DIR}/silk/x86/NSQ_del_dec_sse.c
    ${OPUS_SOURCE_DIR}/silk/x86/x86_silk_map.c
    ${OPUS_SOURCE_DIR}/silk/x86/VAD_sse.c
    ${OPUS_SOURCE_DIR}/silk/x86/VQ_WMat_EC_sse.c
)

set(SILK_SOURCES_ARM_NEON_INTR
    ${OPUS_SOURCE_DIR}/silk/arm/arm_silk_map.c
    ${OPUS_SOURCE_DIR}/silk/arm/NSQ_del_dec_neon_intr.c
    ${OPUS_SOURCE_DIR}/silk/arm/NSQ_neon.c
)

set(SILK_SOURCES_FIXED
    ${OPUS_SOURCE_DIR}/silk/fixed/LTP_analysis_filter_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/LTP_scale_ctrl_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/corrMatrix_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/encode_frame_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/find_LPC_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/find_LTP_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/find_pitch_lags_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/find_pred_coefs_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/noise_shape_analysis_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/process_gains_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/regularize_correlations_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/residual_energy16_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/residual_energy_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/warped_autocorrelation_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/apply_sine_window_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/autocorr_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/burg_modified_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/k2a_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/k2a_Q16_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/pitch_analysis_core_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/vector_ops_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/schur64_FIX.c
    ${OPUS_SOURCE_DIR}/silk/fixed/schur_FIX.c
)

set(SILK_SOURCES_FIXED_SSE4_1
    ${OPUS_SOURCE_DIR}/silk/fixed/x86/vector_ops_FIX_sse.c
    ${OPUS_SOURCE_DIR}/silk/fixed/x86/burg_modified_FIX_sse.c
)

set(SILK_SOURCES_FLOAT
    ${OPUS_SOURCE_DIR}/silk/float/apply_sine_window_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/corrMatrix_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/encode_frame_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/find_LPC_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/find_LTP_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/find_pitch_lags_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/find_pred_coefs_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/LPC_analysis_filter_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/LTP_analysis_filter_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/LTP_scale_ctrl_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/noise_shape_analysis_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/process_gains_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/regularize_correlations_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/residual_energy_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/warped_autocorrelation_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/wrappers_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/autocorrelation_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/burg_modified_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/bwexpander_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/energy_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/inner_product_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/k2a_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/LPC_inv_pred_gain_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/pitch_analysis_core_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/scale_copy_vector_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/scale_vector_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/schur_FLP.c
    ${OPUS_SOURCE_DIR}/silk/float/sort_FLP.c
)

set(CELT_SOURCES
    ${OPUS_SOURCE_DIR}/celt/bands.c
    ${OPUS_SOURCE_DIR}/celt/celt.c
    ${OPUS_SOURCE_DIR}/celt/celt_encoder.c
    ${OPUS_SOURCE_DIR}/celt/celt_decoder.c
    ${OPUS_SOURCE_DIR}/celt/cwrs.c
    ${OPUS_SOURCE_DIR}/celt/entcode.c
    ${OPUS_SOURCE_DIR}/celt/entdec.c
    ${OPUS_SOURCE_DIR}/celt/entenc.c
    ${OPUS_SOURCE_DIR}/celt/kiss_fft.c
    ${OPUS_SOURCE_DIR}/celt/laplace.c
    ${OPUS_SOURCE_DIR}/celt/mathops.c
    ${OPUS_SOURCE_DIR}/celt/mdct.c
    ${OPUS_SOURCE_DIR}/celt/modes.c
    ${OPUS_SOURCE_DIR}/celt/pitch.c
    ${OPUS_SOURCE_DIR}/celt/celt_lpc.c
    ${OPUS_SOURCE_DIR}/celt/quant_bands.c
    ${OPUS_SOURCE_DIR}/celt/rate.c
    ${OPUS_SOURCE_DIR}/celt/vq.c
)

set(CELT_SOURCES_SSE
    ${OPUS_SOURCE_DIR}/celt/x86/x86cpu.c
    ${OPUS_SOURCE_DIR}/celt/x86/x86_celt_map.c
    ${OPUS_SOURCE_DIR}/celt/x86/pitch_sse.c
)

set(CELT_SOURCES_SSE2
    ${OPUS_SOURCE_DIR}/celt/x86/pitch_sse2.c 
    ${OPUS_SOURCE_DIR}/celt/x86/vq_sse2.c
)

set(CELT_SOURCES_SSE4_1
    ${OPUS_SOURCE_DIR}/celt/x86/celt_lpc_sse.c
    ${OPUS_SOURCE_DIR}/celt/x86/pitch_sse4_1.c
)

set(CELT_SOURCES_ARM
    ${OPUS_SOURCE_DIR}/celt/arm/armcpu.c
    ${OPUS_SOURCE_DIR}/celt/arm/arm_celt_map.c
)

set(CELT_SOURCES_ARM_ASM
    ${OPUS_SOURCE_DIR}/celt/arm/celt_pitch_xcorr_arm.s
)

set(CELT_AM_SOURCES_ARM_ASM
    ${OPUS_SOURCE_DIR}/celt/arm/armopts.s.in
)

set(CELT_SOURCES_ARM_NEON_INTR
    ${OPUS_SOURCE_DIR}/celt/arm/celt_neon_intr.c
)

set(CELT_SOURCES_ARM_NE10
    ${OPUS_SOURCE_DIR}/celt/arm/celt_ne10_fft.c
    ${OPUS_SOURCE_DIR}/celt/arm/celt_ne10_mdct.c
)

set(OPUS_HEADER
    ${OPUS_SOURCE_DIR}/include/opus.h
    ${OPUS_SOURCE_DIR}/include/opus_custom.h
    ${OPUS_SOURCE_DIR}/include/opus_defines.h
    ${OPUS_SOURCE_DIR}/include/opus_multistream.h
    ${OPUS_SOURCE_DIR}/include/opus_types.h
)

set(OPUS_SOURCES
    ${OPUS_SOURCE_DIR}/src/opus.c
    ${OPUS_SOURCE_DIR}/src/opus_decoder.c
    ${OPUS_SOURCE_DIR}/src/opus_encoder.c
    ${OPUS_SOURCE_DIR}/src/opus_multistream.c
    ${OPUS_SOURCE_DIR}/src/opus_multistream_encoder.c
    ${OPUS_SOURCE_DIR}/src/opus_multistream_decoder.c
    ${OPUS_SOURCE_DIR}/src/repacketizer.c
)

set(OPUS_SOURCES_FLOAT
    ${OPUS_SOURCE_DIR}/src/analysis.c
    ${OPUS_SOURCE_DIR}/src/mlp.c
    ${OPUS_SOURCE_DIR}/src/mlp_data.c
)

if(OPUS_FIXED_POINT)
    list(APPEND SILK_SOURCES ${SILK_SOURCES_FIXED})
    if(HAVE_SSE4_1)
        list(APPEND SILK_SOURCES ${SILK_SOURCES_SSE4_1} ${SILK_SOURCES_FIXED_SSE4_1})
    endif(HAVE_SSE4_1)
else(OPUS_FIXED_POINT)
    list(APPEND SILK_SOURCES ${SILK_SOURCES_FLOAT})
    if (HAVE_SSE4_1)
        list(APPEND SILK_SOURCES ${SILK_SOURCES_SSE4_1})
    endif(HAVE_SSE4_1)
endif(OPUS_FIXED_POINT)

if (NOT OPUS_DISABLE_FLOAT_API)
    list(APPEND OPUS_SOURCES ${OPUS_SOURCES_FLOAT})
endif(NOT OPUS_DISABLE_FLOAT_API)

if(HAVE_SSE)
    list(APPEND CELT_SOURCES ${CELT_SOURCES_SSE})
endif(HAVE_SSE)

if(HAVE_SSE2)
    list(APPEND CELT_SOURCES ${CELT_SOURCES_SSE2})
endif(HAVE_SSE2)

if(HAVE_SSE4_1)
    list(APPEND CELT_SOURCES ${CELT_SOURCES_SSE4_1})
endif(HAVE_SSE4_1)

if(CPU_ARM)
    list(APPEND CELT_SOURCES ${CELT_SOURCES_ARM})
    list(APPEND SILK_SOURCES ${SILK_SOURCES_ARM})

    if(HAVE_ARM_NEON_INTR)
        list(APPEND CELT_SOURCES ${CELT_SOURCES_ARM_NEON_INTR})
        list(APPEND SILK_SOURCES ${SILK_SOURCES_ARM_NEON_INTR})
    endif(HAVE_ARM_NEON_INTR)

    if(HAVE_ARM_NE10)
        list(APPEND CELT_SOURCES ${CELT_SOURCES_ARM_NE10})
    endif(HAVE_ARM_NE10)

    #if(OPUS_ARM_EXTERNAL_ASM)
    #   noinst_LTLIBRARIES = libarmasm.la
    #   libarmasm_la_SOURCES = $(CELT_SOURCES_ARM_ASM:.s=-gnu.S)
    #   BUILT_SOURCES = $(CELT_SOURCES_ARM_ASM:.s=-gnu.S) \
    #   $(CELT_AM_SOURCES_ARM_ASM:.s.in=.s) \
    #   $(CELT_AM_SOURCES_ARM_ASM:.s.in=-gnu.S)
    #endif(OPUS_ARM_EXTERNAL_ASM)
endif(CPU_ARM)

# It is strongly recommended to uncomment one of these
# VAR_ARRAYS: Use C99 variable-length arrays for stack allocation
# USE_ALLOCA: Use alloca() for stack allocation
# If none is defined, then the fallback is a non-threadsafe global array
if (OPUS_USE_ALLOCA)
    add_definitions(-DUSE_ALLOCA)
else (OPUS_USE_ALLOCA)
    add_definitions(-DVAR_ARRAYS)
endif (OPUS_USE_ALLOCA)

# These options affect performance
# HAVE_LRINTF: Use C99 intrinsics to speed up float-to-int conversion
add_definitions(-DHAVE_LRINTF)

add_definitions(-DOPUS_BUILD)

if (OPUS_FIXED_POINT)
    add_definitions(-DFIXED_POINT=1 -DDISABLE_FLOAT_API)
endif (OPUS_FIXED_POINT)


add_library(opus SHARED ${CELT_SOURCES} ${SILK_SOURCES} ${OPUS_SOURCES})
target_compile_options(opus PRIVATE $<$<OR:$<C_COMPILER_ID:Clang>,$<C_COMPILER_ID:GNU>>:
    -Wall
    -Wextra
    -Wcast-align
    -Wnested-externs
    -Wshadow
    -Wstrict-prototypes
>)
target_include_directories(opus PUBLIC ${OPUS_INCLUDE_DIR})
target_include_directories(opus PRIVATE
    ${OPUS_SOURCE_DIR}/silk
    ${OPUS_SOURCE_DIR}/silk/float
    ${OPUS_SOURCE_DIR}/silk/fixed
    ${OPUS_SOURCE_DIR}/celt
    ${OPUS_SOURCE_DIR}/src
)
