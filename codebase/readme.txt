Natural Language Processing
Project 4: Social Media Mining for Health Monitoring
----------------------------------------------------

The codebase folder contains the code for both milestone 2 and milestone 3. The folder is structed in the following manner:

codebase
|-- baseline
	|-- Folder for each task
|-- data
	|-- Training and validation dataset per task
|-- final_models
	|-- Folder for each task containing the code and related datafiles

Each task has its own folder which contains folder for each model. Example, In final_models, task 1 has BERT, RoBERTa, BioBERT and other sub-folders for each model. Each contain an .ipynb file that contains the code and can be used to run the code.

Note: We used google colab to run our code and uploaded each data file in colab. Hence, the path to the data file would not match in the downloaded .ipynb notebooks. To run the code, please provide the correct path to the `data` folder and proper sub-folder for each task to load the data. Also, if task had additional data, that would be present in the model folder.

`final_models/Task2/Trial_and_error` folder contains our trials for trying some techniques but that didn't not work for us. We have kept all for that task in that folder.

For baseline task 2 normalization part we used GoogleNews-vectors-negative300.bin.gz vectors. When we were working on task 2 for baseline we could download the file from amazon s3 bucket but now it has been removed. To run the file for that task, you would have to download the file from google drive and keep it in the same directory (Size of the file is 1.5GB). Link to the file is https://drive.google.com/file/d/0B7XkCwpI5KDYNlNUTTlSS21pQmM/edit?resourcekey=0-wjGZdNAUop6WykTtMip30g