==============
scrying-docker
==============

A simple Docker image to run the Scrying screenshoting utility in a container.

.. contents:: Table of Contents

Install
-------

To build and run the container:

.. code-block:: bash

   git clone https://github.com/darknetehf/scrying-docker.git
   cd scrying-docker
   # create the screenshots directory manually on the host
   mkdir screenshots

   export HOST_UID=$(id -u)
   export HOST_GID=$(id -g)
   docker compose build --build-arg UID=$HOST_UID --build-arg GID=$HOST_GID
   docker compose up -d

This will create a user with UID and GID matching the current user on the host. This is to align ownership of the screenshots folder (bind mount).

Attach to the running container (default user is: anonymous):

.. code-block:: bash

   docker exec -it scrying /bin/bash

Notes
-----

- The Docker file uses a **bind mount** for the screenshots directory instead of a named volume. This is to make it easier to access the screenshots from the host
- This Docker file uses **Chromium** instead of Chrome
- Because the Chromium browser attempts to run in **sandbox mode**, it needs namespace capabilities that are denied by default in Docker. A possible workaround would be to run the browser with the `--no-sandbox` option. Or running the container with higher privileges eg: `cap_add: SYS_ADMIN`, which can be detrimental to security and isolation. Instead, we attempt to leverage **seccomp** for more granual permissions.
- **xvfb** may be required on a headless environment

Limitations
-----------

- The Docker file is not much optimized amd may result in bigger than necessary image size
- Using a different Linux distro for the base image is encouraged if you don't need to add fancy packages while using Scrying

Usage
-----

Scanning a list of targets for RDP only:

.. code-block:: bash

   scrying -v --output /screenshots/ --file rdplist.csv--mode rdp

Running web screenshots on a headless server:

.. code-block:: bash

   xvfb-run scrying -v --output /screenshots/ --target www.githtub.com --mode web


Limitations & possible caveats
------------------------------

- VNC scraping has not been tested, scrying seems to get stuck on servers that require authentication. See issue: `<https://github.com/nccgroup/scrying/issues/47>`_ 
- RDP may have similar issues - see issue: `<https://github.com/nccgroup/scrying/issues/15>`_ 


Credits
-------

- `NCC Group <https://github.com/nccgroup>`_ for providing the Scrying tool
- `Jess Frazelle <https://github.com/jessfraz>`_ for providing the chrome.json (seccomp)
