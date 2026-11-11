# Prompt Engineering Experiments Report

**Internship:** Valentius Kryptix\
**Task:** AI-2 -- Prompt Engineering\
**Student:** Vaishnavi\
**Tool:** Google AI Studio\
**Model selected:** Gemini 3.8 Flash\
**Experiment date:** 11 November 2026

------------------------------------------------------------------------

## 1. Objective

The objective of this task was to explore and compare three
prompt-engineering techniques:

1.  Zero-shot versus few-shot prompting.
2.  Step-by-step reasoning prompts versus direct prompting for a
    multi-step calculation.
3.  Structured JSON generation and extraction across five test inputs.

The experiments were performed in Google AI Studio using the same
selected Gemini model. The prompts and outputs below record the results
observed during testing.

------------------------------------------------------------------------

## 2. Experiment 1: Zero-shot vs. Few-shot Prompting

### 2.1 Task

Classify the sentiment of a customer review as Positive, Negative, or
Neutral.

### 2.2 Zero-shot prompt

``` text
Classify the sentiment of the following customer review as Positive, Negative, or Neutral.

Return only one sentiment label without any explanation.

Customer Review: "The delivery was late, but the product quality is excellent."
```

### 2.3 Actual zero-shot model output

``` text
Positive
```

### 2.4 Few-shot prompt

``` text
You are a customer review sentiment classifier. Classify each review based on its overall sentiment as Positive, Negative, or Neutral.

Follow these examples:

Example 1:
Review: "The food was delicious and the service was excellent."
Sentiment: Positive

Example 2:
Review: "The product arrived damaged and the seller refused to help."
Sentiment: Negative

Example 3:
Review: "The package arrived on Tuesday as scheduled."
Sentiment: Neutral

Now classify this review:
Review: "The delivery was late, but the product quality is excellent."
Sentiment:
```

### 2.5 Actual few-shot model output

``` text
Positive
```

### 2.6 Comparison and analysis

  Technique   Actual output
  ----------- ---------------
  Zero-shot   Positive
  Few-shot    Positive

Both prompting techniques produced the same classification for this
review. The few-shot prompt supplied three labelled examples that
demonstrated the expected categories and response format. However, the
examples did not change the answer for this particular test case. This
shows that few-shot prompting can guide a model's output format and task
interpretation, but it does not necessarily improve the result on every
individual input.

------------------------------------------------------------------------

## 3. Experiment 2: Direct Prompting vs. Step-by-Step Prompting

### 3.1 Task

Solve a multi-step shopping calculation involving two successive
discounts and a delivery charge.

**Problem:** A customer buys a laptop bag priced at ₹2,400. The store
offers a 20% discount, followed by an additional 10% discount on the
reduced price. The delivery charge is ₹80. What is the final amount the
customer must pay?

### 3.2 Direct prompt

``` text
A customer buys a laptop bag priced at ₹2,400. The store offers a 20% discount, followed by an additional 10% discount on the reduced price. The delivery charge is ₹80.

What is the final amount the customer must pay?

Give only the final amount.
```

### 3.3 Actual direct-prompt output

``` text
₹1,808
```

### 3.4 Step-by-step prompt

``` text
Solve the following problem carefully. Break the solution into clear calculation steps.

Show how each discount is applied sequentially, then add the delivery charge.

Check your arithmetic before giving the final answer.

A customer buys a laptop bag priced at ₹2,400. The store offers a 20% discount, followed by an additional 10% discount on the reduced price. The delivery charge is ₹80.

What is the final amount the customer must pay?
```

### 3.5 Actual step-by-step model output

``` text
Here is the step-by-step calculation to find the final amount the customer must pay:

Step 1: Original Price
- Original price of the laptop bag = ₹2,400

Step 2: Apply the First Discount (20%)
- First discount amount = 20% of ₹2,400 = ₹480
- Price after the first discount: ₹2,400 − ₹480 = ₹1,920

Step 3: Apply the Second Discount (10% on the reduced price)
- Second discount amount = 10% of ₹1,920 = ₹192
- Price after the second discount: ₹1,920 − ₹192 = ₹1,728

Step 4: Add the Delivery Charge
- Delivery charge = ₹80
- Final amount payable: ₹1,728 + ₹80 = ₹1,808

Final Answer:
The customer must pay ₹1,808.
```

### 3.6 Verification and analysis

The calculation can be independently verified:

-   First discount: 20% of ₹2,400 = ₹480; discounted price = ₹1,920.
-   Second discount: 10% of ₹1,920 = ₹192; discounted price = ₹1,728.
-   Add delivery: ₹1,728 + ₹80 = **₹1,808**.

  Technique                  Actual result Verification
  ------------------------ --------------- --------------
  Direct prompting                  ₹1,808 Correct
  Step-by-step prompting            ₹1,808 Correct

