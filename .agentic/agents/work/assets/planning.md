---
Author: {{user}}
Email: {{user@example.com}}
Description: 'A document to capture initial ideas, goals, and planning for {{date:YYYY}}.'
Created: {{date:YYYY-MM-DD}}
tags:
  - work
  - planning
---

# {{date:YYYY}} Planning

## Key Stakeholders

## Capabilities

### Core Engineering Capabilities

### Target Capabilities

## Key Initiatives

### Initiative 1

### Initiative 2

### Initiative ...

## FY{{date:YYYY | add:1}} Pillars

## Pillar Alignment

### Matrix View

| Initiative | Pillar 1 | Pillar 2 | Pillar _n_ |
|------------|----------|----------|------------|
| Initiative 1 | ✓ |   |   |
| Initiative 2 |   | ✓ |   |
| Initiative _n_ |   |   | ✓ |

<!--
# Use for PDF generation
<img src="images/radar-view.png" alt="Radar View" />
-->
<br><br>
<details style="display: flex; justify-content: center; margin: 1.5rem 0;">
<summary><strong>Pillar Distribution</strong></summary>
<br>
```mermaid
---
title: "Initiative Alignment to FY{{date:YY}} Pillars"
config:
  radar:
    axisLabelFactor: 1.1
    width: 800
    marginTop: 100
    marginLeft: 100
    marginRight: 100
    marginBottom: 100
  theme: base
  themeVariables:
    cScale0: "#5EAAF8"
    cScale1: "#8E66F7"
    cScale2: "#AB5FF8"
    cScale3: "#DA52F0"
    cScale4: "#F1B1FC"
    cScale5: "#F57DC0"
    cScale6: "#FCD5EC"
    primaryTextColor: "#FFFFFF"
    secondaryTextColor: "#FFFFFF"
    tertiaryTextColor: "#FFFFFF"
    radar:
      curveOpacity: 0.15
      axisColor: "#99999933"
---
radar-beta
  axis pillar1["Pillar 1"]
  axis pillar2["Pillar 2"]
  axis pillarn["Pillar n"]
  
  curve init1["Initiative 1"]{2, 5, 4, 4, 5}
  curve init2["Initiative 2"]{0, 5, 5, 5, 3}
  curve initn["Initiative n"]{0, 5, 0, 5, 0}

  graticule polygon
  max 5
  min 0
```
</details>

## Prioritization

### Criteria

### Prioritization Matrix

| Initiative | Criteria 1 | Criteria 2 | Criteria _n_ | Total Score |
|------------|------------|------------|--------------|-------------|
| Initiative 1 | 8 | 5 | 7 | 20 |
| Initiative 2 | 3 | 9 | 6 | 18 |
| Initiative _n_ | 6 | 4 | 8 | 18 |
