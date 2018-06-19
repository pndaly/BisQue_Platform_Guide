|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

.. _step7.rst:

Dockerizing The Module (CyVerse Staff Only)
-------------------------------------------

**Now that we have the module running in a (fairly) standard container, we can proceed to dockerize it.**

A combination of a valid *Dockerfile*, *setup.py* and the *runtime-module.cfg* files are required to build the image
for BisQue deployment. These files should be up-to-date within the PlanteomeDeepSegment GitHub repository
(see :ref:`step4.rst`).

However, it also requires a BisQue source code tree to complete the build for BisQue ingestion. The best way to do this
is to use the machine *bisque-web.iplantcollaborative.org* so a valid login and *sudo* privilege is required on that
machine.

A summary of the steps we are about to take are:

 1. :ref:`Setting Up For The Build Process`

 2. :ref:`Building The Docker Image`

 3. :ref:`Testing The Docker Image`

 4. :ref:`Building The BisQue Version Of The Docker Image`

 5. :ref:`Adding Deployment Keys To The GitHub Repository`

 6. :ref:`Adding The GitHub Repository To The BisQue Build`

 7. :ref:`Deploying The Image On The BisQue Testing Stack`

|

Setting Up For The Build Process
````````````````````````````````
.. _s1:

Step 1, above, is handled like so:

.. code-block:: bash
  :emphasize-lines: 2, 5-6, 9, 12-14

  # login to bisque-web.iplantcollaborative.org with *your* username
  % ssh -i~/.ssh/pndaly -p 1657 pndaly@bisque-web.iplantcollaborative.org

  # on bisque-web, become user kkvilekval
  % sudo su kkvilekval
  Password: ********

  # on bisque-web, use the bq059 source code tree
  % workon bq059

  # on bisque-web, clone github repository to this machine
  % cd ~/bq059/modules
  % git clone https://github.com/DimTrigkakis/PlanteomeDeepSegment_0.3.git
  % mv PlanteomeDeepSegment_0.3 PlanteomeDeepSegment

**From here onwards, all commands (in this section) are executed under the kkvilekval account,
on the bisque-web.iplantcollaborative.org machine, in the bq059 sandbox.**

|

Building The Docker Image
`````````````````````````
.. _s2:

Step 2, above, can be used to build an initial (test) image which should complete without error:

.. code-block:: bash
  :emphasize-lines: 2

  # on bisque-web, build initial docker image
  % docker build --no-cache -t bisque_uplanteome -f Dockerfile .

This may take some time to complete and result in a fairly large image (several Gb).

|

Testing The Docker Image
````````````````````````
.. _s3:

Once the *docker build* is complete, you can carry out a simple access test (step 3 above) which should list the
contents of the (running) container's /module/workdir/PlanteomeDeepSegment directory:

.. code-block:: bash
  :emphasize-lines: 2, 6, 17-19

  # on bisque-web, check the test docker image was built
  % docker images | grep '^bisque_uplant' | sed -n '/bisque/s/ \+/   /gp'
  bisque_uplanteome   latest   7c31acc242fd   24   seconds   ago   3.26GB

  # on bisque-web, run the test docker image locally
  % docker run -it bisque_uplanteome:latest ls /module/workdir/PlanteomeDeepSegment
  DeepModels                            PlanteomeDeepSegmentLearning.py
  Dockerfile                            PlanteomeDeepSegmentModels.py
  PlanteomeDeepSegment                  README.md
  PlanteomeDeepSegment.py               images
  PlanteomeDeepSegment.xml              public
  PlanteomeDeepSegmentDGC.py            requirements.txt
  PlanteomeDeepSegmentLeaf.py           runtime-module.cfg
  PlanteomeDeepSegmentLeafMappings.csv  setup.py

  # on bisque-web, stop and remove the test docker container
  % _cid=$(docker container ps -a | grep 'bisque_uplant' | cut -d' ' -f1)
  % docker container stop ${_cid}
  % docker container rm ${_cid}

|

Building The BisQue Version Of The Docker Image
```````````````````````````````````````````````
.. _s4:

If the above steps are error-free, we can proceed to step 4 and build the BisQue version of the same image:

.. code-block:: bash
  :emphasize-lines: 2

  # on bisque-web, execute python setup
  % python setup.py

Once again, this may take some time.

This build will push the tagged image gims.cyverse.org:5000/bisque_uplanteome to CyVerse's local DockerHub-like
resource (gims.cyverse.org).

|

Adding Deployment Keys To The GitHub Repository
```````````````````````````````````````````````
.. _s5:

If the above steps are error free, we have verified that there is a reasonable chance that the module code can be
deployed under BisQue at CyVerse. However, we need to complete a couple of *one-time only* procedures to ensure a
successful build.

a. First, we must generate a set of SSH keys for deployment.

    This allows the BisQue build process to access the repository.
    So, assuming we are still logged into bisque-web.iplantcollaborative.org (as per step 1 above), we can execute:

.. code-block:: bash
  :emphasize-lines: 2, 5-6, 9, 12-14

  # on bisque-web, create SSH deployment keys
  % cd ~/compose-cyverse/cyverse05

  # on bisque-web, (create and) cd to keys sub-directory
  % if [ ! -d keys ]; then mkdir keys; fi
  % cd keys

  # on bisque-web, create new key pair
  % ssh-keygen -t rsa

  # on bisque-web, create config file
  % echo "Host *" >> config
  % echo "    StrictHostKeyChecking no" >> config
  % echo "" >> config

b. Second, we must add the SSH public key (id_rsa.pub) to the GitHub repository to ensure that BisQue can access the code base.

    NB: This is *not* the same as adding secure keys to a user's profile but adds the key to the repositotry itself.

    The best source of information on adding deployment keys to a repository is found at https://developer.github.com/v3/guides/managing-deploy-keys/ in the subsection "Deploy Keys".

*We stress that SSH deployment keys need only be added to private repositories and one-time only.*

|

Adding The GitHub Repository To The BisQue Build
````````````````````````````````````````````````
.. _s6:

Now that we can access the repository, we need to tell BisQue where it is. This is simply done again as a *one-time*
process:

.. code-block:: bash
  :emphasize-lines: 2-5

  # on bisque-web, add the GitHub repository to the module build config file
  % cd ~/compose-cyverse/cyverse05/config
  % _s="git git@github.com:DimTrigkakis/PlanteomeDeepSegment_0.3.git"
  % _x=$(grep "$_s" MODULES)
  % if [ -z "$_x" ]; then echo "$_s PlanteomeDeepSegment" >> MODULES; fi

|

Deploying The Image On The BisQue Testing Stack
```````````````````````````````````````````````
.. _s7:

At this point, we can proceed to create and deploy the image:

.. code-block:: bash
  :emphasize-lines: 2-4

  # on bisque-web, make
  % cd ~/compose-cyverse/cyverse05
  % make
  % make publish

As before, this may take a while. Once complete, we can tell *rancher* that the new image is available:

.. code-block:: bash
  :emphasize-lines: 2-7

  # on bisque-web, deploy using rancher-compose
  % cd ~/compose-cyverse/testing
  % source prod.source
  % rancher-compose stop
  % rancher-compose rm
  % rancher-compose create
  % rancher-compose start

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
