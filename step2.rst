|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

.. _step2.rst:

Get And Run The BisQue Docker Image
-----------------------------------

**First, get the latest dockerized version of BisQue and make sure it runs locally:**

.. code-block:: bash
  :emphasize-lines: 1

  docker run -p 9898:8080 cbiucsb/bisque05:stable

- point your browser to http://localhost:9898/
- login with `username /password` = `admin / admin`
- when done, stop the container (from a different terminal) with:

.. code-block:: bash
  :emphasize-lines: 1

  stop_containers

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