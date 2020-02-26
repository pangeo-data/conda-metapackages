# build conda metapackages for pangeo dependencies

![Action Status](https://github.com/pangeo-data/conda-metapackages/workflows/Metapackage/badge.svg)

Currently push to pangeo/ anaconda cloud repo, maybe later move to conda-forge

* run this locally on a linux system (`docker run -v $PWD:$HOME -it continuumio/miniconda3:4.7.12 /bin/bash`)
* Or push tags tp build and push metapackages with the same version via GitHub Actions
```
git tag 0.0.1 ; git push --tags
```

1) create a metapackage building environment
```
conda env create -f environment.yml
conda activate condabuild
# Anaconda cloud access token for pangeo organization (store as github secret)
export PAT=XXXXXXXXX
```

2) pangeo-dask metapackage with necessary dask packages to run on Pangeo JupyterHub or BinderHub
```
VERSION=0.0.1
conda-metapackage pangeo-dask $VERSION --user pangeo --token $PAT --dependencies `cat dask-packages.txt | xargs`  -c conda-forge --override-channels --license MIT --summary 'pangeo dask dependencies' --home 'http://pangeo.io'
```

3) pangeo-notebook metapackage with necessary packages for user jupyterlab UI + dask
NOTE: update notebook-packages.txt to reflect most recent pangeo-dask version first !!
```
VERSION=0.0.1
conda-metapackage pangeo-notebook test --user pangeo --token $PAT --dependencies `cat notebook-packages.txt | xargs`  -c conda-forge -c pangeo --override-channels --license MIT --summary 'pangeo notebook dependencies' --home 'http://pangeo.io'
```
