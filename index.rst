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

.. TODO: Delete the note below before merging new content to the main branch.


.. _charge: https://sitcomtn-013.lsst.io/
.. _DMTN-126: https://DMTN-126.lsst.io/

.. note::

   **This technote is not yet published.**

   The First-Look Analysis and Feedback Functionality Breakout Group `charge`_ details questions regarding how the Rubin Project will perform on-the-fly analysis and display of both local data reduction artifacts and camera images. This document details the answers to the charge questions and other findings.


Introduction
============

The First-Look Analysis and Feedback Functionality (FAFF) Breakout Group met approximately weekly starting FIXME1 and released it's first report on FIXME2.
The report consists of the use-cases that were used to define the functionality, requirements put on by those use-cases, and recommendations for new functionality.
It should be noted that demonstrators of some of the new functionality were performed, which lengthened the response process, but also resulted in the building of a proof-of-concept tool which enhances the viability of the recommendations.

Some of the responses to the `charge`_ of the FAFF group were built off an Image Display Working Group who reported what was required by the data management group for image display and manipulation capabilities in `DMTN-126`_.
That group was not focused on on-the-fly functionality but many of the findings were applicable and taken into account here when it made sense to do so.



Responses to Charge Questions and Deliverables
==============================================

The use-cases are all found on the :ref:`pg-D1-Use-cases` page.

.. _Deliverable 1:

Deliverable 1:
--------------

.. note:: 

   The deliverable description from the `charge`_ is in the note below to ease readability 

   1. A series of use-cases where feedback to observers is required. A reduced set of use-cases should be created as a regular reference during design and development.

      - Use-cases should be complete, including which inputs are required (e.g. SAL Script, EFD, LFA, external source), desired manipulations, logic-based operations/calculations, and how the desired artefacts are presented to the user (e.g. display images and/or graphs).
  
A solicitation for use-cases was made to the Rubin community via the Commissioning meeting and a general call on the Slack channel.
A meeting was held to define what the group was looking for and provided examples.
Members were also encouraged to pass the call for information to whomever may be able to provide useful input.
The responses from the community can be found `here <https://confluence.lsstcorp.org/display/LSSTCOM/2021-05-14+On-the-fly+analysis+for+observers+Meeting+Minutes#id-20210514OntheflyanalysisforobserversMeetingMinutes-On-the-flyAnalysisUse-Cases>`_.

Many of the examples from the community had significant overlap of information.
To amalgamate the information and create a basis from which to work, a series of standardized use-cases were developed which followed a common format and layout.
A standardized summary was also created to ease comparisons between use-cases.

The standardized use-cases associated with this deliverable are found at :ref:`pg-D1-Use-cases` page.

.. _Deliverable 2:

Deliverable 2:
--------------

.. note:: 

   The deliverable description from the `charge`_ is in the note below to ease readability 

   2. A set of requirements to augment, clarify, or remove ambiguity that may occur from looking only at the use-cases.
      
      - This is not a formal and all-encompassing LSE-type requirements document.

Like the use-cases, the requirement deliverables have been separated into two different categories, those which are focused on tooling and procedures for on-the-fly analysis and image visualization.
The two categories are quite separate in nature but not completely independent.

The detailed list of requirements is found on :ref:`pg-D2-Requirements`.


.. _Deliverable 3:

Deliverable 3:
--------------

.. note:: 

   The deliverable description from the `charge`_ is in the note below to ease readability 

   1. A summary of the suite of currently available tools/systems that may help fulfil the required functionality.

      - Report on their status, limitations, and where their current requirements may need to be expanded to cover any increase in scope
      - Tools which were considered but found to be inadequate must also be reported

Content for this section is found in the `original presentation given to stakeholders <https://docs.google.com/presentation/d/1i4p-sg42FXtEqGVqIZMeFadWSZZ0Lu_CpoqEafkMfy4/edit#slide=id.gd8dafc0d0d_0_30>`_.


**Tools**:

- Camera Visualization Tool
- Watcher
- faro (after burner to the OCPS and prompt processing)
- SAL Scripts
- OCPS 
- Nublado Interface (USDF, Summit etc) with Jupyter Notebook Functionality
- LOVE
- Chronograph 
- EFD and LFA
- Prompt Processing Data Products and delivery `via the Telemetry Gateway <https://docushare.lsst.org/docushare/dsweb/Get/LSE-72#%5B%7B%22num%22%3A54%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C69%2C205%2C0%5D>`_.

**Available Computing Power:**

The following computing resources are available for use but the usage of each is not yet well planned and/or dccumented.

