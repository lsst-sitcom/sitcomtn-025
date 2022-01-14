..
  Technote content.

  See https://developer.lsst.io/restructuredtext/style.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. _charge: https://sitcomtn-013.lsst.io/
.. _DMTN-126: https://DMTN-126.lsst.io/
.. _RSP: https://nb.lsst.io/

.. note::

   The First-Look Analysis and Feedback Functionality Breakout Group `charge`_ details questions regarding how the Rubin Project will perform on-the-fly analysis and display of both local data reduction artifacts and camera images. This document details the answers to the charge questions and other findings.


Executive Summary
=================

This is the initial report by the FAFF working group and contains all of the findings and recommendations that will be provided in the final copy.
Due to the current summit power issues, aspects of the document could not be completed at this time, specifically the documentation of the proof-of-concept demonstrators due to the loss of the Andes cluster and current void of data at the summit.

The working group accumulated a series of use-cases that represent the functionality required to successfully perform on-sky and on-the-fly operations during the commissioning phase.
By analyzing the use-cases and exploring how they would be accomplished using our current toolset, it has been determined that a large fraction of the necessary capabilities are already scoped and/or implementend but require augmenting certain functionalities to enable proper interfacing and inter-operability.
However, there exist critical gaps that require new software, designation of adopted packages or technologies, and the production of documentation such that users can interact with the system efffectively.

A new CSC (Controllable SAL Component) will need to be developed to handle the creation and management of analysis tasks as well as the alerting of newly created artifacts to users.
A design and scope estimate for this CSC has been developed and nearly all the information is ready to support the filing of an LCR, if required.
On-the-fly analysis tasks will produce reports that require a diversity of visualizations to facilitate monitoring, characterizing, and debugging the system.
After exploring several options, it is recommended that dynamic figures that are to be embedded into the LOVE system should use the Bokeh Visualization Library to create an application which can then be deployed using Kubernetes.
The display of images for on-the-fly analysis is most easily accomplished using the Camera Visualization Tool.
With moderate effort, the functionalities of this tool can be expanded to meet the requirements for on-the-fly applications.

The working group has performed several proof-of-concept demonstrations to test and reinforce our recommendations and findings.
Critical to the success of using these tools in an integrated way will be the creation of a series of instructional documentation to guide users on how to design, communicate, build, and deploy their desired tools.
The creation of these instructive documents is beyond the scope of this working group, but all members are keen to assist in developing this documentation.

Lastly, this working group has accumulated numerous findings and has made recommendations that are critical to the success of commissioning, but technically outside the scope of the on-the-fly displays and analysis, as per the charge.
These findings should be strongly considered by Project Management as they are expected to result in simplifying some of the global display challenges, specifically during data analysis.


Introduction
============

The First-Look Analysis and Feedback Functionality (FAFF) Breakout Group met approximately weekly starting June 30, 2021 and released it's first report on January 14, 2022.
The report consists of the use-cases that were used to define the functionality, requirements put on by those use-cases, and recommendations for new functionality.
We created demonstrations of new functionality in several cases, which lengthened the response process, but also resulted in the building of proof-of-concept tools which enhances the viability of the recommendations.

Some of the responses to the `charge`_ of the FAFF group were built off an Image Display Working Group who reported what was required by the data management group for image display and manipulation capabilities in `DMTN-126`_.
That group was not focused on on-the-fly functionality but many of the findings were applicable and taken into account here when it made sense to do so.

This report is structured into sections, where the first addresses each of the charge questions individually.
This is followed by a section on findings that are pertinent to the commissioning team but fall outside the scope of the charge, and subsequently a section outlining the proof-of-concept demonstrators that helped inform our recommendations.


Responses to Charge Questions and Deliverables
==============================================

The use-cases referenced throughout the document are all found on the :ref:`pg-D1-Use-cases` page.

.. _Deliverable 1:

Deliverable 1: Use-Cases
------------------------

.. note:: 

   The deliverable description from the `charge`_ has been directly copied here to ease readability.

   1. A series of use-cases where feedback to observers is required. A reduced set of use-cases should be created as a regular reference during design and development.

      - Use-cases should be complete, including which inputs are required (e.g. SAL Script, EFD, LFA, external source), desired manipulations, logic-based operations/calculations, and how the desired artefacts are presented to the user (e.g. display images and/or graphs).
  
A solicitation for use-cases was made to the Project staff via the Commissioning meeting and a general call on the Slack channel.
A meeting was held to define what the group was looking for and provided examples.
Members were also encouraged to pass the call for information to whomever may be able to provide useful input.
The responses from the community can be found `here <https://confluence.lsstcorp.org/display/LSSTCOM/2021-05-14+On-the-fly+analysis+for+observers+Meeting+Minutes#id-20210514OntheflyanalysisforobserversMeetingMinutes-On-the-flyAnalysisUse-Cases>`_.

