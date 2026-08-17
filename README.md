# Neural Capacity Threshold (NCT) - "Достатъчно" Study

> **Изследване на семантичния капацитет на невронни модели при различни нива на квантизация и скрит слой**

## 🧠 Overview

Това изследване доказва съществуването на **Neural Capacity Threshold (NCT₉₅)** - минималният невронен капацитет, при който моделът запазва способността си да разграничава полисемични значения.

### Ключови открития:
- **NCT₉₅ = 16 hidden units** за MLP с 5 значения на "достатъчно"
- **SSM ≥ 0.8** е най-надеждният индикатор за NCT (монотонна метрика)
- **Externalization Gain = +100%** под NCT при използване на външна памет/инструменти
- **3 фази**: CPI 0 Колапс, CPI 1 Частична, CPI 2 Стабилна

## 📊 Експерименти

### Експеримент 1: MLP + "достатъчно"
- 5 ортогонални семантични измерения: количество, качество, условие, отрицание, императив
- Capacity sweep: 64, 32, 16, 8, 4 hidden units
- Резултат: 99.5%, 79%, 100%, 78%, 54.5% accuracy
- NCT₉₅ = 16 units

### Експеримент 2: Llama 3.2 + External Memory & Tools
- 3 задачи: Memory (ACME 2014), Computation (387*419), Coordination (САМО)
- Precision: FP16, 8bit, 4bit
- Модели: 1B и 3B
- Резултат: Neural-Only колабира при 8bit/4bit, Externalized дава +100% Gain

### Нови метрики:
- **SLR** (Semantic Load Ratio) - % стабилни класове
- **SSM** (Semantic Stability Margin) - нормализирано минимално разстояние
- **IRI** (Intervention Reliability Index) - % успешни интервенции
- **CPI** (Capacity Phase Index) - фазова класификация 0/1/2
- **E** (Efficiency) = Acc × SSM, средно 0.753 при NCT
- **d** (Effect Size) Cohen's d = 83.65 между фази (огромен ефект)

## 🚀 Quick Start

```bash
pip install -r requirements.txt
python code/nct_experiment_1_mlp.py
python code/nct_metrics_fixed.py
```

## 📁 Структура

```
nct-github/
├── README.md
├── requirements.txt
├── code/
│   ├── nct_experiment_1_mlp.py      # MLP експеримент с "достатъчно"
│   ├── nct_experiment_2_llama.py    # Llama 3.2 бенчмарк
│   ├── nct_metrics_fixed.py         # Фиксирани метрики SLR/SSM/IRI/CPI
│   └── nct_full_benchmark.py        # Комбиниран бенчмарк
├── docs/
│   ├── NCT_Standalone_Offline.html  # Интерактивен доклад v1
│   ├── NCT_Full_v2_with_New_Metrics.html # Доклад v2 с нови метрики
│   └── ...                          # Други HTML доклади
└── assets/
    └── images/                      # Графики от експериментите
```

## 📈 Резултати

### Capacity → Performance
| Capacity | Accuracy | SLR_fixed | SSM   | IRI  | CPI | Фаза |
|----------|----------|-----------|-------|------|-----|------|
| 64 units | 99.5%    | 1.00      | 1.000 | 1.00 | 2   | ✅ Стабилна |
| 16 units | 100%     | 1.00      | 0.823 | 0.25 | 2   | ✅ Стабилна (NCT) |
| 8 units  | 78%      | 1.00      | 0.555 | 0.25 | 1   | ⚠️ Частична |
| 4 units  | 54.5%    | 1.00      | 0.494 | 0.50 | 0   | ❌ Колапс |

### Llama 3.2 - Neural vs Externalized
| Model | Precision | Neural-Only | Externalized | Gain |
|-------|-----------|-------------|--------------|------|
| 1B    | FP16      | 66.7%       | 100%         | +33% |
| 1B    | 8bit      | 0%          | 100%         | **+100%** |
| 3B    | 8bit      | 66.7%       | 100%         | +33% |
| 3B    | 4bit      | 0%          | 100%         | **+100%** |

## 🔬 Формални дефиниции

```
SLR = |{c : min_j≠c ||μ_c - μ_j|| > τ}| / K

SSM = min_{i≠j} ||μ_i - μ_j||_current / min_{i≠j} ||μ_i - μ_j||_baseline

IRI = (1/N) Σ 1[ f(x+α·d_{a→b}) ≠ f(x) ]

CPI = 0 if acc<0.6 & SSM<0.55
      2 if acc≥0.90 & SSM≥0.80
      1 otherwise

E = Acc × SSM
d = (μ_stable - μ_collapse) / σ_pooled
```

## 📚 Документация

Отвори `docs/NCT_Full_v2_with_New_Metrics.html` в браузър за интерактивен доклад с:
- Интервенционни примери
- PCA визуализации
- Фазова диаграма
- Сравнение MLP vs Llama

## 🤝 Citation

```
@misc{nct2026,
  title={Neural Capacity Threshold: Semantic Collapse and Externalization Gain},
  author={Emil Natov},
  year={2026},
  note={Experiments on polysemy of "достатъчно" and Llama 3.2 quantization}
}
```

## 📄 License

MIT
