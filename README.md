# IR2
Information Retrieval 2
# Tasks

## Compare lexical IR methods

- Fix environment, i.e. what is currently in repo should run without errors __[MA, NH]__
- jelinek_mercer: fix (or understand) performance problem
- jelinek_mercer, dirichlet, absolute_discounting:
    - add to 'retrieval_models' dict (pick a value for hyper parameters)
    - run evaluation and see if results make sense (should be comparable to tfidf)
    - tune hyper parameters by running evaluation on validation set 
    - use optimal hyper parameter for the test run evaluation
- positional language model:
    - implement __[MJ]__
    - implement kernel functions
    - sanity check
    - tune hyper parameters
    - use optimal hyperparameters in test run
- jelinek_mercer, dirichlet, absolute_discounting, positional language model:
    - create plots showing `NDCG@10` with varying values of the parameters
- Analyse the results by identifying specific queries where different methods succeed 
or fail and discuss possible reasons that cause these differences (manual inspection)
- Decide on a method to 'mitigate' the multiple comparison problem with t-tests



<br>
<br>

### Task 1: Implement and compare lexical IR methods [35 points] ### 

In this task you will implement a number of lexical methods for IR using the **Pyndri** framework. 
Then you will evaluate these methods on the dataset we have provided using **TREC Eval**.

Use the **Pyndri** framework to get statistics of the documents (term frequency, document frequency, 
collection frequency; **you are not allowed to use the query functionality of Pyndri**) and implement the 
following scoring methods in **Python**:

- [TF-IDF](http://nlp.stanford.edu/IR-book/html/htmledition/tf-idf-weighting-1.html) and 
- [BM25](http://nlp.stanford.edu/IR-book/html/htmledition/okapi-bm25-a-non-binary-model-1.html) 
with k1=1.2 and b=0.75. **[5 points]**
- Language models ([survey](https://drive.google.com/file/d/0B-zklbckv9CHc0c3b245UW90NE0/view))
    - Jelinek-Mercer (explore different values of 𝛌 in the range [0.1, 0.5, 0.9]). **[5 points]**
    - Dirichlet Prior (explore different values of 𝛍 [500, 1000, 1500]). **[5 points]**
    - Absolute discounting (explore different values of 𝛅 in the range [0.1, 0.5, 0.9]). **[5 points]**
    - [Positional Language Models](http://sifaka.cs.uiuc.edu/~ylv2/pub/sigir09-plm.pdf) define a language model for 
    each position of a document, and score a document based on the scores of its PLMs. 
    The PLM is estimated based on propagated counts of words within a document through a proximity-based 
    density function, which both captures proximity heuristics and achieves an effect of “soft” passage retrieval. 
    Implement the PLM, all five kernels, but only the Best position strategy to score documents. 
    Use 𝛔 equal to 50, and Dirichlet smoothing with 𝛍 optimized on the validation set 
    (decide how to optimize this value yourself and motivate your decision in the report). **[10 points]**
  
Implement the above methods and report evaluation measures (on the test set) using the hyper parameter values 
you optimized on the validation set (also report the values of the hyper parameters). 
Use TREC Eval to obtain the results and report on `NDCG@10`, Mean Average Precision (`MAP@1000`), `Precision@5` and 
`Recall@1000`.

For the language models, create plots showing `NDCG@10` with varying values of the parameters. 
You can do this by chaining small scripts using shell scripting (preferred) or 
execute trec_eval using Python's `subprocess`.

Compute significance of the results using a [two-tailed paired Student t-test](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_rel.html) **[5 points]**. 
Be wary of false rejection of the null hypothesis caused by the 
[multiple comparisons problem](https://en.wikipedia.org/wiki/Multiple_comparisons_problem). 
There are multiple ways to mitigate this problem and it is up to you to choose one.

Analyse the results by identifying specific queries where different methods succeed 
or fail and discuss possible reasons that cause these differences. 
This is *very important* in order to understand who the different retrieval functions behave.

**NOTE**: Don’t forget to use log computations in your calculations to avoid underflows. 