Many of the examples from the community had significant overlap of information.
To amalgamate the information and create a basis from which to work, a series of standardized use-cases were developed that followed a common format and layout.
A standardized summary was also created to ease comparisons between use-cases.

The standardized use-cases associated with this deliverable are found on a separate :ref:`pg-D1-Use-cases` page.

.. _Deliverable 2:

Deliverable 2: Requirements
---------------------------

.. note:: 

   The deliverable description from the `charge`_ has been directly copied here to ease readability.

   2. A set of requirements to augment, clarify, or remove ambiguity that may occur from looking only at the use-cases.
      
      - This is not a formal and all-encompassing LSE-type requirements document.

Like the use-cases, the requirement deliverables have been separated into two different categories: those which are focused on tooling and procedures for on-the-fly analysis, and image visualization.
The two categories are quite separate in nature but not completely independent.

The detailed list of requirements is found on a separate :ref:`pg-D2-Requirements` page.


.. _Deliverable 3:

Deliverable 3: Currently Available Tools/Systems
------------------------------------------------

.. note:: 

   The deliverable description from the `charge`_ has been directly copied here to ease readability.

   1. A summary of the suite of currently available tools/systems that may help fulfil the required functionality.

      - Report on their status, limitations, and where their current requirements may need to be expanded to cover any increase in scope
      - Tools that were considered but found to be inadequate must also be reported

This section contains a summary of the tools currently available for use that are applicable to on-the-fly operations.
What follows is a list of the tools, each with a short description that contain links to pertinent information.

A high-level overview of these tools and computing resources was presented at the `original presentation given to stakeholders <https://docs.google.com/presentation/d/1i4p-sg42FXtEqGVqIZMeFadWSZZ0Lu_CpoqEafkMfy4/edit#slide=id.gd8dafc0d0d_0_30>`_.
These slides may contain additional information and/or a more detailed description and use-case.

Currently Available Tools
^^^^^^^^^^^^^^^^^^^^^^^^^

The following tools are readily available for general use and are expected to be utilized in the context of on-the-fly information gathering and display.
Some of these applications remain under active development, whereas others are considered to be nearly completed.

.. _LOVE:

LSST Observatory Visualization Environement (LOVE)
''''''''''''''''''''''''''''''''''''''''''''''''''

LOVE is a web-based user inteface for the observatory control system. 
It is currently available at the `summit <http://amor01.cp.lsst.org/>`_ (VPN required) and all other test stands.

The system is developed using the popular React JavaScript framework for the frontend, with Python in the backend.
It contains an editing tool that allow users to create and customize views, but this feature relies exclusively on existing widget-like components.
Extending the system beyond the vendor-provided "widgets" would require some knowledge of React and JavaScript, which is not common in the project.
Nevertheless, LOVE does contain a special widget that allows us to embed other web pages into a particular view. 
This could be used, for instance, to embed `Bokeh Plotting Applications`_ alongside other LOVE components.



Nublado (Jupyter Notebook) Interface
''''''''''''''''''''''''''''''''''''

The summit `Nublado interface <https://summit-lsp.lsst.codes/>`_ (VPN required) provides the user with a Jupyter Lab interface and the required libraries/packages to perform standard observatory operations including sending commands and running SAL scripts.
It can also be used to query the EFD.
This tool is set up to mimic the `RSP`_ and test-stand environments to the maximum extent possible, providing all the functionalities of a Jupyter Notebook but with direct access to the control system.
Observers are expected to use this tool to perform commands or sequences that are not encapsulated into a SAL script.
The Nublado interface is also useful for on-the-fly analysis and/or custom monitoring.


Camera Visualization Tool
''''''''''''''''''''''''''
The camera image visualization tool is designed to run on the camera diagnostic cluster and to deliver full-resolution focal plane images to the operator within a few seconds of image acquisition. 
The front-end is web based and supports natural zooming and panning, including zooming to full pixel resolution, as well as the ability to display pixel coordinates and raw pixel value by moving the mouse over the image. 
It supports a simple but extensible set of image processing algorithms which have been found to be useful for viewing images during camera testing.

The tool uses the `International Image Interchange Framework <https://iiif.io/>`_ (IIIF) with the front-end based on the open source `OpenSeadragon <https://openseadragon.github.io/>`_ framework, and the backend based on the open source `Cantaloupe <https://cantaloupe-project.github.io/>`_ product, modified to be able to read images from  FITS files and directly from the DAQ.
The tool is currently deployed at SLAC for camera construction, and on the summit in Chile for Auxtel and ComCam.


