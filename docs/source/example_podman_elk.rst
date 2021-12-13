Example Podman Deployment for ELK
=================================

Example how to deploy a small MinIO Podman pod instance for ELK.

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

Create ``elknet`` podman network from `extra2000/elastic-elasticsearch-pod`_ project.

.. _extra2000/elastic-elasticsearch-pod: https://github.com/extra2000/elastic-elasticsearch-pod

Prerequisites for ``minio-01``
------------------------------

From the project's root directory, ``cd`` into ``deployment/examples/podman-elk``:

.. code-block:: bash

    cd deployment/examples/podman-elk

Create config file:

.. code-block:: bash

    cp -v configmaps/minio-01.yaml{.example,}

Load SELinux Security Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo semodule -i selinux/elk_minio_01_pod_minio_01.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "elk_minio_01_pod_minio_01"

Deployment
----------

Deploy ``minio-01``
~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo podman play kube --network elknet --configmap configmaps/minio-01.yaml --seccomp-profile-root ./seccomp elk-minio-01-pod.yaml

MinIO web console is now accessible at http://127.0.0.1:9001. Login with username ``minio`` and password ``minio123``.
