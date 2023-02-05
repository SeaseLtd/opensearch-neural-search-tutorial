# README #
This is the repository for all the material of the OpenSearch Neural Search Tutorial.
Here you can find everything you need to deploy a simple OpenSearch system to do neural queries.

For a step-by-step description read our blog post.

## Requirements ##
To directly use the existing material, without generating documents and models by yourself, you only need:
- OpenSearch 2.5.0

To create documents by yourself you also need:
- python 3.10

## Repository content ##
- **[documents](documents)**: contains convert_msmarco_data_to_opensearch_format.py python script to generate OpenSearch documents from MS Marco data.
  - **[msmarco_documents](documents/msmarco_documents)**: contains the MS Marco data
  - **[opensearch_documents](documents/opensearch_documents)**: contains the Opensearch documents

## Installation ##
Set up your Docker host environment:
- macOS & Windows: In Docker Preferences > Resources, set RAM to at least 4 GB.
- Linux: Ensure vm.max_map_count is set to at least 262144 as per the documentation.
We download the docker-compose.yml file from: https://opensearch.org/downloads.html
Check the prerequisites: 
### To generate the documents ###
**You can skip this step if you want to use the already provided material.**

To generate documents:
````
python convert_msmarco_data_to_opensearch_format.py
````
### To start OpenSearch ###
To start OpenSearch:
````
docker-compose up
````

## Usage ##
### Approximate Nearest Neighbor Search ###
````
{
  "_source": [
      "general_text"
  ],
  "query": {
    "neural": {
      "general_text_vector": {
        "query_text": "what is a bank transit number",
        "model_id": "loaded_neural_model_id",
        "k": 3
      }
    }
  }
}
````
### Approximate Nearest Neighbor with Query Filter ###
````
{
  "query": {
    "bool": {
      "filter": {
        "term": {
          "color": "white"
        }
      },
      "must": {
        "neural": {
          "general_text_vector": {
            "query_text": "what is a bank transit number",
            "model_id": "loaded_neural_model_id",
            "k": 3
          }
        }
      }
    }
  }
}
````