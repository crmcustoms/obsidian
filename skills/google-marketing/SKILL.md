---
name: google-marketing
description: Работа с Google Ads, Google Analytics (GA4) и Tag Manager через сервис-аккаунт. Позволяет получать отчеты по трафику, расходам и конверсиям.
metadata:
  {
    "openclaw": {
      "emoji": "📊",
      "requires": { "files": ["~/.config/google/service-account.json"] }
    }
  }
---

# google-marketing

Скилл для профессиональной работы с маркетинговыми данными Google. Использует библиотеку `google-analytics-data` для GA4 и `google-ads` для рекламы.

## Подготовка

1. JSON-ключ сервис-аккаунта должен лежать по пути: `~/.config/google/service-account.json`.
2. Сервис-аккаунт `gedevan@gedevan.iam.gserviceaccount.com` должен быть добавлен в ресурсы (Ads, GA4) с правами на чтение.

## Использование (Примеры команд)

### Google Analytics 4 (GA4)

Для получения отчета по визитам используем Python-скрипт (библиотека `google-analytics-data`).

**Пример запроса топ страниц:**
```bash
# Нужно передать PROPERTY_ID (цифры из настроек GA4)
python3 -c "
from google.analytics.data_v1beta import BetaAnalyticsDataClient
from google.analytics.data_v1beta.types import RunReportRequest, DateRange, Dimension, Metric
import os

os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = os.path.expanduser('~/.config/google/service-account.json')
client = BetaAnalyticsDataClient()

request = RunReportRequest(
    property='properties/YOUR_PROPERTY_ID',
    dimensions=[Dimension(name='pageTitle')],
    metrics=[Metric(name='activeUsers')],
    date_ranges=[DateRange(start_date='7daysAgo', end_date='today')],
)
response = client.run_report(request)
for row in response.rows:
    print(f\"{row.dimension_values[0].value}: {row.metric_values[0].value}\")
"
```

### Google Ads

Использует [Google Ads API](https://developers.google.com/google-ads/api/docs/start). Требует `developer_token` в файле конфигурации `google-ads.yaml`.

## Конфигурация

### Google Ads YAML (~/google-ads.yaml)
```yaml
developer_token: YOUR_DEVELOPER_TOKEN
client_id: 109569812717972301614
client_email: gedevan@gedevan.iam.gserviceaccount.com
private_key: \"-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n\"
json_key_file_path: ~/.config/google/service-account.json
# Если используем сервис-аккаунт для Ads, нужно также указать login_customer_id (MCC)
# login_customer_id: INSERT_MCC_CUSTOMER_ID_HERE
```
