# Multitask Learning for Binary and Multi-class Text Classification
This method performs multitask learning for binary and multi-class text classification, designed to handle complex annotations in social science datasets. The method integrates both text and additional features into a deep learning model to predict multiple labels simultaneously, using an architecture that combines LSTM networks and dense layers for feature fusion.

## Description
This method is a multitask learning approach tailored to handle text classification problems with multiple output types, including binary, multi-class, and distribution-based predictions. It is highly applicable in the context of detecting hate speech, misinformation, or online misogyny, where various annotation schemes are employed. The model leverages TensorFlow/Keras to create a flexible architecture that handles multiple tasks, making it suitable for datasets where annotations from multiple human annotators are available.

A related use case from the social science domain can be found in the study of **sexism detection in online comments**, such as the shared task presented by the Austrian Online Newspaper dataset. This dataset includes both binary and soft labels, annotated by multiple people, to reflect different levels of consensus. The method is suitable for analyzing such datasets to understand the various aspects of harmful online discourse.

The below use case addresses the challenges of dealing with subtle and implied sexism in text data and provides insights into how diverging annotator opinions can be handled in machine learning models.

- **Subtask 1:** Predict a binary label indicating the presence or absence of sexism and determine the majority grading assigned by annotators.
- **Subtask 2:** Predict binary soft labels, accounting for differences in annotators' opinions, and estimate the distribution of gradings.

This method addresses the challenges of dealing with subtle and implied sexism in text data and provides insights into how diverging annotator opinions can be handled in machine learning models.

### Subtask 1: Predicting Binary Labels

In this subtask, the goal is to predict binary labels based on the human annotations of sexism or misogyny present in the texts. Each text in the dataset is labeled based on annotators' ratings of sexism, which ranges from:

- 0: Kein (No sexism)
- 1: Gering (Mild sexism)
- 2: Vorhanden (Sexism present)
- 3: Stark (Strong sexism)
- 4: Extrem (Extreme sexism)

The labels to predict are based on the following strategies:

- **bin_maj:** Predict 1 if a majority of annotators assigned a label other than 0, otherwise predict 0. If no majority exists, both 0 and 1 are counted as correct.
- **bin_one:** Predict 1 if at least one annotator assigned a label other than 0, otherwise predict 0.
- **bin_all:** Predict 1 if all annotators assigned a label other than 0, otherwise predict 0.
- **multi_maj:** Predict the majority label. If no majority exists, any assigned label is correct.
- **disagree_bin:** Predict 1 if there is disagreement between annotators on the presence (0) vs. absence of sexism (all other labels), otherwise predict 0.

### Subtask 2: Predicting Label Distributions

In this subtask, the goal is to predict the distribution of annotators' ratings. Annotators assigned one of five labels indicating the level of sexism, and the distributions reflect the following:

- **Binary distribution (dist_bin):** Predict the portion of annotators assigning 0 (no sexism) vs. all other labels combined.
- **Multi-score distribution (dist_multi):** Predict the distribution for each individual label (0-4).


## Keywords
- Multitask learning
- Text classification
- Binary classification
- Multi-class classification
- Annotator consensus
- Sexism Detection
- Gender Discrimination
- Online Comments
- Content Analysis
- Misinformation detection

## Science Usecase(s)
This method is well-suited for research in **computational social science**, particularly in the analysis of online discourse:
- **Detecting harmful content**: This method can be used to identify hate speech, online sexism, or misinformation by handling both hard (binary) and soft (distribution) labels, providing more nuanced insights into online discussions.
- **Understanding annotator disagreement**: By predicting multiple outputs that account for different annotators' opinions, this method supports research in understanding where human annotators diverge, which is crucial for content moderation and automated classification systems.

Example research question: *How does human annotator disagreement influence the classification of misogynistic comments in online discourse?*

## Repo Structure
│
├── output/               # Directory for output data generated by the Jupyter notebook
│   ├── MH_subtask1_output_model.tsv  # Model predictions for subtask 3
│   ├── MH_subtask2_output_model.tsv   # Model predictions for subtask 2
│   └── predictions.csv         # Model predictions
│
├── saved_models/               # Directory for saving trained models 
│   └── development_model_MH.keras     # Saved model 
│
├── pretrained_embeddings/       # Directory for storing pre-trained embeddings
│   └── cc.de.300.vec            # Example pre-trained embeddings file
│
├── script.ipynb                # Main Jupyter notebook containing the three parts: 1. Data Loading and Preprocessing, 2. Model Definition and Training and 3. Prediction and Evaluation
│
├── README.md                   # Project overview, installation instructions, and usage

