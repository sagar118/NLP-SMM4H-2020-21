# NLP SMM4H 2020 & 21

## Introduction
This project aims to tackle various problems in the health care domain. The rise of COVID-19 brought a new norm in ways in which humans communicate. Platforms such as Twitter have proven to be effective and generate loads of user generated data. As people have more freely shared their life experiences on the platform, it gives opportunity are the pharmaceutical firms to leverage insights from the data using natural language processing.

The Social Media Mining for Health Applications (SMM4H) shared tasks aims to tackle various healthcare application in social media texts. We worked upon total of 4 tasks, Task 2: Automatic classi- fication of multilingual tweets that report adverse effects (only English language) and Task 3: Automatic extraction and normalization of adverse effects in English tweets extraction and normalization of adverse effects in English tweets from SMM4H 2020 maps to Task 1 & 2 in our problem statement. Also, Task 6: Perform three-way classification of COVID tweets containing symptoms and Task 5: Classification of tweets self-reporting potential COVID19 cases from SMM4H 2021 maps to Task 3 & 4 in our problem statement.

## Task and Data Description
### Task 1: Automatic classification of tweets that report adverse effects
This task involves distinguishing tweets that report adverse effect (AE) to a medication from those that do not. Table 1 shows the data distribution for the train and validation set, along with positive and negative split of data for both training and validation set.

### Task 2: Automatic extraction and normalization of adverse effects in English tweets
This task is an end-to-end task involving extraction of span text that contain an AE of medication from the tweets that report an AE. After extraction we map the extracted AE to a standard concept ID in the MedDRA vocabulary. Table 1 shows the data distribution for the train and validation set, along with positive and negative split of data for both training and validation set.

### Task 3: Classification of COVID19 tweets containing symptoms
This task is a multi-class classification problem. The target classes are self-reports, non-personal reports, and literature/news mentions. Self-reports are personal mentions of COVID19 symptoms, non-personal reports are mentions of symptoms experience by other person, and literature/news mentions are tweets containing symptoms mentioned in some news or scientific articles. Table 2 shows the data distribution for the train and validation set, along with per class distribution of data for both training and validation set.

### Task 4: Classification of tweets self-reporting potential COVID19 cases
This task involves distinguishing tweets that self-report potential cases of COVID19 from those that do not. “Potential case” tweets discuss topics such as testing, symptoms, traveling, or social distancing that indicates higher risk of exposure to COVID-19. “Other” tweets are related to COVID-19 and may discuss on the same topics but do not indicate that the user or a member of the user’s household may be infected. Table 1 shows the data distribution for the train and validation set, along with positive and negative split of data for both training and validation set.

<p align="center">
<img width="837" alt="image" src="https://user-images.githubusercontent.com/24275587/177448954-46d28259-6fc1-46a3-94e2-6660411a6290.png">
</p>

## Method & Model Architecture
### Pre-processing
Twitter data contains a lot of noise, to clean the data we applied Ekphrasis (Baziotis et al., 2017) text- preprocessing techniques that is geared towards text from social media. Using this technique includes lower case the tweets, normalize & annotate URL’s, mentions, hashtags, redundant characters, emojis, emoticons, punctuation’s, repeated characters, extra white-spaces, and expand contractions. This tech- nique is common across task 1, 3, and 4.

### Task 1: Automatic classification of tweets that report adverse effects
We experimented with different transformer models like BERT base uncased, RoBERTa base, SciBERT with scivocab, and BioBERT base v1.1. We fine-tuned each model on the pre-processed data and per- formed classification through a linear layer. The model were trained for 5 epochs with batch size 32 and learning rate as 2e-5. We found the best result for RoBERTa base on the validation set. Table 4 shows the performance of the model on validation set.

<p align="center">
  <img width="712" alt="image" src="https://user-images.githubusercontent.com/24275587/177449279-a4898801-8529-478a-a083-9b3d77606d03.png">
</p>

### Task 2: Automatic extraction and normalization of adverse effects in English tweets
The given task can be divided into two subtasks:
1. Extraction of Adverse Effects from tweets
  - Classification of tweets containing ADR mentions
  In the first stage of the pipeline, before extracting any span of text we pre-processed the tweets with the same preprocessing techniques mentioned in section 4.1. These tweets would be passed to train RoBERTa to classify whether tweets contain ADR mentions or not.
  - NER tagger
  We posed the problem as a Custom Named Entity Recognition problem using the training dataset. We found that the start and end index of the extracted text did not align with the indexes in the tweets. To rectify these we found the correct start and end indexes and used those indexes for building our model. We fine tuned SciBERT and BioBERT for extracting the ADR mention extract. Before training the model, we tagged each tweet using the ‘BIO’ tagging scheme, where ‘B’ stands for begin of the token, ‘I’ stands for inside the token, and ‘O’ stands for outside the token. We only extracted text from tweets that were classified containing ADR mentions from the previous step. We trained our model for 5 epochs with a learning rate of 3e-5 and found the best results using BioBERT as our model.
  
2. Normalization of extracted Adverse Effect to a standard concept ID
Considering that there are more MedDRA terms present beyond the ones from the train set, to incorporate the wide range of terms we added the whole list MedDRA code and term mapping. We found the word embedding (average of the last two hidden layers) for each MedDRA term using SciBERT and created aa dictionary mapping for each code and its representation. To normalize we find the cosine similar of the word embedding of the extracted text with all the MedDRA terms and tag the term that is the most similar.

### Task 3: Classification of COVID19 tweets containing symptoms
We experimented with different transformer models like BERT base uncased, RoBERTa base, SciBERT with scivocab, and BioBERT base v1.1. Also, tried domain specific BERT model such as CT-BERT. We fine-tuned each model on the pre-processed data and performed classification through a linear layer. The model were trained for 5 epochs with batch size 32 and learning rate as 2e-5. We found the best result for RoBERTa base and BioBERT on the validation set. Table 4 shows the performance of the model on validation set.

### Task 4: Classification of tweets self-reporting potential COVID19 cases
We experimented with different transformer models like BERT base uncased, RoBERTa base, SciBERT with scivocab, and BioBERT base v1.1. Also, tried domain specific BERT model such as CT-BERT. We fine-tuned each model on the pre-processed data and performed classification through a linear layer. The model were trained for 5 epochs with batch size 32 and learning rate as 2e-5. We found the best result for RoBERTa base on the validation set. Table 4 shows the performance of the model on validation set.

## Results
Below we summarize the performance different models on all tasks. The best score among the different models is highlighted in bold for each task. Table 3 and 4 shows the performance of the models on validation set. For Task 1, the best F1-score we achieved was 0.64 using RoBERTa model which was an 16% improvement over the baseline model. Task 2, the best F1-score for NER was achieved using BioBERT which was an 11.5% and 16.9% improvement over the baseline model for strict and relaxed measure. For NER+Normalization we got the best F1-score as 59% and 55% which was an 49% and 40.3% improvement over the baseline for strict and relaxed scores respectively. Task 3, RoBERTa and BioBERT both gave us the best F1-micro score which was 0.984 which was an 2% improvement over the baseline model. Finally, task 4, RoBERTa model gave us the best F1-score with 0.75 which was an 23% improvement over the baseline model
<p align="center">
<img width="753" alt="image" src="https://user-images.githubusercontent.com/24275587/177450575-8e7fd642-659e-4fdb-b3b7-a4edf0948227.png">
</p>

The report (`Report.pdf`) contains details about the result and error analysis in more detail.

### Collaboration:
- Anuja Katkar
- Sagar Thacker
