## Semantic Role Labeling for Forms with a Dative Marker 

This reprository contains the code and the data, which are described in the term paper for [HSE University](https://www.hse.ru/ba/ling) *«Semantic Role Labeling for Forms with a Dative Marker»*. 

The concept of semantic roles is a model that unites similar participants of different situation on the basis of similarity of their meaning and their morphosyntactic behaviour. 

**The aim of the term paper** is to prove that the classification models can classify semantic roles expressed by the same marker on the example of the Dative case marker in Russian. Our hypothesis is that the predictions of the models will be based on the semantic features, and the confusion matrix will reflect the structure of the category of the Dative case in Russian. 

### Theoratical background: semantic roles expressed by the Dative case in Russian

* Recipient (Я дала мальчику книгу)
* Beneficiary (Я купила сыну игрушку)
* Experiencer (Это кажется мне странным)
* External Possessor (Она подстригла мне ногти)
* Counteragent (Сотрудник подчинился начальству)
* Direction (Он поехал к бабушке)

### Data

The data was originally taken from [pre-processed FrameBank](http://nlp.isa.ru/framebank_parser/data/annotated_corpus_fixed+syntaxnet.json) and then manually annotated. The manually annotated dataset is available [here](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/data/data_from_framebank.csv). The features were extracted automatically, see the code for [feature extraction](https://github.com/EkaterinaVoloshina/classification_of_dative_semantic_roles/blob/main/notebooks/preprocessing.ipynb). 

* **annotated_data.csv**: the dataset with semantic roles labels and features extracted from [RNC](https://ruscorpora.ru/new/) morphological and semantic annotation and [SyntaxNet](https://ai.googleblog.com/2016/05/announcing-syntaxnet-worlds-most.html) syntactic annotation
* **data_from_framebank.csv**: the dataset with the examples of the Dative case from FrameBank before feature extraction
* **unannotated_data.csv**: the dataset without semantic roles but with features extracted from RNC morphological and semantic annotation and SyntaxNet syntactic annotation


### Usage 


\section{Данные}

Данные взяты из оффлайн-версии FrameBank. В FrameBank используется 91 семантическая роль (\cite{lyashevskaya2015framebank}). Мы вручную объединили и переразметили роли, следуя описанной выше классификации. В нашей выборке составило 1703 примера. 

\begin{table}[h]
\centering{%
\begin{tabular}{@{}lc@{}}
\toprule
\multicolumn{1}{c}{\textbf{Семантическая роль}} & \textbf{Количество примеров} \\ \midrule
Бенефактив                                      & 225                          \\
Контрагент                                      & 275                          \\
Направление                                     & 396                          \\
Поднятый Посессор                               & 125                          \\
Реципиент                                       & 400                          \\
Экспериенцер                                    & 282                          \\ \bottomrule
\end{tabular}
\caption{Распределение данных по классам
}
%
}
\end{table}

Кроме этого, FrameBank содержит много примеров, неразмеченных по семантическим ролям, но имеющим синтаксическую и морфологическую разметку. Из них мы отобрали 94864 примеров с существительным или местоимением в дативе. 

Все примеры были автоматически размечены по следующим признакам:
\begin{itemize}
\item \textit{синтаксические}: наличие или отсутствие прямого объекта, наличие или отсутствие предлога, управляющего местоимением в дативе;

\item \textit{морфологические}: часть речи, одушевленность, число для объекта в дативе и субъекта, часть речи для предиката и, если это глагол, наклонение и вид глагола;

\item \textit{семантические}: леммы предиката, непрямого объекта и субъекта, а также их семантические характеристики, взятые из Национального корпуса русского языка: для непрямого объекта и субъекта - разряд существительного (предметное, непредметное и имя собственное), таксономический класс существительного, топологический класс, мереологический класс, коннотация и словообразовательная структура (например, диминутивы, аугментивы), для предиката - семантический класс глагола, каузативный / некаузативный глагол, служебный ли глагол, словообразовательная структура.
\end{itemize}

\section{Эксперименты}

В исследовании использованы два типа методов: обучение с учителем и обучение с частичным привлечением учителя. Основное отличие между двумя подходами заключается в том, что для обучения с учителем используются только размеченные данные, в то время как методы обучения с частичным привлечением учителя используют небольшую выборку размеченных данных, и в процессе обучения модели размечают неаннотированные данные и в последующие итерации обучаются на них. Результаты моделей обучения с учителем представлены в таблице:

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
\end{tabular}
\caption{ Сравнение результатов моделей обучения с частичным привлечением учителя}
\label{tab:my-table}
\end{table}

Обучение с частичным привлечением учителя не дает улучшения качества, так как модели не выучивают ничего нового из данных, аннотированных на основании уже имеющейся разметки. 

\section{Обсуждение}
\begin{itemize}
\item \textit{Качество классификации}: лучшая модель в нашем исследовании показала качество 0.744 (micro F1-Score) и 0.742 (macro F1-Score). В исследовании (\cite{larionov2019semantic}), где был разработан первый пайплайн для русского языка, 
модель, основанная на признаках, показала качество 0.769  (Micro F1) и 0.736 (macro F1-Score).
\item \textit{Важность признаков}: для логистической регрессии важными оказываются леммы глаголов, а также семантический класс глагола и леммы объекта в дательном падеже, для модели XGBoost - семантический класс предиката, словообразовательная структура субъекта и семантический класс непрямого объекта. 
\item \textit{Качество классификации по отдельным ролям}:
Самое низкое качество классификаторы показывали на примерах с ролью Бенефактива. В отличие от других семантических ролей, предикаты, которые вводят роли Бенефактива, не составляют естественного класса и имеют тенденцию к коэрции: 

    (1) \textit{Среди ночи она жарила \textbf{ему} яичницу на электроплитке.}
    
    
Роли Реципиента и Экспериенцера вводятся глаголами из нескольких семантических классов (см. (\cite{janda2002case}): обе роли имеют три подкатегории), что влияет на качество модели.

Сочетаемость с большим количеством предикатов, не ограниченным одним семантическим классом, - одна из характерных черт прототипа радиальной категории. Таким образом, Реципиент, Экспериенцер и Бенефактив являются одними из центральных значений категории датива.

\begin{table}[h]
\centering
\begin{tabular}{@{}llllll@{}}
\toprule
\textbf{Семантическая роль} &
  \textbf{XBoost} &
  \textbf{RF} &
  \textbf{LogReg} &
  \textbf{FNN} &
  \textbf{Average} \\ \midrule
Бенефактив &
  \textit{0.6} &
  \textit{0.68} &
  \textit{0.63} &
  \textit{0.65} &
  0.64 \\
Контрагент        & \textit{0.76} & \textit{0.78} & \textit{0.81} & \textit{0.84} & 0.8  \\
Направление &
  \textit{\textbf{0.82}} &
  \textit{\textbf{0.82}} &
  \textit{\textbf{0.85}} &
  \textit{\textbf{0.87}} &
  \textbf{0.84} \\
Поднятый посессор & \textit{0.69} & \textit{0.54} & \textit{0.65} & \textit{0.71} & 0.65 \\
Реципиент         & \textit{0.71} & \textit{0.69} & \textit{0.67} & \textit{0.65} & 0.68 \\
Экспериенцер      & \textit{0.7}  & \textit{0.65} & \textit{0.71} & \textit{0.73} & 0.7 \\ \bottomrule
\end{tabular}
\caption{Сравнение F1-Score разных классификаторов по семантическим ролям}
\label{tab:my-table}
\end{table}

\item \textit{Ошибки классификаторов}: модели часто присваивают метку \textbf{Реципиента}, что соответствует представлению о Реципиенте как о прототипе дательного падежа, который служит источником для других ролей. Семантический переход от одной роли к другой основывается на общих чертах двух ролей, поэтому роль Реципиента имеет больше всего общих черт с другими ролями.

Модели совершают ошибки при различении Реципиента и \textbf{Бенефактива}. Как показывают данные диахронии   (\cite{haspelmath2003geometry}), роль Бенефактива часто служит источником для развития других семантических ролей, в том числе и для Реципиента. Примеры с Бенефактивом могут определяться как Экспериенцер: эти роли оказываются близкими в русском языке (\cite{janda2002case}).

Некоторые примеры из классов Реципиента и Экспериенцера получают метку \textbf{Направления}. Направление в русском языке является периферийным значением для дательного падежа, так как он не может выражаться дательным падежом без предлога, однако Направление является единственной семантической ролью, имеющей пространственное значение, что является одним из признаков прототипического значения. Типологически Направление является источником для диахронического перехода к другим семантическим ролям (\cite{haspelmath2003geometry}). 



