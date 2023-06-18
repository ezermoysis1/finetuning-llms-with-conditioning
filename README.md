# finetuning-llms-with-conditioning

## Project Description

UCL Module Project: Statistical Natural Language Processing (COMP0087)

Date: April 2023

This project investigates the effectiveness of finetuning language models (LMs) with multiple control tokens to reduce generated content that is misalignment to human preferences; specifically, content that is toxic and negative. The performance of our proposed LM is evaluated based on its ability to generate non-toxic and positive content. Experiment results demonstrate that conditioning with multiple control tokens is feasible and can improve the LM alignment with human preferences. Therefore, these findings suggest that fine-tuned LMs have the potential to generate content that is free of bias or offensive language, which could be useful in developing safe language models for public use. Further research, however, is needed to optimise conditioning on multiple tokens.

## Setup

### Setting up a virtual environment
First, clone the repository:

```bash
git clone https://github.com/ezermoysis1/finetuning-with-human-preferences
```

Change your directory to where you cloned the files:

```bash
cd finetuning-with-human-preference
```

Create a virtual environment with Python 3.6 or above:

```bash
virtualenv venv --python=python3.7 (or python3.7 -m venv venv or conda create -n multiqa python=3.7)
```

Activate the virtual environment. You will need to activate the venv environment in each terminal in which you want to use the project.

```bash
source venv/bin/activate (or source venv/bin/activate.csh or conda activate multiqa)
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```
    
## Use the code

### Data

To fine-tune the LM, we use randomly sampled sentences from the diverse data set the Pile (Gao et al., 2021). To ensure data compatibility with the LM, we exclude data sources that contain noncompatible content. This content includes coding and multi-lingual information from sources such as
GitHub and EuroParl. We take a 2% sample from the remaining data sources. The sample is processed by removing special characters; adding an <|endoftext|> token to the end of each sentence; and removing short,
low-quality sentences. Control tokens for toxicity; <|toxic|> and <|nontoxic|>; and sentiment; <|pos|>
and <|neg|>; are pre-pended to the processed sentences, based on the classification provided by Detoxify and VADER, respectively. Classifier tokens are not added to a random 1% of the sampled sentences to maintain alignment with the LM, as per Korbak et al. (2023). The resulting training data set is comprised of 800K sentences, and 25M tokens.

### Train

We fine-tune GPT-2 on data containing two binary indicators; toxicity and sentiment. By extending the conditioning from one control token to two, we determine the effectiveness of multiple token conditioning, with the view to extending this to more tokens in the future. The performance of the model is determined by its misalignment score. This is defined as the percentage of model generated sentences that contradict the conditioning token. For example, if the prompt is conditioned on ‘nontoxic’, then the model’s misalignment score is the percentage of generated sentences classified as toxic. Thus, the lower the misalignment score, the more effective the conditioning has been. The impact of increasing the number of tokens during fine-tuning is also considered.

To create the tokenizer, run:

```bash
python scripts/tokenizer_create.py
```

To download and create the dataset, run:

```bash
python main_dataset_create.py args/args_dataset_full
```

To train models finetunned on (i) toxicity, (ii) sentiment, (iii) toxicity & sentiment, (iv) neither, run respectively:
```bash
python main_train args/args_train_tox
```
```bash
python main_train args/args_train_sen
```
```bash
python main_train args/args_train_toxsen
```
```bash
python main_train args/args_train_none
```
### Evaluation 

To evaluate all of the trained models run the following. In the yaml file feel free to change the hyperparameters related to text generation (top_p, top_k, max_length, temperature, no_repeat_ngram_size) as desired. The values set are the ones used for the paper experiments. 

```bash
python main_evaluate.py args/args_evaluate_main1
```

## Authors

- [@ezermoysis1](https://github.com/ezermoysis1)
- [@fclarke1](https://github.com/fclarke1)
- [@laura](https://github.com/)
- [@sam](https://github.com)

## Documentation
Please read the full report of the project [here](https://drive.google.com/file/d/1-lac-XNLTj-8RI8X6_g2iqUPp0JDxfWo/view?usp=sharing)

Link to video [here] [https://drive.google.com/file/d/1bimKTSbA4TTgJOIKiICw0iY0xpcYuSJd/view?usp=sharing]