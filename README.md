

<p align="center">
<a href="https://braindynamicslab.github.io/dyneusr/">
<img src="./docs/assets/logo.png" height="250">
</a>
</p>


<p align="center">
<a href="https://braindynamicslab.github.io/dyneusr/">
<img src="./docs/assets/haxby_decoding_mapper_stages.png">
</a>
</p>



## **Dynamical Neuroimaging Spatiotemporal Representations**

[DyNeuSR](https://braindynamicslab.github.io/dyneusr/) is a Python visualization library for topological representations of neuroimaging data.

DyNeuSR was designed specifically for working with shape graphs produced by the Mapper algorithm from topological data analysis (TDA), as described in the papers "[Generating dynamical neuroimaging spatiotemporal representations (DyNeuSR) using topological data analysis](https://www.mitpressjournals.org/doi/abs/10.1162/netn_a_00093)" (Geniesse et al., 2019) and "[Towards a new approach to reveal dynamical organization of the brain using topological data analysis](https://www.nature.com/articles/s41467-018-03664-4)" (Saggar et al., 2018). Check out this [blog post](https://bdl.stanford.edu/blog/tda-cme-paper/) for more about the initial work that inspired the development of DyNeuSR. 

Note, DyNeuSR _does not_ re-implement Mapper, but rather, it provides an open source set of tools that make it easy to visualize and analyze the graphical objects generated by Mapper with a particular focus on neuroimaging applications. DyNeuSR implements a Python API that connects existing implementations of Mapper (e.g. [KeplerMapper](https://kepler-mapper.scikit-tda.org)) with network analysis tools (e.g. [NetworkX](https://networkx.github.io/)) and other neuroimaging data visualization libraries (e.g. [Nilearn](https://nilearn.github.io/)) and provides a high-level interface for interacting with and manipulating shape graph representations of neuroimaging data and relating such representations back to neurophysiology.

DyNeuSR also provides an interactive web interface for visualizing and exploring shape graphs. To see this visual interface in action, check out the [demos](https://braindynamicslab.github.io/dyneusr/demo/).




## **Demos**
 
- [Trefoil knot](https://braindynamicslab.github.io/dyneusr/demo/trefoil-knot/) ([code](https://github.com/braindynamicslab/dyneusr/blob/master/docs/demo/trefoil-knot/trefoil_knot.py))
- [Trefoil knot (custom layouts)](https://braindynamicslab.github.io/dyneusr/demo/trefoil-knot/) ([code](https://github.com/braindynamicslab/dyneusr/blob/master/docs/demo/trefoil-knot-custom-layouts/trefoil_knot_custom_layouts.py))
- [Haxby fMRI data (t-SNE lens)](https://braindynamicslab.github.io/dyneusr/demo/haxby-tsne/) ([code](https://github.com/braindynamicslab/dyneusr/blob/master/docs/demo/haxby-tsne/haxby_tsne.py))
- [Haxby fMRI data (UMAP lens)](https://braindynamicslab.github.io/dyneusr/demo/haxby-umap/) ([code](https://github.com/braindynamicslab/dyneusr/blob/master/docs/demo/haxby-umap/haxby_umap.py))
- [Haxby fMRI data (supervised UMAP lens)](https://braindynamicslab.github.io/dyneusr/demo/haxby-umap-supervised/) ([code](https://github.com/braindynamicslab/dyneusr/blob/master/docs/demo/haxby-umap-supervised/haxby_umap_supervised.py))




## **References**

If you find DyNeuSR useful, please consider citing:

> Geniesse, C., Sporns, O., Petri, G., & Saggar, M. (2019). [Generating dynamical neuroimaging spatiotemporal representations (DyNeuSR) using topological data analysis](https://www.mitpressjournals.org/doi/abs/10.1162/netn_a_00093). *Network Neuroscience*. Advance publication. doi:10.1162/netn_a_00093


For more information about the Mapper approach, please see:

> Saggar, M., Sporns, O., Gonzalez-Castillo, J., Bandettini, P.A., Carlsson, G., Glover, G., & Reiss, A.L. (2018). [Towards a new approach to reveal dynamical organization of the brain using topological data analysis](https://www.nature.com/articles/s41467-018-03664-4). *Nature Communications, 9*(1). doi:10.1038/s41467-018-03664-4





## **API Usage & Examples**

DyNeuSR provides a Python API for working with and visualizing shape graphs generated by Mapper. This repository includes several [examples](https://github.com/braindynamicslab/dyneusr/tree/master/examples/) that introduce DyNeuSR's API and highlight different aspects of analysis with DyNeuSR. For more detailed tutorials, check out [dyneusr-notebooks](https://github.com/braindynamicslab/dyneusr-notebooks/).



### **_Shape Graph Visualization_** ([trefoil knot](https://github.com/braindynamicslab/dyneusr/blob/master/examples/trefoil_knot/trefoil_knot.py))

```python

from dyneusr import DyNeuGraph
from dyneusr.datasets import make_trefoil
from dyneusr.tools import visualize_mapper_stages
from kmapper import KeplerMapper

# Generate synthetic dataset
dataset = make_trefoil(size=100)
X = dataset.data
y = dataset.target

# Generate shape graph using KeplerMapper
mapper = KeplerMapper(verbose=1)
lens = mapper.fit_transform(X, projection=[0])
graph = mapper.map(lens, X, nr_cubes=6, overlap_perc=0.2)

# Visualize the shape graph using DyNeuSR's DyNeuGraph                          
dG = DyNeuGraph(G=graph, y=y)
dG.visualize('dyneusr_output.html')

```

<p align="center"><a href="https://github.com/braindynamicslab/dyneusr/tree/master/examples/trefoil_knot">
<img src="./docs/assets/dyneusr_trefoil_knot.png">
</a></p>



### **_Mapper Parameter Comparison_** ([trefoil knot](https://github.com/braindynamicslab/dyneusr/blob/master/examples/trefoil_knot/compare_lenses.py))

```python
# Define projections to compare
projections = ([0], [0,1], [1,2], [0, 2])

# Compare different sets of columns as lenses
for projection in projections:

	# Generate shape graph using KeplerMapper
	mapper = KeplerMapper(verbose=1)
	lens = mapper.fit_transform(X, projection=projection)
	graph = mapper.map(lens, X, nr_cubes=4, overlap_perc=0.3)

	# Visualize the stages of Mapper
	fig, axes = visualize_mapper_stages(
		dataset, lens=lens, graph=graph, cover=mapper.cover, 
		layout="spectral")
		
```

<p align="center"><a href="https://github.com/braindynamicslab/dyneusr/tree/master/examples/trefoil_knot">
<img src="./docs/assets/trefoil_knot_mapper_lens_0.png">
<img src="./docs/assets/trefoil_knot_mapper_lens_0_1.png">
<img src="./docs/assets/trefoil_knot_mapper_lens_0_2.png">
<img src="./docs/assets/trefoil_knot_mapper_lens_1_2.png">
</a></p>



### **_Neuroimaging Applications_** ([haxby decoding](https://github.com/braindynamicslab/dyneusr/tree/master/examples/haxby_decoding))

```python

import numpy as np 
import pandas as pd

from nilearn.datasets import fetch_haxby
from nilearn.input_data import NiftiMasker

from kmapper import KeplerMapper, Cover
from sklearn.manifold import TSNE
from sklearn.cluster import DBSCAN

# Fetch dataset, extract time-series from ventral temporal (VT) mask
dataset = fetch_haxby()
masker = NiftiMasker(
    dataset.mask_vt[0], 
    standardize=True, detrend=True, smoothing_fwhm=4.0,
    low_pass=0.09, high_pass=0.008, t_r=2.5,
    memory="nilearn_cache")
X = masker.fit_transform(dataset.func[0])

# Encode labels as integers
df = pd.read_csv(dataset.session_target[0], sep=" ")
target, labels = pd.factorize(df.labels.values)
y = pd.DataFrame({l:(target==i).astype(int) for i,l in enumerate(labels)})

# Generate shape graph using KeplerMapper
mapper = KeplerMapper(verbose=1)
lens = mapper.fit_transform(X, projection=TSNE(2))
graph = mapper.map(lens, X, cover=Cover(20, 0.5), clusterer=DBSCAN(eps=20.))

# Visualize the shape graph using DyNeuSR's DyNeuGraph                          
dG = DyNeuGraph(G=graph, y=y)
dG.visualize('dyneusr_output.html')

```

<p align="center"><a href="https://github.com/braindynamicslab/dyneusr/tree/master/examples/haxby_decoding">
<img src="./docs/assets/dyneusr_haxby_decoding.png">
</a></p>



## **Setup**

Online documentation (*coming soon*) will include more details about how to install and use DyNeuSR.

### **_Dependencies_**

#### [Python 3.6](https://www.python.org/)

#### Required Python Packages
* [numpy](www.numpy.org)
* [pandas](pandas.pydata.org)
* [scipy](www.scipy.org)
* [scikit-learn](scikit-learn.org)
* [matplotlib](matplotlib.sourceforge.net)
* [seaborn](stanford.edu/~mwaskom/software/seaborn)
* [networkx](networkx.github.io)
* [nilearn](nilearn.github.io)
* [kmapper](kepler-mapper.scikit-tda.org)

_For a full list of packages and required versions, see [`requirements.txt`](https://github.com/braindynamicslab/dyneusr/blob/master/requirements.txt) and [`requirements-versions.txt`](https://github.com/braindynamicslab/dyneusr/blob/master/requirements-versions.txt)._


### **_Install with Conda_**

If your default environment is Python 2, we recommend that you install `dyneusr` in a separate Python 3 environment. You can find more information about creating a separate environment for Python 3 [here](https://salishsea-meopar-docs.readthedocs.io/en/latest/work_env/python3_conda_environment.html). 

If you don't have conda, or if you are new to scientific python, we recommend that you download the [Anaconda scientific python distribution](https://store.continuum.io/cshop/anaconda/). 

_To create a new conda environment and install from source:_
```bash
conda create -n dyneusr python=3.6
conda activate dyneusr

git clone https://github.com/braindynamicslab/dyneusr.git
cd dyneusr

conda install --file requirements-conda.txt
pip install -e .

pytest
```

This creates a new conda environment `dyneusr` and installs in it the dependencies that are needed. To access it, use the `conda activate dyneusr` command (if your conda version >= 4.4) and use `source activate dyneusr` command (if your conda version < 4.4).


### **_Install with PIP_**

_To install from source with pip:_
```bash
git clone https://github.com/braindynamicslab/dyneusr.git
cd dyneusr

pip install -r requirements.txt
pip install -e .

pytest
```


## **Support**

Please feel free to [report](https://github.com/braindynamicslab/dyneusr/issues/new) any issues, [request](https://github.com/braindynamicslab/dyneusr/issues/new) new features, or [propose](https://github.com/braindynamicslab/dyneusr/compare) improvements. You can also contact Caleb Geniesse at geniesse [at] stanford [dot] edu.

If you contribute to DyNeuSR, please feel free to add your name to the [list of contributors](https://github.com/braindynamicslab/dyneusr/blob/master/contributors.md). 

## **Citation**

> Geniesse, C., Sporns, O., Petri, G., & Saggar, M. (2019). [Generating dynamical neuroimaging spatiotemporal representations (DyNeuSR) using topological data analysis](https://www.mitpressjournals.org/doi/abs/10.1162/netn_a_00093). *Network Neuroscience*. Advance publication. doi:10.1162/netn_a_00093