Engineering Facilities Database and Large File Annex
'''''''''''''''''''''''''''''''''''''''''''''''''''''

The `Engineering Facilities Database (EFD) <https://sqr-034.lsst.io/#introduction>`_ records all data sent by components over the DDS control network.
This content essentially tracks the observatory state as a function of time and is very useful in diagnosing issues and understanding (both desired and undesired) operational behaviours.
The database is best queried using the `EFD client <https://efd-client.lsst.io/>`_ from the `Nublado (Jupyter Notebook) Interface`_ (or any python-based method/script) when making custom plots.
Instructions on how to access the EFD, and specifically the other instances of the data, are found in `SQR-034 <https://sqr-034.lsst.io/#efd-deployments>`_.
However, the `Chronograf`_ graphical front-end offers a nice solution for building simple plots (dashboards).

The `Large File Annex (LFA) <https://tstn-029.lsst.io/>`_ contains files over ~1 MB that are accessible both from the summit and the `RSP`_.
When a file is published to the LFA its presence (and location of the file) is published via SAL/DDS and therefore the location of LFA files can be found via an EFD query.
It is anticipated that artifacts generated from automated on-the-fly analyses will be stored in this area.
An example of this would be the `Papermill Executed Parameterized Notebooks`_.


Chronograf
''''''''''

Chronograf is a tool that provides a user-friendly graphical interface to each `deployed instance of the EFD <https://sqr-034.lsst.io/#efd-deployments>`_.
During operations, observers will use the `summit instance <https://chronograf-summit-efd.lsst.codes/>`_ (VPN required).
It is particularly useful for quick and simple analysis as well as for creating dashboards to show a specific information about the status of the observatory; especially when a similar functionality is not available in LOVE.

Chronograf is not particularly well suited for complex queries and figures, especially those that require some additional logic to be meaningful.
For example, if a Controllable SAL Component (CSC) crashes, a view displaying its summary state will continue to show the last value in the EFD, which is likely no longer applicable.
To derive relevant information, it is necessary to combine it with some additional data, such as the time since the last published heartbeat.
This is not easily accomplished in Chronograf as it is not designed for this purpose.
Furthermore, Chronograf views (queries, plots, dashboards, etc) are not easily replicated, and can vary significantly for different users, depending on their local configurations.
For these reason (and many others), it is not an appropriate substitute for a true monitoring system, such as what is provided by the LOVE_ interface.

Watcher
'''''''

The Watcher CSC <https://ts-watcher.lsst.io/>_ monitors data from multiple sources and publishes alarm events to DDS depending on predefined conditions.
Although it is currently fed mainly with DDS data published by CSCs, it does has the capability to listen to other external data streams.
Alarms are composed by a set of "rules" that are defined and added to the application software package.
Most alarms are generic enough such that they can be configured to act for multiple CSCs, whereas others are specific to a particular component.
For example, the heartbeat rule can be configured for any CSC, and will issue an alarm if it does not receive a heartbeat for less than a configurable time (usually 5s, which is the default).

The Watcher itself is not a display tool nor does it have any display functionality.
When a condition defined by an rule is met, an alarm is generated and the observer is alerted via a LOVE screen.
The alarm has a series of levels and audible alerts are sent out via LOVE, as well as a visual notification.
Once the alarm is acknowledged by the observer the alert is considered to be completed.
There is no feedback or interaction for the observer beyond the acknowledgment of the alarm.
Alarms will only deactivate (stop re-occuring) if the condition for which they are activated cease to exist, e.g., if a CSC resumes publishing heartbeats after being restored.


SAL Scripts
'''''''''''

`SAL scripts <https://ts-salobj.lsst.io/sal_scripts.html#lsst-ts-salobj-sal-scripts>`_ are a series of coordinated sequences, often consisting of commands to CSCs, that are executed by the `ScriptQueue <https://ts-scriptqueue.lsst.io/>`_.
The LOVE system contains a graphical ScriptQueue interface to control and monitor the execution of scripts.
It is anticipated that most standard operations will utilize scripts.
Also possible, although not standard practice, is to manually execute a script from the `Nublado (Jupyter Notebook) Interface`_.
From within a SAL script, users can send standard commands to components as well as send data to the `OCPS`_ for processing.
The script can then either wait for the analysis to complete and continue, with the ability to act based on the result, or launch the process (e.g. image reduction) and continue executing the script.
Scripts are not intended to perform intensive data analysis and, in general, are not expected to produce artifacts.
Scripts are also unable to display visualizations.


OCPS
''''
The `Observatory Controlled Pipeline Service (OCPS) <https://dmtn-133.lsst.io/>`_ is a CSC that allows observers (and SAL Scripts) to execute pipeTasks to perform data reductions and analyses.
The CSC runs on the summit but the data processing is currently running on the commissioning cluster at the base (although it may be relocated to the summit).
The OCPS is not a display tool, but can be used to produce artifacts (such as images, spectra etc) that observers want to display.
The current scope of this service is to only provide image-related processing.
It cannot currently query the EFD.

