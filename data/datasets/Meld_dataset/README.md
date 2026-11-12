# MELD — Multimodal EmotionLines Dataset

> **Источник:** <https://github.com/declare-lab/MELD>
> Статья: *MELD: A Multimodal Multi-Party Dataset for Emotion Recognition in Conversations* — S. Poria et al., ACL 2019, arXiv:1810.02508
> Сайт проекта: <https://affective-meld.github.io/>

## Что это

**MELD** — большой диалоговый датасет для распознавания эмоций в разговоре. Источник — сериал **«Друзья» (Friends)**:
- **1 433 диалога**, **13 708 реплик**, **7 классов эмоций**
- Несколько участников в диалоге (multi-party) — в этом главная сложность датасета
- Модальность: текст / аудио / видео (здесь скачан **текстовый** слой + метаданные таймингов)

Классы эмоций: `anger, disgust, sadness, joy, neutral, surprise, fear`
(сильно несбалансированы — см. распределение ниже). Дополнительно: `Sentiment` — позитив/негатив/нейтрально.

## Структура каталога (проверено 01.09.2026)

```
E:\Meld_dataset\
├── README.md
└── data\MELD\
    ├── train_sent_emo.csv      #  9 989 реплик, 1 038 диалогов
    ├── dev_sent_emo.csv        #  1 109 реплик
    ├── test_sent_emo.csv       #  2 610 реплик
    └── datasets.yaml           # метаданные со ссылками на исходные видео/аудио (MELD.Raw)
```

## Колонки CSV

| Колонка | Смысл |
|---|---|
| `Sr No.` | порядковый номер |
| `Utterance` | текст реплики |
| `Speaker` | персонаж |
| `Emotion` | метка эмоции (7 классов) |
| `Sentiment` | positive / negative / neutral |
| `Dialogue_ID`, `Utterance_ID` | ключи диалога и реплики (связь с видео/аудио) |
| `Season`, `Episode` | сезон/эпизод «Друзей» |
| `StartTime`, `EndTime` | тайминги реплики в видео |

## Распределение (train, 9 989 реплик)

| Emotion | Кол-во | | Sentiment | Кол-во |
|---|---|---|---|---|
| neutral | 4 710 | | neutral | 4 710 |
| joy | 1 743 | | negative | 2 945 |
| surprise | 1 205 | | positive | 2 334 |
| anger | 1 109 | | | |
| sadness | 683 | | | |
| disgust | 271 | | | |
| fear | 268 | | | |

⚠️ Классы несбалансированы (fear/disgust в ~17 раз реже neutral). Для честных экспериментов —
взвешенный loss / oversampling / macro-F1 как метрика.

## Быстрый старт (Python)

```python
import pandas as pd

train = pd.read_csv(r"E:\Meld_dataset\data\MELD\train_sent_emo.csv")
dev   = pd.read_csv(r"E:\Meld_dataset\data\MELD\dev_sent_emo.csv")
test  = pd.read_csv(r"E:\Meld_dataset\data\MELD\test_sent_emo.csv")

print(train["Emotion"].value_counts())
X, y = train["Utterance"], train["Emotion"]

# baseline за 2 минуты:
# from sklearn.feature_extraction.text import TfidfVectorizer
# from sklearn.linear_model import LogisticRegression
# ...
```

## Видео и аудио

В `datasets.yaml` лежат ссылки на исходные медиафайлы (полный raw-пакет — десятки ГБ).
Для текстовых экспериментов они не нужны; при необходимости мультимодальности —
см. репозиторий проекта (declare-lab/fact-augment и официальные зеркала).

## Цитирование

```bibtex
@inproceedings{poria2019meld,
  title={MELD: A Multimodal Multi-Party Dataset for Emotion Recognition in Conversations},
  author={Poria, Soujanya and Hazarika, Devamanyu and Majumder, Navonil and Naik, Gautam and Cambria, Erik and Mihalcea, Rada},
  booktitle={Proceedings of the 57th Annual Meeting of the ACL},
  pages={527--536},
  year={2019}
}
```

## Скачано

- Дата: 01.09.2026, ветка `master` репозитория declare-lab/MELD
- Назначение: `E:\Meld_dataset\`