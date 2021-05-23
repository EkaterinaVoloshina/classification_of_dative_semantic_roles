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

The data was originally taken from [pre-processed FrameBank](http://nlp.isa.ru/framebank_parser/data/annotated_corpus_fixed+syntaxnet.json) and then manually annotated. The manually annotated dataset is available [here](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/data/data_from_framebank.csv). The features were extracted automatically, see the code for [feature extraction](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/notebooks/preprocessing.ipynb). The distribution by class is presented in the table:

| Semantic role | The number of examples |
| ------------- | ---------------------- |
| Beneficiary   |           225          |                      
| Counteragent  |           275          |   
| Direction     |           396          | 
| External Possessor |      125          |
| Recipient     |           400          |
| Experiencer   |           282          |


Кроме этого, FrameBank содержит много примеров, неразмеченных по семантическим ролям, но имеющим синтаксическую и морфологическую разметку. Из них мы отобрали 94864 примеров с существительным или местоимением в дативе. 

Все примеры были автоматически размечены по следующим признакам:
\begin{itemize}
\item \textit{синтаксические}: наличие или отсутствие прямого объекта, наличие или отсутствие предлога, управляющего местоимением в дативе;

\item \textit{морфологические}: часть речи, одушевленность, число для объекта в дативе и субъекта, часть речи для предиката и, если это глагол, наклонение и вид глагола;

\item \textit{семантические}: леммы предиката, непрямого объекта и субъекта, а также их семантические характеристики, взятые из Национального корпуса русского языка: для непрямого объекта и субъекта - разряд существительного (предметное, непредметное и имя собственное), таксономический класс существительного, топологический класс, мереологический класс, коннотация и словообразовательная структура (например, диминутивы, аугментивы), для предиката - семантический класс глагола, каузативный / некаузативный глагол, служебный ли глагол, словообразовательная структура.
\end{itemize}

\section{Эксперименты}

В исследовании использованы два типа методов: обучение с учителем и обучение с частичным привлечением учителя. Основное отличие между двумя подходами заключается в том, что для обучения с учителем используются только размеченные данные, в то время как методы обучения с частичным привлечением учителя используют небольшую выборку размеченных данных, и в процес


| Semantic role | The number of examples |
| ------------- | ---------------------- |
| Beneficiary   |           225          |                      
| Counteragent  |           275          |   
| Direction
| External Possessor
| Recipient
| Experiencer

* **annotated_data.csv**: the dataset with semantic roles labels and features extracted from [RNC](https://ruscorpora.ru/new/) morphological and semantic annotation and [SyntaxNet](https://ai.googleblog.com/2016/05/announcing-syntaxnet-worlds-most.html) syntactic annotation
* **data_from_framebank.csv**: the dataset with the examples of the Dative case from FrameBank before feature extraction
* **unannotated_data.csv**: the dataset without semantic roles but with features extracted from RNC morphological and semantic annotation and SyntaxNet syntactic annotation





\section{Данные}

се обучения модели размечают неаннотированные данные и в последующие итерации обучаются на них. Результаты моделей обучения с учителем представлены в таблице:

\begin{table}[h]
\centering
\begin{tabular}{@{}lc@{}}
\toprule
\multicolumn{1}{c}{\textbf{Модель}} & \textbf{F1-Score (weighted)} \\ \midrule
Random Forest                         & \textit{0.71}  \\   
Gradient Boosting                     & \textit{0.72}  \\       
Логистическая регрессия               & \textit{0.73}    \\     
Полносвязная нейронная сеть           & \textit{\textbf{0.75}}       \\ \bottomrule
\end{tabular}
\caption{Сравнение результатов моделей обучения с учителем
}
\label{tab:my-table}
\end{table}

В качестве метода обучения с частичным привлечением учителя в этой работе использовался метод Self-training. Для экспериментов с методами обучения с частичным привлечением учителя мы использовали датасет без признаков лемм непрямого объекта, глагола и субъекта, так как размеченных данных гораздо больше, чем неразмеченных, и эти признаки могут только испортить качество моделей: большая часть предикатов из неразмеченных данных не будет содержаться в размеченном датасете. 

\begin{table}[h]
\centering
\begin{tabular}{@{}lcc@{}}
\toprule
\multicolumn{1}{c}{\textbf{Алгоритм}} & \textbf{С леммами} & \textbf{Без лемм} \\
\midrule
Логистическая регрессия               & \textit{0.73}      & \textit{0.6}      \\
XGBoost                               & \textit{0.72}      & \textit{0.6}   
      \\ \bottomrule
 
 
 ### Usage 



