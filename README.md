# HEPSIM Evaluation Summary

The main submission file is `hepsim_evaluation_solution.ipynb`.

It uses the first five standard `QG_jets_*.npz` files, so the analysis is done on 500,000 jets in total. All figures are embedded inline in the notebook itself. 

## (a) Data Loading and Exploration

The dataset is loaded from the standard NumPy `.npz` files. The padded constituent arrays are kept in `X`, the labels are kept in `y`, and zero-padding is removed by masking constituents with `p_T > 0`.

Main results:

- total constituents in quark jets: `8,350,222`
- total constituents in gluon jets: `13,283,118`
- gluon jets have clearly higher multiplicity
- the leading-constituent `p_T` distribution also shifts as expected, while leading `eta` is less discriminating

## (b) Jet Observables

The notebook reconstructs constituent four-vectors, sums them to get the jet four-momentum, and then computes:

- jet mass
- jet width
- `p_T^D`

Main results:

- gluon jets have larger jet mass
- gluon jets are broader
- quark jets have larger `p_T^D`, meaning their transverse momentum is concentrated in fewer constituents

## (c) Boost to the Jet Center-of-Mass Frame

Each jet is boosted with

`beta = p_jet / E_jet`

and the boosted constituents are checked to make sure their total three-momentum vanishes.

Main results:

- median `|sum p_rest| = 2.053018e-07 GeV`
- 95th percentile `|sum p_rest| = 5.493678e-07 GeV`
- the residual is consistent with numerical precision

## (d) Quark vs. Gluon Jet Classification

The notebook first builds the required rest-frame classifier from rest-frame geometry and energy-sharing features, then compares it with a stronger lab-only block and a combined lab+rest feature bank. A simple averaged ensemble is also included as an extra study.

Main results:

- rest-only HistGBT: `AUC = 0.8642`
- lab-only HistGBT: `AUC = 0.8820`
- combined HistGBT: `AUC = 0.8872`
- averaged ensemble: `AUC = 0.8882`
- strongest overall single feature: `multiplicity`
- strongest genuinely rest-frame feature: `rest_dispersion`

Interpretation:

- the rest-frame block by itself is weaker than the stronger lab-only block
- adding the rest-frame block on top of the richer lab features improves the score
- this suggests the rest-frame variables are complementary rather than dominant on their own
- the lab-only block keeps more direct information about radiation profile and particle composition, which is why it does better as a standalone model
- the combined gain is modest but consistent, so the rest-frame features work best here as an extra feature block instead of a replacement
- among the genuinely rest-frame variables, `rest_dispersion` contributes the most signal
- One possible reason the rest-frame block scores a bit lower on its own is that it may be less tied to this narrow lab-frame kinematic window and therefore capture more kinematic-independent structure, which also helps explain why combining it with the richer lab features works better.

## Notes

- The notebook was run end to end under Python `3.13.12`.

## Thank you!
Please mail me at sreenandan.shashidharan@gmail.com or at 24JE0701@iitism.ac.in if anything is amiss. I sincerely apologise in advance. 
