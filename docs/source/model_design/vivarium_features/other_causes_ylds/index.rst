..
  Section title decorators for this document:
  
  ==============
  Document Title
  ==============
  Section Level 1
  ---------------
  Section Level 2
  +++++++++++++++
  Section Level 3
  ~~~~~~~~~~~~~~~
  Section Level 4
  ^^^^^^^^^^^^^^^
  Section Level 5
  '''''''''''''''

  The depth of each section level is determined by the order in which each
  decorator is encountered below. If you need an even deeper section level, just
  choose a new decorator symbol from the list here:
  https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#sections
  And then add it to the list of decorators above.

.. _other_causes_ylds:

.. role:: underline
    :class: underline

=========================================================
YLDs due to other causes
=========================================================

Years lived with disability (YLDs) are an important measure of health loss and 
contribute to estimates of disability adjusted life years (DALYs) in our model. 
This document discusses how YLDs are tracked in vivarium by default, how that 
compares to GBD methodology, and discusses potential improvements to this 
default behavior.

.. contents::
   :local:
   :depth: 2

Background and Motivation
--------------------------

GBD methodology
++++++++++++++++

Disability weight estimation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The GBD study relies on disability weights (DWs) to estimate years lived with 
disability (YLDs). DWs represent the magnitude of health loss associated with a 
specific health outcome and are measured on a scale from 0 to 1; 0 implies a 
state equivalent to full health and 1 equivalent to death.

Disability weights have been estimated through a series of surveys, described 
in detail in the GBD methods appendix along with a complete listing of 
disability weight values for all of the health states included in the GBD study.

Comorbidity correction
~~~~~~~~~~~~~~~~~~~~~~

The disability weights estimated through the process described above consider
each health state individually and must be corrected for the simultaneous 
presence of multiple disability-causing health states within a given 
individual. The GBD study performs this comorbidity correct (termed "COMO") 
using a microsimulation. 

The microsimulation is performed at the age-, sex-, location-, and 
year-specific level. For GBD 2019, the population size of each microsimulation 
was 40,000. In this microsimulation, each simulant is exposed to an independent 
probability of having any of the sequelae including in GBD using the estimated 
sequela prevalence.

.. note::

  The COMO process in GBD assumes total independence between health states and 
  does not account for any correlation that may exist between related health 
  states (beyond correlation induced by location, age, and sex, which is 
  accounted for through its stratified adjustments).

  As reported in the GBD 2019 methods appendix, "[GBD] tested the contribution 
  of dependent and independent comorbidity in the US MEPS data and found that 
  **independent comorbidity was the dominant factor** even though well-known 
  examples of dependent comorbidity exist, such as clustering of conditions 
  like diabetes and stroke or anxiety and alcohol use disorders" (p. 475).

Once this population has been initialized, the comorbidity correction is 
performed as follows:

For each simulant, the overall disability weight is calculated as the 
multiplicative combination of each of the sequela that the simulant possesses 
according to the following equation:

.. math::

  DW_\text{overall} = 1 - \prod_{k=i}^j (1 - DW_k)

Where: :math:`DW_k` is the *Kth* disease sequelae that the simulant possesses.

Once this overall disability weight is computed for each individual simulant, 
it is then disaggregated into its component parts once more to obtain 
cause-specific disability weights adjusted for comorbidity according to the 
following formula:

.. math::

  ADW_k = \frac{DW_k}{\sum_{k=i}^j DW_k} \times DW_\text{overall}

Where: :math:`ADW_k` is the comorbidity-adjusted disability weight for sequela 
*K* among a given simulant.

Then, cause-specific YLDs per capita in an age-sex-location-year are computed 
as the sum of the adjusted DWs for each sequela across all *n* simulants in the 
microsimulation as:

.. math::
  
  \text{YLD rate}_k = \frac{\sum_{simulant=1}^n ADW_k}{n}

The implication of the COMO correction for GBD YLDs attributable to a parent 
cause should be exactly equal to the sum of YLDs attributed to each of its 
sub-causes. Additionally, the YLDs attributable to a given sequela after the 
COMO adjustment will be less than the product of the disability weight and the 
prevalence for that sequela.

Historical behavior in vivarium
++++++++++++++++++++++++++++++++

For a given vivarium simulation, we have typically developed individual cause 
models for specific conditions. Conditions to include in a simulation are 
selected based on relevance to the research question and are not exhaustive of 
all conditions modeled in GBD. This poses a question of how to handle morbidity 
and mortality due to causes not explicitly modeled within our simulation 
("background" causes). Historically, vivarium has handled background morbidity 
differently from background mortality, as described below.

Background mortality
~~~~~~~~~~~~~~~~~~~~

Vivarium automatically models background mortality in addition to mortality due 
to modeled causes as mortality due to :code:`other_causes`. The cause-specific 
mortality rate of :code:`other_causes` is equal to:

.. math::

  CSMR_\text{other causes} = ACMR - \sum_{c=i}^n CSMR_c

Where: :math:`ACMR` is the all-cause mortality rate and :math:`CSMR_c` is the 
cause-specific mortality rate of a modeled cause :math:`c`.

Background morbidity
~~~~~~~~~~~~~~~~~~~~

Historically, vivarium has **not** modeled background morbidity. Rather, by 
default, vivarium models morbidity due to modeled causes only. Among the 
modeled causes, a partial comorbidity adjustment is performed.

Specifically, for a simulant that possesses multiple causes of disability 
simultaneously, the YLDs accrued for that simulant in a given timestep are 
equal to:

.. math::

  YLDs_\text{all causes} = (1 - \prod_{c=i}^n (1 - DW_c)) * \text{time step scalar}

.. math::

  YLDs_\text{cause-specific} = DW_c * \text{time step scalar}

Vivarium can then observe YLDs due to all *modeled* causes AND/OR 
cause-specific YLDs accrued in the simulation. Notably, the sum across 
cause-specific YLDs will be greater than the "all-cause" YLDs in vivarium 
simulation outputs using this methodology.

Problem space
+++++++++++++

There are multiple problems with the default vivarium behavior not to model 
background morbidity, including:

- Comorbidity adjustment issues

  - Cannot calculate cause-specific YLDs adjusted for comorbidity even just among modeled causes, resulting in overestimation of cause-specific YLDs relative to "all cause" YLDs among modeled causes

  - Does not adjust for comorbidity due to unmodelled causes, resulting in overestimation of YLDs due to modeled causes relative to GBD estimates

- Underestimation of total YLDs

  - Only observe a subset of total YLDs within our simulation. While we can calculate YLDs averted between scenarios, we cannot accurately calculate percent reduction in all-cause YLDs or DALYs relative to baseline because we do not model YLDs due to all causes at baseline.

  - Causes us to overestimate impact of a death averted in our simulation. Death averted some number of YLLs averted, but really this person should then start accruing YLDs overtime, which will decrease the number of DALYs averted relative to baseline. 

Proposal
--------------

To address these issues, we propose to update the default vivarium behavior to 
model background morbidity in a similar manner to the default behavior to model 
background mortality. 

.. todo::

  Investigate and document pros and cons associated with 4 different options:

    1. Current/historical behavior

    2. Model DWs due to all sequelae individually (potentially computationally 
    burdensome and requires timestep updates?)

    3. Model DWs due to modeled causes AND composite COMO-adjusted YLDs due to 
    background causes (note that these *should* increase slightly when we 
    reduce YLDs due to modeled causes, but proposal will not include this 
    behavior... need to check to see if it is expected to be more or less 
    biased (compared to "gold standard" option 2) than option #1 is).... we'll 
    also have some potential double-counting issues here that need to be 
    thought through.

    4. Run custom como adjustment among background causes once prior to 
    simulation start to determine como-adjusted DW excluding all modeled causes 
    and then use that as DW for background morbidity throughout simulation. 
    This is probably best compromise, but does require additional upfront work

Potential use cases
+++++++++++++++++++


Challenges to consider
++++++++++++++++++++++
