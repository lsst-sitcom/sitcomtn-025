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
The template includes 

The standardized use-cases associated with this deliverable are found at :ref:`pg-D1-Use-cases` page.

.. _Deliverable 2:

Deliverable 2:
--------------

.. note:: 

   The deliverable description from the `charge`_ is in the note below to ease readability 

   2. A set of requirements to augment, clarify, or remove ambiguity that may occur from looking only at the use-cases.
      
      - This is not a formal and all-encompassing LSE requirements document.

Like the use-cases, the requirement deliverables have been separated into two different categories, those which are focused on tooling and procedures for on-the-fly analysis and image visualization.
The two categories are quite separate in nature yet not completely independent.

The detailed answer is found on :ref:`pg-D2-Requirements`.


.. _Deliverable 3:

Deliverable 3:
--------------

.. note:: 

   The deliverable description from the `charge`_ is in the note below to ease readability 

   1. A summary of the suite of currently available tools/systems that may help fulfil the required functionality.

      - Report on their status, limitations, and where their current requirements may need to be expanded to cover any increase in scope
      - Tools which were considered but found to be inadequate must also be reported


Tools:

- Camera Visualization Tool
- Watcher
- faro
- OCPS 
- Nublado Interface (USDF, Summit etc) with Jupyter Notebook Functionality

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

- Implementation of on-the-fly architecture requires Bokeh to be installed in all dev+RSP environments

   - Draft how to turn a notebook-based Bokeh "plot" into an app (see `Simon's draft <https://gist.github.com/SimonKrughoff/cc02f873a2a1518161d3f3a1839be4a5>`_)
   - Draft how to embed said App into LOVE



- Derive a "Catcher CSC" Design. This design should.

  - "Catcher" CSC, with LOVE GUI(s?)
  - Work flow which includes an "easy" example of how to derive/calculate a property, then create+deploy and App, then send an alert to an observer
  - Appropriate repos and instructions


Out of Scope but completely necessary is the addition of a butler interface to the camera visualization tool.
Ideally, the Bokeh apps should also be available to people on the RSP? 



Augmenting Current Functionality
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


- Camera Visualization tools (as per described in the requirement section above)
- OCPS must be able to operate in a way that is not only associated with images
- OCPS needs access to EFD

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
