---
ms.topic: include
ms.technology: devops-cicd
ms.manager: mijacobs
ms.author: sdanie
author: steved0x
ms.date: 02/12/2020
---

## Unattended config

The agent can be set up from a script with no human intervention.
You must pass `--unattended` and the answers to all questions.

::: moniker range="> tfs-2018"

[!INCLUDE [unattend](./unattended-config.md)]

::: moniker-end

`./config.sh --help` always lists the latest required and optional responses.

## Help on other options

To learn about other options:

```bash
./config.sh --help
```

The help provides information on authentication alternatives and unattended configuration.
