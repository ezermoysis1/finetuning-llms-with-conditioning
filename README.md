# finetuning-with-human-preference

## Project Description

UCL Module Project: Statistical Natural Language Processing (XXX)

Date: April 2023

This project investigates the effectiveness of finetuning language models (LMs) with multiple control tokens to reduce generated content that is misalignment to human preferences; specifically, content that is toxic and negative. The performance of our proposed LM is evaluated based on its ability to generate non-toxic and positive content. Experiment results demonstrate that conditioning with multiple control tokens is feasible and can improve the LM alignment with human preferences. Therefore, these findings suggest that fine-tuned LMs have the potential to generate content that is free of bias or offensive language, which could be useful in developing safe language models for public use. Further research, however, is needed to optimise conditioning on multiple tokens.

## Setup

### Setting up a virtual environment
First, clone the repository:

```bash
git clone https://github.com/ezermoysis1/finetuning-with-human-preference
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


To train a model with with L = 25% labelled and (1-L) = 75% unlabelled data (referred to as 'M-25' in the report) use the following script. This also trains two benchmark supervised models only, with L% labelled data (referred to as 'M25L' or 'M-25L' in the report, and 100% data respectively ('MU' or M-100'). 

```bash
python main_pt.py 0.25
```

To train models with different labelled/unlabelled data splits, change the float after the .py. For example to train a L=35% and the two benchmark models, use the following

```bash
python main_pt.py 0.35
```

### Evaluation 

To evaluate all of the trained models run the following:

```bash
python main_pt.py evaluate
```

## Authors

- [@ezermoysis1](https://github.com/ezermoysis1)
- [@fclarke1](https://github.com/fclarke1)

## Documentation
Please read the full report of the project [here](https://drive.google.com/file/d/1zX3HGt0AiCVF5MfM4lKS9Ag_boOhq-_c/view?usp=sharing)