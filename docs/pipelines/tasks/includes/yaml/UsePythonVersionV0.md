---
ms.topic: include
author: vtbassmatt
ms.author: macoope
ms.date: 04/25/2019
ms.prod: devops
ms.technology: devops-cicd-tasks
---

```YAML
# Use Python version
# Use the specified version of Python from the tool cache, optionally adding it to the PATH
- task: UsePythonVersion@0
  inputs:
    #versionSpec: '3.x' 
    #addToPath: true 
    #architecture: 'x64' # Options: x86, x64 (this argument applies only on Windows agents)
```
