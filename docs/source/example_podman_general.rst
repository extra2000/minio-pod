Example Podman Deployment for General Purpose
=============================================

Example how to deploy a small MinIO Podman pod instance for general purpose.

Getting Started
---------------

Clone this repository:

.. code-block:: bash

    git clone https://github.com/extra2000/minio-pod.git

``cd`` into project's root directory:

.. code-block:: bash

    cd minio-pod

From the project's root directory, ``cd`` into ``deployment/examples/``:

.. code-block:: bash

    cd deployment/examples/

Build our MinIO image:

.. code-block:: bash

    sudo podman build -t extra2000/minio .

Prerequisites for ``minio-01``
------------------------------

From the project's root directory, ``cd`` into ``deployment/examples/podman-general``:

.. code-block:: bash

    cd deployment/examples/podman-general

Create config file:

.. code-block:: bash

    cp -v configmaps/minio-01.yaml{.example,}

Load SELinux Security Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo semodule -i selinux/minio_01_pod_minio_01.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "minio_01_pod_minio_01"

Deployment
----------

Deploy ``minio-01``
~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo podman play kube --configmap configmaps/minio-01.yaml --seccomp-profile-root ./seccomp minio-01-pod.yaml

MinIO web console is now accessible at http://127.0.0.1:9001. Login with username ``minio`` and password ``minio123``.

Generate ``systemd`` files and enable on ``boot``:

.. code-block:: bash

    cd /etc/systemd/system/
    sudo podman generate systemd --files --name minio-01-pod
    sudo systemctl enable pod-minio-01-pod.service container-minio-01-pod-minio-01.service

.. note::

    If the pod is destroyed and recreated, the ``systemd`` files must be recreated using the command above.
