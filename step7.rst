|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

.. _step7.rst:

Dockerizing The Module
----------------------

**Now that we have the module running in a (fairly) standard container, we can proceed to dockerize it.**

To do this, you will need the assistance of CyVerse staff so feel free to reach out to us. The PlanteomeDeepSegment
*Dockerfile* looks (something like) this:

.. code-block:: docker
  :emphasize-lines: 2, 5, 8, 11-13, 16, 19, 22-28, 31-34, 37-39, 42-44

  # use ubuntu base image
  FROM ubuntu:16.04

  # maintenance record
  MAINTAINER Phil Daly "pndaly@cyverse.org"

  # update the distro
  RUN apt-get -y update && apt-get -y upgrade && apt-get -y autoremove

  # install software
  RUN apt-get -y install python-pip python-setuptools python-dev \
    python-lxml python-tk python-matplotlib python-numpy       \
    python-scipy python-skimage gcc g++ curl

  # fix certificate issue?
  RUN curl https://bootstrap.pypa.io/get-pip.py | python

  # install git
  RUN apt-get -y install git-core

  # install python dependencies
  RUN pip install --upgrade pip setuptools
  RUN pip install --ignore-installed scipy==1.0.1
  RUN pip install torch==0.4.0 torchvision==0.2.1 numpy==1.14.3 \
    opencv-python==3.4.0.12 scikit-image==0.13.1 lxml==3.7.3  \
    matplotlib==2.2.2 cython==0.28.2 PyMaxflow==1.2.9         \
    pillow==4.1.1 requests==2.10.0 libtiff==0.4.0             \
    tifffile==0.14.0 bqapi==0.5.9

  # configure to clone a private repo
  RUN mkdir /root/.ssh/
  ADD id_rsa /root/.ssh/id_rsa
  RUN touch /root/.ssh/known_hosts
  RUN ssh-keyscan github.com >> /root/.ssh/known_hosts

  # copy code
  WORKDIR /module/workdir
  RUN git clone git@github.com:DimTrigkakis/PlanteomeDeepSegment_0.3.git
  RUN mv PlanteomeDeepSegment_0.3 PlanteomeDeepSegment

  # run command
  ENV PYTHONPATH /module/workdir/PlanteomeDeepSegment:/module/workdir
  ENV PATH /module/workdir/PlanteomeDeepSegment:/module/workdir:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  CMD [ 'python', 'PlanteomeDeepSegment.py' ]

A simple test is to check that the *Dockerfile* builds an image without error:

.. code-block:: bash
  :emphasize-lines: 1

  % docker build --no-cache -t ubisque:uplanteome -f Dockerfile .

This may take some time to complete. Once the *docker build* is complete, you can carry out a simple access test:

.. code-block:: bash
  :emphasize-lines: 1-3

  % docker run --name uplanteome ubisque:uplanteome ls -l /module/workdir/PlanteomeDeepSegment
  % stop_containers
  % remove_containers

However, the correct version of the dockerized image should be deployed to the *gims* server (see CyVerse staff for assistance).

For the dockerized version to work with *condor*, the runtime-module.cfg should reflect both options:

.. code-block:: config
  :emphasize-lines: 1, 2, 4-8, 10-14

  runtime.platforms = condor,command
  runtime.staging_base = /source/modules/PlanteomeDeepSegment/staging/

  [condor]
  docker.image  =  bisque_uplanteome
  executable    =  PlanteomeDeepSegment
  environments  =  Staged,Docker
  files         =  PlanteomeDeepSegment

  [command]
  executable    =  python PlanteomeDeepSegment.py
  environments  =  Staged
  files         =  PlanteomeDeepSegment.py, PlanteomeDeepSegmentDGC.py, PlanteomeDeepSegmentLeaf.py, PlanteomeDeepSegmentLearning.py, PlanteomeDeepSegmentModels.py, PlanteomeDeepSegmentLeafMappings.csv, DeepModels
  script        =  "python PlanteomeDeepSegment.py --mex_url=$mex_url --module_dir=$module_dir --staging_path=$staging_path --image_url=$image_url --bisque_token=$bisque_token"

Now the magic happens ... *To-Be-Completed*

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
.. |Bisque_AdminMenu| image:: ./img/bisque/Bisque-AdminMenu.png
    :width: 100
    :height: 200
.. |Bisque_ModuleManager| image:: ./img/bisque/Bisque-ModuleManager.png
    :width: 750
    :height: 500
.. |Bisque_ZinniaOutputs| image:: ./img/bisque/Bisque-ZinniaOutputs.png
    :width: 750
    :height: 500
.. |Bisque_ZinniaInputs| image:: ./img/bisque/Bisque-ZinniaInputs.png
    :width: 750
    :height: 500
.. _CyVerse logo: http://learning.cyverse.org/
.. _Home_Icon: http://learning.cyverse.org/
.. _Bisque_Icon: http://bisque.cyverse.org/
.. _Bisque_Logo: http://bisque.cyverse.org/
.. _Bisque_AdminMenu: http://localhost:9898/
.. _Bisque_ModuleManager: http://localhost:9898/
.. _Bisque_ZinniaInputs: http://localhost:9898/
.. _Bisque_ZinniaOutputs: http://localhost:9898/
