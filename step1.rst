|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

.. _step1.rst:

Some Useful Utilities For Working With Docker
---------------------------------------------

**Put these utility functions and aliases in your ${HOME}/.bash_profile (or some other suitable place) to make sure
they are executed at runtime.**

For Docker Container(s)
```````````````````````

.. code-block:: bash
  :emphasize-lines: 1, 2, 4-12, 14-22

  alias list_containers='docker container ls -a'
  alias list_container_ids='docker container ls -aq'

  function stop_containers () {
    _sc=$(docker container ls -aq)
    if [ ! -z "${_sc}" ]; then
      echo "docker container stop ${_sc}"
      docker container stop ${_sc}
    else
      echo "No containers to stop"
    fi
  }

  function remove_containers () {
    _rc=$(docker ps -aq -f "status=exited")
    if [ ! -z "${_rc}" ]; then
      echo "docker container rm --force ${_rc}"
      docker container rm --force ${_rc}
    else
      echo "No containers to remove"
    fi
  }

For Docker Image(s)
```````````````````

.. code-block:: bash
  :emphasize-lines: 1, 2, 4, 5, 7-15, 17-25, 27-30

  alias list_images='docker image ls -a'
  alias list_image_ids='docker image ls -aq'

  alias list_all_images='docker image ls -a -f "dangling=true"'
  alias list_all_image_ids='docker image ls -aq -f "dangling=true"'

  function remove_images () {
    _ri=$(docker image ls -aq)
    if [ ! -z "${_ri}" ]; then
      echo "docker rmi --force ${_ri}"
      docker rmi --force ${_ri}
    else
      echo "No images to remove"
    fi
  }

  function remove_all_images () {
    _ri=$(docker image ls -aq -f "dangling=true")
    if [ ! -z "${_ri}" ]; then
      echo "docker rmi --force ${_ri}"
      docker rmi --force ${_ri}
    else
      echo "No images to remove"
    fi
  }

  function nuke_images () {
    remove_all_images
    remove_images
  }

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