How to contribute to Flux
==========================

Thank you for considering contributing to Flux!

Reporting issues
----------------

- Describe what you expected to happen.
- If possible, include a :report_issue:`minimal, complete, and verifiable <mcve>` example to help
  us identify the issue. This also helps check that the issue is not with your
  own code.
- Describe what actually happened. Include the full traceback if there was an
  exception.

Submitting patches
------------------

- Include tests if your patch is supposed to solve a bug, and explain
  clearly under which circumstances the bug happens. Make sure the test fails
  without your patch.
- Make sure all commits are verified.
- Make sure there are no trailing spaces in the any of the modified files.


Start coding
------------

- Create a branch to identify the issue you would like to work on (e.g.
  ``2287-dry-test-suite``)
- Using your favorite editor, make your changes, :make_changes:`committing as you go <commit-your-changes>`.
- Make sure there are no trailing spaces in the any of the modified files.
- Include tests that cover any code changes you make. Make sure the test fails
  without your patch.
- Push your commits to GitHub and :pull_request:`create a pull request <creating-a-pull-request>`.
- Celebrate 🎉

Running the tests
-----------------

Run the test suite with:

.. code-block:: sh

    pytest --cov-report term-missing --cov=flux tests/   

Building the docs
-----------------

In order to build the doc you need to have :build_docs:`Sphinx 1.8 <installation>` or later installed. If you have it or after installing it you can generate the documentation with:

.. code-block:: sh

    cd docs
    make html

This will create the documentation in the ``build/html`` directory.
