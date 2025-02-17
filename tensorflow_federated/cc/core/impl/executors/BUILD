load("//tensorflow_federated/tools:build_defs.bzl", "tff_cc_cpu_gpu_test_with_tf_deps", "tff_cc_library_with_tf_deps", "tff_cc_library_with_tf_runtime_deps", "tff_cc_test_with_tf_deps", "tff_pybind_extension_with_tf_deps")
load("@pybind11_bazel//:build_defs.bzl", "pybind_extension")
load("@rules_python//python:defs.bzl", "py_binary")

package(default_visibility = [
    ":executors_packages",
    "//tensorflow_federated/cc/core/impl:impl_visibility",
    "//tensorflow_federated/cc/core/impl/executor_stacks:executor_stacks_packages",
])

package_group(
    name = "executors_packages",
    packages = [
        "//tensorflow_federated/cc/core/impl/executors/...",
        "//tensorflow_federated/python/core/impl/executors/...",
    ],
)

licenses(["notice"])

tff_cc_library_with_tf_deps(
    name = "cardinalities",
    srcs = ["cardinalities.cc"],
    hdrs = ["cardinalities.h"],
    deps = [
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
    ],
)

tff_cc_library_with_tf_deps(
    name = "composing_executor",
    srcs = ["composing_executor.cc"],
    hdrs = ["composing_executor.h"],
    tf_deps = ["@org_tensorflow//tensorflow/core/platform:macros"],
    deps = [
        ":cardinalities",
        ":computations",
        ":executor",
        ":federated_intrinsics",
        ":status_macros",
        ":threading",
        ":value_validation",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
        "@com_google_absl//absl/types:variant",
        "@com_google_protobuf//:protobuf",
    ],
)

tff_cc_test_with_tf_deps(
    name = "composing_executor_test",
    srcs = ["composing_executor_test.cc"],
    deps = [
        ":composing_executor",
        ":computations",
        ":executor",
        ":executor_test_base",
        ":federated_intrinsics",
        ":mock_executor",
        ":status_matchers",
        ":value_test_utils",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
    ],
)

tff_cc_library_with_tf_deps(
    name = "computations",
    hdrs = ["computations.h"],
    deps = ["//tensorflow_federated/proto/v0:computation_cc_proto"],
)

tff_cc_library_with_tf_runtime_deps(
    name = "dataset_conversions",
    srcs = ["dataset_conversions.cc"],
    hdrs = ["dataset_conversions.h"],
    tf_deps = [
        "@org_tensorflow//tensorflow/core/data:standalone",
    ],
    deps = [
        ":status_macros",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
    ],
)

tff_cc_test_with_tf_deps(
    name = "dataset_conversions_test",
    srcs = ["dataset_conversions_test.cc"],
    tf_deps = [
        "@org_tensorflow//tensorflow/cc:cc_ops",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core:tensorflow",
    ],
    deps = [
        ":dataset_conversions",
        ":executor",
        ":mock_executor",
        ":status_matchers",
        ":value_test_utils",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
    ],
)

pybind_extension(
    name = "executor_bindings",
    srcs = ["executor_bindings.cc"],
    deps = [
        ":cardinalities",
        ":composing_executor",
        ":executor",
        ":federating_executor",
        ":reference_resolving_executor",
        ":remote_executor",
        ":tensorflow_executor",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_github_grpc_grpc//:grpc++",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@pybind11_abseil//pybind11_abseil:absl_casters",
        "@pybind11_abseil//pybind11_abseil:status_casters",
        "@pybind11_protobuf//pybind11_protobuf:wrapped_proto_caster",
    ],
)

tff_cc_library_with_tf_deps(
    name = "executor",
    srcs = ["executor.cc"],
    hdrs = ["executor.h"],
    tf_deps = ["@org_tensorflow//tensorflow/core/profiler/lib:traceme"],
    deps = [
        ":status_macros",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
    ],
)

tff_cc_library_with_tf_deps(
    name = "executor_service",
    srcs = ["executor_service.cc"],
    hdrs = ["executor_service.h"],
    deps = [
        ":cardinalities",
        ":executor",
        ":status_conversion",
        ":status_macros",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_grpc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_github_grpc_grpc//:grpc++",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:optional",
    ],
)

