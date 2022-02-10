.. _other_models_pregnancy:

..
  Section title decorators for this document:

  ==============
  Document Title
  ==============

  Section Level 1 (#.0)
  ---------------------

  Section Level 2 (#.#)
  +++++++++++++++++++++

  Section Level 3 (#.#.#)
  ~~~~~~~~~~~~~~~~~~~~~~~

  Section Level 4
  ^^^^^^^^^^^^^^^

  Section Level 5
  '''''''''''''''

  The depth of each section level is determined by the order in which each
  decorator is encountered below. If you need an even deeper section level, just
  choose a new decorator symbol from the list here:
  https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#sections
  And then add it to the list of decorators above.

=========================
Pregnancy
=========================

.. contents::
   :local:
   :depth: 1

.. list-table:: Abbreviations
  :widths: 15 15 15
  :header-rows: 1

  * - Abbreviation
    - Definition
    - Note
  * - ASFR
    - Age-specific fertility rate
    - 
  * - SBR
    - Stillbirth to live birth ratio
    - 
  * - LBWSG
    - Low birthweight and short gestation
    - 

Overview
-------------
GBD Modeling Strategy
----------------------

Pregnancy and births are not explicit outcomes in the GBD study. However, there are location- and year-specfic GBD covariates related to fertility and births, including:

.. list-table:: Covariates
  :widths: 15 15 15
  :header-rows: 1

  * - Covariate name
    - Covariate ID
    - Note
  * - Live births (thousands)
    - 60
    - 
  * - Live births by sex
    - 1106
    - 
  * - Live births by sex and maternal age
    - 2298
    - 
  * - Stillbirth to live birth ratio
    - 2267
    - Not specific to maternal age. Upper and lower bound estimates are equal to mean estimate (no uncertainty interval)
  * - Age-specific fertility rate
    - 13
    - 
  * - Total fertility rate
    - 2363
    - 

GBD has estimated the prevalence of pregnancy (as an intermediate variable for the estimation of various outcomes) as:

.. math::

   (ASFR + (SBR * ASFR)) * 46/52

Where :math:`ASFR` is the age-specific fertility rate, :math:`SBR` is the stillbirth to live birth ratio, and :math:`46/52` is the proportion of the year spent pregnant (40 weeks) and postpartum (6 weeks) [GBD-2019-Risk-Factors-Appendix-pregnancy]_.

.. todo::

   Determine the threshold of gestational age at which a loss of pregnancy is classified as a stillbirth rather than miscarriage for the GBD covariate. Standard thresholds are 20 or 24 weeks.

Vivarium Modeling Strategy
----------------------------

We will model pregnancy as a characteristic of women of reproductive age in our simulations. We will inform the *incidence* of pregnancy using the age-specific fertility and stillbirth to live birth ratio covariates from GBD. We will inform the *duration* of pregnancy using the GBD 2019 exposure distribution of gestational age.

.. image:: diagram.svg

.. list-table:: State definitions
  :widths: 15 15 15
  :header-rows: 1

  * - State
    - Description
    - Note
  * - np
    - Not pregnant or postpartum
    - 
  * - p
    - Pregnant
    - 
  * - pp
    - Postpartum
    - 

.. list-table:: State prevalence table
  :widths: 15 15 15
  :header-rows: 1

  * - Exposure category
    - Value
    - Note
  * - np
    - 1 - prevalence_p - prevalence_pp
    - If using a burn-in strategy, initialize all simulants to np state
  * - p
    - :math:`(ASFR + ASFR * SBR) * 40 / 52 + (incidence_\text{c995} + incidence_\text{c374}) * 24 /52`
    - Consider updating to reflect average gestational age for location of interest rather than 40 weeks (Ali will need to perform weighting calculation from LBWSG exposure distributions)
  * - pp
    - :math:`(ASFR + ASFR * SBR + incidence_\text{c995} + incidence_\text{c374}) * 6 / 52`
    -  

.. list-table:: State transition data
  :header-rows: 1

  * - Source state
    - Sink state  
    - Transition name
    - Transition rate
    - Note
  * - np
    - p
    - incidence_p
    - :math:`\frac{ASFR + ASFR * SBR + incidence_\text{c995} + incidence_\text{c374}}{prevalence_\text{np}}`
    - 
  * - p
    - pp
    - duration_p
    - Informed by gestational age (see below section)
    - Duration-based transition
  * - pp
    - np
    - duration_pp
    - 6 weeks (42 days) duration
    - Duration-based transition

.. list-table:: Data values
  :header-rows: 1

  * - Parameter
    - Data type  
    - Data ID
    - Source
    - Note
  * - ASFR
    - Covariate
    - 13
    - get_covariate_estimates: decomp_step='step4' or 'iterative' for GBD 2019, 'step3' or 'iterative' for GBD 2020
    - Assume normal distribution of uncertainty. Regional-level estimates available.
  * - SBR
    - Covariate
    - 1106
    - get_covariate_estimates: decomp_step='step4' or 'iterative' for GBD 2019, 'step3' or 'iterative' for GBD 2020
    - No uncertainty in this estimate: use mean_value as point value for this parameter. Regional-level estimates not available.
  * - incidence_c995
    - Incidence rate of abortion and miscarriage cause
    - c995
    - como; decomp_step='step5'
    - Abortion defined as elective or medically-indicated termination of pregnancy at any gestational age and miscarriage defined as spontaneous loss of pregnancy before 24 weeks gestation
  * - incidence_c374
    - Incidence rate of ectopic pregnancy
    - c374
    - como; decomp_step='step5'
    - 

.. list-table:: Restrictions
   :widths: 15 15 20
   :header-rows: 1

   * - Restriction Type
     - Value
     - Notes
   * - Male only
     - False
     -
   * - Female only
     - True
     -
   * - Age group start
     - 10 to 14 years
     - ID=7
   * - Age group end
     - 50 to 54 years
     - ID=15

.. note::

   At initialization:

    1. Assign pregnancy state according to state prevalence values
    2. Assign pregnancy outcome according to pregnancy outcome table probabilities
    3. Assign sex of infant if pregnancy outcome is a stillbirth or live birth
    4. Assign duration of pregnancy depending on pregnancy outcome and sex of the infant
    5. Determine the amount of time remaining in pregnancy from the duration of pregnancy
    6. Begin simulation

  During simulation:

    Determine pregnancy model state according to state at initialization and state transition rates. Upon transition from the np to p state, follow steps 2-4 described above.

Pregnancy outcome
~~~~~~~~~~~~~~~~~~

A pregnancy outcome must be determined for each pregnancy as either a 1) live birth, 2) stillbirth, or 3) abortion/miscarriage. The probability of each pregnancy outcome is defined in the table below. The outcome of each pregnancy should be determined at the start of pregnancy in our simulation (upon transition from the np to p states or upon initialization into the p state). 

.. list-table:: Pregnancy outcomes
  :header-rows: 1

  * - Outcome
    - Probability
    - Note
  * - Live birth
    - ASFR / (ASFR + ASFR * SBR + incidence_c995 + incidence_c374)
    - This outcome will be used to inform the demography model of children under 5 for the IV iron simulation. The probability of a livebirth outcome will increase as a result of the reduction in the probability of a stillbirth associated with iron interventions during pregnancy (to be implemented in model version III).
  * - Stillbirth
    - (ASFR * SBR) / (ASFR + ASFR * SBR + incidence_c995 + incidence_c374)
    - The probability of a stillbirth outcome will decrease as a result of iron interventions during pregnancy in the IV iron simulation (to be implemented in model version III)
  * - Other (abortion, miscarriage, ectopic pregnancy)
    - (incidence_c995 + incidence_c374) / (ASFR + ASFR * SBR + incidence_c995 + incidence_c374)
    -     

Sex of infant
~~~~~~~~~~~~~~~

For pregnancies that result in live birth or stillbirth outcomes, infant sex should be determined and recorded acording to the probability of male sex shown in the table below (probability of female birth is equal to 1 minus the probability of male birth). This should be performed at the start of pregnancy (transition from np to p states) or upon initialization into the p state. These sex ratios were calculated using the live births by sex 2020 GBD covariate (ID 1106), `shown here <https://github.com/ihmeuw/vivarium_research_iv_iron/blob/main/sex_ratio_calculation.ipynb>`_. 

.. _sex_ratio_table:

.. list-table:: Probability of male birth
    :header-rows: 1

    *   - Location
        - Location ID
        - Value
    *   - South Asia
        - 159
        - 0.522171
    *   - Sub-Saharan Africa
        - 166
        - 0.509677
    *   - World bank lower middle income
        - 44577
        - 0.513906
    *   - World bank low income
        - 44578
        - 0.512684

Duration of pregnancy
~~~~~~~~~~~~~~~~~~~~~~~~

A duration of pregnancy value will need to be assigned to all pregnancies regardless of the pregnancy outcome. This value will inform the duration that the simulant remains in the pregnancy state prior to transitioning to the postpartum state.

For pregnancies that result in abortion/miscarriage/ectopic pregnancy, assign a duration of pregnancy of 24 weeks.

For pregnancies that result in live births or stillbirths, duration of pregnancy should be determined by gestational age exposure, which should be assigned according to the process for assigning LBWSG exposures described in the :ref:`risk correlation document between maternal BMI, maternal hemoglobin, and infant LBWSG exposure <2019_risk_correlation_maternal_bmi_hgb_birthweight>`. The LBWSG exposure distribution used to assign gestational age exposures should be specific to the sex of the infant for a given pregnancy (discussed in the above section). Note that the gestational age distribution is measured in weeks and will need to be converted to the equivalent simulation time measure.

For simulants who are initialized into the pregnancy state at the start of the simulation:

   Assign the simulant a duration of pregnancy/gestational age value and then sample a random value from a uniform distribution between zero and the assigned gestational age value. The randomly sampled value will represent the current gestational duration of that pregnancy. The simulant should remain in the pregnancy state prior to transitioning to the postpartum state for the duration equal to the assigned gestational age value *minus* the randomly sampled value.

Assumptions and limitations
++++++++++++++++++++++++++++

- We assume that the gestational age distribution of stillbirths is equal to the gestational age distribution of live births. This is a limitation of our analysis given the lack of data on the distribution of gestational ages for which these outcomes occur. Given that the gestation for these outcomes is likely shorter than gestation for live births on average, we are likely overestimating the average duration of pregnancy for outcomes other than live births.
- We assume that all abortions, miscarriages and ectopic pregnancies occur at 24 weeks gestatation. 
- We assume that abortions that occur after 24 weeks are not considered stillbirths for estimation of the stillbirth to livebirth ratio. We may overestimate the incidence rate of pregnancy due to this assumption.
- We are limited in the assumption that the stillbirth to livebirth ratio does not vary by maternal age and does not incorporate an uncertainty distribution.
- We do not model any morbidity (YLDs) associated directly with pregnancy.
- We do not distiguish between intended and unintended pregnancies.
- We do not consider the impact of birth interval timing or family size in our model of pregnancy.
- We assume that a new pregnancy cannot occur during the postpartum period but can occur immediately afterward. We do not model any difference in the fertility rate by recent pregnancy status.
- We do not consider the impact of singleton versus non-singleton pregnancies.
- We are limited in that we apply the age-specific fertility rate to the *beginning* of pregnancy when it is actually measured at the *end* of pregnancy (birth). Therefore, maternal age at birth in our simulation will be overestimated due to simulants becoming pregnant in one age group and giving birth in the next.

Verification and validation criteria
++++++++++++++++++++++++++++++++++++++

Person-time spent in each pregnancy state should approximate to the values in the state prevalence table.

The number of transitions into the pregnancy state should validate to the values in the state transition data table at the age-specific level. Across all ages, it should validate to the total fertility rate covariate (ID=1106). Additionally, when scaled to the total population and adjusted for the SBR, the number should approximate the live birth covariate (ID=60).

References
-----------

.. [GBD-2019-Risk-Factors-Appendix-pregnancy]

 `Supplementary appendix 1 to the GBD 2019 Risk Factors Capstone <2019_risk_factors_methods_appendix_>`_:

   **(GBD 2019 Risk Factors Capstone)** GBD 2019 Risk Factors Collaborators.
   :title:`Global burden of 87 risk factors in 204 countries and territories,
   1990–2019: a systematic analysis for the Global Burden of Disease Study
   2019`. Lancet 2020; **396:** 1223–49. DOI:
   https://doi.org/10.1016/S0140-6736(20)30752-2

.. _2019_risk_factors_methods_appendix: https://www.thelancet.com/cms/10.1016/S0140-6736(20)30752-2/attachment/54711c7c-216e-485e-9943-8c6e25648e1e/mmc1.pdf