conda-launch
============

Code implementing the conda-launch command including the ipynb app-server

# Usage


```bash
$ conda launch <ipython-notebook>.ipynb
```

OR

```bash
$ conda launch <tarfile_containing_notebook>.tar
```


# Current Capability

- Reads json metadata in from .ipynb file and sets (some, not all) parameters accordingly
- Accepts .ipynb or .tar file
- Creates environment specified dependencies if an env of the given name does not already exist.a
- launches notebook

# TODO

- Process other parameters and input formats according to this spec:
     There will be at least  three file-types that conda launch can handle:

1. .ipynb    (notebooks with special meta-data)
2. .tar        (collections of conda packages with notebook and data)
3. .tar.bz2  (wakari bundles)

The difference between (2) and (3) is that (2) is self contained and has all the conda packages it needs contained inside.

We will begin with (1) -- and define the meta-data that conda launch will use.

When conda launch notebook.ipynb starts it will:

1. Create a suitable environment in the user (or system) environment directory (app_<name>[N]) if it doesn't exist already
2. Install all conda packages required by the app (in addition to ipython notebook)
3. Copy the data files into ~/conda_app_data (unless over-ridden by app)
4. Launch an IPython notebook in a particular directory with the notebook

We define a notebook app as an .ipynb file with specific metadata added to the
JSON file.  The metadata is under the keyname 'app'.  The
value is a dictionary of metadata:


```
'conda.app' : appdict
```

appdict contains:

```
{
'depends': [list of requirement specification strings],  # default ipython-notebook
'platform-depends': {<platform> : [list of specs], <platform2> : [list of specs]}, # optional
'appname' : default is name of notebook  # optional
'envname' : 'name_of_app_environment' # optional, gives name of environment to create the default is 'appname'
'filesroot' : <relative path name of where files should be placed>  # full-path for where data files should go using $HOME and $PREFIX (default is $HOME/app_name_data)
'data' : {'name1': <base-64 encoded binary>, 'name2': <base-64 encoded binary>}
}
```

- Graceful error handling as noted in the code

- Suggest packages to install based on import statements?

- Integrate with conda so that it can be called with
$ conda launch <myfile>.ipynb
