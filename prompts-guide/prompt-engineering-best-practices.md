<span id="prompt-engineering-简介"></span>
## Prompt engineering
In natural language processing (NLP) and conversational systems, a prompt is usually text or a question entered by a user. With careful prompt crafting, models can generate output more suitable to user needs.
Prompt engineering refers to the process of designing and optimizing prompts so that AI-based models can better understand the user intent and requirements to generate more accurate and useful responses. Prompt engineering aims to:

* Learn how to format and design prompts to make models work best.
* Explore the impact of different prompts on model output.
* Create prompts that optimize model output.

In general, the whole process is divided into prompt design, prompt optimization, and prompt evaluation.
<span id="prompt-设计"></span>
## Prompt design
**Goal**: Select the appropriate prompt format and language to clearly and unambiguously communicate the user intent.
**Process:** Clarify the purpose, that is, what you want the model to do. Then, create prompts while keeping the following points in mind:
<span id="提供更多-query-相关的细节，可以获得更准确的答案"></span>
### Provide more query-related details to obtain more accurate answers.

| | | \
|**Original** |**Optimized** |
|---|---|
| | | \
|Write an article on space exploration. |Write an article on the history of space exploration for a group of kids who are 10-15 years old. |
| | | \
|Please write an article about environmental protection in 500 words. |Write a 500-word article discussing the impact of urban greening on air quality improvement. The article should include the following content: the definition of urban greening, such as more trees and parks, how they can reduce air pollutants, and the feasibility of promoting urban greening in urban planning. Please provide relevant data and case studies to support your argument. |

<span id="使用分隔符去更清晰地区分输入的不同部分"></span>
### Use separators to distinguish different parts of the input.
```bash
Please summarize the three texts enclosed in quotation marks into one sentence
'''Text content 1'''
'''Text content 2'''
```

<span id="引导结构和组织要求"></span>
### **Guide the structural and organizational requirements** In this process, clarify the structural and organizational requirements for your task. Specify the content to be contained in different parts of the task, which helps the model organize thinking and materials.
```plain
Please write a 500-word article discussing the impact of urban greening on improving air quality. The article should include the following aspects:
    1. Introduction: Introduce urban greening and its importance.
    2. Mechanisms of affecting air quality: Explain how trees and parks reduce pollutants in the air.
    3. Feasibility measures: Discuss the methods and challenges of promoting urban greening in urban planning.
    4. Data and case studies: Provide relevant data and at least two successful cases of urban greening to support your argument.
    5. Conclusion: Summarize the positive impact of urban greening on air quality
```

<span id="限制模型输出格式"></span>
### Output format
Restricting the output format of the model can improve readability of the result and facilitate subsequent processing with a higher stability.
Below is a sample demonstrating how to extract symptoms. The required output format is JSON.
```sql
Please extract all the symptoms in the reference materials and return them in json format.
The answer meets the following format requirements:
1. Return the answer in json format. json only includes one key, key="disease", and the corresponding value is a list, which stores the symptoms in the reference materials.
Reference materials:
"""
Insomnia is called "eyes cannot close", "cannot sleep", and "cannot lie down" in the "Inner Canon of Medicine". There are two main reasons: one is the influence of other symptoms, such as coughing, vomiting, abdominal distension, etc., which make people unable to sleep; the other is the disharmony of qi, blood, yin and yang, which makes people unable to fall asleep. Traditional Chinese medicine often uses the method of nourishing the heart and calming the mind to treat insomnia, which can not only treat the symptoms but also the root cause, and can also avoid the drawbacks of easy addiction to Western medicine sleeping pills. Traditional Chinese medicine believes that insomnia is mostly caused by the imbalance of yin and yang in the internal organs and the disharmony of qi and blood. As recorded in "Lingshu Dahuo Lun": "Wei Qi cannot enter the yin, and often stays in the yang. Staying in the yang makes the qi full; Yang Qi is full, Yang is strong, and it cannot enter the yin, so the yin Qi is weak, so the eyes cannot close. "In clinical practice, the treatment of insomnia should focus on regulating the internal organs and qi, blood, yin and yang, so as to "replenish the deficiency, drain the excess, and adjust the deficiency and excess". The treatment methods can be adopted to nourish the heart and spleen, nourish yin and reduce fire, connect the heart and kidney, soothe the liver and nourish blood, replenish qi and calm the nerves, and activate blood circulation and dredge the collaterals, so as to make the qi and blood smooth, balance yin and yang, and restore the normal function of the internal organs.
"""
```

