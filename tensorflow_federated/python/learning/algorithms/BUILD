load("//tensorflow_federated/tools:build_defs.bzl", "py_cpu_gpu_test")
load("@rules_python//python:defs.bzl", "py_library")

package(default_visibility = [
    ":algorithms_packages",
    "//tensorflow_federated/python/learning:learning_visibility",
])

package_group(
    name = "algorithms_packages",
    packages = ["//tensorflow_federated/python/learning/algorithms/..."],
)

licenses(["notice"])

py_library(
    name = "algorithms",
    srcs = ["__init__.py"],
    srcs_version = "PY3",
    visibility = ["//tensorflow_federated/python/learning:__pkg__"],
    deps = [
        ":client_scheduled_federated_averaging",
        ":fed_prox",
    ],
)

py_library(
    name = "client_scheduled_federated_averaging",
    srcs = ["client_scheduled_federated_averaging.py"],
    srcs_version = "PY3",
    deps = [
        ":example_weighted_federated_averaging",
        "//tensorflow_federated/python/aggregators:factory",
        "//tensorflow_federated/python/aggregators:mean",
        "//tensorflow_federated/python/common_libs:py_typecheck",
        "//tensorflow_federated/python/core/api:computations",
        "//tensorflow_federated/python/core/impl/federated_context:intrinsics",
        "//tensorflow_federated/python/core/impl/types:computation_types",
        "//tensorflow_federated/python/core/impl/types:placements",
        "//tensorflow_federated/python/core/templates:measured_process",
        "//tensorflow_federated/python/learning:model",
        "//tensorflow_federated/python/learning:model_utils",
        "//tensorflow_federated/python/learning/optimizers:optimizer",
        "//tensorflow_federated/python/learning/templates:client_works",
        "//tensorflow_federated/python/learning/templates:composers",
        "//tensorflow_federated/python/learning/templates:distributors",
        "//tensorflow_federated/python/learning/templates:finalizers",
        "//tensorflow_federated/python/learning/templates:learning_process",
    ],
)

py_cpu_gpu_test(
    name = "client_scheduled_federated_averaging_test",
    timeout = "long",
    srcs = ["client_scheduled_federated_averaging_test.py"],
    python_version = "PY3",
    shard_count = 10,
    srcs_version = "PY3",
    deps = [
        ":client_scheduled_federated_averaging",
        "//tensorflow_federated/python/core/api:test_case",
        "//tensorflow_federated/python/learning:model_examples",
        "//tensorflow_federated/python/learning:model_update_aggregator",
        "//tensorflow_federated/python/learning/optimizers:sgdm",
    ],
)

py_library(
    name = "example_weighted_federated_averaging",
    srcs = ["example_weighted_federated_averaging.py"],
    srcs_version = "PY3",
    deps = [
        "//tensorflow_federated/python/aggregators:factory",
        "//tensorflow_federated/python/aggregators:mean",
        "//tensorflow_federated/python/common_libs:py_typecheck",
        "//tensorflow_federated/python/core/api:computations",
        "//tensorflow_federated/python/core/impl/federated_context:intrinsics",
        "//tensorflow_federated/python/core/impl/types:computation_types",
        "//tensorflow_federated/python/core/impl/types:placements",
        "//tensorflow_federated/python/core/templates:measured_process",
        "//tensorflow_federated/python/learning:model",
        "//tensorflow_federated/python/learning:model_utils",
        "//tensorflow_federated/python/learning/framework:dataset_reduce",
        "//tensorflow_federated/python/learning/optimizers:optimizer",
        "//tensorflow_federated/python/learning/templates:client_works",
        "//tensorflow_federated/python/learning/templates:composers",
        "//tensorflow_federated/python/learning/templates:distributors",
        "//tensorflow_federated/python/learning/templates:finalizers",
        "//tensorflow_federated/python/learning/templates:learning_process",
        "//tensorflow_federated/python/tensorflow_libs:tensor_utils",
    ],
)

