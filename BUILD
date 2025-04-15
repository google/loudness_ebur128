load("@rules_cc//cc:cc_library.bzl", "cc_library")
load("@rules_license//rules:license.bzl", "license")

package(default_applicable_licenses = [":package_license"])

license(
    name = "package_license",
    license_text = "LICENSE",
)

licenses(["notice"])

exports_files(["LICENSE"])

# Public API.
cc_library(
    name = "ebur128_analyzer",
    hdrs = [
        "include/ebur128_analyzer.h",
    ],
    visibility = ["//visibility:public"],
    deps = [
        ":ebur128_analyzer_internal",
    ],
)

# Internal libraries, to hide various headers and implementation details.
cc_library(
    name = "ebur128_analyzer_internal",
    srcs = [
        "src/ebur128_analyzer.cc",
        "src/k_weighting.cc",
    ],
    hdrs = [
        "include/ebur128_analyzer.h",
        "src/audio_data_access_patterns.h",
        "src/ebur128_constants.h",
        "src/k_weighting.h",
    ],
    visibility = ["//visibility:private"],
)