<span id="prompt-优化"></span>
## Prompt optimization
<span id="让模型扮演一个角色"></span>
### Roleplay
You can let the model play a specific role, so that it can output more understandable and consistent results. For example, in a Q&A system, the model can be asked to act as a subject matter expert. In this case, it can offer answers in line with the knowledge and language habits of the field, thus improving the consistency of the answers.
In the following sample, the model is asked to generate an article themed How Do Black Holes Form as a scientist and a fantasy author. As a scientist, the model explains what a black hole is and how it forms. As a fantasy author, the model is no longer based on scientific facts and gives a fictional and mysterious answer, arousing the reader's interest.
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ccf3befc961a6a09afd34885084c602e.png =767x)
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_6e6499dc7ecdd9e4afb30cf38f15092d.png =769x)
<span id="提供样例"></span>
### Samples
In general, you can add instructions to optimize prompts. If instructions are unclear, provide samples. You can use the model as a binary text classifier to classify user reviews as positive or negative.
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_6585b8526d804eb34fb9a4b4cb0b5293.png =763x)
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a9e1aecc488bc83b209f8f71dbccff2e.png =768x)Under normal circumstances, the model classifies reviews as negative only if they are completely negative. But the goal is to have **the model classify reviews as positive only if they are absolutely positive**. In the case of "I recently ate at this restaurant and thought it was okay but not amazing", the model should classify it as negative. At this point, some samples can be provided for the model to learn from.
```plain
Please help me distinguish whether the user input text is a positive review or a negative review according to the following classification method. Please directly output: positive review/negative review.
Please refer to the following examples:
Example 1:
User input: I went to this restaurant last night and their food and service were amazing. I will definitely patronize again.
Output: positive review
Example 2:
User input: I have read this book. Some plots are okay, but the overall plot is dragging and average.
Output: negative review
Example 3:
User input: I watched this movie yesterday. I think it’s okay, but some parts are a bit boring.
Output: negative review
Example 4:
User input: I watched this movie last week. It was a waste of time. The plot was boring and the actors’ performances were not satisfactory. I really regret watching it.
Output: negative review
Please answer the following questions:
User input: I recently dined at this restaurant. It was okay, but not particularly amazing.
Output:
```
In some difficult cases where labels are not enough, you can offer reasons to help the model understand the intent.
```bash
Please help me distinguish whether the user input text is a positive review or a negative review according to the following classification method. Please output: positive review/negative review and give reasons.
Please refer to the following examples:
Example 1:
User input: I went to this restaurant last night and their food and service were amazing. I will definitely visit again.
Output: Positive review, reason: Food and service are amazing, and customers will visit again
Example 2:
User input: I have read this book, some plots are okay, but the overall plot is dragged and is relatively average.
Output: Negative review, reason: The overall plot is dragged
Example 3:
User input: I watched this movie yesterday, I think it's okay, but some parts are a bit boring.
Output: Negative review, reason: Some parts of this movie are also a bit boring
Example 4:
User input: I watched this movie last week and it was a waste of time. The plot is boring and the performance of the actors is also unsatisfactory. I really regret watching it.
Output: Negative review, reason: The plot was boring, the actors' performances were not satisfactory, and the user regretted watching it.
Please answer the following questions:
User input: I recently dined at this restaurant, it was okay, but not particularly amazing.
Output:
```

<span id="指定完成任务需要的步骤"></span>
### Task steps
In a specific task, specifying required steps can help the model arrange execution and generate better output.
```bash
Please follow these steps to write a story:
1 Set up the story background and characters.
2 Describe the characters' goals and difficulties.
3 Tell how the characters overcome difficulties and finally achieve their goals.
4 End the story with an interesting ending.
```
In this sample, the storytelling steps are explicitly specified, and the model can conceive and organize the story structure more methodically, thus improving the quality and coherence of the story.
<span id="提供参考内容"></span>
### Reference content
For a domain-specific problem, providing reference content can help the model better understand the background and context of the question, thus reducing hallucinations.
```bash
Please refer to the following document to answer user questions:
###Document name: Document content
###Question: [Problem description]
```

<span id="给模型-“思考”时间"></span>
### Giving the model time to think
Chain of Thought (CoT) helps the model think more deeply and come to more complex and comprehensive conclusions by gradually extending and expanding an idea. In some scenarios such as logical inference and mathematical operations, CoT can be used for decomposition to increase the probability of the model outputting correct results with some inference processes.
<span id="zero-shot"></span>
#### Zero-shot
Add `Let's think step by step` to the prompt``.
<span id="few-shots"></span>
#### Few-shot
Below is a sample of performing addition, subtraction, multiplication, and division operations on the numbers entered by a user.
```sql
You are a calculator. Please add 2, subtract 3, multiply by 3, and divide by 2 to the number input by the user, and then directly output the calculation result, using ',' as the separator.
The example is as follows:
"""
Input: 1, 2, 3, 4, 5
Answer: 0, 1.5, 3, 4.5, 6
"""
Input: 2, 4, 6, 8, 10
```
It can be found that the model failed to provide the correct answer. In this case, you can follow these steps: provide samples > decompose the question > provide explanations.
Below is a sample of CoT prompting, with step-specific explanations.
```sql
You are a calculator. Please add 2, subtract 3, multiply by 3, and divide by 2 to the number input by the user, and then directly output the calculation result, and return it with ',' as the separator.
You can refer to the following calculation process to help solve it.
"""
For input: 1, 2, 3, 4, 5
The calculation process is as follows.
First, add 2 to the input 1, 2, 3, 4, 5, and get: 3, 4, 5, 6, 7
Then subtract 3 from 3, 4, 5, 6, 7, and get: 0, 1, 2, 3, 4
Then multiply 0, 1, 2, 3, 4 by 3, and get: 0, 3, 6, 9, 12
Finally, divide 0, 3, 6, 9, 12 by 2, and get: 0, 1.5, 3, 4.5, 6
The answer is: 0, 1.5, 3, 4.5, 6
"""
Input: 2, 4, 6, 8, 10
```

