#course_cs50 

> [!example]
> AI is now all around us, with the technology enabling innovations such as the following:
> 
> - LLMS are all the rage with ChatGPT, Claude, and DeepSeek becoming popular in the last few years. CS50 also has an LLM in the form of a virtual rubber duck to help students with debugging.
>   
> - DALL-E 2 & Midjourney were all the rage in the last few years. There were some tells in the generated images in the early days - AI wasn't great at generating the finer details such as fingers.
>     - However the technology is constantly getting better, this lecture started with a deepfake video of Tom Cruise.
>     - Disinformation is going to become more challenging in a world where images and videos can be 'faked'.

# CS50's Duck Debugger

- Architecturally, here's how the ddb works.
    - A student asks questions to the `CS50.ai`.
    - A vector database containing the script of the lectures for this year as well as past homework and material is first searched and handed over to the model.
    - The question (the *user prompt*) is then also passed to Azure's OpenAI API to handle most of the LLM heavy lifting.

![[Pasted image 20250222231349.png]]

- The devs behind `CS50.ai` focused mainly on *prompt engineering* to ensure the chatbot acts like a good teacher.
    - A *system prompt* is sent to the ddb to coerce it to behave more educationally and constructive. e.g:

> You are a friendly and supportive teaching assistant for CS50. You are also a rubber duck. Answer student questions only about CS50 and the field of computer science; do not answer questions about unrelated topics… Do not provide full answers to problem sets, as this would violate academic honesty….

- The ddb has been extended to other aspects of CS50 - the goal is to approximate a 1-1 student to teacher ratio.
    - The virtual duck now also lives in `CS50.dev`, the web browser-based Visual Studio Code instance for CS50. It can now explain highlighted code.
    - It can also provide style hints as well as the reasoning behind those suggestions.
    - It can answer common questions that students may ask in the course's forums, basically providing students with 24/7 office hours.
        - Given the experimental nature of LLMs, a disclaimer is provided to the student that tells them to not take the response as fact unless 'endorsed' by a member of human staff
    - It can explain arcane error messages.

# Generative artificial intelligence

- AI is a technology that has been with us for some time, however the past 5-6 years have allowed us to take a massive leap forward in what tools such as ChatGPT can do.

> [!example]
> Examples of the use of generative AI are:
> 
> - Email spam detection
> - Handwriting detection
> - Recommendation histories in services such as Netflix
> - Siri, Google Assistant, Alexa

# Decision trees

- This is how we can start translating human intuition into a crude artificial intelligence.