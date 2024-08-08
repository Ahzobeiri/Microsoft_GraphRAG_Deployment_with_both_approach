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

**Using LLama-3 and Ollama:**

As Llama-3 is  open source and free, we tried to use Llama-3 rather than GPT4o and Azure-Open-AI to decrease costs and prices due to the number of requests. We also installed Ollama in the local system to use Llama-3.

1- If you don't have Ollama, you can freely download it from the [website](https://ollama.com/) after creating an account. For Linux systems, you can install it with the following command:

```python
curl -fsSL https://ollama.com/install.sh | sh
```

To check if you correctly install the Ollama you can run ```ollama --help``` and see the output of available commands for Ollama.



