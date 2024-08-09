# Microsoft_GraphRAG_Accelerator_deployment_using_both_pip_install_and_GraphRAG Accelerator_in_Linux_Azure
As you may know, the [Microsoft GraphRAG](https://microsoft.github.io/graphrag/) team published the [Github repo](https://github.com/microsoft/graphrag) and Python code library for implementing GraphRAG in early July 2024. To get started with the GraphRAG system, Microsoft provided several options, including:

**1-** [Use the GraphRAG Accelerator solution](https://github.com/Azure-Samples/graphrag-accelerator) by deploying it in Azure,  

**2-** [Install from pypi](https://pypi.org/project/graphrag/) by using ```pip install graphrag```,

**3-** [Use it from source](https://microsoft.github.io/graphrag/posts/developing/) by Python "[Poetry](https://python-poetry.org/docs/#installing-with-pipx)" packaging.

Microsoft recommends trying the first approach i.e. the "Solution Accelerator package" in Microsoft Azure. In this repo, I will provide all the necessary steps for each of the above three approaches to deploy and develop the GrapRAG, making it ready for your custom datasets.


# 1- Deploy GraphRAG Accelerator in Microsoft Azure
Before installing the GraphRAG in Azure, we first should set the requirements up according to our different systems. As I'm working with the Linux OS in Azure, I will provide the necessary steps for developing GraphRAG based on Linux settings.

To deploy GraphRAG in Azure, initially, the following prerequisite tools should be installed:
* [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) >= v2.55.0
* awk - a standard linux utility
* cut - a standard linux utility
* sed - a standard linux utility
* [curl](https://curl.se) - command line data transfer
* [helm](https://helm.sh/docs/intro/install) - k8s package manager
* [jq](https://jqlang.github.io/jq/download) >= v1.6
* [kubectl](https://kubernetes.io/docs/tasks/tools) - k8s command line tool
* [yq](https://github.com/mikefarah/yq?tab=readme-ov-file#install) >= v4.40.7 - yaml file parser

As these tools should be installed based on the Linux distribution that you have, before installing these tools we first should identify the Linux distribution. To this end, run the following command in the terminal to determine the distribution name and version:
```python
cat /etc/os-release
```


(**NOTE:** You can connect your VSCode to the Azure server by installing the Remote-SSH extension of VSCode and then connect to Azure Host with Remote-SSH by having a User, Pass of the Azure server)

# 2- Install GraphRAG from pypi (using pip install)

**Using LLama-3.1 and Ollama:**

As Llama-3 is  open source and free, we tried to use Llama-3 rather than GPT4o and Azure-Open-AI to decrease costs and prices due to the number of requests. We also installed Ollama in the local system to use Llama-3.

1- If you don't have Ollama, you can freely download it from the [website](https://ollama.com/) after creating an account. For Linux systems, you can install it with the following command:
```python
curl -fsSL https://ollama.com/install.sh | sh
```

To check if you correctly install the Ollama you can run ```ollama --help``` and see the output of available commands for Ollama.

2- Next you should download [Llama3.1](https://ollama.com/library/llama3.1) model based on three options on the number of parameters (8b, 70b, and 405b) that you want. For instance, for using the 8b parameter model, you should run the following code:
```python
ollama run llama3.1:8b
```

3- now you should change the ```settings.yaml``` file based on Llama-3 and Ollama configuration settings. As Llama-3 follows the same API standard as Open-AI API, we don't have much hard work to do. In the ```llm``` part of the ```settings.yaml``` we should make some changes as follow:
```python
llm:
  # api_key: ${GRAPHRAG_API_KEY}
  api_key: ollama #$ or (GROQ_API_KEY)
  type: openai_chat
  model: llama3.1
  model_supports_json: true # recommended if this is available for your model.
  # max_tokens: 4000
  # request_timeout: 180.0
  api_base: https://localhost:11434/v1
```
You should replace the ```api-key``` with ```ollama```, ```type``` to ```openai_chat``` (as ollama follows the same standard as openai); and the ```model``` with ```llama3```; and since the Ollama also supports the JSON mode so you can set the ```model_supports_json``` to ```true```.
It is also important to set the ```api_base: https://localhost:11434/v1```. 

**Using LLama-3 and Groq:**
```python
llm:
  api_key: GROQ_API_KEY
  type: openai_chat
  model: llama3-70b-8192
  model_supports_json: true # recommended if this is available for your model.
  max_tokens: 4000
  # request_timeout: 180.0
  api_base: https://api.qroq.com/openai/v1
  # organization: <organization_id>
  # deployment_name: gpt-4o
  tokens_per_minute: 5500 # set a leaky bucket throttle
  requests_per_minute: 25 # set a leaky bucket throttle
```
You should change the ```api_base``` to the following API endpoint:
```python
api_base: https://api.qroq.com/openai/v1
```

**Note:** Most of the models in groq has Requests_per_minute limits, and so you should also change the ```# tokens_per_minute``` and ```requests_per_minute``` too.
For instance, for llama3-70b-8192 the ```REQUESTS PER MINUTE``` is 30 and ```TOKENS PER MINUTE``` is 6000 so it would be logical to set these numbers properly.






