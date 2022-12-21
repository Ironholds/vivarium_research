.. role:: underline
    :class: underline
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

.. _lipid_based_nutrient_supplements:

========================================================
Small quantity lipid based nutrient supplements (SQ-LNS)
========================================================

.. note::

  This page underwent a revision in December of 2022 to reflect an a desired modeling strategy update of the :ref:`phase II acute malnutrition simulation <2020_concept_model_vivarium_ciff_sam>` in response to updated effect size data and feedback obtained from collaborators. `The pull request associated with these updates can be found here <https://github.com/ihmeuw/vivarium_research/pull/1097>`_. A summary of the associated changes to the modeling strategy includes:

  - Data changes to effect of SQ-LNS on moderate and severe stunting
  - Change to how stunting effects are applied (increases in stunting TMREL category rather than mild stunting category)
  - SQ-LNS effects on stunting will persist until 5 years of age instead of ceasing at 2 years of age
  - SQ-LNS impacts on each wasting transition rate (including remission rates) rather than only the transition rate between mild and moderate wasting

  Prior to the update in December of 2022, this page underwent a revision in August of 2022 to reflect desired modeling strategy of the :ref:`phase II acute malnutrition simulation <2020_concept_model_vivarium_ciff_sam>` updated from the strategy used for the :ref:`phase I acute malnutrition simulation <2019_concept_model_vivarium_ciff_sam>`.

  The PRs associated with these updates are listed below.

  - `https://github.com/ihmeuw/vivarium_research/pull/961 <https://github.com/ihmeuw/vivarium_research/pull/961>`_
  - `https://github.com/ihmeuw/vivarium_research/pull/963 <https://github.com/ihmeuw/vivarium_research/pull/963>`_

  A summary of the associated changes to modeling strategy includes:

  - Reduction in the age end parameter from 5 years to 2 years
  - Update of the effect size on child wasting in accordance with newly published literature, also now sex-specific
  - New utilization algorithms
  - Potential for new intervention effect on ACMR (not currently included in this document)

Ideally, infants are breastfed for two years or longer, with complementary food introduced at six months of age. Diets of infants and young children aged six to 23 months need to include a variety of nutrient-dense foods, preferably from local sources, to ensure their nutrient needs are met. However, children's diets are likely to be deficient in macronutrients and micronutrients, specifically essential fatty acids, when nutrient-rich diets are not available to them in resource-poor settings. Various interventions are recommended, or have been used, to improve child malnutrition. This document focuses on small quantity lipid based nutrient supplements (SQ-LNS) as an intervention to improve malnutrition, particularly child wasting and stunting. Particularly, it draws on the 2019 Cochrane Review by Das et. al. [DAS_Cochrane_Review_2019]_

.. contents::
   :local:
   :depth: 1

.. list-table:: Abbreviations
  :widths: 15 15 15
  :header-rows: 1

  * - Abbreviation
    - Definition
    - Note
  * - LNS
    - lipid-based nutrient supplements
    -
  * - SQ
    - small quantity
    -
  * - FBF
    - fortified blended foods
    -
  * - CSB ++
    - corn soy blend plus
    -
  * - MNP
    - Multiple micronutrient powders
    -

Intervention Overview
-----------------------

:underline:`Lipid-based nutrient supplements`

Supplementary feeding is a strategy that includes provision of extra food to children beyond the normal ration of their home diets
and is aimed at improving the nutritional status or preventing the nutritional deterioration of the target population. One of the nutritional
interventions advocated to address malnutrition among children is **lipid-based nutrient supplements (LNS)**. LNS are a family of
products designed to deliver nutrients to vulnerable people. They are considered 'lipid-based' because most of the energy provided
by these products is from lipids (fats). All LNS provide a range of vitamins and minerals, but unlike most other micronutrient supplements,
LNS also provide *energy, protein and essential fatty acids*. LNS recipes can include a variety of ingredients, but typically have included vegetable fat, peanut or groundnut paste, milk powder and sugar. Based on the energy content, LNS can be small quantity (SQ LNS) providing ˜ 110 to 120 kcal/day (20 g dose), medium quantity (MQ LNS) providing ˜ 250 to 500 kcal/day (45 g to 90 g dose) or large-quantity (LQ LNS) providing
more than 280 kcal/day (> 90 g dose). Notably, MQ-LNS and LQ-LNS can be used in :ref:`management of acute malnutrition interventions <intervention_wasting_treatment>`. Additionally, :ref:`antenatal supplementation with balanced energy protein (BEP) <maternal_supplementation_intervention>` utilitizes LNS products. LNS are nutrient dense, require no cooking before use, and can be stored for months even in warm conditions.
[DAS_Cochrane_Review_2019]_

