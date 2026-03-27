# Data Science — Яндекс.Практикум (2020)

Учебные проекты курса Data Science. 15 проектов — от предобработки данных до компьютерного зрения. Курс пройден в 2020 году, проекты сохранены в оригинальном виде.

> **Актуальные проекты с современным стеком** → **[ds-projects](https://github.com/vovlov/ds-projects)**
>
> Пять production-ready ML-систем (MLOps, RAG, NER, GNN, Real-time Anomaly Detection), которые выросли из фундамента, заложенного в этом курсе. Подробнее о пути от учебных ноутбуков к production: [EVOLUTION.md](https://github.com/vovlov/ds-projects/blob/main/docs/EVOLUTION.md)

## Проекты

### Фундамент: данные и аналитика

| # | Проект | Задача | Стек | Эволюция |
|---|--------|--------|------|----------|
| 2 | [Предобработка данных](project_2_Data_pre-processing) | Кредитный скоринг: влияние семейного положения на погашение кредита | pandas | — |
| 3 | [Исследовательский анализ данных](project_3_Exploratory_data_analysis) | Анализ рынка недвижимости Санкт-Петербурга, выявление аномалий | pandas, matplotlib, seaborn | — |
| 4 | [Статистический анализ данных](project_4_Statistical_data_analysis) | Сравнение тарифов телеком-оператора, проверка гипотез | pandas, scipy | — |
| 5 | [Сборный проект 1](project_5_Modular_project-1) | Анализ рынка видеоигр, прогноз успешности | pandas, sklearn | — |

### Классический ML

| # | Проект | Задача | Стек | Эволюция |
|---|--------|--------|------|----------|
| 6 | [Введение в ML](project_6_Intro_ML) | Рекомендация тарифов — классификация (accuracy 0.81) | sklearn | — |
| 7 | [Обучение с учителем](project_7_Training_teacher) | Прогноз оттока клиентов банка (F1 ≥ 0.59) | sklearn | → [01-customer-churn-mlops](https://github.com/vovlov/ds-projects/tree/main/01-customer-churn-mlops) |
| 8 | [ML в бизнесе](project_8_ML_in_business) | Выбор локации для бурения скважин, Bootstrap-анализ рисков | sklearn, numpy | → [04-graph-fraud-detection](https://github.com/vovlov/ds-projects/tree/main/04-graph-fraud-detection) |
| 9 | [Сборный проект 2](project_9_Modular_project-2) | Предсказание коэффициента извлечения золота из руды | pandas, matplotlib, sklearn | — |

### Продвинутый ML

| # | Проект | Задача | Стек | Эволюция |
|---|--------|--------|------|----------|
| 10 | [Линейная алгебра](project_10_Linear_algebra) | Защита данных матричными преобразованиями | numpy, sklearn | — |
| 11 | [Численные методы](project_11_Numerical_method) | Оценка стоимости автомобилей, градиентный бустинг | sklearn | — |
| 12 | [Временные ряды](project_12_Time_series) | Прогноз заказов такси (RMSE ≤ 48) | sklearn, statsmodels | → [05-realtime-anomaly](https://github.com/vovlov/ds-projects/tree/main/05-realtime-anomaly) |
| 13 | [ML для текстов](project_13_ML_for_text) | Классификация токсичных комментариев (F1 ≥ 0.75) | nltk, TF-IDF, BERT | → [02-rag-enterprise](https://github.com/vovlov/ds-projects/tree/main/02-rag-enterprise), [03-ner-service](https://github.com/vovlov/ds-projects/tree/main/03-ner-service) |

### Специализация

| # | Проект | Задача | Стек | Эволюция |
|---|--------|--------|------|----------|
| 14 | [Извлечение данных](project_14_Data_extraction) | Анализ авиаперевозок — SQL + PySpark + Python | SQL, PySpark, pandas | — |
| 15 | [Компьютерное зрение](project_15_Computer_vision) | Определение возраста по фото (CNN) | TensorFlow, Keras | — |
| 17 | [Дипломный проект](project_17_Graduation%20project) | Прогнозирование оттока клиентов телеком-оператора | pandas, sklearn | → [01-customer-churn-mlops](https://github.com/vovlov/ds-projects/tree/main/01-customer-churn-mlops) |

## Технологии (2020)

| Категория | Инструменты |
|-----------|------------|
| Данные | pandas, numpy |
| Визуализация | matplotlib, seaborn |
| ML | scikit-learn |
| Статистика | scipy, statsmodels |
| NLP | nltk, TF-IDF, BERT |
| Deep Learning | TensorFlow, Keras |
| Big Data | PySpark |
| SQL | PostgreSQL |