At this time, the OCPS is being used to perform the analysis of daily calibrations executed from the scriptQueue.


Prompt Processing
'''''''''''''''''
The Prompt Processing Pipeline is expected to run at the United States Data Facility (USDF).
Within 60s, the images taken on-sky get reduced and a series of data products are made available.
A small number of these data products are sent back to the summit `via the Telemetry Gateway <ls.st/lse-72>`_.
This service is not yet in place.

It is expected that metrics coming from Prompt Processing (and `faro`_) will be used in on-the-fly displays.

faro
''''
..
   Link to github repo:
   https://github.com/lsst/faro

`faro <https://pipelines.lsst.io/v/daily/modules/lsst.faro/index.html>`_ is a framework for automatically and efficiently computing scientific performance metrics on the outputs of the LSST Science Pipelines. 
It supports multiple units of data of varying granularity, ranging from single-detector to full-survey summary statistics, and persists the results as scalar metric values alongside the input data products in the same butler repo.

In the "first-look analysis" context, it is intended that faro would be run as an afterburner to the `OCPS`_ and `Prompt Processing`_ (run automatically as part of the same pipeline) and would compute scalar metrics to quantify the performance of individual visits as they are acquired. 
For example, faro could be used to estimate the effective depth of individual images by computing the flux of a point source that would be measured with signal-to-noise of 10, or to measure variations in effective depth across the focal plane that would be indicative of variable atmosphere transparency. 
The intent is that these metrics would be available within minutes to the observers to inform nighttime operations. 
faro is designed to be modular and configurable so that additional metrics can be readily added to support summit operations.

faro is NOT itself a visualization tool, but rather generates scalar metric values that could be used as input to visualization tools.


RubinTV
'''''''

The `Rubin TV image display service <https://roundtable.lsst.codes/rubintv>`_ is a collection of auto-generated webpages that displays the output (PNGs) of a series of manually deployed analysis scripts.
It was relatively straightfoward to implement and has proved to be a highly useful tool during early AuxTel Commissioning.
We expect it to continue to be useful as the framework recommended in this report is developed and the systems are matured.
It is not expected to be one of the long-term solutions to the Rubin Visualization challenges because its capabilities will be incorporated into the other toolsets discussed in `Deliverable 4: Augmenting Current and Adding New Functionalities`_.

Available Computing Power
^^^^^^^^^^^^^^^^^^^^^^^^^

The following computing resources are available for use but how the hardware will be utilized is not yet well planned and/or documented.

On Cerro Pachón:

- Yagan - the summit-based cluster hosting nearly all services (Nublado, CSCs, EFD etc)

 - 7 nodes
 - 64 cores/node (448 total), 4 GB RAM/core (256 GB RAM/node)
 - 168TB storage (5.4TB local disk per node)

- LSSTCam Diagnostic Cluster
  
 - 10 nodes
 - 32 cores/node (320 total), 4 GB RAM/core (128 GB RAM/node)
 - 10 TB/node (100 TB total)
 N.b. there is also an 11th node to be used as the ComCam Diagnostic Cluster

In La Serena:
- The Commissioning Cluster ("Antu")
  
 - 15 Heterogeneous nodes
 - 24-72 cores/node (832 total)
 - 3.2T RAM total (average 3.9 GB/core)
 - 26 TB storage
 - Expect 1.6 PB (c. 800 TB formatted) in April 2022.  The usable ratio will improve as we add more disk.

The use of these clusters remains unclear and is called out in the `Other Findings and Identified Issues`_ section, under the :ref:`Diagnostic_and_Commissioning_Cluster` heading.


.. _Deliverable 4:

Deliverable 4: Augmenting Current and Adding New Functionalities
----------------------------------------------------------------

.. note:: 

   The deliverable description from the `charge`_ has been directly copied here to ease readability.

   1. A mapping of the use-cases into the currently available systems, clearly identifying where new functionality is required.

      - This could be by augmenting current systems or the creation of a new system if required
      - Deliver a proposed implementation for each use-case

Each of the use-cases presented in `Deliverable 1`_ contain a heading regarding a suggested implementation.
The contents of each section refer to new and/or augmented functionality that is seen accross many of them.
Because the explicit identification of new functionality would add unnecessary noise and confusion for the reader, the content is accumulated here and explained in greater detail.

The items for this deliverable have been separated into two areas:

#. A description of areas where `Entirely New Functionality`_ is required.
#. A description where the requirements can be met by `Augmenting Current Functionality`_.

