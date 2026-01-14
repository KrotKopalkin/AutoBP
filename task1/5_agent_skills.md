# TO-BE Диаграмма: Автоматизированный пайплайн валидации ML-проектов

```mermaid
sequenceDiagram
    participant USER as Заказчик
    participant ORCH as AI Orchestrator (n8n Main)
    participant AI_BA as Agent: Business Analyst (LLM)
    participant AI_DATA as Agent: Data Inspector (Code+LLM)
    participant DS as Lead Data Scientist

    %% ЭТАП 1: Сбор требований
    USER->>ORCH: Инициация запроса
    ORCH->>AI_BA: Запуск сценария интервью (CRISP-DM)
    
    loop Формализация
        AI_BA->>USER: Запрос параметров (Цель, Метрика, Горизонт)
        USER-->>AI_BA: Ответы
        AI_BA->>AI_BA: Валидация полноты контекста
    end
    
    AI_BA-->>ORCH: Формализованное ТЗ (Structured JSON)

    %% ЭТАП 2: Инспекция данных
    ORCH->>USER: Запрос сэмпла данных (Upload Link)
    USER-->>ORCH: Загрузка датасета (CSV/JSON)
    
    ORCH->>AI_DATA: Команда: Perform Data Audit
    
    par Технический аудит
        AI_DATA->>AI_DATA: Python Script execution<br>(Data types, Nulls ratio, Duplicates)
    and Семантический аудит
        AI_DATA->>AI_DATA: LLM Semantic Check<br>(Соответствие контента ТЗ)
        note right of AI_DATA: Детекция нерелевантной выгрузки
    end
    
    AI_DATA-->>ORCH: Комплексный отчет о качестве данных

    %% ЭТАП 3: Принятие решения
    alt Данные не соответствуют требованиям
        ORCH->>USER: Отказ в регистрации проекта
        note left of ORCH: Автоматическая генерация отчета<br>с указанием причин отклонения
    else Валидация успешна
        ORCH->>AI_BA: Генерация Project Charter
        AI_BA-->>ORCH: Итоговый документ
        ORCH->>DS: Создание задачи в Backlog
        note right of DS: Входные данные:<br>1. Четкое ТЗ<br>2. Проверенный датасет<br>3. Технический отчет
    end