package(default_visibility = ["//visibility:public"])

load("//bzl:plaidml.bzl", "plaidml_cc_binary", "plaidml_cc_library")

plaidml_cc_library(
    name = "lib",
    srcs = ["pmlc.cc"],
    hdrs = ["pmlc.h"],
    defines = select({
        "//toolchain:windows_x86_64": [],
        "//conditions:default": ["ENABLE_LLVM_BITCODE"],
    }),
    deps = [
        "//base/config",
        "//tile/base",
        "//tile/codegen",
        "//tile/lang",
        "//tile/lib",
        "//tile/util",
        "//tile/targets/cpu",
        "@boost//:program_options",
    ],
)

plaidml_cc_binary(
    name = "pmlc",
    srcs = ["main.cc"],
    deps = [":lib"],
)
