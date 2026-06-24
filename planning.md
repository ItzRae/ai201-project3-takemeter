# Planning

## **Community**

I chose **r/petitefitness**, a subreddit focused on fitness/nutrition, and body composition advice for petite women. I chose this community because I am personally a petite woman that enjoys fitness and the posts are highly focused around fitness goals but people ask for help in many different ways, including workout routines, nutrition questions, and progress updates.

This variety makes it a good classification task because posts are similar enough to share a common domain while still having meaningful distinctions between categories.

---

## Labels

### `advice`

The author is seeking recommendations, solutions, guidance, or actionable help for a personal fitness, nutrition, health, or lifestyle problem.

**Key signal:** The author wants advice they can apply to their own situation.

**Examples:**
- "What is your best way to curb cravings during luteal?"
- "I've been in a calorie deficit for weeks and nothing has changed. What should I do?"

---

### `results`

The author is primarily sharing personal achievements, milestones, transformations, progress, or fitness outcomes.

**Key signal:** The focus is on sharing what happened to the author rather than asking for help or starting a discussion.

**Examples:**
- "Down 20 pounds after six months of lifting!"
- "Finally hit my goal of doing 10 pull-ups."

---

### `discussion`

The author is inviting opinions, experiences, perspectives, stories, or conversation from the community.

**Key signal:** The author wants to hear what others think or have experienced, rather than receive a specific solution.

**Examples:**
- "What's the worst fitness advice you've ever received?"
- "Are you ever finally happy with your fitness and looks?"

---

## How to Handle Ambiguous Cases

Some posts genuinely fit more than one label. That's expected. When this happens, label the post according to its **primary intent**, not its topic.

### Results Post That Ends With a Request for Help

Some authors share their progress, setbacks, or fitness journey before asking for recommendations.

**Example:**

> "I've lost 40 pounds but still don't see changes in my stomach. What should I do?"

**Label:** `advice`

The post contains progress information, but the author's goal is obtaining guidance.

---

### Discussion Post Phrased as a Question

Many discussion posts are written as questions.

**Example:**

> "Are you ever finally happy with your fitness and looks?"

Even though the post asks a question, it is seeking opinions and experiences rather than a specific solution.

**Label:** `discussion`

Ask:

> Is the author asking "What should I do?" or "What do you think?"

If the answer is "What do you think?" label it as `discussion`.

---

### Advice Post Asking for Other People's Experiences

Some advice-seeking posts request personal experiences as evidence.

**Example:**

> "Has anyone been able to maintain fitness while in nursing school? Any tips?"

Even though the author wants experiences from others, the purpose is to obtain guidance for their own situation.

**Label:** `advice`

If the author is trying to solve a personal problem, use `advice`.

---

### Personal Story Used to Start a Conversation

Some authors share a personal experience before asking the community for opinions.

**Example:**

> "I lost 10 pounds and noticed my bra size got smaller. Where have you noticed weight loss that you wish hadn't happened?"

**Label:** `discussion`

The personal story provides context, but the purpose of the post is community conversation.

---

### Emotional Posts, Rants, or Frustration

Some posts contain frustration, discouragement, or venting.

**Example:**

> "I've tried everything and I don't know what to do anymore."

If the author is ultimately asking for help, suggestions, or solutions, label it as `advice`.

If the author is primarily venting or inviting others to share similar experiences, label it as `discussion`.

---

### Advice vs. Discussion Boundary

This is the most important distinction in the dataset.

**Advice:**
- The author wants recommendations, solutions, strategies, or guidance.
- The post could reasonably be answered with "You should..."

**Discussion:**
- The author wants opinions, perspectives, experiences, or conversation.
- The post could reasonably be answered with "In my experience..."

**Examples:**

**Advice**
- "What activewear brand would you recommend?"
- "How do I stop binge eating?"
- "What supplements help with cravings?"

**Discussion**
- "What's the worst fitness advice you've ever received?"
- "Where did you lose weight that you wish you hadn't?"
- "Do you think reaching your goal weight makes you happier?"

---

### When You're Genuinely Unsure

Ask:

1. Is the author trying to solve a personal problem?
   - → `advice`

2. Is the author primarily sharing their own achievement or progress?
   - → `results`

3. Is the author primarily inviting opinions, experiences, or conversation?
   - → `discussion`

Label based on the author's **primary intent**, not the fitness topic being discussed.

---

## Valid Labels

```text
advice
results
discussion
```

## **Data Collection Plan**

I will collect posts from **r/petitefitness**. My goal is to gather approximately **50 examples per label**, resulting in a dataset of around **200 total posts**.

If one label is underrepresented after collecting 200 examples, I will use subreddit search filters, keywords, and older posts to intentionally gather additional examples from that category. If necessary, I will rebalance the dataset by collecting more examples from underrepresented labels.

---

## **Evaluation Metrics**

I will evaluate the classifier using **Accuracy, Precision, Recall, and F1 Score**.

- **Accuracy** measures the overall percentage of correctly classified posts.
- **Precision** measures how often predictions for a given label are correct.
- **Recall** measures how many posts belonging to a label are successfully identified.
- **F1 Score** balances precision and recall, making it especially useful when class distributions are not perfectly balanced.

Accuracy alone may be misleading if some labels occur more frequently than others, so Precision, Recall, and F1 Score provide a more complete evaluation of model performance.

---

## **Definition of Success**

I would consider the classifier successful if it achieves at least **80% overall accuracy** while maintaining reasonably balanced **F1 scores** across all labels.

For real-world deployment, I would want the classifier to correctly identify the primary intent of most posts without requiring substantial moderator correction. A model achieving approximately **85% accuracy** with strong per-label F1 scores would likely be useful for organizing posts, improving search functionality, or assisting community moderators.

---

## **Community Description**

**r/petitefitness** is a community where petite women discuss fitness, nutrition, weight management, and body composition goals. While members share a common interest in health and fitness, posts vary significantly in intent, ranging from advice-seeking and nutrition questions to progress updates and motivation-related discussions.

These distinctions matter because community members visit the subreddit for different purposes. Automatically identifying the primary intent of posts could improve content organization, make information easier to find, and help users connect with the most relevant discussions.

## **AI Tool Plan**

### **Label Stress-Testing**

Before annotating the full dataset, I will use Claude Code to stress-test my label definitions. I will give it my four labels, definitions, and hard edge case rules, then ask it to generate 5–10 example posts that sit near the boundary between two labels.

If the generated posts are difficult to classify consistently, I will revise my label definitions before collecting and annotating all 200 examples. This will help make the labels more mutually exclusive and reduce confusion during annotation.

### **Failure Analysis**

After evaluating the classifier, I will use an AI tool to help analyze the model’s incorrect predictions. I will provide the AI with examples of misclassified posts and ask it to identify patterns, such as confusion between `advice` and `discussion`

I will verify any patterns myself by reviewing the original posts and comparing them to my label definitions. If the model repeatedly confuses two categories, I will discuss whether the issue came from unclear labels, overlapping post types, limited training examples, or model limitations.