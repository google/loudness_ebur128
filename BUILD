load("@rules_cc//cc:cc_library.bzl", "cc_library")
load("@rules_license//rules:license.bzl", "license")

package(default_applicable_licenses = [":package_license"])

license(
    name = "package_license",
    license_text = "LICENSE",
)

licenses(["notice"])

exports_files(["LICENSE"])

cc_library(
    name = "ebur128_analyzer",
    srcs = [
        "src/ebur128_analyzer.cc",
        "src/k_weighting.cc",
    ],
    hdrs = [
        "src/audio_data_access_patterns.h",
        "src/ebur128_analyzer.h",
        "src/ebur128_constants.h",
        "src/k_weighting.h",
    ],
)