py_cpu_gpu_test(
    name = "example_weighted_federated_averaging_test",
    timeout = "long",
    srcs = ["example_weighted_federated_averaging_test.py"],
    python_version = "PY3",
    shard_count = 10,
    srcs_version = "PY3",
    deps = [
        ":example_weighted_federated_averaging",
        "//tensorflow_federated/python/core/api:test_case",
        "//tensorflow_federated/python/learning:model_examples",
        "//tensorflow_federated/python/learning:model_update_aggregator",
        "//tensorflow_federated/python/learning:model_utils",
        "//tensorflow_federated/python/learning/framework:dataset_reduce",
        "//tensorflow_federated/python/learning/optimizers:sgdm",
    ],
)

py_library(
    name = "fed_prox",
    srcs = ["fed_prox.py"],
    srcs_version = "PY3",
    deps = [
        "//tensorflow_federated/python/aggregators:factory",
        "//tensorflow_federated/python/aggregators:mean",
        "//tensorflow_federated/python/common_libs:py_typecheck",
        "//tensorflow_federated/python/core/api:computations",
        "//tensorflow_federated/python/core/impl/federated_context:intrinsics",
        "//tensorflow_federated/python/core/impl/types:computation_types",
        "//tensorflow_federated/python/core/impl/types:placements",
        "//tensorflow_federated/python/core/templates:measured_process",
        "//tensorflow_federated/python/learning:model",
        "//tensorflow_federated/python/learning:model_utils",
        "//tensorflow_federated/python/learning/framework:dataset_reduce",
        "//tensorflow_federated/python/learning/optimizers:optimizer",
        "//tensorflow_federated/python/learning/templates:client_works",
        "//tensorflow_federated/python/learning/templates:composers",
        "//tensorflow_federated/python/learning/templates:distributors",
        "//tensorflow_federated/python/learning/templates:finalizers",
        "//tensorflow_federated/python/learning/templates:learning_process",
        "//tensorflow_federated/python/tensorflow_libs:tensor_utils",
    ],
)

py_cpu_gpu_test(
    name = "fed_prox_test",
    timeout = "long",
    srcs = ["fed_prox_test.py"],
    python_version = "PY3",
    shard_count = 10,
    srcs_version = "PY3",
    deps = [
        ":fed_prox",
        "//tensorflow_federated/python/core/api:test_case",
        "//tensorflow_federated/python/learning:model_examples",
        "//tensorflow_federated/python/learning:model_update_aggregator",
        "//tensorflow_federated/python/learning:model_utils",
        "//tensorflow_federated/python/learning/framework:dataset_reduce",
        "//tensorflow_federated/python/learning/optimizers:sgdm",
    ],
)

py_library(
    name = "federated_sgd",
    srcs = ["federated_sgd.py"],
    srcs_version = "PY3",
    deps = [
        "//tensorflow_federated/python/aggregators:factory",
        "//tensorflow_federated/python/aggregators:mean",
        "//tensorflow_federated/python/common_libs:py_typecheck",
        "//tensorflow_federated/python/core/api:computations",
        "//tensorflow_federated/python/core/impl/federated_context:intrinsics",
        "//tensorflow_federated/python/core/impl/types:computation_types",
        "//tensorflow_federated/python/core/impl/types:placements",
        "//tensorflow_federated/python/core/templates:measured_process",
        "//tensorflow_federated/python/learning:model",
        "//tensorflow_federated/python/learning:model_utils",
        "//tensorflow_federated/python/learning/framework:dataset_reduce",
        "//tensorflow_federated/python/learning/optimizers:optimizer",
        "//tensorflow_federated/python/learning/templates:client_works",
        "//tensorflow_federated/python/learning/templates:composers",
        "//tensorflow_federated/python/learning/templates:distributors",
        "//tensorflow_federated/python/learning/templates:finalizers",
        "//tensorflow_federated/python/learning/templates:learning_process",
        "//tensorflow_federated/python/tensorflow_libs:tensor_utils",
    ],
)

py_cpu_gpu_test(
    name = "federated_sgd_test",
    timeout = "long",
    srcs = ["federated_sgd_test.py"],
    python_version = "PY3",
    shard_count = 10,
    srcs_version = "PY3",
    deps = [
        ":federated_sgd",
        "//tensorflow_federated/python/common_libs:test_utils",
        "//tensorflow_federated/python/core/api:test_case",
        "//tensorflow_federated/python/learning:model_examples",
        "//tensorflow_federated/python/learning:model_update_aggregator",
        "//tensorflow_federated/python/learning:model_utils",
        "//tensorflow_federated/python/learning/framework:dataset_reduce",
    ],
)
