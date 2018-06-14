|CyVerse logo|_

|Home_Icon|_
`Learning Center Home <http://learning.cyverse.org/>`_

.. _index.rst:

Adding a Python Dockerized Module to BisQue
===========================================

Goal
----

The objective of this guide is to allow scientists to incorporate a Python-based module into *BisQue* and develop a
Dockerfile for condor deployment.

----

|

.. toctree::
    :maxdepth: 2
    
    Overview <self>
    Some Useful Utilities For Working With Docker <step1.rst>
    Get And Run The BisQue Docker Image <step2.rst>
    Add Local Directories To The BisQue Docker Image <step3.rst>
    Clone The GitHub Repository <step4.rst>
    Update The Dependencies In The BisQue Docker Image <step5.rst>
    Run The Module <step6.rst>
    Dockerizing The Module (CyVerse Staff Only) <step7.rst>
    Summary <stepn.rst>

|

----

Downloads, Access and Services
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to complete this tutorial you *might* need access to the following services/software:

.. list-table::
    :header-rows: 1

    * - Pre-Requisite
      - Preparation/Notes
      - Link/Download
    * - CyVerse account
      - You may need a CyVerse account to complete this exercise
      - `Register for CyVerse Here <https://user.cyverse.org/>`_
    * - BisQue account
      - You may need a BisQue account to complete this exercise
      - `Register for BisQue Here <https://bisque.cyverse.org/>`_
    * - BisQue code base
      - v0.5.10 or later
      - https://bitbucket.org/CBIucsb/bisque

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
