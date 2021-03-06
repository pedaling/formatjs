load("@build_bazel_rules_nodejs//:index.bzl", "generated_file_test", "pkg_npm")
load("//tools:index.bzl", "check_format", "ts_compile")
load("//tools:jest.bzl", "jest_test")

PACKAGE_NAME = "ts-transformer"

pkg_npm(
    name = PACKAGE_NAME,
    package_name = "@formatjs/%s" % PACKAGE_NAME,
    srcs = [
        "LICENSE.md",
        "README.md",
    ],
    deps = [":dist"],
)

alias(
    name = "publish",
    actual = "%s.publish" % PACKAGE_NAME,
)

alias(
    name = "pack",
    actual = "%s.pack" % PACKAGE_NAME,
)

SRCS = glob([
    "src/*.ts",
    "*.ts",
])

SRC_DEPS = [
    "@npm//typescript",
    "@npm//@types/node",
    "//packages/icu-messageformat-parser:dist",
]

ts_compile(
    name = "dist",
    package_name = "@formatjs/%s" % PACKAGE_NAME,
    srcs = SRCS,
    deps = SRC_DEPS,
)

jest_test(
    name = "unit",
    srcs = SRCS + glob([
        "tests/*.ts*",
        "tests/fixtures/*.ts*",
        "tests/__snapshots__/*.snap",
    ]),
    deps = SRC_DEPS + [
        "//packages/icu-messageformat-parser:types",
    ],
)

jest_test(
    name = "integration",
    srcs = SRCS + glob([
        "tests/integration/*.ts*",
    ]),
    jest_config = "tests/integration/jest.config.js",
    deps = [
        ":dist",
        "//packages/icu-messageformat-parser:types",
        "@npm//@types/react",
        "@npm//@types/react-dom",
        "@npm//react",
        "@npm//react-dom",
    ],
)

generated_file_test(
    name = "tsconfig_json",
    src = "tsconfig.json",
    generated = "//tools:tsconfig.golden.json",
)

check_format(
    name = "prettier",
    srcs = glob(
        [
            "**/*",
        ],
        exclude = [
            "CHANGELOG.md",
            "tests/__snapshots__/*",
        ],
    ),
)

jest_test(
    name = "integration-vue",
    srcs = ["tests/integration-vue/integration.test.ts"] + glob([
        "tests/integration-vue/fixtures/*",
    ]),
    flaky = True,
    deps = [
        ":dist",
        ":types",
        "//packages/icu-messageformat-parser:types",
        "@npm//@types/node",
        "@npm//@types/webpack",
        "@npm//@vue/compiler-sfc",
        "@npm//ts-loader",
        "@npm//vue",
        "@npm//vue-class-component",
        "@npm//vue-loader",
        "@npm//webpack",
    ],
)
