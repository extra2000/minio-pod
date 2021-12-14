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

    podman build -t extra2000/minio .

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

Create pod file:

.. code-block:: bash

    cp -v elk-minio-01-pod.yaml{.example,}

Create CA-signed SSL Certificate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create certificate configs:

.. code-block:: bash

    cp -v secrets/certs/configs/minio-ca-crt.conf{.example,}
    cp -v secrets/certs/configs/minio-ca-policy.conf{.example,}
    cp -v secrets/certs/configs/minio-01-csr.conf{.example,}

.. note::

    You may need to edit ``*conf`` files in ``secrets/certs/configs/``.

``cd`` into ``secrets/certs/`` directory:

.. code-block:: bash

    cd secrets/certs/

Create private key for the ``minio-01`` host:

.. code-block:: bash

    openssl genrsa -out output/minio-01.key 2048

Create CSR for the ``minio-01`` host:

.. code-block:: bash

    openssl req -new -key output/minio-01.key -out output/minio-01.csr -config configs/minio-01-csr.conf

Create private key for MinIO CA certificate:

.. code-block:: bash

    openssl genrsa -aes128 -out output/minio-ca.key 2048

.. note::

    You can use password ``abcde12345`` for testing purpose.

Create MinIO CA certificate:

.. code-block:: bash

    openssl req -new -x509 -key output/minio-ca.key -out output/minio-ca.crt -config configs/minio-ca-crt.conf -days 1825

Create ``index.txt`` and ``serial`` files required during signing certificates:

.. code-block:: bash

    touch index.txt
    echo '01' > serial

Sign ``minio-01`` certificate:

.. code-block:: bash

    openssl ca -config configs/minio-ca-policy.conf -out output/minio-01.crt -infiles output/minio-01.csr

Verify certificates:

.. code-block:: bash

    openssl verify -CAfile output/minio-ca.crt output/minio-01.crt

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

    podman play kube --network elknet --configmap configmaps/minio-01.yaml --seccomp-profile-root ./seccomp elk-minio-01-pod.yaml

MinIO web console is now accessible at https://127.0.0.1:9001. Login with username ``minio`` and password ``minio123``.
