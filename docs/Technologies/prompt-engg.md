---
hide:
    - navigation
    - toc
    - path
---
# Prompt Engineering

## Principles of prompting

??? note "Explore"
    ### Introduction
    - **What is a prompt?**
        - *A prompt is an input to be given to an AI model. It serves as a set of instructions to the large language model.*

    - **What is prompt engineering?**
        - *The process of discovering prompts that yield desired results reliably.* 

    - **General issues in prompts given to AI models:**
        - *Vague direction*
            - *Tell AI proper direction to take. Style emulation, language levels etc.*
        - *Unformatted output*
            - *Tell what kind of output is needed. For example csv.*
        - *Missing examples*
            - *Provide examples of what a god response looks like.*
        - *Limited evaluation*
            - *Setup a evaluation methodology to understand how the prompt is performing.*
        - *No task division*
            - *In many tasks there are multiple steps that need task specialization. This needs task specialization.*

    - **Tokens and Token optimization**
        - *LLMs work by continuously predicting the next token starting from what was there in the prompt.*
        - *A token is generally 3/4th of a word.*
        - *Each new token is predicted based on the probablity of it appearing next.*
        - *This is controlled by the temperature parameter.*
        - **Average prompts give average responses.**
        - *AI companies charge by the tokens i.e. the number of tokens used in the prompt and response.*
        - *This means we need to optimize for tokens to get the best output in terms of cost, quality and reliability.*

    ### Five principles of prompting

    | Principle      | Description                          |
    | ----------- | ------------------------------------ |
    |Give Direction|Specify style or persona|
    |Specify format|Define rules, structure of the response|
    |Provide examples|Diverse set of tests that define task done successfully|
    |Evaluate quality|Identify errors, rate responses, drive performance|
    |Divide labor|chain tasks to achieve complex goals|

    These principles are model agnostic and work equally well for any type of response generation.

    - For reference on how to apply these principles use the below 1 pager guides:
        - [Text generation one-page prompt guide](https://github.com/BrightPool/prompt-engineering-for-generative-ai-examples/blob/main/images/OnePager-Text.png)
        - [Image generation one-page prompt guide](https://github.com/BrightPool/prompt-engineering-for-generative-ai-examples/blob/main/images/OnePager-Images.png)

    #### 1. Give Direction

    - **Human-like brief**
        - For this step imagine what brief a human would require to work on the task. 
        - Using a similar brief to ask AI To do the task would do the trick most of the times.
        - Although its not a 1-1 mapping
    - **Role playing** 
        - E.g. ask AI to name a product how Elon musk or Steve jobs would do.
        - Provide examples related to *role playing* that will set the tone for the response. E.g. provide product description and ex. product names
    - **Pre warming or Internal retrieval**
        - Start the conversation by asking for best practice advice.
        - Ask AI to follow its own advice.
        - This way you can ask AI to generate its own direction.
    - **Best advice out there strategy**
        - Take the best advice available and insert it into the prompt.
        - This one can make the prompt longer but can improve the result significantly.
        - Consider the cost tradeoffs though if using API for example.
    - **Scale back**
        - Sometimes too much instruction could be bad. It can restrict the creativity of the model.
        - As there might not be enough training data out there to generate a response.
        - So in this case scale back on some aspects of direction needed and try to get a desired response.
        - Prioritize which elements are important.

    #### 2. Specify format
    - AI models tend to change output formats for the same prompt given multiple times if not specified.
    - Hence, to keep the output consistent its important we specify the format.
    - This helps us to plan as well as we can use the output of the prompt for another step.
    - For eg. if we specify a json output we can use it to render front end.
    - Format can be specified the following ways:
        - One off: In the prompt text itself. Eg. Share a list in json format.
        - Chain of prompts: Specify at the start and set as a persona. Eg. You are an assistant that responds in json.
        - Setup in model parameters. Called grammars in llama models.
    - Remove other aspects from the prompt that might clash with specified format.
    - "The more layers of unrelated elements the more likely you are to get an unsuitable image."
    - If there is a clash in the specify format principle and give direction principle lose the one less important.

    ### 3. Provide examples
    - AI models are few shot learners.
    - The reliability of an output increases tremendously if a few examples are provided instead of none.
    - The tradeoff is creativity as more examples help provide constrains to the ask which reduces the creative index.
    - Providing 3-5 examples will give a very reliable output at the cost of creativity.
    - Providing 1-3 examples has a positive effect almost always.
    - Above 3 consider the effect it might have and provide diverse examples to cover edge cases.
    - Rule of thumb. Provide direction first then provide examples.
    - For image generation providing base image is considered as giving an example.

    ### 4. Evaluate quality
    - There is a need to evaluate quality when same prompt is relied on for production usecase.
    - Having a simple thumbs-up/down system without adding much overhead to the optimization process.
    - Another test is to see if examples are worth the additional cost as compared to giving no examples at all.
    - Often while evaluating quality many elements of the prompt can be deemed superfluous or unnecessary.
    - Removing them can shorten the prompt and improve latency resulting in overall scaling of operations.
    - For images quality can be evaluated by permutation prompting.
    - Provide a variety of input and do a side by side comparison / use classifiers for deciding the validity of each input.

## Introduction to LLMs for text generation

## Text Generation: standard practices

## Advanced text generation with Langchain

## Vector databases with FAISS and Pinecone

## Autonomous agents with memory & tools

## Diffusion models for image generation

## Image generation: Standard practices with mid-journey

## Image generation: Advanced practices with stable diffusion

## Building AI-powered applications