<span id="prompt-评测"></span>
## Prompt evaluation
<span id="系统地测试变更"></span>
### Systematical test on changes
After prompt design and optimization are completed, you can test whether your prompts have improved the system. You can check a few samples but cannot tell if the results are real improvement or luck due to the small number of samples. Therefore, it's necessary to design an evaluation set and iterate it several times before system evaluation.
A good evaluation set typically has the following characteristics:

* Diversity: The evaluation set includes sufficient diversity to cover different fields, topics, and contexts.
* High quality: The data in the evaluation set is of high quality and accurately reflects the business situation.
* Moderate size: The evaluation set is large enough to adequately evaluate the performance of prompts but not so large to overconsume computing resources. It's reasonable to include hundreds and thousands of samples.

<span id="重复和迭代"></span>
### Repetition and iteration
The process of prompt generation is highly experimental, where different methods need to be tried and adjusted to find the best prompt. In a typical iteration path, you should design prompts, obtain the experimental results based on the designed prompts, analyze bad cases, solve bad cases, and optimize the prompts. The whole process may be repeated several times before the optimal effect is obtained.
The prompt engineering iteration process is as follows:
![Image](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ad43efa221ab096b5ec4cb045b90e632.png =966x)Note that the best output depends on not only good prompts but also feedback and corrections from the user. After prompt optimization is completed, the model understands and meets user requirements through continuous feedback and corrections online.
<span id="附录"></span>
## Appendix
<span id="参数设置"></span>
### Parameter settings
<span id="关键参数"></span>
#### Key parameters
When using prompts, you interact with large language models through the experience center or APIs. Different parameters contribute to different prompts.

* temperature: It controls the randomness of the output. The higher the value, the greater the randomness. The lower the value, the smaller the randomness. In a classification task, set `temperature` to a lower value to get a more realistic and concise answer from the model. For poetry generation or other creative tasks, increase the value of `temperature` for greater diversity and creativity.
* top_p: It controls the degree of certainty of the model in generating responses, thus affecting the diversity and creativity of the generated results. When the prompt is long and clear enough to trigger an output of a high quality and confidence level, Top_p can be set to a higher value. However, if the prompt is short and vague, a higher Top_p value may lead to an unstable output.
* repeat_penalty: It controls the repetition of the output. Value range: 1.1–1.3.

<span id="参考配置"></span>
#### Reference configuration
Default: temperature = 0.7, top_p = 0.9
Classification or tasks requiring a stable output: temperature = 0.01, top_p = 0.7
Tasks that require diversity and creativity: temperature = 0.7
The actual business situation prevails.
<span id="关键概念"></span>
### Key concepts
**Token:** A Chinese word, an English word, a number, or a symbol is counted as a token. Due to the different tokenizers used in different models, the same piece of text may correspond to different numbers of tokens.
<span id="参考模版"></span>
### Reference templates
<span id="任务型模版"></span>
#### Task-based templates
```bash
If you are {a certain role}, you will solve {a specific task} based on {context information}. Follow the following rules step by step:
1. Rule 1
2. Rule 2
Reference example:
Example 1:
Question: {specific question}
Output: {result of the question}
Example 2:
Question: {specific question}
Output: {result of the question}
Please answer the question:
Question: {specific question}
Output:
Requirements:
1 Specify the output format
2 Detailed specifications that need to be met in the format
```

<span id="角色型模版（生成system-prompt）"></span>
#### Role-based templates (system prompts)
A system prompt provides information and instructions to the model. Write a system prompt in the second person****.
```bash
You are {a specific person}, called {xxx}, born in {explain background information and context}.
Personality traits:
Language style:
Interpersonal relationships:
Past experiences:
Classic lines or catchphrases:
{Lines 1 (Supplementary information: You can put actions, expressions, tone, psychological activities, and story background in () to provide supplementary information for the dialogue.)}
{Lines 2}
```

<span id="参考代码（python3）"></span>
### 


