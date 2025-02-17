Next Release
------------

Breaking changes:

- deprecate the function ``pytest_container.container_from_pytest_param``,
  please use
  :py:func:`~pytest_container.container.container_and_marks_from_pytest_param`
  instead.


Improvements and new features:


Documentation:


Internal changes:


0.3.0 (26 September 2023)
-------------------------

Breaking changes:

- Removed the function ``OciRuntimeABC.get_image_id_from_stdout`` as docker
  buildx does not print the image digest to stdout when invoking
  :command:`docker build`.


Improvements and new features:

- Add :py:attr:`~pytest_container.container.ContainerBaseABC.baseurl` property
  to get the registry url of the container on which any currently existing
  container is based on.


Documentation:


Internal changes:

- use ``--cidfile`` and ``--iidfile`` flags to get the container and image
  hashes from files instead of stdout.


0.2.0 - DevConf.cz edition (14 June 2023)
-----------------------------------------

Breaking changes:


Improvements and new features:

- Log the the output of :command:`$runtime logs $container` using Python's
  logging framework for easier debugging

- Automatically set the image format to ``docker`` when using :command:`buildah`
  if the base image is using ``HEALTHCHECK`` (with :command:`buildah` version
  1.25 and later).

- Add support for Python 3.11

- Log the container's logs even if launching the container fails, e.g. due to a
  failing ``HEALTHCHECK``.

Documentation:


Internal changes:


0.1.1 (21 March 2023)
---------------------

This release only fixes the README.rst formatting. There are no functional
changes compared to 0.1.0.


0.1.0 (20 March 2023)
---------------------

Breaking changes:

- ``ContainerBase.healtcheck_timeout_ms`` got renamed to
  :py:attr:`~pytest_container.container.ContainerBase.healthcheck_timeout` and was
  changed as follows: it is now a :py:class:`~datetime.timedelta` with the
  default value being ``None`` and implies that ``pytest_container`` figures the
  maximum timeout out itself. If a positive timedelta is provided, then that
  timeout is used instead of the inferred default and if it is negative, then no
  timeout is applied.

- :py:attr:`~pytest_container.container.ContainerBase.entry_point` is no longer
  a property. It is instead a setting how the entry point for a container image
  is picked. Consequently, the attribute ``ContainerBase.default_entry_point``
  was removed.

- ``OciRuntimeABC.get_container_healthcheck`` was removed, use
  :py:attr:`~pytest_container.container.ContainerData.inspect` instead.

Improvements and new features:

- The Entrypoint is now picked automatically from the image, removing the need
  for setting `default_entry_point=True`.

- Cleanup automatically created volumes from ``VOLUME`` directives in
  :file:`Dockerfile`.

- Allow to inspect containers via a pythonic interface via
  :py:attr:`~pytest_container.container.ContainerData.inspect`

- Add support for creating podman pods for testing via the
  :py:class:`~pytest_container.pod.Pod` class.

- Add support for automatically exposing ports in containers via the
  :py:attr:`~pytest_container.container.ContainerBase.forwarded_ports`
  attribute: Container Images can now define which ports they want to publish
  automatically and let the `container_*` fixtures automatically find the next
  free port for them. This allows the user to launch multiple containers from
  Container Images exposing the same ports in parallel without marking them as
  ``singleton=True``.

- The attribute :py:attr:`~pytest_container.container.ContainerData.container`
  was added to :py:class:`~pytest_container.container.ContainerData` (the
  datastructure that is passed to test functions via the ``*container*``
  fixtures). This attribute contains the
  :py:class:`~pytest_container.container.ContainerBase` that was used to
  parametrize this test run.

- Add support to add tags to container images via
  :py:attr:`~pytest_container.container.DerivedContainer.add_build_tags`.

- Lock container preparation so that only a single process is pulling & building
  a container image.

- Add the helper class :py:class:`~pytest_container.runtime.Version` for parsing
  and comparing versions.

- Container volumes and bind mounts can now be automatically created via the
  :py:class:`~pytest_container.container.ContainerVolume` and
  :py:class:`~pytest_container.container.BindMount` classes and adding them to
  the :py:attr:`~pytest_container.container.ContainerBase.volume_mounts`
  attribute.


Documentation:

- Add a tutorial how to start using ``pytest_container``

- Document most public and private functions, classes and modules


Internal changes:

- Switch from tox to nox and nox-poetry.

- Add `typeguard <https://typeguard.readthedocs.io/en/stable/index.html>`_ to
  the test runs to check type hints.

- Use context managers in the fixtures to make the code more readable and
  robust.


0.0.2 (01 February 2022)
------------------------

Breaking changes:


Improvements and new features:

 - Support healthcheck in Container images
 - Add support for internal logging and make the level user configurable
 - Allow for singleton container images
 - Add support for passing run & build arguments via the pytest CLI to podman/docker
 - Add support for adding environment variables into containers

Documentation:

 - treat unresolved references as errors
 - enable intersphinx

Internal changes:

 - Provide a better error message in auto_container_parametrize
 - Add support for using pytest.param instead of Container classes