- Camera Diagnostic Cluster
- Commissioning Cluster (Antu)
   
Add discussion with regards to how these tools are used for on-sky data and calibration data? Could also put this in the `Other Findings and Recommendations`_  section.

.. _Deliverable 4:

Deliverable 4:
--------------

.. note:: 

   The deliverable description from the `charge`_ is in the note below to ease readability 

   1. A mapping of the use-cases into the currently available systems, clearly identifying where new functionality is required.

      - This could be by augmenting current systems or the creation of a new system if required
      - Deliver a proposed implementation for each use-case



Entirely New Functionality
^^^^^^^^^^^^^^^^^^^^^^^^^^

See `Other Findings and Recommendations`_ regarding commissioning needs that are Out-Of-Scope for this committee.



A "Catcher CSC"
'''''''''''''''''

At the moment, there is no CSC that supports evaluation of logical condition, that then can launch, monitor, and manage the analysis tasks that are to happen when the condition is satisfied.
Upon completion of the report, an alert can then be sent to the operator if appropriate.
This new "Catcher" CSC is meant to handle this functionality.
It is still being evaluated if it is requierd to generate a new CSC or if the Watcher CSC can be augmented to handle this new functionality. 
It also requires a LOVE display to show which tasks are running and which reports have been generated.

More details on the design and implementation can be found in the `Catcher Design Document <https://docs.google.com/document/d/1mbmfqjebOuHIV8CwC7jFHcFKCRMtyBDHPXeGfBO1EPE/edit#>`_) currently being worked on as a google doc.

The reports from the Catcher can be derived in multiple ways. 
There is no requirement that the analysis as[ects generated by the Catcher+analysis be persistent, but it is recommended.
When possible, this committee recommends using parameterized Jupyter Notebooks that get executed by Papermill.
These notebooks then get executed and archived to the LFA, where they can be looked at either immediately or at a later date.
The capability of the notebooks is wide, allowing image analysis via sending commands to the OCPS, queries of the EFD, grabbing of SAL events or data from the butler.
The data to create any plots etc are also contained in the notebook allowing plots to be modified and re-generated as required.
Lastly, the notebooks can be used to create a dataset that gets displayed by a Bokeh Application that is nested inside a LOVE display, which is one of the key use-cases that will be encountered during observations.
There are details that remain to be solved, specifically aspects such as how to account for multiple reports that can be generated by re-running the same notebook multiple times.
These are presumed to be solvable problems but will require further investigation that goes beyond the scope of this group.

Bokeh Applications are extremely flexible in design and can render data from multiple sources if configured to do so.
This includes SAL events, Butler served information, or files from the LFA.
The apps can then create dynamic (or static) plots, display images, or even be setup to send commands to move the telescope based on a calculation (e.g. offsetting to a star).
One caveat is that they can only be used where they are deployed.
Should they wish to be used at the RSP for instance, they will need to be deployed there as well (and obviously any SAL commands will not work).

Bokeh Plotting Applications
''''''''''''''''''''''''''''

- Implementation of on-the-fly architecture requires Bokeh to be installed in all dev+RSP environments

   - Draft how to turn a notebook-based Bokeh "plot" into an app (see `Simon's draft <https://gist.github.com/SimonKrughoff/cc02f873a2a1518161d3f3a1839be4a5>`_)
   - Draft how to embed said App into LOVE 
   - Proof-of-concept of "press a button in the Camera Visualiation Tool" and get information from the callback in a Bokeh App

