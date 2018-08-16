cmake_minimum_required(VERSION 3.2)

project(secp256k1custom LANGUAGES C)


add_library(secp256k1custom SHARED ${SOURCES} src/secp256k1.c)
#target_include_directories(secp256k1 PUBLIC . ./src PRIVATE ${GMP_INCLUDE_DIR})
target_include_directories(secp256k1custom PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}" PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include" "${CMAKE_CURRENT_SOURCE_DIR}/contrib" )

target_compile_definitions(secp256k1custom
	PUBLIC -DUSE_NUM_NONE=1 -DUSE_FIELD_5X52=1 -DUSE_FIELD_INV_BUILTIN=1 -DUSE_SCALAR_4X64=1 -DUSE_SCALAR_INV_BUILTIN=1 -DHAVE___INT128=1 -DVERIFY=1
	PRIVATE -DENABLE_MODULE_ECDH -DENABLE_MODULE_RECOVERY -DENABLE_MODULE_SCHNORR
	)

add_executable(bench_ecdh src/bench_ecdh.c)
target_link_libraries(bench_ecdh secp256k1custom)

add_executable(bench_ecmult src/bench_ecmult.c)
target_link_libraries(bench_ecmult secp256k1custom)

add_executable(bench_internal src/bench_internal.c)
target_link_libraries(bench_internal secp256k1custom)

add_executable(bench_recover src/bench_recover.c)
target_link_libraries(bench_recover secp256k1custom)

add_executable(bench_sign src/bench_sign.c)
target_link_libraries(bench_sign secp256k1custom)

add_executable(bench_verify src/bench_verify.c)
target_link_libraries(bench_verify secp256k1custom)

add_executable(gen_context src/gen_context.c)
target_link_libraries(gen_context secp256k1custom)

add_executable(tests src/tests.c)
SET_TARGET_PROPERTIES(tests PROPERTIES COMPILE_FLAGS "-Wno-nonnull")
target_link_libraries(tests secp256k1custom)
