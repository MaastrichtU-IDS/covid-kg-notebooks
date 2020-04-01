## Run

From a root Jupyterlab Docker image.

* Automatically clone the repository and install dependencies in `/data/covid-kg-notebooks`

```bash
docker run --rm -it -p 8888:8888 -v /data/covid-kg-notebooks:/notebooks -e PASSWORD="<your_secret>" -e GIT_URL="https://github.com/vemonet/covid-kg-notebooks" umids/jupyterlab:latest
```

*  Or use the current folder and files in the Docker container (also install dependencies)

On Linux or MacOS

```bash
docker run --rm -it -p 8888:8888 -v $(pwd):/notebooks -e PASSWORD="<your_secret>" umids/jupyterlab:latest
```

On Windows

```bash
docker run --rm -it -p 8888:8888 -v ${pwd}:/notebooks -e PASSWORD="<your_secret>" umids/jupyterlab:latest
```

## Download data

This notebook uses a 166M compressed ntriples file as Knowledge Graph which needs to be downloaded

Get the `kg.nt` download URL from Kaggle: https://www.kaggle.com/group16/covid19-literature-knowledge-graph

> In firefox after starting the download you can right click on the file in the download list and "Copy Download Link"

Go to the Jupyter notebook, open a new `Terminal` (Bash) and download the file using the direct download URL from Kaggle:

```bash
cd /notebooks/input/covid19-literature-knowledge-graph

wget -O kg.nt.zip https://storage.googleapis.com/kaggle-data-sets/564132/1049255/compressed/kg.nt.zip?GoogleAccessId=web-data@kaggle-161607.iam.gserviceaccount.com&Expires=1585986845&Signature=NtLuBIRmNrmBwc4RxpHtB0oZ0sXuPisf3nwMc3aonqIOqpA%2BDRTT%2BQTd9T4JE0fmlNVrNDk5Rb%2BZSrVF58GndDlW2FgUzTcs8llO8OXgq6TO6tc5iAs%2FIWZqq0a9RIgTYlF3gZgmNDO2GUkUHXh%2BAmxs%2F2fkUp3olN%2BB4F4B7WVlAEfNYupNee9QXdlJVh0dZEKXn3FKHTZ9c45ig4IFCMSdCjvp3ZV6QpVoThp8CvAZ%2BvIwykPhyP0bzzSGUfUMBu49Ao4xCC%2FJGLINOZM4rnr8JOWtozSnjfYpHdjKvC4keJrpSSx7hS8zTqtU%2FlmDWELrdWehM5Xt01cA6CJojA%3D%3D&response-content-disposition=attachment%3B+filename%3Dkg.nt.zip

unzip kg.nt.zip

# Convert to turtle using Raptor to fix encoding issue (using rapper installed locally)
rapper -i ntriples -o turtle kg.nt > ugent-covid-kg.ttl
# Using Docker
docker run -it --rm -v $(pwd):/data rdfhdt/hdt-cpp rdf2hdt /data/kg.nt /data/ugent-covid-kg.ttl

# Still issue with lang tag:
#<http://dbpedia.org/property/country> "United States"^^rdf:langString ;
# Replace it by @en
find ugent-covid-kg.ttl -type f -exec sed -i "s/\^\^rdf:langString/@en/g" {} +
```

> To load in graph http://idlab.github.io/covid19#datasetVersion9

After removing all `\u` GraphDB still issue the following:

```
datatype rdf:langString requires a language tag 
```

## Sources

* Source code to build the RDF Knowledge Graph: https://github.com/GillesVandewiele/COVID-KG

* Available on Kaggle: https://www.kaggle.com/group16/covid19-literature-knowledge-graph
* Those notebooks sources on Kaggle: 
  * Start with RDF: https://www.kaggle.com/group16/covid-19-knowledge-graph-starter/notebook
  * Knowledge graphs embeddings: https://www.kaggle.com/group16/covid-19-knowledge-graph-embeddings
* Article: https://medium.com/@gillesvandewiele/9297236055c8
* A Comunica instance to perform queries can be found here:
  https://query-covid19.linkeddatafragments.org/