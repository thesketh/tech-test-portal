# NEWS2 Score Calculation

## Background

Write a function to calculate the NEWS2 score for a patient. This is a simple rule-of-thumb score used
to identify actutely ill patients.

## Introduction

The National Early Warning Score (version 2, NEWS2) is a simple aggregate scoring system in which a score
is calculated based on patients' physiological measurements. These observations are already routinely
taken when patients present to (or are being monitored in) hospital.

The properties that are measured are:
 1. respiration rate
 2. oxygen saturation
 3. systolic blood pressure
 4. pulse rate
 5. level of consciousness or new confusion (whether the patient is newly confused, disorientated or agitated)
 6. temperature

A score for each property is allocated as the property is measured. The score for the property reflects how different
the measurement is from the range of expected values.

The scores for each property are added together, and 2 additional points are added for patients requiring
supplemental oxygen to maintain their oxygen saturation level. This final sum is the NEWS2 score, which ranges from
0 to 23.

## Application Requirements

The NEWS2 score for a patient is the **sum** of the scores for each property in the following table. Ranges are inclusive.

Your function can either take these measures as separate parameters, or take a single struct/object containing these values as attributes/properties. The NEWS2 score for the patient should be returned as an **integer**.

| Property                       | Score 3  | Score 2      | Score 1         | Score 0                        | Score 1               | Score 2               | Score 3          |
| ------------------------------ | -------- | ------------ | --------------- | ------------------------------ | --------------------- | --------------------- | ---------------- |
| Air or oxygen?                 |          | Oxygen       |                 | Air                            |                       |                       |                  |
| Consciousness                  |          |              |                 | Alert                          |                       |                       | CVPU             |
| Pulse (per minute)             | &le;40   |              | 41&ndash;50     | 51&ndash;90                    | 91&ndash;110          | 111&ndash;130         | &ge;131          |
| Respiration rate (per minute)  | &le;8    |              | 9&ndash;11      | 12&ndash;20                    |                       | 21&ndash;24           | &ge;25           |
| SpO~2~ Scale 1 (%)             | &le;91   | 92&ndash;93  | 94&ndash;95     | &ge;96                         |                       |                       |                  |
| SpO~2~ Scale 2 (%)             | &le;83   | 84&ndash;85  | 86&ndash;87     | 88&ndash;92 (or &ge;93 on air) | 93&ndash;94 on oxygen | 95&ndash;96 on oxygen | &ge;97 on oxygen |
| Systolic blood pressure (mmHg) | &le;90   | 91&ndash;100 | 101&ndash;110   | 111&ndash;219                  |                       |                       | &ge;220          |
| Temperature (&deg;C)           | &le;35.0 |              | 35.1&ndash;36.0 | 36.1&ndash;38.0                | 38.1&ndash;39.0       | &ge;39.1              |                  |

The values of the observations will be the following data types:

| Property                | Data type      | Comment                                           |
| ----------------------- | -------------- | ------------------------------------------------- |
| Air or oxygen?          | Enum (integer) | `0` if air, `2` if on oxygen                      |
| Consciousness           | Enum (integer) | `0` if alert, non-zero if CVPU                    |
| Pulse                   | Integer        |                                                   |
| Respiration range       | Integer        |                                                   |
| SpO~2~ Scale 1          | Integer        |                                                   |
| SpO~2~ Scale 2          | Integer        |                                                   |
| Systolic blood pressure | Integer        |                                                   |
| Temperature             | Float          | This should be rounded to a single decimal place. |

## Examples

Here are some examples of patients and their NEWS2 scores.

### Patient 1

| Property                | Observation | Score | Comment                                                                  |
| ----------------------- | ----------- | ----- | ------------------------------------------------------------------------ |
| Air or oxygen?          | 0           | 0     | The patient is breathing air, and does not require supplementary oxygen. |
| Consciousness           | 0           | 0     | The patient is conscious.                                                |
| Pulse                   | 65          | 0     |                                                                          |
| Respiration range       | 15          | 0     |                                                                          |
| SpO~2~ Scale 1          | 98          | 0     |                                                                          |
| SpO~2~ Scale 2          | 95          | 0     | As the patient is breathing air, this is a normal range.                 |
| Systolic blood pressure | 120         | 0     |                                                                          |
| Temperature             | 37.1        | 0     |                                                                          |

This patient's final NEWS2 score is **0**.

### Patient 2

| Property                | Observation | Score | Comment                                               |
| ----------------------- | ----------- | ----- | ----------------------------------------------------- |
| Air or oxygen?          | 2           | 2     | The patient requires supplementary oxygen.            |
| Consciousness           | 0           | 0     | The patient is conscious.                             |
| Pulse                   | 95          | 1     |                                                       |
| Respiration range       | 17          | 0     |                                                       |
| SpO~2~ Scale 1          | 98          | 0     |                                                       |
| SpO~2~ Scale 2          | 95          | 2     | As the patient is breathing oxygen, this is elevated. |
| Systolic blood pressure | 105         | 1     |                                                       |
| Temperature             | 37.1        | 0     |                                                       |

This patient's final NEWS2 score is **6**.

### Patient 3

| Property                | Observation | Score | Comment                                                     |
| ----------------------- | ----------- | ----- | ----------------------------------------------------------- |
| Air or oxygen?          | 2           | 2     | The patient requires supplementary oxygen.                  |
| Consciousness           | 1           | 3     | The patient is unconscious or confused.                     |
| Pulse                   | 125         | 2     |                                                             |
| Respiration range       | 23          | 2     |                                                             |
| SpO~2~ Scale 1          | 96          | 0     |                                                             |
| SpO~2~ Scale 2          | 88          | 0     | As the patient is breathing oxygen, this is a normal range. |
| Systolic blood pressure | 95          | 2     |                                                             |
| Temperature             | 38.5        | 1     |                                                             |

This patient's final NEWS2 score is **12**.

## References

 - https://www.england.nhs.uk/ourwork/clinical-policy/sepsis/nationalearlywarningscore/
 - https://www.yas.nhs.uk/media/2970/nhs-news-document.pdf