tff_cc_test_with_tf_deps(
    name = "executor_service_test",
    srcs = ["executor_service_test.cc"],
    deps = [
        ":executor",
        ":executor_service",
        ":mock_executor",
        ":protobuf_matchers",
        ":status_conversion",
        ":status_matchers",
        ":value_test_utils",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_github_grpc_grpc//:grpc++",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
    ],
)

tff_cc_library_with_tf_deps(
    name = "executor_test_base",
    testonly = True,
    srcs = [],
    hdrs = ["executor_test_base.h"],
    deps = [
        ":executor",
        ":mock_executor",
        ":protobuf_matchers",
        ":status_matchers",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_googletest//:gtest",
    ],
)

tff_cc_library_with_tf_deps(
    name = "federated_intrinsics",
    srcs = ["federated_intrinsics.cc"],
    hdrs = ["federated_intrinsics.h"],
    deps = [
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
    ],
)

tff_cc_library_with_tf_deps(
    name = "federating_executor",
    srcs = ["federating_executor.cc"],
    hdrs = ["federating_executor.h"],
    tf_deps = ["@org_tensorflow//tensorflow/core/platform:macros"],
    deps = [
        ":cardinalities",
        ":executor",
        ":federated_intrinsics",
        ":status_macros",
        ":threading",
        ":value_validation",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
        "@com_google_absl//absl/types:variant",
    ],
)

tff_cc_test_with_tf_deps(
    name = "federating_executor_test",
    srcs = ["federating_executor_test.cc"],
    deps = [
        ":executor",
        ":executor_test_base",
        ":federating_executor",
        ":mock_executor",
        ":status_matchers",
        ":value_test_utils",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
    ],
)

tff_cc_library_with_tf_deps(
    name = "mock_executor",
    testonly = True,
    hdrs = ["mock_executor.h"],
    deps = [
        ":executor",
        ":protobuf_matchers",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
        "@com_google_googletest//:gtest",
    ],
)

tff_cc_library_with_tf_deps(
    name = "mock_grpc",
    testonly = True,
    hdrs = ["mock_grpc.h"],
    deps = [
        "//tensorflow_federated/proto/v0:executor_cc_grpc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_github_grpc_grpc//:grpc++",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest",
    ],
)

tff_cc_library_with_tf_deps(
    name = "protobuf_matchers",
    testonly = True,
    hdrs = ["protobuf_matchers.h"],
    deps = [
        "@com_google_absl//absl/memory",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_googletest//:gtest",
        "@com_google_protobuf//:protobuf",
    ],
)

tff_cc_library_with_tf_deps(
    name = "reference_resolving_executor",
    srcs = ["reference_resolving_executor.cc"],
    hdrs = ["reference_resolving_executor.h"],
    tf_deps = ["@org_tensorflow//tensorflow/core:tensorflow"],
    deps = [
        ":executor",
        ":status_macros",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:variant",
    ],
)

tff_cc_test_with_tf_deps(
    name = "reference_resolving_executor_test",
    timeout = "short",
    srcs = ["reference_resolving_executor_test.cc"],
    tf_deps = [
        "@org_tensorflow//tensorflow/cc:array_ops",
        "@org_tensorflow//tensorflow/cc:math_ops",
        "@org_tensorflow//tensorflow/cc:scope",
        "@org_tensorflow//tensorflow/core:core_cpu_base",
        "@org_tensorflow//tensorflow/core:framework",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core/platform:byte_order",
        "@org_tensorflow//tensorflow/core/platform:status",
    ],
    deps = [
        ":executor",
        ":executor_test_base",
        ":mock_executor",
        ":protobuf_matchers",
        ":reference_resolving_executor",
        ":status_matchers",
        ":value_test_utils",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
    ],
)

tff_cc_library_with_tf_deps(
    name = "remote_executor",
    srcs = ["remote_executor.cc"],
    hdrs = ["remote_executor.h"],
    deps = [
        ":cardinalities",
        ":executor",
        ":status_conversion",
        ":status_macros",
        ":threading",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_grpc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_github_grpc_grpc//:grpc++",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/meta:type_traits",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:optional",
    ],
)

