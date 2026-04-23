# The documentation generation pipeline is started by running the `docs`
# target.  It takes the C++ code and the existing reStructuredText
# documentation and turns them into HTML files.  It is a bit complicated.
#
#  /─────────\  ┌───────┐  ┌───────┐  ┌──────┐    ┌──────┐
#  │Developer│  │Doxygen│  │Breathe│  │Exhale│    │Sphinx│
#  \────┬────/  └───┬───┘  └───┬───┘  └──┬───┘    └──┬───┘
#       │ C++ Code  │          │         │           │
#       │──────────>│          │         │           │
#       │           │          │         │           │
#       │           │   XML    │         │           │
#       │           │─────────>│         │           │
#       │           │          │         │           │
#       │           │          │  reST   │           │
#       │           │          │────────>│           │
#       │           │          │         │           │
#       │           │          │         │ API (reST)│
#       │           │          │         │ ──────────>
#       │           │          │         │           │
#       │           │ Tutorials (reST)   │           │
#       │───────────────────────────────────────────>│
#       │           │          │         │           │
#       │           │       HTML         │           │
#       │<───────────────────────────────────────────│
#  /────┴────\  ┌───┴───┐  ┌───┴───┐  ┌──┴───┐    ┌──┴───┐
#  │Developer│  │Doxygen│  │Breathe│  │Exhale│    │Sphinx│
#  \─────────/  └───────┘  └───────┘  └──────┘    └──────┘
#
# The goal is to create documentation that can be hosted on a website,
# resembling "Read the Docs" (https://docs.readthedocs.io).
#
# Read the Docs uses the Sphinx Python module to generate documentation.
# Sphinx (https://www.sphinx-doc.org) is designed to generate docs for Python.
#
# pntOS is written in C and C++, which Sphinx does not understand. A popular
# C/C++ documentation generator is Doxygen (https://doxygen.nl), which
# extracts API documentation from C/C++ files and presents it in a variety of
# formats, including HTML and XML.
#
# While we could just use Doxygen HTML output, we want to use Sphinx because
# we want to write our documentation in reStructuredText (.rst), and Doxygen
# does not support this.  Plus "Sphinx looks nicer".
#
# Sphinx cannot extract API docs from C/C++.  Doxygen can.  Sphinx does not
# understand Doxygen output.  This is where we start using Sphinx extensions.
#
# The Breathe extension (https://breathe.readthedocs.io) understands Doxygen
# XML output and can rewrite it into reStructuredText for Sphinx to consume.
# However, Breathe requires you to add directives to print this information
# for individual classes, functions, etc. Breathe provides directives like
# `.. doxygenindex::` which prints everything but is hard to navigate.
#
# The Exhale extension (https://exhale.readthedocs.io) rebuilds the Doxygen
# Class Hierarchy, File Hierarchy, and Page Hierarchy using Breathe directives
# for Sphinx to consume.  Then we do not need to use any Breathe directives to
# display API documentation, and can instead include `api-exhale/library_root`
# to show these automatically-generated hierarchies.
#
# The MathJax extension uses the MathJax javascript library
# (https://www.mathjax.org/) to render LaTeX equations.  For more info, see:
# https://www.sphinx-doc.org/en/master/usage/extensions/math.html
#
# With MathJax, the embedded search engine, and themes, we are careful to
# avoid anything which loads assets from 3rd-party domains.  This is both a
# good security practice, and required for the docs to render correctly when
# viewed on a local machine (`file://` protocol in the browser) as opposed to
# being hosted on a server.

subproject('mathjax')
subproject('pntos-data')

python = import('python').find_installation('python3')

run_command([python,
    meson.current_source_dir() / 'generate_version.py',
    meson.current_source_dir() / 'version.py'],
    meson.project_version(),
    check: true)

make = find_program('make', required: false, disabler: true)
run_target('docs', command: [make, '-C', '@CURRENT_SOURCE_DIR@', 'html'])