This working group also created a proof-of-concept of the critcal implementation recommendations and found them to be successful in satisfying the requirements and being relatively straightforward to implement.
Details are found in the `Proof-of-concept Demonstrations`_ section.

One should also note that there were functionalities that the group found to be critical to the success of commissioning, but not directly for on-the-fly applications, which therefore resulted in the requirement being out-of-scope. 
These types of issues are a grouped into the `Other Findings and Identified Issues`_ and should be strongly considered for implementation as part of the change requests that will result from this charge.


Augmenting Current Functionality
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When considering how to implement the use-cases, effort was always made to ensure that currently available tools (presented in `Deliverable 3`_) would be used wherever appropriate.
In most cases, specifically in regards to image display, augmenting functionality of existing tools is a perferred path to starting from scratch.

The list of new functionalities required for already existing tools include:

#. Numerous `Camera Visualization Improvements <pg-D2-Requirements_for_image_display>`_ were described as part of `Deliverable 2`_ and are therefore not repeated here.
   An example is the `callback functionality <demo_callback>`_ in the `Proof-of-concept Demonstrations`_ section.
#. The OCPS (really the butler) requires access to EFD. This is not currently captured in a use-case but one can envision how having a pipeTask be capable to correlate image quality against items in the EFD could be useful.

   - No code has been written to integrate butler directly with EFD, but it is possible to do
   - Would enable users to define pipelines that explicitly specify EFD datasets as pipeline inputs. 
     Currently, it would be required to sort out the mapping of exposure dataId to an EFD call in (potentially) a special runQuantum method in the pipeline task
  

Entirely New Functionality
^^^^^^^^^^^^^^^^^^^^^^^^^^

This section identifies functionalities that are required and could not be ascertained by upgrading already existing components.
The largest piece of missing functionalty is the framework to perform on-the-fly analyses which are triggered on specific events or conditions and then be able to perform calculations, generate a report (including plots etc), and have the operator be alerted.
Implementing this type of capability requires numerous pieces to work together.


Catcher CSC
'''''''''''

A series of new functionality, which for the purposes of this document we have grouped into a single "Catcher" Controllable SAL Component (CSC) is required to handle the low-level coordiation of identifying when a specific condition is met, then launching and monitoring an analysis process.
It is still being evaluated if it is required to generate a new CSC or if the Watcher CSC can be augmented to handle this new functionality. 
For the purposes of this document we will assume the functionality is to be captured in a new CSC.
The Catcher functionality also requires a LOVE display to show which tasks are running, links to generated reports, and alarms or notifications for observers.

More details on the design and implementation can be found in the `Catcher Design Document <https://docs.google.com/document/d/1mbmfqjebOuHIV8CwC7jFHcFKCRMtyBDHPXeGfBO1EPE/edit#>`_ currently being worked on as a google doc.

At the end of an analysis task, a "report" is generated and produced as a result.
The reports from the Catcher can be derived in multiple ways and take on multiple formats.
There is no requirement that the analysis results generated by the Catcher-managed tasks be persistent.
However, it should be recommended practice as the results can be re-run at a later date.
When possible, this committee recommends that analysis tasks produce reports in the form of `Papermill Executed Parameterized Notebooks`_.
Once executed, the notebooks and their contents and archived to the LFA, where they can be looked at (and even re-run) either immediately or at a later date.
The capabilities of the notebooks is vast; allowing image analysis via sending commands to the OCPS, queries of the EFD, grabbing of SAL events or data from the Butler.
The queries to obtain the data to create any plots or other displays are also contained in the notebook.
This allows plots to be modified and re-generated as required at a later date.
Lastly, the notebooks can be used to create a dataset that can be displayed by a Bokeh Application that is nested inside a LOVE display, which is one of the key use-cases that will be encountered during observations.
There are details that remain to be solved, specifically aspects such as how to account for multiple reports that can be generated by re-running the same notebook multiple times.
These are presumed to be solvable problems but will require further investigation that goes beyond the scope of this group.

It is worth mentioning that having analysis results persisted in the Butler is not a viable option in many cases.
The Butler requires an artifact to be associated with a dataId.
For many analyses, there will be no image associated with it, and therefore the Butler cannot be used.


Bokeh Plotting Applications
''''''''''''''''''''''''''''

Multiple plotting packages were considered before ultimately settling on the `Bokeh Visualization Library <https://bokeh.org/>`_.
The majority of Python users are familiar with matplotlib and therefore finding a system to support this was the preferred option.
However, matplotlib is very limited with regards to the creation of interactive and dynamic web-embedded plots, for this reason the packages of vega-lite and Bokeh were considered.
Both of these systems could be directly integrated into the LOVE framework with the currently existing functionality.
Vega-lite is able to create apps to embed on webpages, however, Vega-Lite does not expose the handles for more sophisticated call-back actions that go beyond the plotting itself. 
This was the most significant inadequacy of Vega-lite, however, the capabilities of vega-lite are also limited and certain use-cases required the use of Vega, which means ultimately working with javaScript; to which very few people on the project have familiarity.
Based on the evaluation of the use-cases Bokeh was the preferred solution to support all of our requirements (see :ref:`pg-D2-Figure_Generation_Requirements`)
Furthermore, it is already familiar to certain people on the project and can be easily integrated into our current system design.

