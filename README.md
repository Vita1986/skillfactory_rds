#Цель проекта
В датасете содержится информация о 395 учениках. 
В первых 3-х столбцах (shool, sex, age) нет пропусков, остальные имеют в значениях пропуски от 1 до 45. 

Данные представлены в 30 столбцах, из них 17 столбцов имеют строковый тип, остальные 13 - числовой тип.

Также в числовые столбцы попали упорядоченные категориальные переменные, у которых значения закодированы числовым диапазоном.

Поэтому по факту у нас есть только 3 истинно количественных переменных: 
age (непрерывная количиственная), absences и score (дискретные количественные переменные), при этом score - целевая переменная.

Так как в некоторых столбцах некорректное написание заголовков, приведём названия к нормальному виду.

Промежуточный выводы:
Данные достаточно чистые, однако есть некоторые проблемы, а именно:

в некоторых переменных (higher, Pstatus, school, famrel) наблюдается дисбаланс классов; на основе age можно создать доп. переменную, объединяющую редких великовозрастных школьников; на основе признаков поддержки (schoolsup, famsup) можно создать признак наличия поддержки в общем виде; в переменных Fedu, famrel обнаружены ошибки: значения недопустимы по условию задания; в переменной Absences, "studytime,granular" в наличии выбросы; целевая переменная score содержит 1,5%: пропусков; распределение неописанной в задании studytime, granular напоминает распределение studytime.

Обращаем внимание:¶
более 30 учеников, получивших 0 баллов
'яма' в диапазоне от 0 до 20 после чего начинается нормальное распределение
выбросов и ошибок нет
Провал, на наш взгляд, можно объяснить только наличием "проходного балла", т.е., если школьник не набрал "пороговое" значение 20, то ему проставлялся 0. Вообще, большое количество 0 оценок выглядит подозрительно. Но на этапе разведывательного анализа считаем, что правильнее эти значения оставить, и посмотреть сможет ли будущая модель предсказывать склонных к провалу экзамену учеников.

Корреляционный анализ количественных признаков. Выясним какие столбцы коррелируют с оценкой на госэкзамене по математике. Это поможет понять какие признаки стоит оставить для модели, а какие нужно будет исключить из анализа

по количественным переменным

Из  таблицы делаем вывод, что больше всего в обучении мешают:

количество внеучебных неудач
Юный возраст
Проведение времени с друзьями
А позитивно на результатах сказывается:

Образование родителей
Самостоятельное обучение.
Менее значительно, позитивно влияют:

семейные отношения
Как ни странно, количество прогулов и свободного времени после занятий не оказывает заметного влияния на результаты экзамена.

В целях очистки датасета, удалим стобцы с значением корелляции ниже 0.1, однако для будущей модели, возможно, их тоже можно было бы принять к рассмотрению, если останется время на эксперименты.

Промежуточный вывод: После осмотра boxsplot перспективными для моделирования предикторами представляются:

schoolsup Fedu P_edu failures Mjob Medu is_dad_teacher higher age_cat goout school address studytime

t-test номинативных и смешанных переменных С помощью теста Стьюдента проверим есть ли статистическая разница в распределении оценок по номинативным признакам, проверив нулевую гипотезу о том, что распределение оценок по госэкзамену в зависимости от уровней категорий неразличимы

Итоговый вывод.¶
В результате проведенного EDA можно сделать следующие заключения относильно датасета:

Данные достаточно чистые:
количество пропущенных значений варьируется от 1% до 11%. Есть три переменные, в которых данные 100% заполнены;
ошибки обнаружены в переменных Fedu, famrel и заменены на основе некоторых предположений исходя из здравого смысла;
переменная Absences содержала 2 аномальных значения (>200), которые были заменены на медиану с целью сохранения информации, содержащейся в других предикторах.
После подробного осмотра распределений, было решено:
создать признак наличия поддержки в общем виде - is.sup, который оказался незначимым в итоге;
создать создать доп. переменную - age_cat, объединяющую редких великовозрастных школьников в одну группу;
удалить пропуски из целевой переменной за ненадобностью;
оставить 0 значения в score для выяснения возможности моделирования этих случаев.
В результате корреляционного анализа:
обнаружена сильная обратная корреляция между studytume и studytume_granular, поэтому один из них был удален за ненадобностью;
обнаружена линейная зависимость между Fedu, Medu, которая была использована для создания нового значимого признака и взаимного восстановления пропусков;
экспертно исключены пременные с коэффициентом корреляции менее 0.1 по модулю как самые бесперспективные на этапе EDA.
Анализ номинативных и смешанных переменных с помощью boxplot и t-testa позволил выделить следующие значимые признаки:
age_cat, goout, sex, paid, is_dad_teacher, Mjob, failures, Medu, Fedu, address, romantic, schoolsup, school, studytime, higher, P_edu
Итоговый ответ:
Для дальнейшего моделирования рекомендуется использовать параметры: sex, address, Mjob, schoolsup, paid, higher, romantic, age, Medu, Fedu, studytime, failures, goout - как наиболее перспективные.
