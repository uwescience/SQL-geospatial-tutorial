---

title: "Python Examples"
teaching: 15
exercises: 0
questions:
objectives:
keypoints:

---

## Introduction

This lesson provides an example of how to use a simple Python script within ADAPT to visualize and subset a gridded climate product.

We will use the following ADAPT resources:

* Windows VM
* Python 3.6 installed in local user directory ([Anaconda](https://geohackweek.github.io/Introductory/00-conda-tutorial/) distribution) and running Jupyter Notebook

Sample dataset:

```
/att/pubrepo/MERRA2/local/M2I1NXASM.5.12.4/1980/01/MERRA2_100.inst1_2d_asm_Nx.19800101.nc4
```

## Read and visualize the data

We will use the Python library xarray to examine the MERRA2 product. Start by importing the library:
 
~~~
import xarray as xr
~~~
{: .python}

Next, open the dataset (here the data are located in my "nobackup" folder on the Windows VM):

~~~
ds = xr.open_dataset(r'i:\ppl\aarendt\MERRA2_100.inst1_2d_asm_Nx.19800101.nc4', engine='netcdf4')
~~~
{: .python}