Bokeh Applications are extremely flexible in design and can render data from multiple sources if configured to do so.
This includes SAL events, EFD queries, Butler served information, or files from the LFA.
The apps can then create dynamic (or static) plots, display images, or even be setup to send commands to move the telescope based on a calculation (e.g. offsetting to a star).
One major advantage of Bokeh is that the very high majority of the application can be developed inside a notebook.
Once functioning as expected, it can be ported to a python file with minimal intervention required.

As part of the selection criteria, demonstation projects were created and executed to verify certain aspects of the required functionality and usability.
Examples of Bokeh apps and their use is found in the `Proof-of-concept Demonstrations`_ section. 
It was possible to build out and deploy the Jitter application in a few hours.
The centering application, which can be used for determining offsets, took roughly two hours. 
Rough notes on how to turn a notebook-based Bokeh plot into an application can be found as part of `Simon's draft <https://gist.github.com/SimonKrughoff/cc02f873a2a1518161d3f3a1839be4a5>`_.
These will be turned into more formal instructions before the close-out of this working group.
One caveat is that Bokeh Apps can only be used where they are deployed.
Should they wish to be used at the Rubin Science Platform (`RSP`_) for instance, they will need to be deployed there as well (and obviously any SAL commands will not work).

Papermill Executed Parameterized Notebooks
'''''''''''''''''''''''''''''''''''''''''''

`Papermill`_ introduces the concept of *parameterized notebooks* and provides tools to execute them in a batch-like mode.

.. _Papermill: https://papermill.readthedocs.io/en/latest/

*Parameterized notebooks* are similar to regular Jupyter notebooks.
The process consists of:

  - identifing variables in the notebook that one wants to expose as parameters,
  - have all these variables defined in a single cell (ideally, with default values),
  - tagging the cell with the "parameters" keyword.

More details on how to parameterize a notebook on a particular environment can be found in the `papermill usage documentation page <https://papermill.readthedocs.io/en/latest/usage-parameterize.html>`__.

Notebooks can then be executed using `Papermill`_ command line interface or through its Python API.

Benefits of using `Papermill`_ includes:

  - Simplify the process of creating on-the-fly reports and re-runable notebooks that will store the parameters used for the execution and generation of plots etc.

    - Users would be able to develop their reports directly on notebooks, with practicaly no overhead in integrating them with the system aftewards.

    - The reports can be stored directly as notebooks and/or be expored and rendered as webpages with `nbconvert <https://nbconvert.readthedocs.io/en/latest/>`__.

  - Store notebooks in the LFA.

    `Papermill`_ can save the output directly to the s3bucket, which is used as the backend for the LFA.

  - Possible to interact with the control system with salobj from the notebooks, the same way we execute tasks with the control system from nublado.

  - Can also send information to Bokeh app (if Bokeh app is configured to do so).

Some challenges with using Jupyter Notebooks and Papermill are:

  - It is notably difficult to develop unit testing for Jupyter Notebooks.

    `Papermill`_ does make it possible to write unit tests for Jupyter Notebooks, but certain tasks (like using mocks to isolate external libraries) can be exceedingly difficult. 

  - Overhead on loading the environment.

    When executing a notebook through `Papermill`_, there is an overhead associated with loading the environment prior to the execution by the Jupyter lab server.
    The size of the overhead depends on the environment which the notebook is running.
    
    For reference, in the ScriptQueue environment the overhead is about 2s.
    At the same time, some highly customized users environments on nublado take up to 10s to load.

.. 
   .. important::

      The mapping and implementation for each use-case assumes that the tools described in `Deliverable 5`_ and the additional required functionalities described in `Deliverable 6`_ have been incorporated.
      It may be useful to read those sections before this one.

      The response to this deliverable is found in each of the use-cases.

.. _Deliverable 5:

Deliverable 5: Prioritized Task List and Delivery Milestones
------------------------------------------------------------

.. note:: 

   The deliverable description from the `charge`_ has been directly copied here to ease readability.

   1.  A prioritized list of tasks to build-out the new functionalities with recommended end-dates. 
       - These dates shall correspond to integration milestones.


