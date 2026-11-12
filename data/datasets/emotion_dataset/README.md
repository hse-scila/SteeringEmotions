# GoEmotions — датасет эмоциональной разметки текстов

> **Источник:** <https://huggingface.co/datasets/google-research-datasets/go_emotions>
> Google Research, 2020. Статья: *GoEmotions: A Dataset of Fine-Grained Emotions* — V. Bazarevsky et al., arXiv:2005.00554

## Что это

**GoEmotions** — корпус из **58 009 комментариев Reddit** (английский язык), размеченных вручную на
**27 категорий эмоций + нейтральный класс**. Крупнейший публичный датасет классификации эмоций
мелкой гранулярности (fine-grained emotion classification).

Классы (27 эмоций): `admiration, amusement, anger, annoyance, approval, caring, confusion, curiosity,
desire, disappointment, disapproval, disgust, embarrassment, excitement, fear, gratitude, grief, joy,
love, nervousness, optimism, pride, realization, relief, remorse, sadness, surprise` + `neutral`.

## Структура каталога (проверено 01.09.2026)

```
E:\emotion_dataset\
├── README.md
├── raw\                          # конфиг RAW: multi-label, полный корпус наблюдений
│   └── train.parquet             # 211 225 строк, 37 колонок (text + метаданные + 28 бинарных меток)
└── simplified\                   # конфиг SIMPLIFIED: метки списком (обычно одна главная эмоция)
    ├── train.parquet             #  43 410 строк  (text, labels[], id)
    ├── validation.parquet        #   5 426 строк
    └── test.parquet              #   5 427 строк
```

- **raw** — полный корпус (211 225 наблюдений разметчиков): `text`, метаданные Reddit (`author`, `subreddit`,
  `created_utc`, ...), флаг `example_very_unclear` и 28 колонок меток: 27 эмоций + `neutral` (1/0).
- **simplified** — упрощённая версия: колонка `labels` — список кодов эмоций (обычно один код; код 27 = neutral).
  Готовые сплиты train/validation/test.

## Быстрый старт (Python)

```python
import pandas as pd

# simplified: колонка labels - список кодов эмоций (код 27 = neutral)
df = pd.read_parquet(r"E:\emotion_dataset\simplified\train.parquet")
print(df.shape)   # (43410, 3): text, labels, id

# распределение меток:
from collections import Counter
c = pd.Series([l for row in df["labels"] for l in row]).value_counts()
print(c.head(10))

# raw: 28 бинарных колонок-меток по именам эмоций
raw = pd.read_parquet(r"E:\emotion_dataset\raw\train.parquet")
label_cols = [c for c in raw.columns if c not in ("text","id","author","subreddit","link_id","parent_id","created_utc","rater_id","example_very_unclear")]
print(raw[label_cols].sum().sort_values(ascending=False).head(10))
```

## Цитирование

```bibtex
@article{bazarevsky2020goemotions,
  title={GoEmotions: A Dataset of Fine-Grained Emotions},
  author={Bazarevsky, Valentin and Kolesnikov, Grigoriy and ... },
  journal={arXiv preprint arXiv:2005.00554},
  year={2020}
}
```

## Лицензия

CC BY-SA 4.0 (контент Reddit — по пользовательскому соглашению Reddit).

## Скачано

- Дата: 01.09.2026
- Источник: huggingface.co (parquet-эндпоинт датасета)
- Назначение: `E:\emotion_dataset\`