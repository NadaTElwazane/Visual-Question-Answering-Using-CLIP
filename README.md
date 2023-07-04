# Visual Question Answering using CLIP

## Table of Contents
- [Introduction](#introduction)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Results](#results)
- [Notebook](#notebook)
- [Collaborators](#collaborators)

## Introduction
Visual Question Answering (VQA) is a rapidly growing field that involves answering open-ended questions based on visual input. VQA has numerous applications, including helping visually impaired people, medical VQA, education, surveillance, and others. In this project, we focus on using the [VizWiz dataset](https://www.kaggle.com/datasets/ingbiodanielh/vizwiz).

To tackle this problem, we use a CLIP-based approach to VQA using the VizWiz dataset. The CLIP model, developed by OpenAI, has shown promising results in various natural language processing tasks and has the potential to improve performance in VQA tasks as well. Our
methodology involves loading and splitting the data using stratified sampling on answer type and answerability, selecting the most common answer for each question using Levenshtein distance to break ties, and encoding image-question pairs using a CLIP ViT-L/14@336px model with data augmentation. We then train a VQA model using auxiliary answer type loss and an answerability model and evaluate our approach using accuracy and answerability metrics. This approach closely follows the implementation described in [Less is More: Linear Layers on CLIP Features as Powerful VizWiz Model](https://arxiv.org/abs/2206.05281) by Fabian Deuser, Konrad Habel, Philipp J. Rösch, Norbert Oswald.

## Dataset
The VizWiz dataset consists of images taken by blind people along with recorded spoken questions about the images and 10 crowdsourced answers per visual question.

## Methodology
Our methodology for tackling the VQA problem using the VizWiz dataset involves several steps. First, we load the data and split it using stratified sampling on answer type and answerability. We then select the most common answer for each question using Levenshtein distance to break ties, as per the instructions in (Deuser et al., 2022). This results in a total of 6178 classes.

Next, we create a model using a CLIP ViT-L/14@336px model to encode the given image and question pair. We encode the image and question pairs with data augmentation (0, 90, 180, 270 rotations) and save the encoded data to a file to reduce training time. After trying different approaches, we found that using a weighted sum of 0, 90, 270 rotations (0.5, 0.25, 0.25) gave us the best results. We then create a VQA model as per the instructions in (Deuser et al., 2022), using auxiliary answer type loss in the model. The features are concatenated and passed to linear layers with layer normalization and a high dropout value (0.5). We also create an answerability model. We train both models up to 250 epochs with early stopping and learning rate decay, saving the model with the best validation accuracy. We use cross entropy loss during training. We plot the loss and accuracy during training. Finally, we evaluate our approach on the test dataset using accuracy and answerability metrics.

## Results
We evaluated our approach on the test dataset using accuracy and answerability metrics. Our VQA model achieved an accuracy lower than the 60% reported in the referenced paper (Deuser et al., 2022). However, it is likely that the paper used a different accuracy scoring measure. Our answerability model achieved a diagonal answerability that was comparable to the results reported in the paper. It is important to note that the referenced paper used a newer version of the VizWiz dataset, which could have contributed to the difference in results.

| Source | VQA | Answerability |
| --- | --- | --- |
| Our Results | 42.0% | 82.8% |
| Referenced Paper | 60.7% | 83.5% |

## Notebook
The notebook for this project can be found [here](clip-vqa.ipynb). 

As well as a report in the form of a short paper [here](CLIP_Based_VizWiz_Question_Answering.pdf).

## Collaborators
- [Manar Amgad](https://github.com/manaramgadd)
- Ahmed Dusuki