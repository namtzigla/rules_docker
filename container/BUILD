# Copyright 2017 The Bazel Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
package(default_visibility = ["//visibility:public"])

licenses(["notice"])  # Apache 2.0

load("@bazel_skylib//:skylark_library.bzl", "skylark_library")

exports_files(["push-tag.sh.tpl"])

filegroup(
    name = "incremental_load_template",
    srcs = ["incremental_load.sh.tpl"],
    visibility = ["//visibility:public"],
)

py_binary(
    name = "extract_config",
    srcs = ["extract_config.py"],
    visibility = ["//visibility:public"],
    deps = [
        "@containerregistry",
    ],
)

py_library(
    name = "utils",
    srcs = [
        "__init__.py",
        "utils.py",
    ],
)

py_binary(
    name = "join_layers",
    srcs = ["join_layers.py"],
    visibility = ["//visibility:public"],
    deps = [
        ":utils",
        "@bazel_tools//third_party/py/six",
        "@containerregistry",
    ],
)

py_binary(
    name = "create_image_config",
    srcs = ["create_image_config.py"],
    visibility = ["//visibility:public"],
    deps = [
        ":utils",
        "@bazel_tools//third_party/py/six",
        "@containerregistry",
    ],
)

py_binary(
    name = "build_tar",
    srcs = ["build_tar.py"],
    srcs_version = "PY2AND3",
    visibility = ["//visibility:public"],
    deps = [
        "@bazel_source//tools/build_defs/pkg:archive",
        "@bazel_tools//third_party/py/gflags",
    ],
)

TEST_TARGETS = [
    ":base_with_entrypoint",
    ":base_with_volume",
    ":derivative_with_cmd",
    ":derivative_with_shadowed_cmd",
    ":derivative_with_volume",
    ":directory_with_tar_base",
    ":files_base",
    ":files_with_files_base",
    ":files_in_layer_with_files_base",
    ":files_with_tar_base",
    ":tar_base",
    ":tar_with_files_base",
    ":tar_with_tar_base",
    ":tars_in_layer_with_tar_base",
    ":docker_tarball_base",
    ":layers_with_docker_tarball_base",
    # TODO(mattmoor): Re-enable once archive is visible
    # "generated_tarball",
    ":with_unix_epoch_creation_time",
    ":with_millisecond_unix_epoch_creation_time",
    ":with_rfc_3339_creation_time",
    ":with_stamped_creation_time",
    ":with_default_stamped_creation_time",
    ":with_env",
    ":layers_with_env",
    ":with_double_env",
    ":with_label",
    ":with_double_label",
    ":with_stamp_label",
    ":with_user",
    ":workdir_with_tar_base",
    ":link_with_files_base",
    ":build_with_tag",
    ":with_passwd",
    ":with_passwd_tar",
    ":with_group",
    ":with_empty_files",
    ":with_empty_dirs",
    ":gen_image",
    ":data_path_image",
    ":no_data_path_image",
    ":absolute_data_path_image",
    ":root_data_path_image",
    ":dummy_repository",
    # TODO(mattmoor): Re-enable once archive is visible
    # "extras_with_deb",
    ":bundle_test",
    ":stamped_bundle_test",
    ":pause_based",
    ":dashdash_entrypoint",
    ":base_based_warning",
    ":cc_based_warning",
    "/pause_piecemeal:image",
    "/pause_piecemeal_gz:image",
    ":cc_image",
    # Re-enable once https://github.com/bazelbuild/rules_d/issues/14 is fixed.
    # "d_image",
    ":go_image",
    ":groovy_image",
    ":java_image",
    ":java_runfiles_image",
    ":java_runfiles_as_lib_image",
    ":nodejs_image",
    ":py3_image",
    ":py_image",
    ":py_image_complex",
    ":rust_image",
    ":scala_image",
    ":war_image",
    ":flat",
]

TEST_DATA = [
    "//testdata%s.tar" % t
    for t in TEST_TARGETS
] + [
    "//testdata:stamped_bundle_test",
]

py_test(
    name = "image_test",
    size = "medium",
    srcs = ["image_test.py"],
    data = TEST_DATA,
    deps = ["@containerregistry"],
)

skylark_library(
    name = "bundle",
    srcs = ["bundle.bzl"],
    deps = [
        ":layer_tools",
        ":providers",
        "//skylib:label",
    ],
)

skylark_library(
    name = "container",
    srcs = ["container.bzl"],
    deps = [
        ":bundle",
        ":flatten",
        ":image",
        ":import",
        ":layer",
        ":push",
    ],
)

skylark_library(
    name = "flatten",
    srcs = ["flatten.bzl"],
    deps = [
        ":layer_tools",
        ":providers",
        "//skylib:label",
    ],
)

skylark_library(
    name = "image",
    srcs = ["image.bzl"],
    deps = [
        ":layer_tools",
        ":providers",
        "//skylib:filetype",
        "//skylib:label",
        "//skylib:path",
        "//skylib:serialize",
        "//skylib:zip",
        "@bazel_tools//tools/build_defs/hash:hash.bzl",
    ],
)

skylark_library(
    name = "import",
    srcs = ["import.bzl"],
    deps = [
        ":layer_tools",
        ":providers",
        "//skylib:filetype",
        "//skylib:path",
        "//skylib:zip",
        "@bazel_tools//tools/build_defs/hash:hash.bzl",
    ],
)

skylark_library(
    name = "layer",
    srcs = ["layer.bzl"],
    deps = [
        ":layer_tools",
        ":providers",
        "//skylib:filetype",
        "//skylib:path",
        "//skylib:zip",
        "@bazel_tools//tools/build_defs/hash:hash.bzl",
    ],
)

skylark_library(
    name = "layer_tools",
    srcs = ["layer_tools.bzl"],
    deps = ["//skylib:path"],
)

skylark_library(
    name = "load",
    srcs = ["load.bzl"],
    deps = [":pull"],
)

skylark_library(
    name = "providers",
    srcs = ["providers.bzl"],
)

skylark_library(
    name = "pull",
    srcs = ["pull.bzl"],
)

skylark_library(
    name = "push",
    srcs = ["push.bzl"],
    deps = [
        ":layer_tools",
        ":providers",
        "//skylib:path",
    ],
)
