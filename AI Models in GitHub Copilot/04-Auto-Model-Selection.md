# Auto Model Selection

## Overview

GitHub Copilot's **Auto** setting automatically selects the optimal AI model for each interaction. It balances quality, speed, and cost — so you get the best result without manually choosing a model every time.

---

## How Auto Works

When you select "Auto" as your model preference, Copilot evaluates each request and routes it to the most appropriate model based on:

| Factor | How It's Used |
|--------|---------------|
| **Task complexity** | Simple questions → lightweight models; complex reasoning → powerful models |
| **Context length** | Large context → models with long context tiers |
| **Task type** | Agentic tasks → Codex/agent-optimized models; chat → versatile models |
| **Model availability** | Routes around capacity constraints in real-time |
| **Cost optimization** | Selects the cheapest model that can handle the task well |

---

## Benefits of Using Auto

### 1. Cost Discount
Paid Copilot plan users receive a **discount on model costs** when using Auto. This makes it cheaper than manually selecting the same model yourself.

### 2. No Decision Fatigue
You don't need to know the difference between 20+ models. Auto handles it.

### 3. Adaptive Per-Interaction
Each message in a conversation can use a different model. A simple follow-up might use a lightweight model, while a complex debugging request in the same thread escalates to a powerful one.

### 4. Future-Proof
As new models are added or existing ones are improved, Auto automatically incorporates them without you changing anything.

---

## When Auto Is the Right Choice

| Scenario | Use Auto? |
|----------|-----------|
| General day-to-day coding | ✅ Yes |
| You're unsure which model to pick | ✅ Yes |
| Cost optimization is important | ✅ Yes |
| Mixed tasks (chat + code + debug) | ✅ Yes |
| You want the latest models automatically | ✅ Yes |

---

## When to Override Auto

| Scenario | Better Choice |
|----------|---------------|
| You need maximum reasoning for a hard problem | GPT-5.5, Claude Opus 4.8 |
| You want the absolute cheapest option | GPT-5.4 nano |
| You prefer a specific model's coding style | Select that model directly |
| You need long context guaranteed | GPT-5.4/5.5 or Gemini 3.1 Pro |
| You're doing autonomous/agentic work | GPT-5.3-Codex, Claude Fable 5 |
| You need consistency across a session | Pin a specific model |

---

## How to Set Auto

### In VS Code / IDE
1. Open Copilot Chat
2. Click the model selector (dropdown at top of chat)
3. Select **"Auto"**

### In GitHub.com
1. Open any Copilot Chat interface
2. The model selector shows available models
3. Select **"Auto"**

### As an Admin (Default for Org)
- Auto is the default selection for new users
- Admins can set recommended models but cannot force-lock to Auto
- Users can always override for individual interactions

---

## How Auto Differs from Manually Picking

| Aspect | Auto | Manual Selection |
|--------|------|-----------------|
| Cost | Discounted rates | Standard rates |
| Model choice | Varies per interaction | Fixed for session/conversation |
| Optimization | System-optimized | Your judgment |
| New models | Automatic access | Must manually select |
| Predictability | Less (model may vary) | More (you know what you get) |

---

## Tips for Getting the Best from Auto

1. **Write clear prompts** – Auto determines complexity from your prompt. Vague prompts may get routed to a cheaper model.
2. **Specify when you need deep analysis** – Phrases like "analyze thoroughly" or "think step by step" signal complexity.
3. **Trust it for routine work** – The cost savings add up over hundreds of daily interactions.
4. **Override only when needed** – If Auto's response isn't sufficient, try a more powerful model for that specific task.

---

## References

- [About Copilot Auto Model Selection](https://docs.github.com/en/copilot/concepts/auto-model-selection)
- [Changing Your AI Model](https://docs.github.com/en/copilot/using-github-copilot/asking-github-copilot-questions-in-your-ide#changing-your-ai-model)
