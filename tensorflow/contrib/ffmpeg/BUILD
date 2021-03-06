# Ops that process audio and/or video files using FFmpeg.
# (https://www.ffmpeg.org/)

licenses(["notice"])  # Apache 2.0

exports_files(["LICENSE"])

package(default_visibility = ["//tensorflow:__subpackages__"])

load("//tensorflow:tensorflow.bzl", "tf_gen_op_wrapper_py")
load("//tensorflow:tensorflow.bzl", "tf_copts")
load("//tensorflow:tensorflow.bzl", "tf_custom_op_library")
load("//tensorflow:tensorflow.bzl", "tf_py_test")

filegroup(
    name = "test_data",
    srcs = glob(["testdata/*"]),
)

cc_library(
    name = "decode_audio_op_cc",
    srcs = ["decode_audio_op.cc"],
    copts = tf_copts(),
    linkstatic = 1,
    visibility = ["//visibility:private"],
    deps = [
        "//third_party/eigen3",
        "//tensorflow/contrib/ffmpeg/kernels:ffmpeg_lib",
        "//tensorflow/core:framework_headers_lib",
    ],
    alwayslink = 1,
)

tf_custom_op_library(
    name = "decode_audio_op.so",
    deps = [
        ":decode_audio_op_cc",
    ],
)

tf_gen_op_wrapper_py(
    name = "decode_audio_op_py",
    require_shape_functions = True,
    visibility = ["//visibility:private"],
    deps = [
        ":decode_audio_op_cc",
    ],
)

tf_py_test(
    name = "decode_audio_op_test",
    srcs = ["decode_audio_op_test.py"],
    additional_deps = [
        ":ffmpeg_ops_py",
        "//third_party/py/tensorflow",
        "//tensorflow/python:platform",
    ],
    data = [
        ":decode_audio_op.so",
        ":test_data",
    ],
    tags = [
        "local",
        "manual",
    ],
)

py_library(
    name = "ffmpeg_ops_py",
    srcs = [
        "__init__.py",
        "ffmpeg_ops.py",
    ],
    data = [
        ":decode_audio_op.so",
    ],
    srcs_version = "PY2AND3",
    deps = [
        ":decode_audio_op_py",
    ],
)

filegroup(
    name = "all_files",
    srcs = glob(
        ["**/*"],
        exclude = [
            "**/METADATA",
            "**/OWNERS",
        ],
    ),
    visibility = ["//tensorflow:__subpackages__"],
)
