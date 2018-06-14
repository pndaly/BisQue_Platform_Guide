|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

.. _step5.rst:

Update The Dependencies In The BisQue Docker Image
--------------------------------------------------

**There are several ways of accomplishing this but we take the simplest route. This requires that, after this step,
you leave the container running. If the container stops, you will have to re-execute these commands!**

Run The Docker Image With Local Directories Attached
````````````````````````````````````````````````````

Run the image, with the local directories and the GitHub repository attached:

.. code-block:: bash
  :emphasize-lines: 1-4

  % docker run -p 9898:8080 \
  -v ~/bisque-docker/container-modules:/source/modules \
  -v ~/bisque-docker/container-data:/source/data \
  cbiucsb/bisque05:stable

Get The Name Or ID Of The Running Container
```````````````````````````````````````````

Now, in a separate window, we want to connect to the running container. To do this you will need to know the ID or
name of the running container which can be found like so:


.. code-block:: bash
  :emphasize-lines: 2

  # get the container name
  % list_containers | grep bisque05 | rev | cut -d' ' -f1
  trusting_lovelace

or:

.. code-block:: bash
  :emphasize-lines: 2

  # get the container ID
  % list_containers | grep bisque05 | cut -d' ' -f1
  d5eb3d8e117f

In the above example, we see that the ID is `d5eb3d8e117f` and the name is `trusting_lovelace`.

Connect To The Running Container
````````````````````````````````

Connect to this (in this example using the name):

.. code-block:: bash
  :emphasize-lines: 1

  % docker exec -it trusting_lovelace bash
  root@d5eb3d8e117f:/source#

Note that the prompt changes (in this case to *root@d5eb3d8e117f:/source#*) indicating that we are connected as *root*
inside the running container with ID *d5eb3d8e117f* (also known as *trusting_lovelace* in this example)

Update The Running Container
````````````````````````````

Once *inside* the container, we need to update the operating system:

.. code-block:: bash
  :emphasize-lines: 2-6

  # inside the container, run these command(s)
  % apt-get -y update && apt-get -y upgrade && apt-get autoremove
  % apt-get -y install python-pip python-setuptools python-dev \
    python-lxml python-tk python-matplotlib python-numpy \
    python-scipy python-skimage gcc g++ curl
  % curl https://bootstrap.pypa.io/get-pip.py | python

These commands might take some time to run.

Update The Python Dependencies Within The Running Container
```````````````````````````````````````````````````````````

However, the BisQue code also runs in its own sandbox so, within the container, we must activate it *before* continuing
to update the system:

.. code-block:: bash
  :emphasize-lines: 2

  # inside the container, run this command
  % source /usr/lib/bisque/bin/activate
  (bisque) root@d5eb3d8e117f:/source/modules/PlanteomeDeepSegment#

Now, we can update the system for the specific requirements of the PlanteomeDeepSegment module:

.. code-block:: bash
  :emphasize-lines: 2, 3

  # inside the container and the sandbox, run this command
  % cd /source/modules/PlanteomeDeepSegment
  % pip install -r requirements.txt

This, too, may take some time to run. At the present time, any errors should be ignored.

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
