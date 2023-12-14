# LLM Detection

This project is an analysis on LLM detection based on [this Kaggle Challenge](https://www.kaggle.com/competitions/llm-detect-ai-generated-text).

The methodology, analysis and implementation details as well as explanations and supporting graphs can be found in the [main notebook](notebooks/llm_detection.ipynb). The [report](report.pdf) only contains supporting graphs for the in-class project presentation.

## Project Structure

```
│   README.md - this file
│   report.pdf - generated report file exported for presentation
│   report.tex - source code for report file
│
├───data - input data 
│   │   chatgpt_cars.md
│   │   chatgpt_electoral.md
│   │   cluster_augmentation_cars.md
│   │   sample_submission.csv
│   │   test_essays.csv
│   │   train_essays.csv
│   │   train_prompts.csv
│
├───notebooks
│   │   llm_detection.ipynb - The main notebook the project is built upon
│
├───output - output data and figures
│       attribution.png
│       augmentation.csv - Formatted generated data
│       augmentation_stats.png 
│       clusters.png
│       dataset_size.png
│       similarity.png
│
└───src - Personal library
    │   crawling.py
    │   data.py
    │   ml.py
└── .gitignore
```

## The Dataset

### Human Essays

The Human essays were downloaded from the competition and can be found at [data/train_essays.csv](data/train_essays.csv).


### Generated Essays

The raw files comprising downloaded ChatGPT conversations can be found at [data/chatgpt_electoral.md](data/chatgpt_electoral.md), [data/chatgpt_electoral.md](data/chatgpt_electoral.md) and [data/cluster_augmentation_cars.md](data/cluster_augmentation_cars.md).

The final, formatted augmentation dataset used in this project was generated by hand using ChatGPT. It features roughly 500 generated essays with an almost even split between the two essay prompts. The dataset can be found at [output/augmentation.csv](output/augmentation.csv). The file is structured as follows:

| Column | Description  |
|---|---|
| id  | A unique identifier for the essay  |
| text  | The text of the essay  |
|  prompt_id | Which prompt the essay was generated from, relates to [data\train_prompts.csv](data\train_prompts.csv) |
| generated | Whether the essay was generated by an LLM, all 1  |
| cluster | The assigned cluster of the text, as described in the notebook |  


## Prompts used

The two basic prompts, as well as the source texts accompanying them can be found at [data/train_prompts.csv](data/train_prompts.csv) and will be refered to as **original prompts** from this point forward. The source texts contained in the same file will be refered to as **sources**.

Many prompting strategies were utilized in order to maximize the variation and data quality of the generated essays by the LLM. Below we list the more prominent prompt structures used:

### Simple prompt

```
<original prompt>

Do not include a greeting/title at all. Remove placeholder tags such as [Your Name] entirely. 
```

### Simple prompt with sources

```
<original prompt>

Do not include a greeting/title at all. Remove placeholder tags such as [Your Name] entirely. 

Sources:
<sources>
```


### Simple prompt with less reliance on sources 

```
<original prompt>

Do not include a greeting/title at all. Remove placeholder tags such as [Your Name] entirely. 

Use the sources only as inspiration, do not just cite them.

Sources:
<random sample of sources>
```


### Role prompt
```
<original prompt>

You are a US high school student. Use language fitting an average US teenager (more informal at places). Do not include a greeting/title at all. Remove placeholder tags such as [Your Name] entirely. 

Examples:
<random samples of student essays>
```

### Role prompt with sources
```
<original prompt>

You are a US high school student. Use language fitting an average US teenager, imitating the structure and tone of the examples below. Do not include a greeting/title at all. Remove placeholder tags such as [Your Name] entirely. 

Use the sources only as inspiration, do not just cite them.

Examples:
<random samples of student essays>

Sources:
<random sample of sources>
```