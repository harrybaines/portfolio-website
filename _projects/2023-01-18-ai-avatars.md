---
title: AI Avatars
desc: Dreambooth Stable Diffusion to create high-quality, personalised AI avatars
date: 2023-01-18
thumbnail: /assets/images/projects/2023-01-18-ai-avatars/avatar.png
layout: project-page
tag: Machine Learning
code_link: https://github.com/harrybaines/ai-avatar-generator
weight: 2
---

Recently I fine-tuned Stable Diffusion using Dreambooth to create an AI avatar generator model. The model was trained on Google Colab (took about 40 minutes to train) and is now hosted on the Hugging Face platform. I also built a user interface with React.js and Next.js, connected to the Hugging Face API endpoints to access the trained model, and deployed the final app on [Railway](https://railway.app/){:target="_blank"}. The user can enter a prompt and receive an AI avatar as a response (due to the model being tied to a billing account, this will remain private for now!):

<img src="/assets/images/projects/2023-01-18-ai-avatars/UI.png" style="width: 100%;" alt="ai-avatars-ui" />

I also decided I wanted to create variations of images given my input prompt, so I decided to build a Discord bot in Python to provide a convenient chatbot-like interface to my model in my private Discord server. This made it even easier to generate my avatars.

The overall workflow is as follows:

- The user inputs a prompt in a Discord slash command.
- The prompt is sent from the frontend to the Hugging Face API endpoint (via Python) where the trained model is hosted. For X variations, we send X requests to the endpoint.
- Receive the generated avatars from all the API responses and stich the images together in Python.
- Return the final image to the requester, which can then be downloaded.

The results are pretty insane!

<img src="/assets/images/projects/2023-01-18-ai-avatars/discord.png" style="width: 100%;" alt="ai-avatars-ui" />