:underline:`Alternative recipes and formulations, other than LNS`

Alternative recipes and formulations, other than LNS, are currently being explored using cereals mixed with other ingredients, including
whey, soy protein isolate, dried skimmed milk, and sesame, cashew and chickpea paste, among others. These are
fortified with vitamins and minerals and are commonly called **fortified blended foods (FBF)**. An example of a commonly used FBF
is **corn soy blend plus (CSB ++)**, which is a cooked blend of milled, heat-treated corn and soybeans that is fortified with a vitamin and
mineral premix. **Multiple micronutrient powders (MNP)** are also an alternative way of providing micronutrients. These are single-dose
packets of vitamins and minerals in powder form that can be sprinkled onto any ready to eat semi-solid food consumed at home,
school or any other point of use. [DAS_Cochrane_Review_2019]_

:underline:`Description of intervention`

The intervention is the supplementation of children from aged **6 months to 23 months** with **LNS + complementary feeding** (intervention) compared with no intervention (control). The setting of the intervention is the community.

:underline:`Summary of existing intervention literature`

There have been several recent meta-analyses on the effects of SQ-LNS, outlined below:

- A Cochrane systematic review and meta-analysis [DAS_Cochrane_Review_2019]_. Please see this memo for a summary of the studies and the effect sizes :download:`SQ-LNS interventions memo<sqlns_memo_das2019.docx>`

- A meta-analysis of RCTs on all-cause mortality [Stewart-et-al-2020]_

  - All-cause mortality was significantly lower in the SQ-LNS arm than the non-SQ-LNS arm (RR: 0.73; 95% CI: 0.59, 0.89; 13 trials)

- A series of individual participant data meta-analyses (with analysis of effect modification), as summarized by [Dewey-et-al-2021a]_. These papers included eligible studies from the review published by [DAS_Cochrane_Review_2019]_ as well as additional data published following the publication of the cochrane review. Minimum supplementation duration for inclusion was three months between the ages of six and 24 months of age. Most studies began supplementation at six months of age with intended supplementation duration of 6-18 months. The series included individual analyses on the following outcomes:

  - Growth outcomes: [Dewey-et-al-2021b]_

    - Stunting prevalence ratio: 0.88 (95% CI: 0.85, 0.91)

    - Wasting prevalence ratio: 0.86 (95% CI: 0.80, 0.93) and acute malnutrition prevalence ratio: 0.86 (95% CI: 0.80, 0.93)

      - Effect modified by sex

  - Anemia and micronutrient status: [Wessells-et-al-2021]_

    - Significantly decreased anemia and vitamin A deficiency

    - No effect on plasma zinc or retinol

  - Developmental outcomes: [Prado-et-al-2021]_

    - Increased mean language, social-emotional, and motor scores.

    - Increased prevalence of walking without support at 12 months

Note, we have received data directly from the [Dewey-et-al-2021b]_ authors on the 4-category severity-specific prevalence ratios of SQ-LNS wasting and stunting. This data can be found at :code:`J:\Project\simulation_science\ciff_malnutrition\Data\sqlns_effects\ipd_list request_20220727.xlsx` and will be what we use to inform our analysis.

.. _`sqlns-baseline-parameters`:

Baseline Coverage Data
++++++++++++++++++++++++

No baseline coverage of SQ-LNS (0%)

Vivarium Modeling Strategy
--------------------------

.. _utilization-definition:

Utilization algorithms
++++++++++++++++++++++++

We will consider two concepts of SQ-LNS services, including coverage and utilization:

- **Coverage:** *access* to the intervention (such as living in an area where SQ-LNS products are available and in use). This will be determined by coverage scale-up algorithms in the concept model document.

Possible coverage values include:

