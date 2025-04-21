# Arctic Text Classifier

## 📁 Структура проекта

- `text_classifier_library.ipynb` — библиотека для классификации с использованием предобученных моделей Decision Tree, Zero-Shot, SVM.
	- `classify_text_single_theme` — функция классификации по одной теме.
	- `classify_text` — функция классификации по всем темам.
- `classification_models.ipynb` — файл с обучением моделей для библиотеки; классификация методами: Decision Tree, Zero-Shot (Одноклассовая, Одноклассовая оптимальная, Многоклассовая), Метод опорных векторов (SVM), Naive Bayes, Logistic Regression; проверка устойчивости методов
- `models/` — предобученные модели Decision Tree и SVM и векторизаторы для соответствующих моделей.
- `metrics_results/` — сохранённые метрики и результаты проверки устойчивости для всех методов из файла `classification_models.ipynb`.
- `library_results/` — сохранённые метрики и логи работы библиотеки `text_classifier_library.ipynb`.

---

## Описание проекта

Библиотека для интеллектуальной классификации текстов по тематикам, связанным с Арктикой, на основе голосования между тремя методами: **Decision Tree**, **SVM** и **Zero-Shot классификацией**.

Цель проекта — классификация текстов, связанных с арктической тематикой, с применением различных методов машинного обучения. Это может быть полезно для анализа новостей, медиа, научных публикаций, политических документов и других текстов, связанных с регионом Арктики.

### Возможности библиотеки:

1. Классификация текста по одной выбранной теме.
2. Классификация текста сразу по всем темам.
3. Использование голосования между тремя различными методами (Decision Tree, SVM, Zero-Shot). Если хотя бы 2 из 3 методов отнесли текст к тематке, текст считается отнесенным к ней.
4. Поддержка краткого и расширенного вывода результатов классификации.
5. Список тем задан заранее и фиксирован.

### Поддерживаемые темы (`THEMES`):

1. Arctic Indigenous Peoples  
2. Arctic Militarization  
3. Arctic Resource Extraction  
4. Arctic Shipping  
5. International Governance of Arctic Regions  
6. Non-Arctic Countries  
7. Permafrost  
8. Science & Diplomacy  
9. Svalbard  

## Описание функций

### `classify_text_single_theme(text: str, theme_index: int, full_output: bool = False)`

Классифицирует **один текст по одной выбранной теме**.

#### Аргументы:
- `text` *(str)* — текст для анализа.
- `theme_index` *(int)* — порядковый номер темы из списка `THEMES` (нумерация начинается с **1**).
- `full_output` *(bool, по умолчанию=False)* — если `True`, возвращает результат каждого метода и финального голосования; если `False`, возвращает только краткий ответ — `True` или `False` — принадлежит ли текст теме.

#### Возвращаемое значение:
- При `full_output=False`: `bool` True или False — принадлежит ли текст теме 
- При `full_output=True`: `dict` следующего формата:
  - `'theme'`: `str` название темы
  - `'Decision Tree'`: `bool` True или False — принадлежит ли текст теме, согласно решению метода Decision Tree
  - `'SVM'`: `bool` True или False — принадлежит ли текст теме, согласно решению метода  SVM
  - `'Zero-Shot'`: `bool` True или False — принадлежит ли текст теме, согласно решению Zero-Shot метода
  - `'Final'`: `bool` True или False — принадлежит ли текст теме, согласно решению голосования (2 из 3)

---

### `classify_text(text: str, full_output: bool = False)`

Классифицирует текст по **всем темам** из списка `THEMES`.

#### Аргументы:
- `text` *(str)* — текст для анализа.
- `full_output` *(bool, по умолчанию=False)* — если `True`, возвращает результаты всех методов и итоговое голосование по темам.

#### Возвращаемое значение:
- При `full_output=False`: список тем, к которым относится текст
- При `full_output=True`: `dict` с ключами:
  - `'Decision Tree'`: список тем, определённых методом Decision Tree
  - `'SVM'`: список тем, определённых методом SVM
  - `'Zero-Shot'`: список тем, определённых Zero-Shot методом
  - `'Final'`: список тем, полученных в результате голосования (2 из 3)

---

## 💡 Примеры использования

### Пример 1: Классификация по одной теме (краткий вывод)

Задача — определить, относится ли текст к теме **Permafrost** (номер 7 в списке тем):

theme_index = 7
result = classify_text_single_theme(text, theme_index)
print("Итоговая классификация:", result)

**Пример вывода:**

Итоговая классификация: True

---

### Пример 2: Классификация по одной теме (подробный вывод)

С тем же текстом, но с подробной информацией:

theme_index = 7
detailed_result = classify_text_single_theme(text, theme_index, full_output=True)

print("\nПодробно:")
for k, v in detailed_result.items():
    print(f"{k}: {v}")

**Пример вывода:**

Подробно:
theme: Permafrost
Decision Tree: True
SVM: True
Zero-Shot: True
Final: True

---

### Пример 3: Классификация по всем темам (краткий вывод)

predicted_themes = classify_text(text)
print("Темы:", predicted_themes)

**Пример вывода:**

Темы: ['Permafrost']

---

### Пример 4: Классификация по всем темам (подробный вывод)

detailed = classify_text(text, full_output=True)

print("--- Текст #1 ---")
print("Предсказание:")
print(f"Decision Tree: {detailed['Decision Tree']} | SVM: {detailed['SVM']} | Zero-Shot: {detailed['Zero-Shot']}")
print("Final:", detailed['Final'])

**Пример вывода:**

--- Текст #1 ---
Предсказание:
Decision Tree: ['Permafrost'] | SVM: ['Permafrost'] | Zero-Shot: ['International Governance of Arctic Regions']
Final: ['Permafrost']

---
