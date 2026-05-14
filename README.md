# Phishing Website Classifier
 
A BERT-based binary text classifier that detects phishing websites from URLs and page content. Fine-tuned on the [shawhin/phishing-site-classification](https://huggingface.co/datasets/shawhin/phishing-site-classification) dataset.
 
## Results
 
| Metric | Score |
|--------|-------|
| Accuracy | 89.1% |
| AUC | 0.945 |
| Macro F1 | 0.87 |
 
### Confusion Matrix
 
![Confusion Matrix](<img width="490" height="374" alt="cm" src="https://github.com/user-attachments/assets/17ff6572-371e-478d-95b2-fd070bca382a" />
)
 
The model performs consistently across both classes (F1: 0.87 for Safe, 0.87 for Not Safe), showing no bias toward either class.
 
## Approach
 
- **Base model:** `google-bert/bert-base-uncased`
- **Strategy:** Froze all base BERT layers and only fine-tuned the pooler layer — keeping pretrained language knowledge intact while adapting the classification head to this specific task
- **Classes:** Safe (0) / Not Safe (1)
- **Dataset split:** Train / Validation / Test
## Why only the pooler layer?
 
Fine-tuning all of BERT on a small dataset risks overfitting. By freezing the base layers and only training the pooler, the model retains general language understanding from pretraining and learns only the task-specific classification boundary. This also significantly reduces training time.
 
## Training Configuration
 
```python
learning_rate = 2e-4
batch_size = 8
num_epochs = 10
```
 
## How to Run
 
1. Clone the repo
```bash
git clone https://github.com/sunayan1/phising-website-classifier-
cd phising-website-classifier-
```
 
2. Install dependencies
```bash
pip install transformers datasets evaluate scikit-learn seaborn
```
 
3. Run the notebook
```bash
jupyter notebook phising_classifieripynb.ipynb
```
 
## What I Learned
 
- How BERT's classification head works — the [CLS] token's 768-dimensional output gets projected to class logits via a linear layer
- Why AUC is a better metric than accuracy for classification tasks — accuracy can be misleading on imbalanced datasets
- The tradeoff between freezing and fine-tuning layers — more frozen layers means less overfitting but less task adaptation