tff_cc_test_with_tf_deps(
    name = "remote_executor_test",
    timeout = "short",
    srcs = ["remote_executor_test.cc"],
    tags = ["nomacos"],
    tf_deps = [
        "@org_tensorflow//tensorflow/cc:array_ops",
        "@org_tensorflow//tensorflow/cc:math_ops",
    ],
    deps = [
        ":executor",
        ":mock_grpc",
        ":protobuf_matchers",
        ":remote_executor",
        ":status_matchers",
        ":value_test_utils",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
        "//tensorflow_federated/proto/v0:executor_cc_grpc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:optional",
    ],
)

tff_cc_library_with_tf_deps(
    name = "sequence_intrinsics",
    srcs = ["sequence_intrinsics.cc"],
    hdrs = ["sequence_intrinsics.h"],
    deps = [
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
    ],
)

tff_pybind_extension_with_tf_deps(
    name = "serialization_bindings",
    srcs = ["serialization_bindings.cc"],
    tf_deps = [
        "@org_tensorflow//tensorflow/c:tf_status",
        "@org_tensorflow//tensorflow/c:tf_tensor",
        "@org_tensorflow//tensorflow/core/platform:status",
        "@org_tensorflow//tensorflow/core:framework",
        "@org_tensorflow//tensorflow/python/lib/core:ndarray_tensor",
        "@org_tensorflow//tensorflow/python/lib/core:safe_ptr",
    ],
    tf_python_dependency = True,
    deps = [
        ":status_macros",
        ":tensor_serialization",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status:statusor",
        "@pybind11_abseil//pybind11_abseil:status_casters",
        "@pybind11_protobuf//pybind11_protobuf:wrapped_proto_caster",
    ],
)

tff_cc_library_with_tf_deps(
    name = "status_conversion",
    srcs = ["status_conversion.cc"],
    hdrs = ["status_conversion.h"],
    deps = [
        "@com_github_grpc_grpc//:grpc++",
        "@com_google_absl//absl/status",
    ],
)

tff_cc_library_with_tf_deps(
    name = "status_macros",
    hdrs = ["status_macros.h"],
    deps = [
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
    ],
)

tff_cc_library_with_tf_deps(
    name = "status_matchers",
    testonly = True,
    srcs = ["status_matchers.cc"],
    hdrs = ["status_matchers.h"],
    deps = [
        "@com_github_grpc_grpc//:grpc++",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_googletest//:gtest",
    ],
)

tff_cc_library_with_tf_deps(
    name = "tensor_serialization",
    srcs = ["tensor_serialization.cc"],
    hdrs = ["tensor_serialization.h"],
    tf_deps = [
        "@org_tensorflow//tensorflow/core:framework",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core/platform:byte_order",
    ],
    deps = [
        ":status_macros",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
    ],
)

tff_cc_library_with_tf_deps(
    name = "session_provider",
    srcs = ["session_provider.cc"],
    hdrs = ["session_provider.h"],
    tf_deps = [
        "@org_tensorflow//tensorflow/core:framework",
        "@org_tensorflow//tensorflow/core:graph",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core/common_runtime:device_mgr",
        "@org_tensorflow//tensorflow/core/common_runtime:session",
        "@org_tensorflow//tensorflow/core/common_runtime:session_options",
    ],
    deps = [
        ":status_macros",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:optional",
    ],
)

tff_cc_test_with_tf_deps(
    name = "session_provider_test",
    srcs = ["session_provider_test.cc"],
    tf_deps = ["@org_tensorflow//tensorflow/core:tensorflow"],
    deps = [
        ":session_provider",
        ":status_matchers",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
    ],
)

