[![Build Status](https://travis-ci.org/OMS-NetZero/FAIR.svg?branch=master)](https://travis-ci.org/OMS-NetZero/FAIR)
[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/OMS-NetZero/FAIR/master?filepath=Example-Usage.ipynb)

# FAIR
Finite Amplitude Impulse-Response simple climate-carbon-cycle model

## Installation
1. Make sure you have Python 2 or 3 and pip installed
2. From terminal/command prompt `pip install fair`

## Usage
FAIR takes emissions of greenhouse gases, aerosol and ozone precursors, and converts these into greenhouse gas concentrations, radiative forcing and temperature change.


There are two ways to run FAIR:
1. Carbon dioxide emissions only with all other radiative forcings specified externally (specify `useMultigas=False` in the call to `fair_scm`);
2. All species included in the RCP emissions datasets, with, optionally, solar and volcanic forcing still specified externally. For convenience, the RCP datasets are provided in the RCP subdirectory and can be imported:

```
from fair.forward import fair_scm
from fair.RCPs import rcp85
emissions = rcp85.Emissions.emissions
C,F,T = fair_scm(emissions=emissions)
```

The main engine of the model is the `fair_scm` function in `forward.py`. This function can be imported into a Python script or iPython session. The most important keyword to `fair_scm` is the `emissions`. This should be either a (nt, 40) numpy array (in multigas mode) or (nt,) numpy array (in CO<sub>2</sub> only mode), where nt is the number of model timesteps. The outputs are a tuple of `(C, F, T)` arrays which are GHG concentrations ((nt, 31) in multigas mode, (nt,) in CO<sub>2</sub>-only mode), forcing ((nt, 13) or (nt,)) and temperature change (nt,). The index numbers corresponding to each species will be given in tables 1 to 3 of the revised version of the Smith et al. paper reference below (we hope to make this object-oriented in the future). For now, note that the input emissions follow the ordering of the RCP datasets, which are included under `fair/RCPs`, and the GHG concentrations output are in the same order, except that we don't output the year, only use one column for total CO<sub>2</sub>, and the short-lived species (input indices 5 to 11 inclusive) are not included, reducing the number of columns from 40 to 31. In multigas mode the forcing output indices are:

<ol start="0">
<li>CO<sub>2</sub></li>
<li>CH<sub>4</sub></li>
<li>N<sub>2</sub>O</li>
<li>Minor GHGs (CFCs, HFCs etc)</li>
<li>Tropospheric ozone</li>
<li>Stratospheric ozone</li>
<li>Stratospheric water vapour from methane oxidation</li>
<li>Contrails</li>
<li>Aerosols</li>
<li>Black carbon on snow</li>
<li>Land use</li>
<li>Volcanic</li>
<li>Solar</li>
</ol>


For further information, see the example ipython notebook contained in the GitHub repo at https://github.com/OMS-NetZero/FAIR.

## References:
Smith, C. J., Forster, P. M., Allen, M., Leach, N., Millar, R. J., Passerello, G. A., and Regayre, L. A.: FAIR v1.1: A simple emissions-based impulse response and carbon cycle model, Geosci. Model Dev. Discuss., https://doi.org/10.5194/gmd-2017-266, in review, 2017

Millar, R. J., Nicholls, Z. R., Friedlingstein, P., and Allen, M. R.: A modified impulse-response representation of the global near-surface air temperature and atmospheric concentration response to carbon dioxide emissions, Atmos. Chem. Phys., 17, 7213-7228, https://doi.org/10.5194/acp-17-7213-2017, 2017.

Bibtex references:
@Article{gmd-2017-266,
AUTHOR = {Smith, C. J. and Forster, P. M. and Allen, M. and Leach, N. and Millar, R. J. and Passerello, G. A. and Regayre, L. A.},
TITLE = {FAIR v1.1: A simple emissions-based impulse response and carbon cycle model},
JOURNAL = {Geoscientific Model Development Discussions},
VOLUME = {2017},
YEAR = {2017},
PAGES = {1--45},
URL = {https://www.geosci-model-dev-discuss.net/gmd-2017-266/},
DOI = {10.5194/gmd-2017-266}
}

@Article{acp-17-7213-2017,
AUTHOR = {Millar, R. J. and Nicholls, Z. R. and Friedlingstein, P. and Allen, M. R.},
TITLE = {A modified impulse-response representation of the global near-surface air temperature and atmospheric concentration response to carbon dioxide emissions},
JOURNAL = {Atmospheric Chemistry and Physics},
VOLUME = {17},
YEAR = {2017},
NUMBER = {11},
PAGES = {7213--7228},
URL = {https://www.atmos-chem-phys.net/17/7213/2017/},
DOI = {10.5194/acp-17-7213-2017}
}