- **Uncovered:** Not supplemented by SQ-LNS currently or in the past. Simulants aged 6 months to 5 years are eligible for this category.
- **Covered:** Actively receiving SQ-LNS supplementation. Simulants aged 6 months to 2 years are eligible for this category. Simulants in this category are subject to the SQ-LNS effects on wasting and stunting.
- **Received:** No longer actively receiving SQ-LNS supplementation, but did receive SQ-LNS supplementation before the age of 2. Simulants aged 2 to 5 years are eligible for this category. Simulants in this category are subject to the SQ-LNS effects on stunting, but not wasting.

- **Utilization:** *use* of the intervention (actually taking the supplements and receiving the effects). This will be determined by the utilization algorithms below.

There are various SQ-LNS utilization algorithms that may be desired under differing scenarios. They include:

- **Universal coverage:** All covered simulants receive effects starting at six months of age.
- **Targeted to AM treatment:** Covered simulants who transition from MAM or SAM to mild wasting will receive intervention effects starting at that timestep.
- **Targeted to mild wasting:** Covered simulants who are initialized into or transition into the mild wasting state will receive intervention effects starting at that timestep.

SQ-LNS effects on wasting will persist until 24 months of age and effects on stunting will persist until five years of age.

.. list-table:: SQ-LNS Utilization Restrictions
  :widths: 15 15 15
  :header-rows: 1

  * - Restriction
    - Value
    - Note
  * - Male only
    - No
    -
  * - Female only
    - No
    -
  * - Age group start
    - 389 (6-11mo)
    - intervention starts at 6 months
  * - Age group end (exclusive)
    - 34 (2-4yr)
    - Children >24 months of age **not** eligible
  * - Other
    -
    -

Affected Outcomes
+++++++++++++++++

Wasting
~~~~~~~

.. note::

  These values changed in both the December and August, 2022 updates

Since the effect of SQ-LNS on child wasting was measured in prevalence ratios, it is not known whether SQ-LNS reduces wasting prevalence through a reduction of wasting incidence or duration. Therefore, we will run a sensitivity analysis in which SQ-LNS affects wasting incidence rates and another in which SQ-LNS affects wasting recovery rates. Additionally, due to the multicompartment transition model of child wasting used in our simulation, we cannot apply the observed prevalence ratios directly to wasting transition rates to replicate the intended prevalence ratios. Rather, we `solved for specific transition rate ratios (separately for incidence and recovery rates) that resulted in the intended prevalence ratios of SQ-LNS <https://github.com/ihmeuw/vivarium_research_wasting/blob/main/misc_investigations/Prevalence%20ratio%20nano%20sim%20.ipynb>`_. Notably, these values were calibrated to the child population in Ethiopia and the calibration may not hold for all other populations and should be tested before applying to different locations. 

Wasting transition rates affected by SQ-LNS are documented on the :ref:`dynamic wasting transition model document <2020_risk_exposure_wasting_state_exposure>`. The intervention effect should apply immediately upon coverage of the intervention and should be applied *multiplicatively* to the affected measure. The SQ-LNS effects on wasting transition rates should apply to simulants covered by SQ-LNS from the start of coverage (at six months of age) until they are 2 years of age, at which point SQ-LNS should no longer affect their transition rates. In other words, the :code:`covered` SQ-LNS coverage state affects wasting transitions rates, but the :code:`received` and :code:`uncovered` states do not.

.. note::

  Lognormal distributions of uncertainty should be used for all effect sizes in the table below and the uncertainty intervals.

.. list-table:: Wasting outcome effect sizes
  :header-rows: 1

  * - Outcome
    - Effect size measure
    - Effect size
    - Sensitivity analysis
    - Note
  * - i3 rate from wasting TMREL to mild wasting
    - Relative risk
    - 0.88 (0.825, 0.957)
    - Incidence effects
    - 
  * - i2 rate from mild wasting to MAM
    - Relative risk
    - 0.895 (0.82, 0.96)
    - Incidence effects
    - 
  * - i1 rate from MAM to SAM
    - Relative risk
    - 0.771 (0.655, 0.885)
    - Incidence effects
    - 
  * - r4 rate from mild wasting to wasting TMREL
    - Relative risk
    - 1.125 (1.045, 1.187)
    - Recovery effects
    - 
  * - r3 rate from MAM to mild wasting
    - Relative risk
    - 1.086 (1.025, 1.165)
    - Recovery effects
    - 
  * - r1 (SAM to MAM) and t1 (SAM to mild) rates 
    - Relative risk
    - 1.28 (1.12, 1.465)
    - Recovery effects
    - Apply this effect to both r1 and t1 transition rates

