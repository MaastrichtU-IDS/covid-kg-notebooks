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
# Go to the right folder
cd /notebooks/input/covid19-literature-knowledge-graph

# Get the URL from Kaggle
wget -O kg.nt.zip https://storage.googleapis.com/kaggle-data-sets/564132/1049255/compressed/kg.nt.zip?GoogleAccessId=web-data@kaggle-161607.iam.gserviceaccount.com&Expires=00000000000&response-content-disposition=attachment%3B+filename%3Dkg.nt.zip

# Unzip the downloaded file
unzip kg.nt.zip

# Convert to turtle using Raptor to fix encoding issue (using rapper installed locally)
rapper -i ntriples -o turtle kg.nt > ugent-covid-kg.ttl

# Still issue with lang tag:
#<http://dbpedia.org/property/country> "United States"^^rdf:langString ;
# Replace it by @en
find ugent-covid-kg.ttl -type f -exec sed -i "s/\^\^rdf:langString/@en/g" {} +
```

> To load in graph http://idlab.github.io/covid19#datasetVersion9

> Using Raptor within Docker:
>
> ```bash
> docker run -it --rm -v $(pwd):/data rdfhdt/hdt-cpp rdf2hdt /data/kg.nt /data/ugent-covid-kg.ttl
> ```

## Sources

* Source code to build the RDF Knowledge Graph: https://github.com/GillesVandewiele/COVID-KG

* Available on Kaggle: https://www.kaggle.com/group16/covid19-literature-knowledge-graph
* Those notebooks sources on Kaggle: 
  * Start with RDF: https://www.kaggle.com/group16/covid-19-knowledge-graph-starter/notebook
  * Knowledge graphs embeddings: https://www.kaggle.com/group16/covid-19-knowledge-graph-embeddings
* Article: https://medium.com/@gillesvandewiele/9297236055c8
* A Comunica instance to perform queries can be found here:
  https://query-covid19.linkeddatafragments.org/