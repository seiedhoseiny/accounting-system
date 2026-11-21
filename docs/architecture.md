# معماری سیستم حسابداری دولتی توزیع‌شده

## ۱. نمای کلان

```mermaid
graph TB
    subgraph "مرکز وزارت بهداشت"
        CENTRAL_API[Central API<br/>ASP.NET Web API]
        CENTRAL_DB[(Central DB<br/>SQL Server)]
        DASHBOARD[داشبورد وضعیت<br/>و گزارش‌ها]
        CENTRAL_API --> CENTRAL_DB
        CENTRAL_API --> DASHBOARD
    end

    subgraph "واحد سطح ۱ - A"
        WEB_A[Web Forms]
        CORE_A[Core Domain]
        DB_A[(SQL Server)]
        QUEUE_A[SyncQueue]
        SVC_A[Sync Windows Service]
        WEB_A --> CORE_A --> DB_A
        CORE_A --> QUEUE_A
        SVC_A --> QUEUE_A
    end

    subgraph "واحد سطح ۱ - B"
        WEB_B[Web Forms]
        CORE_B[Core Domain]
        DB_B[(SQL Server)]
        QUEUE_B[SyncQueue]
        SVC_B[Sync Windows Service]
        WEB_B --> CORE_B --> DB_B
        CORE_B --> QUEUE_B
        SVC_B --> QUEUE_B
    end

    subgraph "سطح ۲ (زیرمجموعه A)"
        SUB_A1[کاربران سطح ۲]
        SUB_A2[کاربران سطح ۲]
    end

    SUB_A1 -->|همان DB واحد A| DB_A
    SUB_A2 -->|همان DB واحد A| DB_A

    SVC_A -->|HTTPS/JSON| CENTRAL_API
    SVC_B -->|HTTPS/JSON| CENTRAL_API
