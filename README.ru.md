# n8n AI Resume Matching

[English version](README.md)

Портфельный проект на n8n для end-to-end обработки PDF-резюме и сопоставления кандидатов с вакансиями: Gmail принимает письмо с резюме, дубликаты отсекаются до вызова AI, из PDF извлекаются данные кандидата, вакансии читаются из Google Sheets, LLM формирует структурированный результат matching, после чего результат сохраняется и workflow выбирает следующую ветку.

> Основной акцент: orchestration AI workflow, Gmail/PDF processing, structured extraction, экономное использование LLM, Google Sheets, structured matching и human-in-the-loop подход.

![Архитектура](docs/images/architecture.svg)

## Pipeline

```text
Gmail + PDF-резюме
       │
       ▼
Нормализация письма
       │
       ▼
Проверка дубля ── дубль ──► остановка без AI-затрат
       │
       ▼
Извлечение текста из PDF
       │
       ▼
Structured AI extraction
       │
       ├───────────────┐
       ▼               ▼
Данные кандидата   Чтение вакансий
       │               │
       └──────┬────────┘
              ▼
       AI vacancy matching
              │
              ▼
      Структурированный JSON
              │
              ▼
       Google Sheets log
              │
              ▼
   Ветка поддержки решения
```

## Что демонстрирует проект

- Gmail Trigger с PDF-вложениями.
- Дедупликацию до обращения к LLM, чтобы не тратить токены повторно.
- Детерминированное извлечение текста из PDF.
- Structured extraction данных кандидата.
- Чтение и агрегацию вакансий из Google Sheets.
- Structured matching с баллами, аргументами и рисками.
- Сохранение результата для ручной проверки и аудита.
- Ветвление по результату matching при обязательном human review для реальных кадровых решений.

## End-to-end проверка

Исходная реализация была реально запущена через Gmail Trigger с подключёнными Gmail, Google Sheets и OpenAI credentials. Успешный execution зафиксирован в исходном `execution history.png`; дополнительно сохранены скриншоты Gmail Trigger, Extract Candidate Data и общего workflow.

Кроме этого workflow проходил валидацию нод, `validate_workflow` и проверку connections. Подробнее: [Evidence](docs/evidence.md) и [Testing](docs/testing.md).

## Responsible portfolio variant

В публичной версии я исключил `sex` и `birth_date` из данных, используемых для matching. Эти признаки не нужны для оценки профессионального соответствия и не должны влиять на автоматизированные рекомендации.

В исходном demo workflow есть ветки invite/reject, потому что они были частью проверенного сценария. Для реального найма перед любым решением или письмом кандидату должен быть **human approval gate**.

Подробнее: [Privacy & Human Review](docs/privacy-and-human-review.md).

## Стек

`n8n` · `Gmail` · `PDF extraction` · `OpenAI` · `AI Agent` · `Structured Output` · `Google Sheets`

## Быстрый запуск

1. Импортировать [`workflow/resume-matching-pipeline.json`](workflow/resume-matching-pipeline.json) в n8n.
2. Подключить свои Gmail, Google Sheets и OpenAI credentials.
3. Создать листы `Vacancies` и `Candidates` по схемам из [Setup](docs/setup.md).
4. После импорта заново выбрать таблицу в Google Sheets nodes.
5. Для тестов использовать только синтетические/демонстрационные резюме.
6. Проверить duplicate, matching, persistence и обе decision-support ветки до активации.

Публичный JSON очищен от исходных workflow/project IDs, credential IDs, account names, spreadsheet IDs и environment-specific ссылок.

## Контракт matching

```json
{
  "matches": [
    {
      "vacancy": "Backend Engineer",
      "score": 78,
      "reasons": ["Relevant Python experience", "REST API experience"],
      "risks": ["Limited production Kubernetes exposure"]
    }
  ]
}
```

Score — это сигнал для поддержки решения, а не автономное кадровое решение.

## Для production

Следует добавить human approval, retention/deletion правила для персональных данных, RBAC, централизованный error handling, audit/correlation IDs, idempotency, evaluation datasets и bias/fairness review критериев matching.

## Лицензия

MIT — см. [`LICENSE`](LICENSE).
