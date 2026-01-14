# AS-IS Диаграмма процесса запуска ML-задачи

```mermaid
sequenceDiagram
    participant BIZ as "Заказчик (Бизнес)"
    participant PO as Product Owner
    participant DS as Data Scientist
    participant DE as Data Engineer

    Note over BIZ,DE: Этап 1: Хаотичная инициация
    BIZ->>PO: 1. Формирование запроса (Free text)
    Note right of BIZ: Отсутствие формализации:<br>неясны метрики и критерии успеха
    
    PO->>DS: 2. Делегирование анализа
    DS-->>PO: Запрос уточнений (Iterative loop)
    
    PO->>BIZ: 3. Уточняющие встречи
    
    Note over BIZ,DE: Этап 2: Работа с данными
    BIZ->>DE: 4. Запрос выгрузки данных
    DE->>DS: 5. Передача сырого дампа (CSV/Excel)
    Note right of DE: Выгрузка "как есть", без<br>валидации содержимого
    
    DS->>DS: 6. Ручной EDA (Exploratory Data Analysis)
    Note right of DS: Затраты: 5-10 человеко-часов.<br>Результат: выявлено несоответствие<br>данных задаче.
    
    DS->>PO: 7. Отрицательный вердикт
    PO->>BIZ: 8. Отказ в реализации / Запрос новых данных
    
    Note over BIZ,DE: Результат: Потеря времени DS