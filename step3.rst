|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

.. _step3.rst:

Add Local Directories To The BisQue Docker Image
------------------------------------------------

**Now, create some directories where you can develop code and store images:**

.. code-block:: bash
  :emphasize-lines: 1-3

  % mkdir ~/bisque-docker
  % mkdir ~/bisque-docker/container-data
  % mkdir ~/bisque-docker/container-modules

**Attach these directories to the docker container:**

.. code-block:: bash
  :emphasize-lines: 1-4

  % docker run -p 9898:8080 \
  -v ~/bisque-docker/container-modules:/source/modules \
  -v ~/bisque-docker/container-data:/source/data \
  cbiucsb/bisque05:stable

- point your browser to http://localhost:9898/
- login with `username /password` = `admin / admin`
- when done, stop the container (from a different terminal) with:

.. code-block:: bash
  :emphasize-lines: 1, 2

  % stop_containers
  % remove_containers

|

----

**Fix or improve this documentation:**

- On Github: https://github.com/pndaly/BisQue_Platform_Guide.git
- Send feedback: `Tutorials@CyVerse.org <Tutorials@CyVerse.org>`_

----

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

|Bisque_Icon|_
`Bio-Image Semantic Query User Environment <http://bisque.cyverse.org>`_

.. |CyVerse logo| image:: ./img/cyverse_rgb.png
    :width: 500
    :height: 100
.. |Home_Icon| image:: ./img/homeicon.png
    :width: 25
    :height: 25
.. |Bisque_Icon| image:: ./img/bisque/Bisque-Icon.png
    :width: 25
    :height: 25
.. |Bisque_Logo| image:: ./img/bisque/Bisque-Logo.png
    :width: 50
    :height: 20
.. _CyVerse logo: http://learning.cyverse.org/
.. _Home_Icon: http://learning.cyverse.org/
.. _Bisque_Icon: http://bisque.cyverse.org/
