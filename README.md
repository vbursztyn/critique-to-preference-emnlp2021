## "It doesn't look good for a date": Transforming Critiques into Preferences for Conversational Recommendation Systems
---

This file contains data, software, and reproducibility instructions for our three methods: *critique understanding*, embeddings-based recommendation search (*f_cos*), and fine tuning-based recommendation ranking (*f_LM*).

### Critique Understanding

Run `pip install -r requirements.txt` before attempting to run any code.

In folder `critique_understanding`, you may find both our entire prompt (`GPT3_prompt.txt`) and a Jupyter Notebook that reads the critiques from `queries/queries.csv` and uses OpenAI's Completion API with the parameters described in the paper (p.2) to generate positive preferences. However, please notice that `queries.csv` already contains the generated preferences used in our experiments. This step requires an OpenAI Beta account, as an API_KEY must be provided in the Notebook.

### Embeddings-based Recommendation Search (f_cos)

In folder `f_cos`, you may find all the data behind Table 2 (`f_cos_evaluation_and_characterization.xlsx` - first tab), including human-made assessments and Precision@n measures. You may also find the characterization of the 170 cases where f_cos^pref outperformed f_cos^crit (`f_cos_evaluation_and_characterization.xlsx` - second tab), supporting the following analysis (p. 4): "Analyzing the results of f_cos^pref and f_cos^crit for the 340 individual critiques, we found 170 cases where f_cos^pref outperformed f_cos^crit. Within these, 40 belong to the first pattern (23.53%), 78 to the second (45.88%), and 38 to the third (22.35%)."

At the time of this paper, the dependencies for f_cos are closely connected to a larger chatbot application that is not part of the scope of this submission (e.g., all the data models that include restaurants and their reviews). For this reason, we are not including runnable code, but the description on page 2 supplies the steps, references, and the only parameter chosen for EmoNet.

### Fine Tuning-based Recommendation Ranking (f_LM)

In folder `f_LM`, you may find all the data necessary to reproduce Table 3: folds 1 and 2 used to train and test f_LM^crit, f_LM^pref, and f_LM^concat on tasks 1, 2, and 3. As described in the paper (p.3), all data is already encoded using 512 of maximum sequence length. Also, all the steps for reproducing this part are thoroughly documented at this link:

http://cognitiveai.org/2020/09/08/using-tensorflow-ranking-bert-tfr-bert-an-end-to-end-example/

We specifically used the following command line to run our experiments:
```
./bazel-bin/tensorflow_ranking/extension/examples/tfrbert_example_py_binary --bert_init_ckpt=${BERT_DIR}/bert_model.ckpt --bert_max_seq_length=512 --model_dir="${OUTPUT_DIR}" --list_size=15 --loss=softmax_loss --train_batch_size=1 --eval_batch_size=1 --learning_rate=1e-5 --num_train_steps=10000 --num_eval_steps=20 --checkpoint_secs=3000 --num_checkpoints=4
```

Using BERT-Base (L=12, H=768, 110M parameters) from this source:
https://storage.googleapis.com/bert_models/2020_02_20/uncased_L-12_H-768_A-12.zip

