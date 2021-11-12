workspace(name = "tf_serving")
# Import Yggdrasil Decision Forests.
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
http_archive(
    name="ydf",
    urls=[
        "https://github.com/google/yggdrasil-decision-forests/archive/refs/tags/0.2.0.zip"],
    strip_prefix="yggdrasil-decision-forests-0.2.0",
    sha256 = "3a295aeece7267eda600866452e0edadf862b099dbb629080ff76ff289f10beb",
)

# Load the YDF dependencies. However, skip the ones already injected by
# TensorFlow.
load("@ydf//yggdrasil_decision_forests:library.bzl",
     ydf_load_deps="load_dependencies")
ydf_load_deps(
    exclude_repo=[
        "absl",
        "protobuf",
        "zlib",
        "farmhash",
        "gtest",
        "tensorflow",
        "grpc"
    ],
    repo_name="@ydf",
)

# Import TensorFlow Decision Forests.
load("//tensorflow_serving:repo.bzl", "tensorflow_http_archive")
http_archive(
    name="org_tensorflow_decision_forests",
    urls=[
        "https://github.com/tensorflow/decision-forests/archive/refs/tags/0.2.0.zip"],
    strip_prefix="decision-forests-0.2.0",
)
# ===== TensorFlow dependency =====
#
# TensorFlow is imported here instead of in tf_serving_workspace() because
# existing automation scripts that bump the TF commit hash expect it here.
#
# To update TensorFlow to a new revision.
# 1. Update the 'git_commit' args below to include the new git hash.
# 2. Get the sha256 hash of the archive with a command such as...
#    curl -L https://github.com/tensorflow/tensorflow/archive/<git hash>.tar.gz | sha256sum
#    and update the 'sha256' arg with the result.
# 3. Request the new archive to be mirrored on mirror.bazel.build for more
#    reliable downloads.
load("//tensorflow_serving:repo.bzl", "tensorflow_http_archive")
tensorflow_http_archive(
    name = "org_tensorflow",
    sha256 = "8e0a3d56536bf902deb635e3b194b1a70084373d0b683a72a969ec10eceab3c4",
    git_commit = "da585e4c3aee1745aa3e425efb4f1b60019f3e26",
)

# Import all of TensorFlow Serving's external dependencies.
# Downstream projects (projects importing TensorFlow Serving) need to
# duplicate all code below in their WORKSPACE file in order to also initialize
# those external dependencies.
load("//tensorflow_serving:workspace.bzl", "tf_serving_workspace")
tf_serving_workspace()

# Check bazel version requirement, which is stricter than TensorFlow's.
load(
    "@org_tensorflow//tensorflow:version_check.bzl",
    "check_bazel_version_at_least"
)
check_bazel_version_at_least("3.7.2")

# Initialize TensorFlow's external dependencies.
load("@org_tensorflow//tensorflow:workspace3.bzl", "workspace")
workspace()
load("@org_tensorflow//tensorflow:workspace2.bzl", "workspace")
workspace()
load("@org_tensorflow//tensorflow:workspace1.bzl", "workspace")
workspace()
load("@org_tensorflow//tensorflow:workspace0.bzl", "workspace")
workspace()

# Initialize bazel package rules' external dependencies.
load("@rules_pkg//:deps.bzl", "rules_pkg_dependencies")
rules_pkg_dependencies()

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies", "rules_proto_toolchains")

rules_proto_dependencies()
rules_proto_toolchains()

