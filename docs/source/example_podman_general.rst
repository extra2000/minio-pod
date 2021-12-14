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

    podman build -t extra2000/minio .

Prerequisites for ``minio-01``
------------------------------

From the project's root directory, ``cd`` into ``deployment/examples/podman-general``:

.. code-block:: bash

    cd deployment/examples/podman-general

Create config file:

.. code-block:: bash

    cp -v configmaps/minio-01.yaml{.example,}

Create pod file:

.. code-block:: bash

    cp -v minio-01-pod.yaml{.example,}

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

Upload required certificates to the ``minio-01`` host:

.. code-block:: bash

    scp -P 22 ./secrets/certs/output/minio-01.crt USER@MINIO-01-IP:extra2000/minio-pod/deployment/examples/podman-general/secrets/certs/output/
    scp -P 22 ./secrets/certs/output/minio-01.key USER@MINIO-01-IP:extra2000/minio-pod/deployment/examples/podman-general/secrets/certs/output/

On the ``minio-01`` host, make sure the certificates are mountable by Podman:

.. code-block:: bash

    chcon -R -v -t container_file_t secrets/certs/output/

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

    podman play kube --configmap configmaps/minio-01.yaml --seccomp-profile-root ./seccomp minio-01-pod.yaml

MinIO web console is now accessible at https://127.0.0.1:9001. Login with username ``minio`` and password ``minio123``.

Generate ``systemd`` files and enable on ``boot``:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name minio-01-pod
    systemctl --user enable pod-minio-01-pod.service container-minio-01-pod-minio-01.service

.. note::

    If the pod is destroyed and recreated, the ``systemd`` files must be recreated using the command above.
