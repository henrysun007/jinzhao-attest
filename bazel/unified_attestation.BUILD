load("@rules_proto//proto:defs.bzl", "proto_library")

package(default_visibility = ["//visibility:public"])

config_setting(
    name = "hyper",
    values = {"define": "tee_type=hyper"},
)

config_setting(
    name = "sgx1",
    values = {"define": "tee_type=sgx1"},
)

config_setting(
    name = "sgx2",
    values = {"define": "tee_type=sgx2"},
)

occlum_cc_library(
    name = "generation",
    srcs = glob([
        "ual/generation/core/**/*.cpp",
        "ual/generation/platforms/**/*.cpp",
        "ual/generation/trusted/trusted_enclave.cpp",
        "ual/generation/untrusted/**/*.cpp",
    ]),
    copts = select({
        ":hyper": ["-DTEE_TYPE_HYPERENCLAVE"],
        ":sgx1": ["-DTEE_TYPE_SGX1"],
        ":sgx2": ["-DTEE_TYPE_SGX2"],
        "//conditions:default": [],
    }),
    strip_include_prefix = "ual",
    deps = [
        ":generation_headers",
        ":network",
        ":sgx_headers",
    ],
)

occlum_cc_library(
    name = "verification",
    srcs = glob([
        "ual/verification/core/**/*.cpp",
        "ual/verification/platforms/**/*.cpp",
        "ual/verification/uas/**/*.cpp",
    ]),
    strip_include_prefix = "ual",
    deps = [
        ":network",
        ":sgx2_qvl_headers",
        ":verification_headers",
    ],
)

occlum_cc_library(
    name = "network",
    srcs = glob(["ual/network/**/*.cpp"]),
    deps = [
        ":common",
        ":sgx2_qvl",
        ":verification_headers",
        "@com_github_curl//:curl",
    ],
)

occlum_cc_library(
    name = "common",
    srcs = glob([
        "ual/common/**/*.cpp",
        "ual/utils/untrusted/**/*.cpp",
    ]),
    hdrs = glob([
        "ual/include/attestation/**/*.h",
        "ual/include/utils/**/*.h",
        "ual/include/sgx/**/*.h",
        "ual/include/unified_attestation/**/*.h",
        "ual/include/network/**/*.h",
    ]),
    strip_include_prefix = "ual/include",
    deps = [
        ":cc_attestation_proto",
        ":sgx_headers",
        "@com_github_openssl_openssl//:openssl",
        "@com_github_rapidjson//:rapidjson",
        "@cppcodec",
    ],
)

occlum_cc_library(
    name = "verification_headers",
    hdrs = glob(["ual/verification/**/*.h"]),
    strip_include_prefix = "ual",
)

occlum_cc_library(
    name = "generation_headers",
    hdrs = glob(["ual/generation/**/*.h"]),
    strip_include_prefix = "ual",
)

occlum_cc_library(
    name = "sgx2_qvl",
    srcs = glob([
        "ual/external/dcap/QuoteVerification/QvE/Enclave/qve.cpp",
        "ual/external/dcap/QuoteVerification/QVL/Src/**/*.cpp",
        "ual/verification/platforms/sgx2/Utils/*.cpp",
        "ual/external/dcap/QuoteVerification/QVL/Src/AttestationLibrary/*.cpp",
        "ual/external/dcap/QuoteVerification/QVL/Src/AttestationParsers/*.cpp",
        )
    ]),
    deps = [
        "@com_github_rapidjson//:rapidjson",
        ":sgx2_qvl_headers",
        "@com_github_openssl_openssl//:openssl",
    ],
)

occlum_cc_library(
    name = "sgx2_qvl_headers",
    hdrs = glob(["ual/external/dcap/QuoteVerification/QVL/Src/dcap_quoteverify/inc/**/*.h"]),
    strip_include_prefix = "ual/external/dcap/QuoteVerification/QVL/Src/dcap_quoteverify/inc/",
    deps = [
        ":sgx2_qvl_common_headers",
        ":sgx2_qvl_lib2_headers",
        ":sgx2_qvl_lib_headers",
        ":sgx2_qvl_parser2_headers",
        ":sgx2_qvl_parser_headers",
        ":sgx2_qvl_utils_headers",
        ":sgx_headers",
        ":sgx2_qve_headers",
    ],
)

occlum_cc_library(
    name = "sgx2_qvl_common_headers",
    hdrs = glob(["ual/external/dcap/QuoteVerification/QVL/Src/AttestationCommons/include/**/*.h"]),
    strip_include_prefix = "ual/external/dcap/QuoteVerification/QVL/Src/AttestationCommons/include",
)

occlum_cc_library(
    name = "sgx2_qvl_lib_headers",
    hdrs = glob(["ual/external/dcap/QuoteVerification/QVL/Src/AttestationLibrary/include/**/*.h"]),
    copts = ["-DENV_TYPE_OCCLUM"],
    strip_include_prefix = "ual/external/dcap/QuoteVerification/QVL/Src/AttestationLibrary/include",
)

occlum_cc_library(
    name = "sgx2_qvl_parser_headers",
    hdrs = glob(["ual/external/dcap/QuoteVerification/QVL/Src/AttestationParsers/include/**/*.h"]),
    strip_include_prefix = "ual/external/dcap/QuoteVerification/QVL/Src/AttestationParsers/include",
)

occlum_cc_library(
    name = "sgx2_qvl_lib2_headers",
    hdrs = glob(["ual/external/dcap/QuoteVerification/QVL/Src/AttestationLibrary/src/**/*.h"]),
    strip_include_prefix = "ual/external/dcap/QuoteVerification/QVL/Src/AttestationLibrary/src",
)

occlum_cc_library(
    name = "sgx2_qvl_parser2_headers",
    hdrs = glob(["ual/external/dcap/QuoteVerification/QVL/Src/AttestationParsers/src/**/*.h"]),
    strip_include_prefix = "ual/external/dcap/QuoteVerification/QVL/Src/AttestationParsers/src",
)

occlum_cc_library(
    name = "sgx2_qvl_utils_headers",
    hdrs = glob(["ual/external/dcap/QuoteVerification/QVL/Src/AttestationCommons/include/Utils/**/*.h"]),
    strip_include_prefix = "ual/external/dcap/QuoteVerification/QVL/Src/AttestationCommons/include/Utils",
)

occlum_cc_library(
    name = "sgx_headers",
    hdrs = glob(["ual/include/sgx/**/*.h"]),
    strip_include_prefix = "ual/include/sgx",
)

occlum_cc_library(
    name = "sgx2_qve_headers",
    hdrs = glob(["ual/external/dcap/QuoteVerification/QvE/Include/**/*.h"]),
    strip_include_prefix = "ual/external/dcap/QuoteVerification/QvE/Include",
)

cc_proto_library(
    name = "cc_attestation_proto",
    visibility = ["//visibility:public"],
    deps = [
        ":attestation_proto",
    ],
)

proto_library(
    name = "attestation_proto",
    srcs = glob([
        "ual/proto/*.proto",
    ]),
    strip_import_prefix = "ual/proto",
    visibility = ["//visibility:public"],
)
