# DIAMONDs
A Dataset for Dynamic Information And Mental modeling Of Numeric Discussions
```
├── data
│   ├── base-conv-QA.jsonl
│   ├── distr-conv-QA.jsonl
│   └── underspec-conv-QA.jsonl
└── README.md
```

### Datasets
- **base-conv-QA** - Conversation QA dataset for base conversations
- **distr-conv-QA** - Conversation QA dataset for conversations that are distractor variants of the base conversation
- **underspec-conv-QA** - onversation QA dataset for conversations that are underspecified (unanswerable) variants of the base conversation

### TASK
Answer the **final_question** based on the **conversation**

### Data Dictionary
- 'id' : conversation (or scenario) ID
- 'qa_type': question ID for the conversation
- 'script': conversation Script that is translated into the **conversation**
- **'conversation'**: conversation based on which the answer to 
- 'question': question about a quantity based on the **conversation**
- **'final_question'**: Question from the Omniscient/Participant-centric view
- 'conv_access_grp': Group of participants sharing the same information access (having same view of the conversation)
- 'full_participants': All the participants involved in the conversation
- 'solution_gpt': Solution to the final_question (by GPT) based on the script (Was used for generating the ground truth)
- 'solution_claude': Solution to the final_question (by Claude) based on the script (Was used for generating the ground truth)
- **'answer'**: Final answer/ground truth

additonally *distr* & *underspec* datasets have **data_type** which indicates the approach type for creating the *distr*/*underspec* variant of the base conversation.

### Computing the accuracy

```python
def get_ans_equivalence(row):

    if row['true_ans'] == 'NA':
        return int(row['pred_ans'] == 'NA')
    elif row['pred_ans'] == 'NA':
        return 0
    else:
        return 1 if math.isclose(row['true_ans'], row['pred_ans'], rel_tol=2e-2) else 0    

def get_answerability_equivalence(row):
    return int(row['true_ans_type'] == row['pred_ans_type'])
```