This section is not yet completed.
In the final version, a prioritized list of required functionalities will be presented.
Note that the requirements are already prioritized to a degree and will help develop this list.
Furthermore, an single list for prioritization may not be the best deliverable as many of the efforts can be pursued in parrallel.
This is especially true when comparing tasks such as the Catcher CSC development and updates to the Camera Visualization Tool.

The development of the Catcher is the highest priority item as it is required to develop the on-the-fly analysis framework and will take time to produce.


.. _Other Findings and Identified Issues:

Other Findings and Identified Issues
====================================

During the existance of this working group, numerous items were identified as problematic and needing to be addressed but either were not well fit to a charge question or fell out of the scope of the charge.
This section contains information regarding numerous issues which were identified and require attention.

The recommendation of this committee is that a follow-up committee be created to address these items as they are required to successfully commission the observatory.

.. _Diagnostic_and_Commissioning_Cluster:

Diagnostic And Commissioning Cluster Usage Needs Definition
-----------------------------------------------------------

This working group was not able to find any documented strategy on how the commissioning and diagnostic clusters are to be used during commissioning and the survey.
High-level descriptions exist from early in the project, however they are not sufficient to build out the system and do not take into account much of the as-built software and hardware capabilities.
Although beyond the scope of this working group, it is strongly suggested that a strategy be developed that identifies and documents the use-cases, specifically in regards to the differences between how calibration and on-sky data is handled.
Currently, the camera diagnostic cluster hardware is on the summit but not being used, largely in part due to a lack of definition of it's use-cases and how it is to interact within the global data analysis workflow of the Rubin Observatory, including whether or not DM tooling must be supported.


Camera Visualization Tool Functionality Limitations for General Commissioning
-----------------------------------------------------------------------------

The Camera Visualization Tool, once augmented with the new specifications, will be sufficient for on-the-fly applications but will not be able to satisfy many of the more general commissioning use-cases. 
The most obvious example is the ability to display and interact with full-frame images once they arrive at NCSA.
Because the committee was formed to look only at on-the-fly analyses, the following specifications are out of scope, however, for the general commissioning effort to be successful the following functionalities will need to be implemented, or covered by a different suite of tooling.

.. note::

   The items presented here do *not* form a complete set of specifications for general commissioning.
   They account for merely a subset that we identified as not specifically required for on-the-fly analysis at the mountain top.
   If the capabilites were in place, then the on-the-fly users would certainly take advantage of them.

FAFF-REQ-014
^^^^^^^^^^^^

**Specification:** The camera visualization tool shall support being deployed in places where historical data is available.

**Rationale:** This will need to be available to people using the `RSP`_ and commissioning cluster.
The general commissioning / SV use case is to be able to examine aspects of image quality that cross detector boundaries (e.g., stray and scattered light, satellite trails, pervasive issues across detectors) for which full focal plane visualization is critical. 
Of course, these studies will involve looking at images that date back in time, and therefore will need to be executed from the RSP (or other processing center).
This could be useful even for summit operations if it allows display of historic images (for comparison with new images).
The historical data on the summit is currently limited to 30 days.

.. **Priority: 1**

**Current shortcomings:** Firefly may not meet all of the requirements for all image visualization, specifically in regards to full-frame visualization.
Deployment not nested into current `RSP`_ deployment strategy.
It requires a mechanism to locate the data for a given obsid, but this is also presumably be possible. 

**Applicable Use-cases:** Rapid per sensor image display and inspection.

**Suggested Implementation to fulfill requirement:** 
Deploy the camera visualization tool and Bokeh apps as part of the standard `RSP`_ packaging.
Installing the camera image visualization server at the USDC (SLAC) is certainly feasible. 


FAFF-REQ-026
^^^^^^^^^^^^

**Specification:** The display tool should be able to display data obtained from the butler, or obtained from a users interactive Jupyter session

**Rationale:** Displaying images with full DM ISR applied, co-added images etc.
This is required to perform much of the post on-the-fly analysis during commissioning.

.. **Priority:** 1

**Current shortcomings:** DM has an abstract image visualization interface (afw). 
Needs to be evaluated to see if this could be used to meet all the requirements.

**Applicable Use-cases:** 

**Suggested Implementation to fulfill requirement:** 

FAFF-REQ-036
^^^^^^^^^^^^

**Specification:** Ability to overlay markings at user-provided pixel positions

**Rationale:** Used to indicate which sources are used in PSF analysis, blends, from catalogs etc.

.. **Priority:** 1

**Current shortcomings:** Currently unable to interface to DM (essentially the butler, pre-req is FAFF-REQ-026_) 

**Applicable Use-cases:**

**Suggested Implementation to fulfill requirement:**


FAFF-REQ-017
^^^^^^^^^^^^

**Specification:** Ability to choose between minimal ISR versus some more sophisticated ISR (for example, the calexp images served from a butler)

