[![Documentation Status](https://readthedocs.org/projects/molmap/badge/?version=latest)](https://molmap.readthedocs.io/en/latest/?badge=latest)[![Build Status](https://travis-ci.com/shenwanxiang/bidd-molmap.svg?branch=master)](https://travis-ci.com/shenwanxiang/bidd-molmap)

# Out-of-the-Box Deep Learning Prediction of Pharmaceutical Properties by Broadly Learned Knowledge-Based Molecular Representations 

## MolMap
MolMap is generated by the following steps:

* Step1: Data sampling 
* Step2: Feature extraction 
* Step3: Feature pairwise distance calculation --> cosine, correlation, jaccard
* Step4: Feature 2D embedding --> umap, tsne, mds
* Step5: Feature grid arrangement --> grid, scatter
* Step5: Transform --> minmax, standard
* Step6: Get MolMap



### Construction of the MolMap Objects
![molmap](https://github.com/shenwanxiang/bidd-molmap/blob/master/paper/images/Overall.png)


### The MolMapNet Architecture

![net](https://github.com/shenwanxiang/bidd-molmap/blob/master/paper/images/net.png)

## Installation

1. install [rdkit]('http://www.rdkit.org/docs/Install.html) first:
```bash
conda create -c rdkit -n my-rdkit-env rdkit
conda activate my-rdkit-env
```
2. in your "my-rdkit-env" env, install molmap by:

```bash
git clone https://github.com/shenwanxiang/bidd-molmap.git
cd bidd-molmap
pip install -r requirements.txt --user

# add molmap to PYTHONPATH
echo export PYTHONPATH="\$PYTHONPATH:`pwd`" >> ~/.bashrc

# init bashrc
source ~/.bashrc
```

3. [deepchem](https://github.com/deepchem/deepchem) (optional). In our paper, deepchem has been used as a dataset provider, so you may install it by:
```bash
pip install deepchem==2.2.1.dev54
```

4. If you have gcc problems when you install molmap, please installing g++ first:
```bash
sudo apt-get install g++
```


## Out-of-the-Box Usage

![code](https://github.com/shenwanxiang/bidd-molmap/blob/master/paper/images/code_example.png)


```python
import molmap
# Define your molmap
mp_name = './descriptor.mp'
mp = molmap.MolMap(ftype = 'descriptor', fmap_type = 'grid',
                   split_channels = True,   metric='cosine', var_thr=1e-4)
```

```python
# Fit your molmap
mp.fit(method = 'umap', verbose = 2)
mp.save(mp_name) 
```

```python
# Visulization of your molmap
mp.plot_scatter()
mp.plot_grid()
```

```python
# Batch transform 
smiles_list = ['CC(=O)OC1=CC=CC=C1C(O)=O']
X = mp.batch_transform(smiles_list,  scale = True, 
                       scale_method = 'minmax', n_jobs=4)
print(X.shape)
```

* [Click for More Example](https://github.com/shenwanxiang/bidd-molmap/blob/master/paper/05_solubility_prediction_model_interpretation/05_MolMapNet/03_build_model_by_optimized_hyper_params.ipynb)










## Out-of-the-Box Performances

| Dataset   | Task Metric | MoleculeNet (GCN Best Model) | Chemprop (D-MPNN model) | MolMapNet (MMNB model) |
|-----------|-------------|-----------------------------|------------------------|-----------------------|
| ESOL      | RMSE        | 0.580 (MPNN)                | 0.555                  | 0.575                 |
| FreeSolv  | RMSE        | 1.150 (MPNN)                | 1.075                  | 1.155                 |
| Lipop     | RMSE        | 0.655 (GC)                  | 0.555                  | 0.625                 |
| PDBbind-F | RMSE        | 1.440 (GC)                  | 1.391                  | 0.721                 |
| PDBbind-C | RMSE        | 1.920 (GC)                  | 2.173                  | 0.931                 |
| PDBbind-R | RMSE        | 1.650 (GC)                  | 1.486                  | 0.889                 |
| BACE      | ROC_AUC     | 0.806 (Weave)               | N.A.                   | 0.849                 |
| HIV       | ROC_AUC     | 0.763 (GC)                  | 0.776                  | 0.777                 |
| PCBA      | PRC_AUC     | 0.136 (GC)                  | 0.335                  | 0.276                 |
| MUV       | PRC_AUC     | 0.109 (Weave)               | 0.041                  | 0.096                 |
| ChEMBL    | ROC_AUC     | N.A.                        | 0.739                  | 0.750                 |
| Tox21     | ROC_AUC     | 0.829 (GC)                  | 0.851                  | 0.845                 |
| SIDER     | ROC_AUC     | 0.638 (GC)                  | 0.676                  | 0.68                  |
| ClinTox   | ROC_AUC     | 0.832 (GC)                  | 0.864                  | 0.888                 |
| BBBP      | ROC_AUC     | 0.690 (Weave)               | 0.738                  | 0.739                 |
