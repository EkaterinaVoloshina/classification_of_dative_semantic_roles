## Semantic Role Labeling for Forms with a Dative Marker 

This reprository contains the code and the data, which are described in the term paper for [HSE University](https://www.hse.ru/ba/ling) *«Semantic Role Labeling for Forms with a Dative Marker»*. 

The concept of semantic roles is a model that unites similar participants of different situation on the basis of similarity of their meaning and their morphosyntactic behaviour. 

**The aim of the term paper** is to prove that the classification models can classify semantic roles expressed by the same marker on the example of the Dative case marker in Russian. Our hypothesis is that the predictions of the models will be based on the semantic features, and the confusion matrix will reflect the structure of the category of the Dative case in Russian. 

### Theoretical background: semantic roles expressed by the Dative case in Russian

* Recipient (Я дала мальчику книгу)
* Beneficiary (Я купила сыну игрушку)
* Experiencer (Это кажется мне странным)
* External Possessor (Она подстригла мне ногти)
* Counteragent (Сотрудник подчинился начальству)
* Direction (Он поехал к бабушке)

### Data

The data was originally taken from [pre-processed FrameBank](http://nlp.isa.ru/framebank_parser/data/annotated_corpus_fixed+syntaxnet.json) and then manually annotated. The manually annotated dataset is available [here](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/data/data_from_framebank.csv). The features were extracted automatically, see the code for [feature extraction](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/notebooks/preprocessing.ipynb).  The labeled dataset is available [here](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/data/annotated_data.csv). The distribution by class is presented in the table:

| Semantic role | The number of examples |
| ------------- | :--------------------: |
| Beneficiary   |           225          |                      
| Counteragent  |           275          |   
| Direction     |           396          | 
| External Possessor |      125          |
| Recipient     |           400          |
| Experiencer   |           282          |

Apart from that, FrameBank contains many unlabeled examples. 94864 examples with a Dative marker were taken from FrameBank, the unlabeled dataset can be found [here](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/data/unannotated_data.csv)

The features were extracted from [RNC](https://ruscorpora.ru/new/) morphological and semantic annotation and [SyntaxNet](https://ai.googleblog.com/2016/05/announcing-syntaxnet-worlds-most.html) syntactic annotation presented in FrameBank.

Все примеры были автоматически размечены по следующим признакам:
\begin{itemize}
\item \textit{синтаксические}: наличие или отсутствие прямого объекта, наличие или отсутствие предлога, управляющего местоимением в дативе;

\item \textit{морфологические}: часть речи, одушевленность, число для объекта в дативе и субъекта, часть речи для предиката и, если это глагол, наклонение и вид глагола;

\item \textit{семантические}: леммы предиката, непрямого объекта и субъекта, а также их семантические характеристики, взятые из Национального корпуса русского языка: для непрямого объекта и субъекта - разряд существительного (предметное, непредметное и имя собственное), таксономический класс существительного, топологический класс, мереологический класс, коннотация и словообразовательная структура (например, диминутивы, аугментивы), для предиката - семантический класс глагола, каузативный / некаузативный глагол, служебный ли глагол, словообразовательная структура.
\end{itemize}

## Results of the experiments

In this research both [supervised](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/notebooks/supervised_methods.ipynb) and [semi-supervised](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/notebooks/semi_supervised_methods.ipynb) methods are used. 

The results of supervised methods models are presented in the table:

| Model | Weighted F1-Score |
| ------------- | :----------------------: |
| [Random Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)  |  0.71 |    
| [XGBoost](https://xgboost.readthedocs.io/en/latest/python/python_api.html)  |  0.72 |      
| [Logistic regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) | 0.73 |      
| FNN | 0.75 |  

For training models based on semi-supervised methods the lemmas were excluded from the dataset. [Self-training](https://scikit-learn.org/stable/modules/semi_supervised.html#self-training) algorithm was used with two base estimators: Logistic Regression and XGBoost. The comparison of models based on the supervised methods and trained with lemmas and models based on semi-supervised methods and trained without lemmas is illustrated with the table, the metrics is weigthed F1-score:

| Model | Supervised methods | Semi-supervised methods |
| ------------- | :----------------------: | :----------------------: |
| XGboost |  0.72 |      0.6  |  
| Logistic regression | 0.73 |     0.6   |  

The results for each semantic role can be found in the following table, the metrics is F1-score:

| Semantic Role | XGBoost | Random Forest | LogReg | FNN  | Average  |
| ------------- | :---: | :---: | :---: | :---: | :---: | 
| Beneficiary   |          0.6         |        0.68   | 0.63  | 0.65   | *0.64*  |               
| Counteragent  |           0.76        |  0.78   | 0.81  | 0.84  | *0.8*   |  
| Direction   | 0.82   | 0.82    | 0.85 | 0.87  | *0.84*  | 
| External Possessor  | 0.69   | 0.54  | 0.65  | 0.71  | *0.65*   | 
| Recipient   | 0.71   | 0.69  | 0.67  | 0.65   | *0.68*   | 
| Experiencer   | 0.7   |  0.65   | 0.71   | 0.73   | *0.7*   | 



### Discussion 

For further discussion see [the handout](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/Voloshina_handout.pdf).


 ### Usage 

To run the notebook with the preprocessing code, please download this [file](http://nlp.isa.ru/framebank_parser/data/annotated_corpus_fixed+syntaxnet.json) and clone this repository:

´´´{python}
git clone https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles
´´´

The notebooks with experiments can be opened in [Google Colaboratory](https://colab.research.google.com/?utm_source=scs-index). Be aware that you need GPU to run the notebook with supervised methods models.