## Environment Setup
Make sure you have the following libraries installed:
```R
pip install pandas
pip install scikit-learn
pip install keras
pip install tensorflow
```
Additionally, ensure your environment supports Jupyter notebooks for exploratory analysis.



## Hardware Requirements (Optional)
This method can be run on most standard computing environments. For large-scale datasets, it is recommended to use a machine with a minimum of 16GB RAM and a GPU for faster model training, though it can run on CPU for smaller datasets.

## Input Data
The input data consists of annotated comments provided by multiple human annotators, with labels reflecting the severity of sexism. The data is in JSONL format, where each entry includes:

id: Unique identifier for the comment.
text: The content of the comment.
annotations: An array of dictionaries, each containing:
user: An anonymized ID for the annotator.
label: The label assigned by the annotator (0: Kein, 1: Gering, 2: Vorhanden, 3: Stark, 4: Extrem).
For more details, refer to the competition website.

## Sample Input and Output Data
Sample Input:
```R
{
	"id": "4e299683daa40693478edfe789aea22f", 
	"text": "jo, so ganz ohne ansichtssache.. ^^", 
	"annotations": [
		{"user": "A002", "label": "0-Kein"},
		{"user": "A008", "label": "1-Gering"}, 
		{"user": "A009", "label": "0-Kein"}, 
		{"user": "A010", "label": "3-Stark"}
	]
}
```
## Sample Output (Subtask 1 - Binary Label Prediction):
```R
id	dist_bin_0	dist_bin_1	dist_multi_0	dist_multi_1	dist_multi_2	dist_multi_3	dist_multi_4
79d22e82ee0fd4c9817c385c6b17b58f	0.8693843	0.13061567	0.9656597	0.0020012283	0.012344101		0.013989335		0.006005645
a8df16f1f9ca7eda74feddc38ef451b4	1.0			0.0			1.0			1.5071736e-12	2.693453e-11	6.5322976e-11	5.945082e-11
a7ed7076a08dc3e76fa5575b2d03903c	0.9994901	0.00050989	0.9999965	9.682307e-08	7.6432013e-07	1.7158852e-06	9.1648184e-07
49b50013b79e75ce2c75f959a9e66703	0.9732194	0.026780576	0.9984353	4.8993938e-05	0.00039118106	0.00082619494	0.00029827084
af5d90ddea21ed854b0c21953c0aef33	0.99989855	0.000101448	0.9999997	7.2824538e-09	5.8958264e-08	1.3266862e-07	1.2663253e-07
```

## Sample Output (Subtask 2 - Label Distribution Prediction):
```R
id									dist_bin_0	dist_bin_1	dist_multi_0	dist_multi_1	dist_multi_2	dist_multi_3	dist_multi_4
79d22e82ee0fd4c9817c385c6b17b58f	0.8693843	0.13061567	0.9656597		0.0020012283	0.012344101		0.013989335		0.006005645
a8df16f1f9ca7eda74feddc38ef451b4	1.0			0.0			1.0				1.5071736e-12	2.693453e-11	6.5322976e-11	5.945082e-11
a7ed7076a08dc3e76fa5575b2d03903c	0.9994901	0.0005098	0.9999965		9.682307e-08	7.6432013e-07	1.7158852e-06	9.1648184e-07
49b50013b79e75ce2c75f959a9e66703	0.9732194	0.026781	0.9984353		4.8993938e-05	0.00039118106	0.00082619494	0.00029827084
```

## How to Use
Installation: Clone the repository and install the dependencies as described in the environment setup.


Download Pre-trained Embeddings:.
The script requires pre-trained FastText embeddings. Due to file size limitations, the embeddings are not included in this repository. To download the embeddings:
1. Navigate to the main directory (where the script.ipynb file is located):
```R
cd path/to/main/directory
```
2. Create the pretrained_embeddings directory and go to the pretrained_embeddings directory::
```R
mkdir pretrained_embeddings && cd pretrained_embeddings
```
3. Download the embeddings and extract the embeddings:

```R
wget https://dl.fbaipublicfiles.com/fasttext/vectors-crawl/cc.de.300.vec.gz
gunzip cc.de.300.vec.gz
```

Once everything is set up, you can run the script.ipynb in a Jupyter environment:
```R
jupyter notebook script.ipynb
```
jupyter notebook script.ipynb

## Contact Details
For any questions or issues regarding this method, please reach out to Fakhri Momeni via email: fakhri.momeni@gesis.org.

## Acknowledgements
This method was developed as part of a collaboration to address the challenges of online content moderation and sexism detection. Special thanks to the organizers of the sexism detection competition for providing the dataset and competition framework.
