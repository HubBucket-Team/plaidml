# Copyright 2019 Intel Corporation.

load("//vendor/mlir:mlir.bzl", "COPTS", "mlir_tblgen")
load(
    "//bzl:plaidml.bzl",
    "plaidml_cc_library",
    "plaidml_cc_test",
)

mlir_tblgen(
    name = "gen-op-decls",
    src = "ops.td",
    out = "ops.h.inc",
    action = "-gen-op-decls",
    also = ["//pmlc/dialect/eltwise:predicates.td"],
    incs = ["@mlir//:include"],
)

mlir_tblgen(
    name = "gen-op-defs",
    src = "ops.td",
    out = "ops.cpp.inc",
    action = "-gen-op-defs",
    also = ["//pmlc/dialect/eltwise:predicates.td"],
    incs = ["@mlir//:include"],
)

plaidml_cc_library(
    name = "tile",
    srcs = [
        "builder.cc",
        "dialect.cc",
        "internal.h",
        "lowering.cc",
        "ops.cc",
        "ops.cpp.inc",
    ],
    hdrs = [
        "builder.h",
        "dialect.h",
        "lowering.h",
        "ops.h",
        "ops.h.inc",
        "types.h",
    ],
    copts = COPTS,
    visibility = ["//visibility:public"],
    deps = [
        ":gen-op-decls",
        ":gen-op-defs",
        "//base/util",
        "//pmlc/dialect/eltwise",
        "//tile/base",
        "//tile/lang",
        "@boost",
        "@llvm//:support",
        "@mlir//:AffineOps",
        "@mlir//:IR",
        "@mlir//:StandardOps",
    ],
    alwayslink = 1,
)