Stunting
~~~~~~~~~

.. note:: 

  These values changed in the December, but not August, 2022 update

**We will apply the SQ-LNS prevalence ratios on the** :ref:`stunting risk exposure distribution <2020_risk_exposure_child_stunting>` **among simulants covered by SQ-LNS from the start of supplementation (six months of age) until they turn five years of age.** In other words, both the :code:`covered` and :code:`received` SQ-LNS coverage states affect stunting, but the :code:`uncovered` state does not. The application of the SQ-LNS effect on stunting through five years of age (beyond the duration of supplementation) was advised by collaborators, with the rationale that height gains achieved during SQ-LNS supplementation will persist throughout life (unlike wasting-associated weight gains). 

Additionally, as suggested by the observed prevalence ratios from the meta-analysis, we will assume that SQ-LNS results in decreases to the prevalence of moderate and severe stunting, no change to the prevalence of mild stunting, and increases to the stunting TMREL category that are equal to the sum of the decreases in prevalence of moderate and severe stunting. The figure below demonstrates how to apply the effects summarized in the table to the stunting risk exposure distribution of simulants affected by SQ-LNS.

.. list-table:: Stunting outcome effect sizes
  :header-rows: 1

  * - Outcome
    - Effect size measure
    - Effect size
    - Note
  * - Moderate (cat2) stunting exposure
    - Prevalence ratio
    - 0.89 (0.86, 0.93), lognormal distribution of uncertainty
    - 
  * - Severe (cat1) stunting exposure
    - Prevalence ratio
    - 0.83 (0.78, 0.90), lognormal distribution of uncertainty
    - 

.. image:: viviarium_strategy_stunting.svg

Mortality
~~~~~~~~~~

.. todo::
    
  Determine if necessary to include in model... we think no, but this should be discussed in limitations/assumptions of the simulation

Hemoglobin/Anemia
~~~~~~~~~~~~~~~~~~

Not currently modeled as part of the :ref:`wasting simulation <2020_concept_model_vivarium_ciff_sam>` given that only YLDs will be affected.

Vitamin A Deficiency
~~~~~~~~~~~~~~~~~~~~

Not currently modeled as part of the :ref:`wasting simulation <2020_concept_model_vivarium_ciff_sam>` as this is not a primary outcome of interest and any downstream effects on mortality will be included in the `Mortality`_ outcome above.

Cost Model
+++++++++++

Assumptions and Limitations
++++++++++++++++++++++++++++

- We assume a constant effect of SQ-LNS wasting transition rates. This means that wasting prevalence ratios will equal 1 at the start of supplementation and progress towards the measured prevalence ratios until they reach a level of stability at some later point. We make this assumption in the absence of measured prevalence ratios as mutliple follow-up points.

- We assume that these effect generalize from the populations included in the meta-analysis of SQ-LNS trials to our simulated populations.

Validation and Verification Criteria
+++++++++++++++++++++++++++++++++++++

- verification: coverage of SQ-LNS as a function of time and eligible populations in baseline and intervention scenario
- verification: prevalence of stunting in supplemented vs non-supplemented group
- verification: wasting transition rates in supplemented vs non-supplemented group
- validation: check that the wasting prevalence ratios replicate the desired values

References
-----------

.. [DAS_Cochrane_Review_2019]

  View `DAS Cochrane Review 2019`_

    Preventive lipid‐based nutrient supplements given with complementary foods to infants and young children 6 to 23 months of age for health, nutrition, and developmental outcomes

.. _`DAS Cochrane Review 2019`: https://www.cochranelibrary.com/cdsr/doi/10.1002/14651858.CD012611.pub3/full

.. [Dewey-et-al-2021a]

  View `Dewey et al 2021a <https://pubmed.ncbi.nlm.nih.gov/34590696/>`_

    Dewey KG, Stewart CP, Wessells KR, Prado EL, Arnold CD. Small-quantity lipid-based nutrient supplements for the prevention of child malnutrition and promotion of healthy development: overview of individual participant data meta-analysis and programmatic implications. Am J Clin Nutr. 2021 Nov 2;114(Suppl 1):3S-14S. doi: 10.1093/ajcn/nqab279. PMID: 34590696; PMCID: PMC8560310.

