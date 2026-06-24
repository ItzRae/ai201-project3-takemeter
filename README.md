# ai201-project3-takemeter

# Evaluation Report

## Overall Performance

### Baseline LLM Classifier

**Accuracy:** 0.800

| Label | Precision | Recall | F1-Score |
|---------|---------:|---------:|---------:|
| results | 0.90 | 0.82 | 0.86 |
| advice | 0.64 | 1.00 | 0.78 |
| discussion | 1.00 | 0.60 | 0.75 |
| **Macro Avg** | **0.85** | **0.81** | **0.80** |

### Fine-Tuned DistilBERT Classifier

**Accuracy:** 0.533

| Label | Precision | Recall | F1-Score |
|---------|---------:|---------:|---------:|
| results | 0.52 | 1.00 | 0.69 |
| advice | 0.00 | 0.00 | 0.00 |
| discussion | 0.56 | 0.50 | 0.53 |
| **Macro Avg** | **0.36** | **0.50** | **0.40** |

---

## Fine-Tuned Model Confusion Matrix

| Actual \ Predicted | results | advice | discussion |
|-------------------|---------|---------|------------|
| results | 11 | 0 | 0 |
| advice | 5 | 0 | 4 |
| discussion | 5 | 0 | 5 |

Rows represent the true labels and columns represent the model's predictions.



---

## Error Analysis

The baseline LLM classifier substantially outperformed the fine-tuned DistilBERT model, achieving 80.0% accuracy compared to 53.3% accuracy. The confusion matrix shows that the model struggled most with the advice category, which it failed to predict correctly a single time. Instead, advice posts were frequently misclassified as either results or discussion, suggesting that the model had difficulty distinguishing advice-seeking intent from other forms of personal fitness discussion.

Several patterns explain most of the model's errors. First, the model showed a strong tendency to over-predict results: both advice and discussion posts were often labeled as results whenever they contained personal stories, progress-related language, body measurements, lifting statistics, calorie counts, or other fitness metrics. Second, personal narrative openings frequently misled the model. Posts that began with a story, rant, or description of a personal experience were often classified based on that opening context rather than the author's ultimate purpose for posting. Third, the model struggled with intent-based distinctions, particularly when authors described their own situation before asking a question or inviting conversation. Finally, the model's confidence scores were consistently low across many incorrect predictions (roughly 0.34–0.36), suggesting that it was often uncertain and relying on shallow linguistic patterns rather than learning robust boundaries between the labels. Together, these results indicate that the model learned obvious surface-level signals associated with fitness progress, but struggled to capture the more nuanced distinction between sharing results, seeking advice, and starting a discussion.

---
### Error Example 1

**True Label:** `discussion`

**Predicted Label:** `results`

**Post:**

> Ugh I am over this! I am so over getting my period, then I'm bloated during ovulation,
> then I look fine for a week THEN BOOM my boobs are sore and huge and another cycle lmao 😂
> I am struggling to lose some weight and it's driving me insane because the scale moves
> with water weight cause of my cycles! My problem areas are my chest and thighs….

**Analysis:**

I can see why the model made this mistake. The post is written entirely in the first person and focuses on changes in the author's body, which are common features of results posts. However, the actual purpose of the post is not to share progress but to vent frustration and connect with others who may have experienced something similar. This highlights a limitation of the fine-tuned model: it focused more on surface-level patterns such as personal body descriptions than on the broader conversational intent of the post.

---

### Error Example 2

**True Label:** `advice`

**Predicted Label:** `results`

**Post:**

> Very Shallow Question. Where are we getting our workout clothes from? The usual suspect
> big names are generally pretty limited and uninspiring. Anyone have recommendations for
> must have fitness gear online or in store?

**Analysis:**

This prediction surprised me because the post explicitly asks for recommendations. A human reader would likely recognize it as an advice-seeking post almost immediately. One possible explanation is that the model learned to associate certain fitness-related keywords with other categories and did not pay enough attention to the author's request for suggestions. This example suggests that the model struggled to identify advice when it appeared in less common contexts outside of workouts, dieting, or weight loss.

---

### Error Example 3

**True Label:** `discussion`

**Predicted Label:** `results`

**Post:**

> Strength standards for petite lifters (why bodyweight ratios are misleading).
> 5'1", 105lbs. Deadlift 205, squat 155, bench 95. People always say "wow that's almost
> 2x bodyweight on deadlift!" Yeah but I'm still only moving 205 pounds which is
> objectively not that heavy.

**Analysis:**

This post contains several features that resemble a results post, including body measurements and lifting statistics. However, the author's goal is not to celebrate those numbers but to start a discussion about how strength standards are evaluated for petite lifters. I think the model became overly reliant on numerical progress indicators and missed the broader argument being made. This example illustrates how the model sometimes focused on what information was present in the post rather than why the author was sharing it.

----

## Sample Classifications

The table below shows five example posts from the test set run through the fine-tuned
model, including the true label, predicted label, and confidence score.

