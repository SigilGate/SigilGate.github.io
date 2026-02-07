---
title: "Плоскость управления"
description: "Плоскость управления сетью Sigil Gate: SSH-администрирование, PKI, реагирование на инциденты"
summary: "SSH-администрирование, административная привязка, PKI, компрометация и реагирование"
weight: 30
date: 2026-02-07
---

{{< lead >}}
Root-нода администрирует всю сеть по SSH. Доступ — однонаправленный: Root имеет доступ ко всем нодам, обратный доступ запрещен.
{{< /lead >}}

{{< mermaid >}}
graph TD
    Root["Root-нода<br/><small>управление, PKI</small>"]

    Core1["Core-нода #1"]
    Core2["Core-нода #2"]

    Entry1["Entry-нода A"]
    Entry2["Entry-нода B"]
    Entry3["Entry-нода C"]

    Root -->|SSH| Core1
    Root -->|SSH| Core2
    Root -->|SSH| Entry1
    Root -->|SSH| Entry2
    Root -->|SSH| Entry3

    Core1 ---|"mesh"| Core2

    style Root fill:#7c3aed,color:#fff,stroke:#6d28d9
    style Core1 fill:#1d4ed8,color:#fff,stroke:#1e40af
    style Core2 fill:#1d4ed8,color:#fff,stroke:#1e40af
    style Entry1 fill:#0891b2,color:#fff,stroke:#0e7490
    style Entry2 fill:#0891b2,color:#fff,stroke:#0e7490
    style Entry3 fill:#0891b2,color:#fff,stroke:#0e7490
{{< /mermaid >}}

## Административная привязка

Каждая Entry-нода в каждый момент времени **административно** закреплена за одной Core-нодой. Core-нода отвечает за конфигурирование, ротацию путей и мониторинг своих Entry-нод.

Это образует древовидную структуру в плоскости управления:

{{< mermaid >}}
graph TD
    Core1["Core-нода #1"]
    Core2["Core-нода #2"]

    E1["Entry A"]
    E2["Entry B"]
    E3["Entry C"]
    E4["Entry D"]
    E5["Entry E"]

    Core1 --> E1
    Core1 --> E2
    Core1 --> E3
    Core2 --> E4
    Core2 --> E5

    Core1 ---|"mesh"| Core2

    style Core1 fill:#1d4ed8,color:#fff,stroke:#1e40af
    style Core2 fill:#1d4ed8,color:#fff,stroke:#1e40af
    style E1 fill:#0891b2,color:#fff,stroke:#0e7490
    style E2 fill:#0891b2,color:#fff,stroke:#0e7490
    style E3 fill:#0891b2,color:#fff,stroke:#0e7490
    style E4 fill:#0891b2,color:#fff,stroke:#0e7490
    style E5 fill:#0891b2,color:#fff,stroke:#0e7490
{{< /mermaid >}}

Core-ноды могут передавать свои Entry-ноды друг другу — например, для вывода Core-ноды из ротации на обслуживание. Это обеспечивает эксплуатационную гибкость без потери связности.

## Управление с Core-нод

Помимо Root-ноды, типовые операции по управлению сетью могут выполняться с любой Core-ноды: добавление новых Entry-нод, регистрация пользователей, ротация путей. Это позволяет администрировать сеть без постоянного доступа к Root-ноде.

## Инфраструктура открытых ключей (PKI)

Сеть использует трехуровневую иерархию сертификатов:

{{< mermaid >}}
graph TD
    RootCA["Root CA<br/><small>Root-нода</small>"]
    IntCA1["Промежуточный CA<br/><small>Core-нода #1</small>"]
    IntCA2["Промежуточный CA<br/><small>Core-нода #2</small>"]
    Cert1["Сертификат<br/><small>Entry-нода A</small>"]
    Cert2["Сертификат<br/><small>Entry-нода B</small>"]
    Cert3["Сертификат<br/><small>Entry-нода C</small>"]

    RootCA --> IntCA1
    RootCA --> IntCA2
    IntCA1 --> Cert1
    IntCA1 --> Cert2
    IntCA2 --> Cert3

    style RootCA fill:#7c3aed,color:#fff,stroke:#6d28d9
    style IntCA1 fill:#1d4ed8,color:#fff,stroke:#1e40af
    style IntCA2 fill:#1d4ed8,color:#fff,stroke:#1e40af
    style Cert1 fill:#0891b2,color:#fff,stroke:#0e7490
    style Cert2 fill:#0891b2,color:#fff,stroke:#0e7490
    style Cert3 fill:#0891b2,color:#fff,stroke:#0e7490
{{< /mermaid >}}

| Сертификат | Где хранится | Срок жизни | Отзыв |
|------------|-------------|------------|-------|
| Root CA | Root-нода | Долгосрочный | Ручной, критический инцидент |
| Промежуточный CA | Core-нода | Долгосрочный | Ручной, с Root-ноды |
| Сертификат Entry | Entry-нода | Часы / дни | Автоматический |

## Компрометация и реагирование

{{< alert >}}
Под **компрометацией** ноды понимается не только несанкционированный доступ к ноде, но и обнаружение и блокировка системами DPI. Признаки компрометации — потеря HTTPS-связности между Entry- и Core-нодами при сохранении SSH-доступа, или существенная деградация канала связи.
{{< /alert >}}

### Компрометация Entry-ноды

1. Вывести скомпрометированную Entry-ноду из оборота.
2. Если общее количество Core-нод > 2 — вывести Core-ноды, известные этой Entry-ноде.
3. Перевыпустить сертификаты для затронутых компонентов.

### Компрометация Root-ноды (критический инцидент)

1. Немедленно отозвать все SSH-ключи на всех нодах.
2. Перегенерировать Root CA.
3. Перевыпустить все сертификаты Core-нод и Entry-нод.
4. Развернуть новую Root-ноду.