Papermill executed notebooks
'''''''''''''''''''''''''''''

- Suggested implementation for creating on-the-fly reports and re-runable notebooks that will store the parameters
- These will be published to the LFA

- TO DO
  - Work flow which includes an "easy" example of how to derive/calculate a property, then create+deploy and App, then send an alert to an observer
  - Appropriate repos and instructions




Augmenting Current Functionality
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Camera Visualization tools (as per described in the requirement section above)
- OCPS (really the butler) needs access to EFD (no explicit use-case at the moment) but one can invision how having a pipeTask be capable to correlate IQ against items in the EFD could be useful.

   - No code has been written to integrate butler directly with EFD, but it is possible to do
   - Could define pipelines that explicitly specified EFD datasets as pipeline inputs. 
     For now you would have to sort out the mapping of Exposure dataId to and EFD call in, I guess, a special runQuantum method in the pipeline task
  

See `Other Findings and Recommendations`_ regarding commissioning needs that are Out-Of-Scope for this committee.

.. 
   .. important::

      The mapping and implementation for each use-case assumes that the tools described in `Deliverable 5`_ and the additional required functionalities described in `Deliverable 6`_ have been incorporated.
      It may be useful to read those sections before this one.

      The response to this deliverable is found in each of the use-cases.

.. _Deliverable 5:

Deliverable 5:
--------------

.. note:: 

   The deliverable description from the `charge`_ is in the note below to ease readability 

   1.  A prioritized list of tasks to build-out the new functionalities with recommended end-dates. 
       - These dates shall correspond to integration milestones.


This is a prioritized list of which functionalities should be implemented in which order.


.. _Other Findings and Recommendations:

Other Findings and Recommendations
==================================

This section will have information that doesn't fall into the deliverables and/or is slightly outside of the scope.

.. note::

   This is still a bit of a dumping ground at the moment.

Diagnostic And Commissioning Cluster Usage Needs Definition
-----------------------------------------------------------

This working group was not able to find any documented strategy on how the commissioning and diagnostic clusters are to be used during commissioning and the survey.
High-level descriptions exist from early in the project, however they are not sufficient to build out the system and do not take into account much of the as-built software and hardware capabilities.
Although beyond the scope of this working group, it is strongly suggested that a strategy be developed that identifies and documents the use-cases, specifically in regards to the differences between how calibration and on-sky data is handled.
Currently, the camera diagnostic cluster hardware is on the summit but not being used, largely in part due to a lack of definition of it's use-cases and how it is to interact within the global data analysis workflow of the Rubin Observatory, including whether or not DM tooling must be supported.

This committee strongly recommends a follow-up committee be formed to address questions such as these which must be answered in the near term in order to properly prepare for commissioning.


Camera Visualization Tool Functionality Limitations for General Commissioning
-----------------------------------------------------------------------------

The Camera Visualization Tool, once augmented with the new specifications, will be sufficient for on-the-fly applications but will not be able to satisfy many of the more general commissioning use-cases. 
This committee was formed to look only at on-the-fly analyses, and the following specifications are out of scope, however, in order for the general commissioning effort to be successful the following functionalities will need to be implemented, or covered by a different suite of tooling.
Note that this is *not* a complete set of specifications for general commissioning but is merely a subset that we filtered out as being not specifically required for on-the-fly analysis.
If the capabilites were in place, then the on-the-fly users will certainly take advantage of them.

FAFF-REQ-014
^^^^^^^^^^^^

**Specification:** The camera visualization tool shall support being deployed in places where historical data is available.

**Rationale:** This will need to be available to people using the RSP and commissioning cluster.
The general commissioning / SV use case is to be able to examine aspects of image quality that cross detector boundaries (e.g., stray and scattered light, satellite trails, pervasive issues across detectors) for which full focal plane visualization is critical. 
Of course, these studies will involve looking at images that date back in time, and therefore will need to be executed from the RSP (or other processing center).
This could be useful even for summit operations if it allows display of historic images (for comparison with new images).
The historical data on the summit is currently limited to 30 days.

**Priority: 1**

**Current shortcomings:** Firefly may not meet all of the requirements for all image visualization, specifically in regards to full-frame visualization.
Deployment not nested into current RSP deployment strategy.
It requires a mechanism to locate the data for a given obsid, but this is also presumably be possible. 

**Applicable Use-cases:** Rapid per sensor image display and inspection.

**Suggested Implementation to fulfill requirement:** 
Deploy the camera visualization tool and Bokeh apps as part of the standard RSP packaging.
Installing the camera image visualization server at the USDC (SLAC) is certainly feasible. 


FAFF-REQ-026
^^^^^^^^^^^^

**Specification:** The display tool should be able to display data obtained from the butler, or obtained from a users interactive Jupyter session

**Rationale:** Displaying images with full DM ISR applied, co-added images etc.

**Priority:** 1

**Current shortcomings:** DM has an abstract image visualization interface (afw). 
Needs to be evaluated to see if this could be used to meet all the requirements.

**Applicable Use-cases:** 

**Suggested Implementation to fulfill requirement:** 


FAFF-REQ-017
^^^^^^^^^^^^

**Specification:** Ability to choose between minimal ISR versus some more sophisticated ISR (for example, the calexp images served from a butler)

**Rationale:** 

**Priority:**

**Current shortcomings:** Currently unable to interface to DM (butler) 

**Applicable Use-cases:**

**Suggested Implementation to fulfill requirement:**


TO DO BEFORE FINAL REPORT SUBMISSION
====================================

.. important::

   Remove this section before submitting

- Move confluence content into this technote where appropriate. 
  Prints of PDFs may be sufficient.

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
