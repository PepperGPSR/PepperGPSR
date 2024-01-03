# PepperGPSR
This is a base repository for the research titled "Enhancing Autonomous Task Execution in Social Robots with Large Language Models". It was developed for the task generation and evaluation steps of the research.

## Installation
This repository uses Python 3.11.0. To install the required packages, run the following command:
```
pip install -r requirements.txt
```

## Usage
In order to use this repository, an environment file must be created. This file must be named `.env` and must be located in the root folder of the current working directory. For the GPT models, both Azure and the regular OpenAI API can be used. The following variables must be defined in the file:

### Azure
| Variable | Description |
| --- | --- |
| `OPENAI3_KEY` | The API key for the GPT 3.5 Turbo model deployment. |
| `OPENAI3_API_BASE` | The API base URL for the GPT 3.5 Turbo model deployment. |
| `OPENAI3_DEPLOYMENT_NAME` | The deployment name for the GPT 3.5 Turbo model deployment. |
| `OPENAI4_KEY` | The API key for the GPT 4 model deployment. |
| `OPENAI4_API_BASE` | The API base URL for the GPT 4 model deployment. |
| `OPENAI4_DEPLOYMENT_NAME` | The deployment name for the GPT 4 model deployment. |
| `OPENAI_API_TYPE` | The API type to use. This can be either `azure` or `openai`, for the case of this config, `azure` must be used |
| `OPENAI_API_VERSION` | The API version to use. |
| `POSTGRES_USER` | The username for the PostgreSQL database. |
| `POSTGRES_PASSWORD` | The password for the PostgreSQL database. |
| `POSTGRES_HOST` | The host for the PostgreSQL database. |
| `POSTGRES_DBNAME` | The database name for the PostgreSQL database. |

### OpenAI
| Variable | Description |
| --- | --- |
| `OPENAI_KEY` | The OpenAI API key. |
| `POSTGRES_USER` | The username for the PostgreSQL database. |
| `POSTGRES_PASSWORD` | The password for the PostgreSQL database. |
| `POSTGRES_HOST` | The host for the PostgreSQL database. |
| `POSTGRES_DBNAME` | The database name for the PostgreSQL database. |



After setting up the `.env` file, navigate to the `src` folder and run the following command:
```
python main.py
```

## Prompting Examples

The prompting used can be seen in the files `src/ls_generate.py` and `src/chain_generate.py` for the Long String and Chaining approaches respectively.

# Prompting types

For generating the code two prompting strategies were evaluated in this research, these strategies are the following.

## Chain Prompting

This approach seeks to split the main task into smaller problems that can be addressed separately. The diagram below illustrates the sequence of steps used with the language model to obtain better results in code generation.

![Chain Prompting](https://github.com/PepperGPSR/PepperGPSR/assets/62764177/9037a9a6-e304-494f-9c1c-da9cadab098c)

### Task Expansion: 
In this step, the language model is asked to expand the task a bit by providing more details on the steps the robot should follow. For example, some of these details include assuming the robot is never in the location it needs to go, helping the subsequent steps to be executed more effectively and enabling the robot to fulfill the task.

### Entity detection: 
In this step, entities described in the task are identified, such as objects, people, places, and more. The language model is instructed to extract this information and return it as a Python list with the recognized entities.

### Entity Synonyms: 
The extracted list of entities is replaced with entities that the robot understands. This is done because the robot’s interface only contains predefined places and objects established by the machine learning models used by the robot, which must adhere to certain syntactic rules. The model is tasked with returning a Python list replacing the entities with syntactically correct ones.

### Entity Replacement: 
Using the previous result, the language model is instructed to replace the entities from the given task with syntactically correct synonymous entities extracted through the earlier process. 

### Task validation:

With the expanded task, a validation is performed to determine if, with the currently developed capabilities, the robot can complete the assigned task. This involves checking if it recognizes the entities mentioned in the description and if, in general, its capabilities are sufficient for the task. The model is requested to return whether the robot has the capabilities and or not. If the model indicates that the robot lacks the necessary capabilities, error handling is performed, and the LLM explains why it cannot perform the task based on the information returned in this step.

### Code generation:  
If the previous step indicates that the robot does have the capabilities to carry out the task, the language model is assigned to generating the necessary code to fulfill it based on the created interface and refined task. This step returns Python code that is executed directly on the robot to accomplish the proposed task.


## Long String Prompting

This approach uses only one prompt to accomplish the task, the content of this prompt goes as follows:
– The goal of the model: This includes the specification of the main purpose of the model and the role it performs in the whole robot architecture.
– The available functions from the task module: These functions belong to the codebase interface and call services from the Robot, the syntax of the functions, the parameters, and outputs were included in this section.
– The available defined entities and tags: This is important because many services from the robot require precise syntax to work properly, this includes all of the places, objects, and questions tags that are previously predefined.
– The output format: In this section, the specified format of the response is informed. The prompt tells the language model that the response has to be a continuous snippet of code written in python.
– One example of desired output: An example of a real input and expected output is given, using the task module and the available entities.
– The robot constraints: Some of the tasks are not possible to accomplish due to the robot’s current capabilities, these tasks have to be handled by the model, by specifying why the robot can’t accomplish the task instead of trying and failing. 
– The input task: Finally, the task that the model has to solve utilizing the rest of the knowledge.

![Long String Prompting](https://github.com/PepperGPSR/PepperGPSR/assets/62764177/83d111fe-ca54-4e30-9e02-9913e6dd5099)


