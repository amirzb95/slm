load("//tensorflow:tensorflow.default.bzl", "cuda_py_test")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = [
        "//tensorflow:internal",
    ],
    licenses = ["notice"],
)

py_library(
    name = "parallel_for",
    srcs = [
        "__init__.py",
        "control_flow_ops.py",
        "gradients.py",
        "pfor.py",
        "test_util.py",
    ],
    srcs_version = "PY3",
    deps = [
        ":control_flow_ops",
        ":gradients",
        ":test_util",
        "//tensorflow/compiler/tf2xla/python:xla",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:check_ops",
        "//tensorflow/python:cond",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:control_flow_assert",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:control_flow_switch_case",
        "//tensorflow/python:data_flow_ops",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:functional_ops",
        "//tensorflow/python:handle_data_util",
        "//tensorflow/python:map_fn",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:nn_ops",
        "//tensorflow/python:sparse_ops",
        "//tensorflow/python:sparse_tensor",
        "//tensorflow/python:tensor_array_ops",
        "//tensorflow/python:tensor_shape",
        "//tensorflow/python:tensor_util",
        "//tensorflow/python:while_loop",
        "//tensorflow/python/framework:smart_cond",
        "//tensorflow/python/platform:client_testlib",
        "//tensorflow/python/platform:flags",
        "//tensorflow/python/platform:tf_logging",
        "//tensorflow/python/util:compat",
        "//tensorflow/python/util:nest",
        "//tensorflow/python/util:object_identity",
        "//tensorflow/python/util:tf_decorator",
        "//tensorflow/python/util:tf_export",
        "//tensorflow/python/util:variable_utils",
        "@absl_py//absl/flags",
    ],
)

py_library(
    name = "pfor_lib",
    srcs = ["pfor.py"],
    srcs_version = "PY3",
    deps = [
        "//tensorflow/compiler/tf2xla/python:xla",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:check_ops",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:control_flow_assert",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:control_flow_switch_case",
        "//tensorflow/python:data_flow_ops",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:manip_ops",
        "//tensorflow/python:map_fn",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:nn_ops",
        "//tensorflow/python:sparse_ops",
        "//tensorflow/python:sparse_tensor",
        "//tensorflow/python:tensor_array_ops",
        "//tensorflow/python:tensor_shape",
        "//tensorflow/python:tensor_util",
        "//tensorflow/python:while_loop",
        "//tensorflow/python/platform:flags",
        "//tensorflow/python/platform:tf_logging",
        "//tensorflow/python/util:compat",
        "//tensorflow/python/util:nest",
        "//tensorflow/python/util:object_identity",
        "@absl_py//absl/flags",
    ],
)

py_library(
    name = "control_flow_ops",
    srcs = ["control_flow_ops.py"],
    srcs_version = "PY3",
    visibility = ["//visibility:public"],
    deps = [
        ":pfor_lib",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:cond",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:tensor_array_ops",
        "//tensorflow/python:tensor_shape",
        "//tensorflow/python:tensor_util",
        "//tensorflow/python:while_loop",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:function",
        "//tensorflow/python/ops/numpy_ops:numpy",
        "//tensorflow/python/ops/signal",
        "//tensorflow/python/platform:tf_logging",
        "//tensorflow/python/util:nest",
        "//tensorflow/python/util:tf_decorator",
        "//tensorflow/python/util:tf_export",
        "//tensorflow/python/util:variable_utils",
    ],
)

py_library(
    name = "test_util",
    srcs = ["test_util.py"],
    srcs_version = "PY3",
    deps = [
        ":pfor_lib",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:variables",
        "//tensorflow/python/util:nest",
    ],
)

cuda_py_test(
    name = "control_flow_ops_test",
    srcs = ["control_flow_ops_test.py"],
    shard_count = 16,
    tags = [
        "no_rocm",
    ],
    deps = [
        ":control_flow_ops",
        ":test_util",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:cond",
        "//tensorflow/python:control_flow_assert",
        "//tensorflow/python:control_flow_v2_toggles",
        "//tensorflow/python:gradient_checker_v2",
        "//tensorflow/python:gradients",
        "//tensorflow/python:logging_ops",
        "//tensorflow/python:parsing_ops",
        "//tensorflow/python:random_ops",
        "//tensorflow/python:session",
        "//tensorflow/python:tensor_array_grad",
        "//tensorflow/python:while_loop",
        "//tensorflow/python/util:nest",
    ],
)

cuda_py_test(
    name = "xla_control_flow_ops_test",
    srcs = ["xla_control_flow_ops_test.py"],
    tags = [
        # XLA is not enabled by default on Mac or Windows.
        "no_mac",
        "no_windows",
        "no_gpu",  # TODO(b/155761551): Flaky on GPU on TAP
        "no_tfrt",  # Note: Legacy XLA test, which depends on EncapsulateXlaComputationsPass.
        "notsan",  # TODO(b/271486347): Fails TSAN on TAP
    ],
    xla_enabled = True,
    deps = [
        ":control_flow_ops",
        ":test_util",
        "//tensorflow/compiler/tf2xla/python:xla",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:control_flow_v2_toggles",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:random_ops",
        "//tensorflow/python:while_loop",
        "//tensorflow/python/compiler/xla:compiler_py",
        "//tensorflow/python/platform:client_testlib",
    ],
)

cuda_py_test(
    name = "array_test",
    srcs = ["array_test.py"],
    tags = [
        "nogpu",  # b/217374776
    ],
    deps = [
        ":control_flow_ops",
        ":test_util",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:random_ops",
        "//tensorflow/python/eager:backprop",
    ],
)

cuda_py_test(
    name = "math_test",
    srcs = ["math_test.py"],
    shard_count = 5,
    tags = ["optonly"],  # Too slow in non-opt mode
    deps = [
        ":control_flow_ops",
        ":test_util",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:random_ops",
    ],
)

py_library(
    name = "gradients",
    srcs = ["gradients.py"],
    srcs_version = "PY3",
    deps = [
        ":control_flow_ops",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:gradients",
        "//tensorflow/python/util:nest",
    ],
)

cuda_py_test(
    name = "gradients_test",
    srcs = ["gradients_test.py"],
    tags = ["optonly"],  # Too slow in non-opt mode
    deps = [
        ":control_flow_ops",
        ":gradients",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:functional_ops",
        "//tensorflow/python:layers",
        "//tensorflow/python:random_ops",
        "//tensorflow/python:while_loop",
        "//tensorflow/python/ops/losses",
        "//tensorflow/python/util:nest",
        "//third_party/py/numpy",
    ],
)