.. [Dewey-et-al-2021b]

  View `Dewey et al 2021b <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8560308/pdf/nqab278.pdf>`_

    Dewey KG, Wessells KR, Arnold CD, Prado EL, Abbeddou S,
    Adu-Afarwuah S, Ali H, Arnold BF, Ashorn P, Ashorn U, et al.
    Characteristics that modify the effect of small-quantity lipid-based
    nutrient supplementation on child growth: an individual participant
    data meta-analysis of randomized controlled trials. Am J Clin Nutr
    2021;114(Suppl 11):15S–42S.

.. [Prado-et-al-2021]

  View `Prado et al. 2021 <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8560311/pdf/nqab277.pdf>`_

    Prado EL, Arnold CD, Wessells KR, Stewart CP, Abbeddou S, Adu-
    Afarwuah S, Arnold BF, Ashorn U, Ashorn P, Becquey E, et al. Smallquantity
    lipid-based nutrient supplements for children age 6–24 months:
    a systematic review and individual participant data meta-analysis of
    effects on developmental outcomes and effect modifiers. Am J Clin
    Nutr2021;114(Suppl 11):43S–67S.

.. [Stewart-et-al-2020]

  View `Stewart et al 2021 <https://watermark.silverchair.com/nqz262.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAsEwggK9BgkqhkiG9w0BBwagggKuMIICqgIBADCCAqMGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMNfzzSOuNA_O5csENAgEQgIICdA1cPJoXkuhoGC0vdAEuSzMBvtykfenT7Y5u-ZIhoUdkM5b2LI8qwA6-hYJOp0nFwcTrxm6y4IQsCgV_jf2wU78QPZ_xUxcxbaWI6E8ZnZ2sQNiKcYKaQv3435Sa2P1mkCakCXbi7NcTaGai50ULqRoz4F1DN2sg3J8sWUTbvveMYV4y2mfPY3bju8lncm5wssAPrNhBMtjHqopg-6dTj7nQD4mylP8Zk_Vum0mslWjzGs-jwR58jSmZ0uyitMd8zHHY9GbZAjx7oGjZtZOWWzA_E3c_kmfqvbPtBLM3F0Cq3q_EoXEcdG4y-oTx_2uQ340xC77eOxVJNPMuugdZ7PhPJ3YlDmBWCK0pPsoqcdvQvxyI6_jHZrYinjHHbg3eqjz0YTJpNWhwm5slJZ5a41tNFLx8V6O3zytAaquen0PkCa7gsrsj0K5v7017xDWWXeSe91E7KUKtVDsnzBLhtzLFziDup_sp1wRa2MAQ_AYYPj_pjwfLc2ylmo2WVquVe71tipQOcJJvoiYKheF4AjLOYpnH8kUs-cCsAcDz9vaC9sM25v6Cyg8yHSsOYo6Aq39Tm9bgoeG7JmIU5f5kRs1MsfjtDsvQL0YR9pK2aO0Qz-L_qQOHaTexFFV5QdgxTVIAsUzIfNnOFfH_MTF0jbQYagVFwYprlFWZH4Me-5i1VEVUd7_ukic60AuaPH66AqQV_5saJGPja9vhxuieE-SEsie9KrOxdIuUfL_d5CWg5d7NYH5aZnUH1VmAcLM91LJ7fnbBFLiNVt01QUJYRjSMBDeUV4yCRc7JkRpakS82yglg7V53yWb5lgcPKRup5PjHmHs>`_

    Stewart CP,Wessells KR, Arnold CD, Huybregts L, Ashorn P, Becquey
    E, Humphrey JH, Dewey KG. Lipid-based nutrient supplements and
    all-cause mortality in children 6–24 months of age: a meta-analysis of
    randomized controlled trials. Am J Clin Nutr 2020;111:207–18.
.. [Wessells-et-al-2021]

  View `Wessels et al 2021 <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8560313/pdf/nqab276.pdf>`_

    Wessells K, Arnold C, Stewart C, Prado E, Abbeddou S, Adu-
    Afarwuah S, Arnold BF, Ashorn P, Ashorn U, Becquey E, et al.
    Characteristics that modify the effect of small-quantity lipid-based
    nutrient supplementation on child anemia and micronutrient status:
    an individual participant data meta-analysis of randomized controlled
    trials. Am J Clin Nutr2021;114(Suppl 11):68S–94S.