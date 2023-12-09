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
