#
#    Copyright (c) 2020 Project CHIP Authors
#
#    Licensed under the Apache License, Version 2.0 (the "License");
#    you may not use this file except in compliance with the License.
#    You may obtain a copy of the License at
#
#        http://www.apache.org/licenses/LICENSE-2.0
#
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS,
#    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#    See the License for the specific language governing permissions and
#    limitations under the License.
#

#
#    Description:
#      This is the GNU automake file for the CHIP shell unit and functional tests.
#

include $(abs_top_nlbuild_autotools_dir)/automake/pre.am

#
# Local headers to build against and distribute but not to install
# since they are not part of the package.
#
noinst_HEADERS                                 = \
    $(NULL)

#
# Other files we do want to distribute with the package.
#
EXTRA_DIST                                     = \
    $(NULL)

if CHIP_BUILD_TESTS

lib_LIBRARIES                                         = \
    libTestShell.a                                      \
    $(NULL)

libTestShell_a_SOURCES                                = \
    TestShell.cpp                                       \
    TestStreamerStdio.cpp                               \
    $(NULL)

libTestShell_adir                                     = $(includedir)/shell

dist_libTestShell_a_HEADERS                           = \
    TestShell.h                                         \
    $(NULL)


# C/C++ preprocessor option flags that will apply to all compiled
# objects in this makefile.

AM_CPPFLAGS                                    = \
    -I$(top_srcdir)/src                          \
    -I$(top_srcdir)/src/lib                      \
    $(NLASSERT_CPPFLAGS)                         \
    $(NLUNIT_TEST_CPPFLAGS)                      \
    $(PTHREAD_CFLAGS)                            \
    $(NULL)

CHIP_LDADD                                          = \
    $(top_builddir)/src/lib/shell/libChipShell.a      \
    $(top_builddir)/src/lib/libCHIP.a                 \
    $(NULL)

COMMON_LDADD                                        = \
    libTestShell.a                                    \
    $(CHIP_LDADD)                                     \
    $(NLUNIT_TEST_LDFLAGS) $(NLUNIT_TEST_LIBS)        \
    $(PTHREAD_CFLAGS) $(PTHREAD_LIBS)                 \
    $(NULL)

# Test applications that should be run when the 'check' target is run.

check_PROGRAMS                                 = \
    TestStreamerStdio                            \
    TestShell                                    \
    $(NULL)

# Test applications and scripts that should be built and run when the
# 'check' target is run.

TESTS                                          = \
    $(check_PROGRAMS)                            \
    $(NULL)

# The additional environment variables and their values that will be
# made available to all programs and scripts in TESTS.

TESTS_ENVIRONMENT                              = \
    $(NULL)

# Source, compiler, and linker options for test programs.

TestStreamerStdio_LDADD                        = $(COMMON_LDADD)
TestStreamerStdio_SOURCES                      = TestStreamerStdioDriver.cpp

TestShell_LDADD                                = $(COMMON_LDADD)
TestShell_SOURCES                              = TestShellDriver.cpp

#
# Foreign make dependencies
#

NLFOREIGN_FILE_DEPENDENCIES                           = \
   $(CHIP_LDADD)                                        \
   $(NULL)

NLFOREIGN_SUBDIR_DEPENDENCIES                         = \
   $(NLUNIT_TEST_FOREIGN_SUBDIR_DEPENDENCY)             \
   $(NULL)

if CHIP_BUILD_COVERAGE
CLEANFILES                                     = $(wildcard *.gcda *.gcno)

if CHIP_BUILD_COVERAGE_REPORTS
# The bundle should positively be qualified with the absolute build
# path. Otherwise, VPATH will get auto-prefixed to it if there is
# already such a directory in the non-colocated source tree.

CHIP_COVERAGE_BUNDLE                           = ${abs_builddir}/${PACKAGE}${NL_COVERAGE_BUNDLE_SUFFIX}
CHIP_COVERAGE_INFO                             = ${CHIP_COVERAGE_BUNDLE}/${PACKAGE}${NL_COVERAGE_INFO_SUFFIX}

$(CHIP_COVERAGE_BUNDLE):
	$(call create-directory)

$(CHIP_COVERAGE_INFO): check-local | $(CHIP_COVERAGE_BUNDLE)
	$(call generate-coverage-report,${top_builddir},*/usr/include/* */third_party/*)

coverage-local: $(CHIP_COVERAGE_INFO)

clean-local: clean-local-coverage

.PHONY: clean-local-coverage
clean-local-coverage:
	-$(AM_V_at)rm -rf $(CHIP_COVERAGE_BUNDLE)
endif # CHIP_BUILD_COVERAGE_REPORTS
endif # CHIP_BUILD_COVERAGE
endif # CHIP_BUILD_TESTS

include $(abs_top_nlbuild_autotools_dir)/automake/post.am