tff_cc_library_with_tf_deps(
    name = "tensorflow_executor",
    srcs = ["tensorflow_executor.cc"],
    hdrs = ["tensorflow_executor.h"],
    tf_deps = [
        "@org_tensorflow//tensorflow/core:framework",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core/common_runtime:core",
        "@org_tensorflow//tensorflow/core/common_runtime:session",
        "@org_tensorflow//tensorflow/core/platform:macros",
        "@org_tensorflow//tensorflow/core/platform:status",
        "@org_tensorflow//tensorflow/core/platform:tstring",
    ],
    deps = [
        ":executor",
        ":session_provider",
        ":status_macros",
        ":tensor_serialization",
        ":threading",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/meta:type_traits",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
        "@com_google_absl//absl/types:variant",
    ],
)

# Python target which generates an example GraphDef for testing `reduce`
# in `tensorflow_executor_test`.
py_binary(
    name = "make_reduce_lambda_test_graph",
    testonly = True,
    srcs = ["make_reduce_lambda_test_graph.py"],
    python_version = "PY3",
)

genrule(
    name = "reduce_lambda_test_graph",
    testonly = True,
    srcs = [],
    outs = ["reduce_lambda_test.pbtxt"],
    cmd = "$(location :make_reduce_lambda_test_graph) --output \"$@\"",
    exec_tools = [":make_reduce_lambda_test_graph"],
)

tff_cc_cpu_gpu_test_with_tf_deps(
    name = "tensorflow_executor_test",
    srcs = ["tensorflow_executor_test.cc"],
    args = [
        "--reduce_graph_path",
        "$(location reduce_lambda_test.pbtxt)",
    ],
    data = ["reduce_lambda_test.pbtxt"],
    tf_deps = [
        "@org_tensorflow//tensorflow/cc:cc_ops",
        "@org_tensorflow//tensorflow/cc:ops",
        "@org_tensorflow//tensorflow/cc:scope",
        "@org_tensorflow//tensorflow/core:core_cpu_base",
        "@org_tensorflow//tensorflow/core:framework",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core:tensorflow",
        "@org_tensorflow//tensorflow/core/platform:status",
        "@org_tensorflow//tensorflow/core/platform:tstring",
    ],
    deps = [
        ":executor",
        ":protobuf_matchers",
        ":status_macros",
        ":status_matchers",
        ":tensorflow_executor",
        ":value_test_utils",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
    ],
)

tff_cc_library_with_tf_deps(
    name = "threading",
    srcs = ["threading.cc"],
    hdrs = ["threading.h"],
    deps = [
        ":status_macros",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/types:span",
    ],
)

tff_cc_test_with_tf_deps(
    name = "threading_test",
    timeout = "short",
    srcs = ["threading_test.cc"],
    deps = [
        ":status_matchers",
        ":threading",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
        "@com_google_absl//absl/status",
    ],
)

tff_cc_library_with_tf_deps(
    name = "value_test_utils",
    testonly = True,
    hdrs = ["value_test_utils.h"],
    tf_deps = [
        "@org_tensorflow//tensorflow/cc:client_session",
        "@org_tensorflow//tensorflow/cc:const_op",
        "@org_tensorflow//tensorflow/cc:dataset_ops_internal",
        "@org_tensorflow//tensorflow/cc:ops",
        "@org_tensorflow//tensorflow/cc:scope",
        "@org_tensorflow//tensorflow/core:framework",
        "@org_tensorflow//tensorflow/core:protos_all_cc",
        "@org_tensorflow//tensorflow/core/platform:tstring",
    ],
    deps = [
        ":executor",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_absl//absl/types:span",
    ],
)

tff_cc_library_with_tf_deps(
    name = "value_validation",
    srcs = ["value_validation.cc"],
    hdrs = ["value_validation.h"],
    deps = [
        ":cardinalities",
        ":federated_intrinsics",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
    ],
)

tff_cc_test_with_tf_deps(
    name = "value_validation_test",
    timeout = "short",
    srcs = ["value_validation_test.cc"],
    deps = [
        ":status_matchers",
        ":value_test_utils",
        ":value_validation",
        "//tensorflow_federated/cc/common_libs:oss_test_main",
        "//tensorflow_federated/proto/v0:computation_cc_proto",
        "//tensorflow_federated/proto/v0:executor_cc_proto",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/types:span",
    ],
)
