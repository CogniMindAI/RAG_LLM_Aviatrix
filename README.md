## Technical Details 

# Environment Setup 

1. Clone the repo using git:

```shell
git clone https://github.com/Aizaz2024/localGPT.git
```

2. Create and activate a new virtual environment.

```shell
conda create -n localGPT python=3.10.0
conda activate localGPT
```

3. Install the dependencies using pip

To set up your environment to run the code, first install all requirements:

```shell
pip install -r requirements.txt
```

***Installing LLAMA-CPP :***

LocalGPT uses [LlamaCpp-Python](https://github.com/abetlen/llama-cpp-python) for GGML (you will need llama-cpp-python <=0.1.76) and GGUF (llama-cpp-python >=0.1.83) models.

For `NVIDIA` GPUs support, use `cuBLAS`

```shell
# Example: cuBLAS
CMAKE_ARGS="-DLLAMA_CUBLAS=on" FORCE_CMAKE=1 pip install llama-cpp-python==0.1.83 --no-cache-dir
```

### Ingest

Run the following command to ingest all the data.

If you have `cuda` setup on your system.

```shell
python ingest.py
```
You will see an output like this:
<img width="1110" alt="Screenshot 2023-09-14 at 3 36 27 PM" src="https://github.com/PromtEngineer/localGPT/assets/134474669/c9274e9a-842c-49b9-8d95-606c3d80011f">


Use the device type argument to specify a given device.
To run on `cpu`

```sh
python ingest.py --device_type cpu
```


Use help for a full list of supported devices.

```sh
python ingest.py --help
```

This will create a new folder called `DB` and use it for the newly created vector store. You can ingest as many documents as you want, and all will be accumulated in the local embeddings database.
If you want to start from an empty database, delete the `DB` and reingest your documents.

Note: When you run this for the first time, it will need internet access to download the embedding model (default: `Instructor Embedding`). In the subsequent runs, no data will leave your local environment and you can ingest data without internet connection.

## Ask questions to your documents, locally!

In order to chat with your documents, run the following command (by default, it will run on `cuda`).

```shell
python run_localGPT.py
```
You can also specify the device type just like `ingest.py`

```shell
python run_localGPT.py --device_type cuda # to run on Nvidia GPU
```

This will load the ingested vector store and embedding model. You will be presented with a prompt:

```shell
> Enter a query:
```

After typing your question, hit enter. LocalGPT will take some time based on your hardware. You will get a response like this below.
<img width="1312" alt="Screenshot 2023-09-14 at 3 33 19 PM" src="https://github.com/PromtEngineer/localGPT/assets/134474669/a7268de9-ade0-420b-a00b-ed12207dbe41">

Once the answer is generated, you can then ask another question without re-running the script, just wait for the prompt again.


***Note:*** When you run this for the first time, it will need internet connection to download the LLM (default: `TheBloke/Llama-2-7b-Chat-GGUF`). After that you can turn off your internet connection, and the script inference would still work. No data gets out of your local environment.

Type `exit` to finish the script.

### Extra Options with run_localGPT.py

You can use the `--show_sources` flag with `run_localGPT.py` to show which chunks were retrieved by the embedding model. By default, it will show 4 different sources/chunks. You can change the number of sources/chunks

```shell
python run_localGPT.py --show_sources
```

Another option is to enable chat history. ***Note***: This is disabled by default and can be enabled by using the  `--use_history` flag. The context window is limited so keep in mind enabling history will use it and might overflow.

```shell
python run_localGPT.py --use_history
```

You can store user questions and model responses with flag `--save_qa` into a csv file `/local_chat_history/qa_log.csv`. Every interaction will be stored. 

```shell
python run_localGPT.py --save_qa
```

# Run the Graphical User Interface
```shell
streamlit run localGPT_UI.py
```

    
## Docker 🐳

In progress
