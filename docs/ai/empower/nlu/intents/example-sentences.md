---
title: "Example Sentences" 
slug: "example-sentences"
description: "Cognigy.AI Intent Example Sentences are phrases that represent the possible ways a user might express their intention or request to the AI Agent."
hidden: false
tags:
  - Cognigy NLU
  - intents
  - intent example sentences
---

# Intent Example Sentences

* _Intent Example Sentences_
  * == phrases / 👀enables the AI Agent -- to recognize a -- larger variety of user inputs👀
    * Reason: 🧠train the NLU model🧠

## Limitations

- <= 2000 example sentences / Intent
- \>=5
  - == MINIMUM number of example sentences 
  - if you use fewer -> | complete the build, appear [red indicator](intent-analyzer.md#explore-intent-analyzer)

## Managing Intent Sentences

* TODO:
??? info "Create Intent Sentences"

    === "Manually"
        To create an Intent sentence, go to the Intent in **NLU > Intents** in your Flow.
        Enter the example sentence and press ++enter++ or ++tab++.
    
    === "Automatically (Generative AI)"
         To use Generative AI for creating Intent example sentences, read the [Generative AI](../../../empower/generative-ai.md) article.

??? info "Delete Intent Sentences"
    If you need to delete or remove example sentences, you can hover over the sentence and click ![recycle bin](../../../../_assets/icons/recycle-bin.svg).

### Recommended Number of Example Sentences

For optimal performance, use 5-100 example sentences.
With this range, you can provide the AI Agent with enough data to recognize the Intent and avoid overcomplicating the model with unnecessary examples.

If you don't meet the minimum of 5 example sentences or exceed the maximum of 100, Cognigy.AI shows a warning icon prompting you to adjust the number of sentences. 

### Tips for Writing Effective Example Sentences

- **Avoid punctuation marks**. Keep the sentences simple and without punctuation. Using punctuation marks in example sentences, such as commas and question marks, can reduce recognition accuracy.
- **Avoid repetition**. Don't write similar sentences like `I want to order Pizza` and `I want to order Pasta`. Instead, use [Lexicons](../slots/user-defined/lexicon.md) to group similar terms and attach them to your Flow. This way, you can create a more general sentence like `I want to order food`, which will automatically recognize `Pizza` and `Pasta` as food-related items.
- **Use natural language instead of placeholders**. Avoid placeholder variables like `LEX_SLOT_123` and limit uppercase usage, for example, don't write `FOOD` in all caps unless it's an abbreviation. 
- **Ensure uniqueness**. Make sure your example sentences are different from each other. If they're too similar, the AI Agent may struggle to differentiate Intents, which could cause incorrect results or trigger confirmation sentences often.

## More Information

- [Overview](overview.md)
- [Annotations](annotations.md)
