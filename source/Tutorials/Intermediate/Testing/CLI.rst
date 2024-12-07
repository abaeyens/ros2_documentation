.. TestingCLI:

Running Tests in ROS 2 from the Command Line
============================================

Build and run your tests
^^^^^^^^^^^^^^^^^^^^^^^^

To compile and run the tests, simply run the `test <https://colcon.readthedocs.io/en/released/reference/verb/test.html>`__ verb from ``colcon``.

.. code-block:: console

  colcon test --ctest-args tests [package_selection_args]

(where ``package_selection_args`` are optional package selection arguments for ``colcon`` to limit which packages are built and run)

:ref:`Sourcing the workspace <colcon-tutorial-source-the-environment>` before testing should not be necessary.
``colcon test`` makes sure that the tests run with the right environment, have access to their dependencies, etc.

The above command suppresses the test output and exposes little about which tests succeed and which fail.
Therefore while developing tests the ``--event-handlers`` option is useful to print all test output while the tests are running:

.. code-block:: console

  colcon test --event-handlers console_direct+

Examine Test Results
^^^^^^^^^^^^^^^^^^^^

To see the results, simply run the `test-result <https://colcon.readthedocs.io/en/released/reference/verb/test-result.html>`__ verb from ``colcon``:

.. code-block:: console

  $ colcon test-result --all
  build/app/Testing/20241013-0810/Test.xml: 1 tests, 0 errors, 1 failure, 0 skipped
  build/app/test_results/app/test_test_myapp.py.xunit.xml: 3 tests, 0 errors, 1 failure, 0 skipped

  Summary: 4 tests, 0 errors, 2 failures, 0 skipped

In the example above, the command lists two files:

* a ctest-formatted XML file (a result of the ``CMakeLists.txt``)
* an XUnit-formatted XML file, which is suitable for automatic report generation in automated testing in CI/CD pipelines.

To see the exact test cases which fail, use the ``--verbose`` flag:

.. code-block:: console

  colcon test-result --all --verbose

A suitable non-ROS specific tool to visualize them all together is the
`NodeJS package Xunit Viewer <https://github.com/lukejpreston/xunit-viewer>`_.
It converts the XUnit files to HTML or straight into the terminal.
For example, command and response (without highlighting):

.. code-block:: console

  $ xunit-viewer -r build/app/test_results -c
    app.test_integration.launch_tests
        ✗ test_publishes_pose time=0.52
          - Traceback (most recent call last):
    File "/home/user/ros_workspace/src/app/test/test_myapp.py", line 67, in test_publishes_pose
        assert len(msgs_rx) > 100
            ^^^^^^^^^^^^^^^^^^
    AssertionError
        ✓ test_exit_codes time=0.0
        ✓ test_logs_spawning time=0.197

  1 failure, 2 passed
  Written to: /home/user/ros_workspace/index.html

Debugging tests with GDB
^^^^^^^^^^^^^^^^^^^^^^^^

For detailed guidance on debugging tests using GDB, refer to the :doc:`GDB Tutorial <../../../How-To-Guides/Getting-Backtraces-in-ROS-2>`.
