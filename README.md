
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

## Sources

* Source code to build the RDF Knowledge Graph: https://github.com/GillesVandewiele/COVID-KG

* Available on Kaggle: https://www.kaggle.com/group16/covid19-literature-knowledge-graph
* Those notebooks sources on Kaggle: 
  * Start with RDF: https://www.kaggle.com/group16/covid-19-knowledge-graph-starter/notebook
  * Knowledge graphs embeddings: https://www.kaggle.com/group16/covid-19-knowledge-graph-embeddings
* A Comunica instance to perform queries can be found here:
  https://query-covid19.linkeddatafragments.org/