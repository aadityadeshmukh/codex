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

## Introduction to LLMs for text generation

## Text Generation: standard practices

## Advanced text generation with Langchain

## Vector databases with FAISS and Pinecone

## Autonomous agents with memory & tools

## Diffusion models for image generation

## Image generation: Standard practices with mid-journey

## Image generation: Advanced practices with stable diffusion

## Building AI-powered applications