| Post (truncated) | True Label | Predicted Label | Confidence |
|---|---|---|---|
| "Down 12 lbs, trying to focus on the wins 🖤 I'm 5'3", currently 155 lbs. I can finally see the existence of my ribs again!" | `results` | `results` | 0.39 |
| "Proud result of a lifestyle change. 2020>2024. Highest weight of about 128? To now 107. Running 3x a week, lifting, eating a healthier diet, leaving toxic relationship..." | `results` | `results` | 0.36 |
| "Why can't I just workout again? I used to be so dedicated while I was on my journey. Since then I've started a new job that has me working later and longer..." | `discussion` | `discussion` | 0.35 |
| "how do I stay confident and not let people’s looks or comments affect me while I’m still on the way there? I started gaining weight at 20 after my first breakup. I didn’t even realize I was emotionall..." | `advice` | `discussion` | 0.36 |
| "bottomless pit the day before my period i’m normally pretty good with eating consistently when my routine is in order…", 105lbs. Deadlift 205, squat 155, bench 95..." | `discussion` | `results` | 0.34 |

The first two results predictions are reasonable: both posts contain explicit milestone
language ("down 12 lbs," "highest weight of about 128 to now 107") paired with first-person
achievement framing, which are the clearest signals of a results post in the dataset.
The discussion prediction in row three is also sensible, since the post describes a personal struggle without asking for specific guidance, fitting the pattern of inviting community perspective rather than looking for a solution.

## Discussion

The fine-tuned model struggled most with the **advice** category, which achieved an F1 score of 0.00. This suggests that the model was unable to reliably learn the distinction between advice seeking posts and discussion-oriented posts. Many posts in both categories were phrased as questions and often requested personal experiences, making the boundary difficult for a small model trained on only 200 examples.

Based on the confusion matrix and error analysis, the primary issue appears to be the difficulty of the advice-versus-discussion boundary rather than class imbalance. The dataset was relatively balanced across labels, but the semantic distinction between these two categories remained subtle.

To improve performance, I would collect additional examples that explicitly demonstrate the difference between advice and discussion posts. I would also refine the label definitions with more examples of difficult edge cases and potentially increase the size of the training dataset beyond 200 examples to help the model learn these distinctions more reliably.

## AI Usage

### 1. Annotation Validation and Edge Cases

I used ChatGPT while labeling posts to help evaluate difficult or ambiguous examples. When I encountered posts that could reasonably fit multiple labels, I provided the post along with my taxonomy and asked the model to explain which label it would assign and why. I did not automatically accept the model's recommendation. Instead, I compared its reasoning against my label definitions and made the final labeling decision myself. In several cases, I overrode the model's initial suggestion after determining that a different label better matched the author's primary intent according to my annotation guidelines.

### 2. Label Stress-Testing and Taxonomy Refinement

Before collecting the full dataset, I used ChatGPT to stress-test my label definitions by discussing ambiguous examples and edge cases. The model helped identify situations where labels such as **advice** and **discussion** overlapped heavily. From these discussions I simplified my original taxonomy and refined the annotation rules to focus on the author's primary intent. I made the final decisions about the taxonomy and updated the definitions myself.

### 3. Evaluation and Error Analysis

After training and evaluating the models, I used ChatGPT to help interpret the classification reports, confusion matrix, and performance metrics. The model helped identify patterns in the errors and suggested possible explanations for why the fine-tuned model struggled with the boundary between **advice** and **discussion**. I reviewed these suggestions and incorporated only those that were supported by the actual results and examples from my dataset.

## Reflection

One interesting observation from this project is that the model did not seem to learn the distinction between **advice** and **discussion** in the same way that I intended during annotation. My label definitions focused on the author's primary intent: advice posts were seeking actionable guidance, while discussion posts were inviting opinions, experiences, or conversation. However, the fine-tuned model appeared to rely more heavily on surface-level features such as whether a post was phrased as a question. Because many discussion posts were written as questions and many advice posts asked community members to share personal experiences, the model frequently treated these categories as similar. In contrast, the **results** category was easier for the model to learn because posts often contained clear signals such as progress updates, weight loss milestones, or achievement-oriented language. Overall, the model captured obvious linguistic patterns but struggled to learn the more subtle intent-based distinction that I was trying to encode in the taxonomy. If I were to start over I would think more carefully about the labels picked to avoid any overlap - so that the differences are more blatant from the start. 

## Spec Reflection

The specification was helpful because it forced me to carefully define my labels and think through ambiguous cases before collecting data. Creating clear annotation rules for advice, results, and discussion posts helped me stay consistent while labeling 200 examples and made it easier to identify difficult boundaries during evaluation. One way my implementation diverged from the original plan was that I initially considered using four labels, including a separate mindset category. After reviewing posts from r/petitefitness, I found that many mindset-related posts overlapped heavily with advice and discussion, so I simplified the taxonomy to three labels. This made the annotation process more consistent and better reflected the patterns that actually appeared in the community.