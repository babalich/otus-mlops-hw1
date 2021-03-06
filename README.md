# OTUS MLOps
### Домашнее задание 1.

#### 1. Опишите в Readme.md цели проекта и его метрики. Как они связаны между собой.

Цель проекта - с помощью машинного обучения свести к минимуму количество мошеннических операций с банковскими картами. При этом  нужно учитывать, что предиктивная модель является составной частью общей системы детекции мошенничества.

Для выбора метрики договоримся о следующих обозначения:
* мошенническая транзакция - класс 1
* нормальная транзакция - класс 0 

а так же типы ошибок:
* TP - модель верно определила мошенническую транзакцию
* FN - модель пропустила мошенническую транзакцию
* TN - модель верно определила нормальную транзакцию
* FP - модель заблокировала нормальную транзакцию 
  
В качестве основной метрики будем максимизировать <b>Recall</b>. К данном случае это можно интерпретировать как способность модели найти все подозрительные транзакции. Второй выбранной метрикой будет <b>Precision</b>, для контроля того, что наша модель все-таки раз умеет разделять классы. Не забываем, что наша ML модель работает в паре с созданными экспертами правилами безопасности, и её задача выявлять те паттерны, которые не уловили hand-crafted признаки.
  
Следующий вопрос - ранжирование (приоритизация) исследования транзакций, которые наша модель определила как мошенническии, а так же способы взаимодействия с клиентом. Например, блокировка крупной транзакции и высокой вероятностью мошенничества позволит предотвратить большие убытки <b>(score * transaction_amount)</b>. В то время как клиент с множеством небольших подозрительных операций в течении короткого промежутка времени может быть просто предупрежден об этом автоматически.

#### 2. Проведите декомпозицию проекта и сформируйте набор задач по типам (инфраструктура, подготовка данных, моделирование, развертывание и проч.).
![Схема](https://raw.githubusercontent.com/babalich/otus-mlops-hw1/main/docs/images/scheme.png)
<b> Хранилища данных </b>
* к локальному хранилищу данных повышенные требования безопасности, поскольку нам нужно хранить персональные данные
* хранилище данных для обучения модели используем в облаке, поскольку там будет храниться не сырая информация, я предобработанные и сгенерированные признаки, следовательно объем будет больше локального. Мы можем переобучать модель по расписанию или при необходимости (если модель стала работать хуже, для этого на схеме есть этап мониторинга), поэтому высоких требований к быстродействию здесь нет.
* часть признаков необходимо обрабатывать и присоединять к текущей транзакции "на лету", поскольку инференс модели должен быть максимально быстрым. Для таких признаков нужно подобрать хранилище с максимальным быстродействием.

<b> Препроцессинг данных</b>
* для текущей транзакции необходимо произвести такую же обработку признаков, как и при обучении модели. Поэтому мы имеет избыточный блок, которых сможет делать это максимально быстро online

<b> Мониторинг и оценка моделей</b>
* оценивает, насколько хорошо работает наша модель как на валидационной выборке при обучении, так и в живых условиях
* проверяет, изменилось ли распределение в исторических и текущих данных, уведомляет о необходимости переобучить модель


  



  