**Rationale:** For general SV purposes, it should be possible to inspect images at various stages of data processing both to evaluate the ISR performance and to visualize more generic representations of pixel-level information (e.g., mask planes).

.. **Priority:**

**Current shortcomings:** Currently unable to interface to DM (essentially the butler, pre-req is FAFF-REQ-026_)  

**Applicable Use-cases:**

**Suggested Implementation to fulfill requirement:**


Proof-of-concept Demonstrations
===============================

To confirm the recommendations of this committee, several examples were created to provide a proof-of-concept and help identify details regarding implementation.
The examples in the following subsections were proven using data from the summit from previous AuxTel runs.

.. note::
   Due to the recent power losses at the summit and the loss of the andes cluster, there has been no new data in the last 30-days and therefore they are not presently able to show data.
   As a result, the documentation for each demonstration is sparse.
   Further screenshots and evidence of their functionality will be provided once data is back on the summit.


.. _demo_jitter:

Creation and Display of the Jitter Plots in Bokeh
-------------------------------------------------

Following the plot example in the `Jitter Use-Case <Mount Jitter Measurement and Reporting>`_, a Bokeh app was created where all 9-plots were shown simultaneously and the user could actively drag and zoom on the plots to adjust the ranges of the axes. 
Users are able to select an obsID, then the app would query the EFD and create the plot in real-time. 
The app was then manually deployed to Andes and embedded in a custom LOVE display window.
Each user navitaging to the window was presented with the same original plot, however the user-experience was specific to that user (meaning if one person adjusts the ranges only they see the change).

Deploying the app via Kubernetes was identified as feasible and the preferred method for deployement, however it has not yet been performed.

.. _demo_offset:

Calculation of Offset Measurements in Bokeh
-------------------------------------------

One regularly occuring use-case was regarding how we will determine and ultimately apply telescope offsets.
Although this is not expected to be a regular survey operation, it is a very standard operation carried out in most observatories and serves to represent functionalities that will be required regarding interaction between image display and the control system.

To demonstrate the ability for Bokeh to provide information from images a demonstrator application was built in Bokeh that loads a single detector image that has been injested by the Butler.
The image to be loaded is designated in the URL to the application, and therefore is one way that the Bokeh Apps can interact with the Camera Visualization Tool.
Although not performed, the Camera Visualiation Tool is supports callbacks.
One easy way to interact with deployed Bokey apps is to send the information (such as obsID and detector determined via a cursor position and/or a pressed key) via a URL. 
Once in the Bokeh App, the user can then zoom and scroll on the image to find their desired target and select (click) it.
The app then uses that user-defined position and performs a rapid centroiding to determine and return the center of the designated target.

Developing a REST API to support interactions (callbacks) between the Camera Visualization Tool and Bokeh Apps is feasible, but is ultimately more work to demonstrate then sending the callback information via URLs. 
Whether or not it is worth implementing this is dependent upon use-case(s) but we predict it is worthwhile, especially if the Camera Visualization Tool is to be used in places other than the summit (see `Camera Visualization Tool Functionality Limitations for General Commissioning`_).

Of course, the finding/centroiding capability could be built into the Camera Visualization tool.
However, rapidly adding functionality to the Camera Visualization Tool then redeploying carries far more risk and has a broader impact than a Bokeh App.
Currently, the camera tool does not have access to the WCS data to calculate offsets, however, this will be available soon.
Commanding the telescope from the Camera Visualization Tool would be a very significant augmentation of functionality.

Although not performed, it is then possible (if desired) to have a button/drop-down that sends commands to the TCS.
For example, it could center the target, or put the target on a specific pixel, without having to leave the App.

Readers may be curious if a Bokeh App can display a full LSST image.
This was tested and shown to be possible, however, it takes several minutes to render and is therefore not advised.


.. 

   .. _demo_callback:
   Offsetting example using Camera Visualiation Tool Callback
   ----------------------------------------------------------

   This is currently an action item of a example of what can be done when requirement :ref:`FAFF-REQ-025` is implemented and will be populated once completed.



Remaining Work by the Working Group
===================================

- Answer `Deliverable 5`_ with a more explicit list.
- Populate the `Proof-of-concept Demonstrations`_ section with screenshots and examples.
- Populate (4) outstanding TBRs in `Deliverable 2`_.
- Move confluence content into this technote where appropriate. 
  Prints of PDFs may be sufficient.
- `Bokeh Plotting Applications`_ section requires more detailed notes on how to go from functionality developed in a notebook, to an app, and then how to get that embedded in LOVE.
- Create repo for building apps with template(s)
- Create a sample work flow which includes an "easy" example of how to derive/calculate a property, then create+deploy and App, then send an alert to an observer. 
  This may be too much scope and depends upon when the Catcher demonstrator can be implemented.

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
