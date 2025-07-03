# 🗺️ Mi:dm 2.0 Prompt Examples

## 🧭 Mi:dm Prompting Examples Overview

We provide users with a guide that demonstrates how to effectively prompt the **Mi:dm 2.0**.

Through several trials, we have identified that Mi:dm 2.0 performs particularly well on four key task types across six domains:

### ✅ Tasks
- [Document Generation](#task-1-document-generation)
- [Dialogue/Question Answering](#task-2-dialoguequestion-answering)
- [Text Summarization](#task-3-text-summarization)
- [Reasoning](#task-4-reasoning)

### 🏷️ Domains
- **General**
- **Public**
- **Legal**
- **Finance**
- **Education**
- **Healthcare**

---

## 🧩 Prompt Composition

Effective prompts are key to leveraging the full capabilities of Mi:dm 2.0. This section outlines how to structure prompts for optimal performance across supported tasks and domains.

To make prompts more structured and effective, we define the following six components:

| Component   | Importance | Description |
|-------------|------------|-------------|
| **Instruction** | ⭐⭐⭐ | Clearly states the task or instruction the model should perform. |
| **Context**     | ⭐⭐  | Provides background information, the situation, or purpose to help the model understand the task. |
| **Role**        | ⭐⭐  | Assigns the model a specific perspective, such as that of an expert, professional, or character. |
| **Example**     | ⭐⭐  | Provides reference inputs or cases the model can learn from or imitate. |
| **Format**      | ⭐   | Specifies the desired structure, layout, or output format of the response. |
| **Tone**        | ⭐   | Adjusts the style of the response by specifying level of formality, emotion, or manner of expression. |

---

## Prompt Use Cases 
We offer a variety of use cases to guide users in prompt engineering.
> [!NOTE]
> The curly braces `{}` are used to denote each prompt component for explanatory purposes.  
> When writing actual prompts, do not include the curly braces or labels.  
> For example, instead of writing ```{Instruction} 회의 요약을 작성해 주세요.```, simply write ```회의 요약을 작성해 주세요```.


<br>

### Task 1. Document Generation

**General**
```
{Role} 당신은 KT 영업본부 직원입니다.
{Instruction} 각 영업 대리점에 KT 상품을 소개하는 AI 챗봇 서비스 내용을 설명하는 자료를 만들어 주세요.
{Tone} AI 서비스에 생소한 고객과 직원을 대상으로 쉽고 재미있게 설명해 주세요.
```

**Public**
```
{Instruction} OO기관에 협조를 요청하는 공문 초안을 작성해 주세요.
{Tone} AI 서비스에 생소한 고객과 직원을 대상으로 쉽고 재미있게 설명해 주세요.
{Example} 아래 기준을 지켜 주세요:
1. 문서 목적: 2024년 청년 일자리 박람회’ 공동 개최에 대한 협조 요청
2. 포함 내용:
* 행사 개요 (일시, 장소, 주최/주관)
* 협조 요청 배경 및 필요성
* 요청 사항 (예: 장소 제공, 홍보 협력, 인력 지원 등)
* 담당자 연락처 등 후속 절차 안내
3. 형식 요구:
* 공문 문서 번호
* 제목 포함
* 수신처(기관명)을 명시
* 본문은 3~5단락 구성
* 끝맺음에는 “협조에 깊이 감사드립니다.” 또는 “검토 후 회신 부탁드립니다.” 등의 마무리 문장 포함
```

**Education**
```
{Instruction} 아래 텍스트에서 비교 항목을 추출하여 객관식 문제로 만들어 주세요.
{Example} “식물세포는 세포벽과 엽록체를 가지며, 광합성을 통해 에너지를 얻습니다. 반면 동물세포는 세포벽이 없고, 엽록체도 없으며 주로 세포호흡으로 에너지를 만듭니다.”
```

<br>

### Task 2. Dialogue/Question Answering

**General**
```
{Instruction} 두꺼비집 짓기가 무엇인가요?
```

**Finance**
```
{Context} 양적완화(QE)와 양적긴축(QT)이 기술주와 가치주의 성과에 각각 어떤 영향을 미쳤는지
{Format} 과거 사례(예: 2008년 금융위기 이후, 코로나19 팬데믹)를 들어
{Role} 전문가의 관점으로
{Instruction} 분석해줘.
```

<br>

### Task 3. Text Summarization

**Legal**
```
{Instruction} 대법원 2023. 12. 7. 선고 2023다269139 판결문을 요약해 줘.
{Format} 사실관계, 주요 법적 쟁점, 그리고 대법원의 최종 결론을 나누어서 설명해 줘.
{Example} <판결문 원문 일부>
[1] 일반적으로 계약을 해석할 때에는 형식적인 문구에만 얽매여서는 안 되고 쌍방당사자의 진정한 의사가 무엇인가를 탐구하여야 한다. 
계약 내용이 명확하지 않은 경우 계약서의 문언이 계약 해석의 출발점이지만, 당사자들 사이에 계약서의 문언과 다른 내용으로 의사가 합치된 경우 그 의사에 따라 계약이 성립한 것으로 해석하여야 한다. 
당사자 사이에 계약의 해석을 둘러싸고 이견이 있어 당사자의 의사 해석이 문제 되는 경우에는 계약의 형식과 내용, 계약이 체결된 동기와 경위, 계약으로 달성하려는 목적, 당사자의 진정한 의사,
거래 관행 등을 종합적으로 고려하여 논리와 경험의 법칙, 그리고 사회일반의 상식과 거래의 통념에 따라 합리적으로 해석하여야 한다.
```

<br>

### Task 4. Reasoning

**Healthcare**
```
{Instruction} 환자 A의 과거 3년간의 치과 방문 기록과 진단명을 분석하여, 다음 내원 예상 시기를 예측해 주세요.
{Format} 예측된 시기에 맞춰 진료를 권유하는 50자 이내의 내원 장려 문자 초안을 작성해 주세요.
{Example} <데이터>
방문번호,방문일,진단명,진료과,방문유형
DV001,2022-07-21,임플란트 정기검진,치과,외래
DV002,2022-09-03,치아마모,치과,외래
DV003,2022-09-16,스케일링,치과,외래
...
DV023,2025-02-12,보철물 이상,치과,외래
DV024,2025-04-03,충치,치과,외래
DV025,2025-05-25,치아균열,치과,외래
```

<br>

We hope this guide makes your experience with Mi:dm 2.0 even better. <br>
If you discover other exciting ways to use it, we’d love to hear from you!