Both prompts produced the correct final amount. The step-by-step prompt
made the intermediate calculations visible and easier to verify. It
improved the explainability of the response in this test, but did not
improve final-answer accuracy because the direct prompt was already
answered correctly.

------------------------------------------------------------------------

## 4. Experiment 3: Structured JSON Output

### 4.1 Task

Extract a person's name, date, and payment amount from five short text
inputs and return the information as a JSON array.

### 4.2 Prompt used

``` text
You are an information extraction assistant. Extract the person's name, date, and amount from each of the five text inputs below.

Return a valid JSON array containing exactly five objects. Every object must contain these three keys:
- "name": string or null
- "date": string or null
- "amount": number or null

Rules:
1. Extract only information explicitly available in the text.
2. Do not guess missing values. Use null when information is missing.
3. Represent dates in YYYY-MM-DD format when the date is unambiguous. Otherwise use the original date text.
4. Represent amounts as numbers without currency symbols.
5. Return only valid JSON. No Markdown code fences or explanations.

TEST INPUTS:
1. "Rahul paid Rs. 1500 on 12 October 2026 for the course."
2. "Priya purchased a laptop for ₹45,999 on 5 November 2026."
3. "A payment of 2500 was made by Arjun on 2026-11-08."
4. "Sneha registered for the workshop on 10 November 2026. No payment has been recorded."
5. "The customer paid Rs. 799 yesterday. The transaction was successful."

Extract the information from all five inputs.
```

### 4.3 Actual model output

``` json
[
  {
    "name": "Rahul",
    "date": "2026-10-12",
    "amount": 1500
  },
  {
    "name": "Priya",
    "date": "2026-11-05",
    "amount": 45999
  },
  {
    "name": "Arjun",
    "date": "2026-11-08",
    "amount": 2500
  },
  {
    "name": "Sneha",
    "date": "2026-11-10",
    "amount": null
  },
  {
    "name": null,
    "date": "yesterday",
    "amount": 799
  }
]
```

### 4.4 Validation

The response was checked for the required three keys in every object,
correct extraction of available values, use of `null` for missing
values, and valid JSON syntax.

  Test input                   Validation
  ---------------------------- ------------
  Rahul                        Pass
  Priya                        Pass
  Arjun                        Pass
  Sneha                        Pass
  Customer with missing name   Pass

**Observed result: 5 out of 5 records passed (100%).**

The fifth record correctly retained the relative date text `"yesterday"`
instead of guessing an absolute date. The missing customer name was
represented as `null`, and the missing payment amount for Sneha was also
represented as `null`.

### 4.5 What improved reliability?

The prompt specified the exact JSON keys and data types, required an
array of five objects, instructed the model not to guess missing values,
and gave a consistent date-formatting rule. It also explicitly
prohibited explanatory text and Markdown fences. These constraints made
the expected output easier to validate. This experiment tested
prompt-based JSON formatting; a dedicated JSON mode or schema-enforced
API response was not separately tested.

------------------------------------------------------------------------

## 5. Overall Results

  Experiment                           Result
  ------------------------------------ --------------------
  Zero-shot sentiment classification   Positive
  Few-shot sentiment classification    Positive
  Direct calculation                   ₹1,808 -- correct
  Step-by-step calculation             ₹1,808 -- correct
  Structured JSON extraction           5/5 records passed

## 6. Key Learnings

1.  **Zero-shot prompting** is useful when the task is clear and the
    model can respond without examples.
2.  **Few-shot prompting** provides examples that guide the model's
    expected labels and response format. In this test, it produced the
    same result as zero-shot prompting.
3.  **Step-by-step prompting** can make multi-step calculations easier
    to inspect and verify. In this test, both direct and step-by-step
    prompts reached the correct answer.
4.  **Structured output prompting** benefits from explicit keys, data
    types, formatting rules, and instructions for missing values.
5.  Prompt quality should be evaluated using actual outputs and honest
    validation, rather than assuming that a more detailed prompt always
    performs better.

## 7. Limitations

These were small demonstrations using one selected model and a limited
number of test cases. The sentiment experiment used one target review,
and the calculation experiment used one problem. The JSON experiment
used five records. Therefore, the results demonstrate the techniques but
are not sufficient to establish that one technique is universally more
accurate than another. Repeated tests with larger and more varied
datasets would provide stronger evidence.

## 8. Conclusion

This task demonstrated three practical prompt-engineering techniques
using Google AI Studio. Zero-shot and few-shot prompts both classified
the review as Positive. Direct and step-by-step prompts both solved the
discount problem correctly, while the step-by-step response made the
calculation easier to verify. The structured-output experiment
successfully extracted the required fields from all five test inputs and
achieved a 100% record-level validation rate. Overall, the experiments
showed how clear instructions, examples, explicit constraints, and
output schemas can help make LLM responses more consistent and usable.

------------------------------------------------------------------------

**Declaration:** The experiments were conducted in Google AI Studio.
This report documents the prompts and outputs observed during testing,
along with the author's analysis.
