---
title: "Generative AI Kata: How We Won the Challenge?"
seoTitle: "Winning Strategies in Generative AI Kata"
seoDescription: "Winning the Generative AI Kata: Our journey, challenges, and solutions in modernizing a legacy application using AI and prompt engineering"
datePublished: Fri May 24 2024 03:54:12 GMT+0000 (Coordinated Universal Time)
cuid: clwk5dxvh00040ala3ip91o2q
slug: generative-ai-kata-how-we-won-the-challenge
tags: express, artificial-intelligence, reactjs, katas, cloud-native, modernization, generative-ai

---

Recently, some team members and I participated in a Generative AI Kata held across EPAM India. We won the challenge against other teams and received first prize. In this article, I will share details about our experience, the challenge itself, and how we solved it.

### **What is Generative AI Kata**

"[Kata](https://en.wikipedia.org/wiki/Kata)" is a Japanese word often used in martial arts to describe practicing different patterns, either alone or in small groups, to memorize and perfect them.

*Generative AI Kata is an immersive journey into the realm of artificial intelligence, where you'll delve into the art of creating, training, and fine-tuning AI models to generate novel and creative outputs. It's not just about coding; it's about unleashing the full potential of AI to drive innovation and solve real-world challenges.*

### Why should anyone attend Kata

**Deepen our AI Expertise:** Whether you're an experienced AI practitioner or just starting, Generative AI Kata offers a unique opportunity to deepen our understanding and master the latest techniques in AI development.

**Hands-on Exploration:** Given an interactive experience like no other! We can dive into hands-on exercises and start an exciting journey of discovery with our peers.

**Collaborative Energy:** We can join fellow tech enthusiasts, share ideas, and work together on exciting AI projects. Generative AI Kata is more than just an event; it's about a shared passion for exploring, learning, and growing together.

**Stay Ahead of the Curve:** In today's fast-changing tech world, keeping up is essential. Generative AI Kata gives us the knowledge and skills to stay at the cutting edge of AI innovation.

### How it works

**Step 1:** The panel explains the problem statement to all participants. Participants can ask any questions or request clarifications.

**Step 2:** Participants are divided into teams of 3-5 members.

**Step 3:** Teams will go to their discussion rooms and brainstorm different possible solutions.

**Step 4:** The team will come up with the best solution and present it to the panel in the open room. They should be ready to explain or answer any questions during this Challenge phase. This process is managed in a round-robin style by a facilitator.

**Step 5:** Finally, the jury will discuss and announce the top 2 winning solutions (teams) and give out rewards.

### Challenge

Below is the problem statement given to all participants during the Kata, along with the expected solution and artifacts that need to be delivered.

**Problem Statement:**

*Our QR code generator app, despite its long-standing presence and large user base, is facing issues due to its outdated legacy monolithic application using Winforms. The lack of documentation and turnover in personnel has further complicated the modernization process of the application. This is hindering our market competitiveness and affecting the app's performance, scalability, and maintainability.*

Below is the current application, developed in WinForms - a .Net-based desktop application.

![QR Code Generator with Logo photo](https://raw.githubusercontent.com/ArdeshirV/QrCodeGeneratorWithLogo/main/QrCodeGeneratorWithLogo/img/OuP.jpg align="center")

Major features:

* Generate a QR code for the given URL
    
* Embed the image as a logo in the generated QR code
    
* Provision to download and save the image
    
* Capability to display the logo background with various shapes and colors
    

*Source:*[*https://ardeshirv.github.io/QrCodeGeneratorWithLogo/*](https://ardeshirv.github.io/QrCodeGeneratorWithLogo/)

**Solution Required:**

*The solution involves a strategic approach of static whitebox reverse engineering. This includes analyzing the current state of the application, defining the desired state, and conducting a gap analysis. The key deliverables from this process will be a comprehensive documentation of the current state of the application and a detailed gap analysis report. This will help us enhance the performance, scalability, and maintainability of the app, thereby improving our competitiveness in the market.*

Also, a major point to note is that we should leverage GenAI to accomplish these tasks. The submitted solution will be evaluated based on the below criteria.

* Prompt Effectiveness & Techniques Used
    
* Less Human Intervention
    
* Context given to LLM
    
* Output Expectations
    
* Design Patterns used
    
* Tech Stack-specific Output
    
* And other documentation
    

### How we solved it

As planned, each team should consist of 5-6 members with roles such as Architect, Lead Developer, Automation Engineer, Product Owner, and Testing Engineer. However, due to unforeseen circumstances, our Architect and Lead Developer didn't join the session. This left us with just myself (Lead Automation Engineer/SDET) and two other colleagues, both of whom are Senior Automation Engineers. Also, there is no Product Owner assigned to our team!

We have only 2 hours to complete the major task of modernizing the given legacy and monolithic application. Except for myself, the team has limited knowledge of application architecture and development. We accepted the challenge and took on multiple roles to complete it.

***While the requirement was to use GenAI to generate the solution and documentation, we used EPAM's own*** [***DIAL***](https://epam-rail.com/platform)***(a tool similar to ChatGPT with access to multiple LLM models from different vendors) as our tool.***

* **Acting as a Business Analyst:**
    
    * We began brainstorming on the given application. We used the desktop application as a user to understand its functionality. From this, we identified the necessary features for our new application.
        
    * With the necessary functionalities as input, we used prompt engineering to define the high-level requirements for our new application. We included columns such as *Requirement ID, Requirement Description*, and *Priority* in the prompt, and the LLM successfully generated the output as we intended.
        
    * We even included features for the performance, security, and usability requirements of the application.
        
    * After this, we gathered all the data into a table format and created our final requirement document.
        
    * The final document we submitted to the Jury is available here on [GitHub](https://github.com/rakesh-vardan/Team_C/blob/main/Documentation/Requirements.md) for reference.
        
    * The exported prompts from GenAI are available [here](https://github.com/rakesh-vardan/Team_C/blob/main/Prompts/BusinessAnalyst.json). This includes our entire conversation with the AI acting as a business analyst.
        
* **Acting as an Architect & Lead Developer**
    
    * This was the most challenging part of the entire session for us.
        
    * We provided the LLM with all the technical details about the application, the challenges we faced with the monolith and our goals. We aimed to gather information on a new architecture for the app that would be modular, scalable, and efficient using the latest technologies.
        
    * Based on the prompts and AI output, we decided to build or migrate the application into different modules, consisting of front-end and back-end layers.
        
    * For the backend, we chose [*ExpressJS*](https://expressjs.com/) and began with prompts to obtain the necessary backend logic for the application. We used the existing npm library [qrcode](https://www.npmjs.com/package/qrcode) and added a single route for generating the image in the `app.js`.
        
    * With the core back-end functionality working well, we focused on building the front-end application as a separate module. Using the prompts, we chose ReactJS as our solution and obtained the necessary code to build a simple web application.
        
    * After we integrated and started both apps, we encountered CORS errors, and the application wasn't working. With the help of AI, we resolved the issue by adding a new npm dependency, [`cors`](https://www.npmjs.com/package/cors), and some new configurations for the backend app.
        
    * Voila! Now both the frontend and backend apps are integrated successfully, and the application is running smoothly in a browser on our local systems. Finally, we can pass a URL to the QR code generator app, and it generates the image as expected.
        
    * We improved the app's appearance and added new features like image download and disabling the URL textbox until the user enters text. All these enhancements were made using prompt engineering via AI.
        
    * We even generated the high-level architecture diagram and the README.md file for the repository using only prompts.
        
    * Our application architecture is shown below. We used AI-generated code snippets along with [PlantUML](https://www.plantuml.com/) to create this diagram.
        
        ![Architecture](https://github.com/rakesh-vardan/Team_C/raw/main/images/QR-arch.png align="center")
        
    * The final GitHub code repository we submitted to the Jury is available [here](https://github.com/rakesh-vardan/Team_C) for reference. It includes both [front-end](https://github.com/rakesh-vardan/Team_C/tree/main/qr-generator-frontend) and [back-end](https://github.com/rakesh-vardan/Team_C/tree/main/qr-generator-backend) code.
        
    * The exported prompts from GenAI are available [here](https://github.com/rakesh-vardan/Team_C/blob/main/Prompts/ApplicationDevelopment.json). This includes our entire conversation with the AI acting as an Architect and Developer.
        
* **Acting as a Test Lead & Test Engineer**
    
    * While developing the application, we also used similar prompting techniques to generate the artifacts for testing.
        
    * After setting the application context and going through multiple iterations of prompts, we created a detailed Test Strategy document. You can refer to the final document [here](https://github.com/rakesh-vardan/Team_C/blob/main/Documentation/TestStrategy.md).
        
    * Developing all the features within the given time frame wasn't possible. So, we focused on some critical features and created an MVP release for the migration. Based on these prioritized features, we created a test plan to execute the testing activities and validate the application.
        
    * Initially, the generated content was generic, but we explicitly changed our prompts to include the necessary information for some sections. As a result, our final [Test Plan](https://github.com/rakesh-vardan/Team_C/blob/main/Documentation/TestPlan.md) is ready.
        
    * Next, we created the required test [cases](https://github.com/rakesh-vardan/Team_C/blob/main/Documentation/TestCases.md) to validate the application and compiled them in a table with all the necessary columns.
        

### Demo

Once we finished developing our application, we manually validated the scenarios. We didn't have time to prepare a deck with our design and artifacts, so we used the README.md file as our presentation to the jury. We explained our approach to modernizing the application, our different roles, and how we achieved the desired results using Generative AI and prompt engineering. The jury had some questions and clarifications. They also asked us to show the documents we created and the prompts in the DIAL. We explained and demonstrated them to the best of our knowledge.

Here is the application that we developed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1716128457418/0474b838-d679-4af4-a836-c95f17e28aa5.gif align="center")

### Lessons Learned

Winning the Kata does not mean that I and my team made everything perfect and implemented all requirements. Not at all. Given the 2 hrs time, we needed to make trade-offs and skip certain implementations for the application.

* We missed generating some important documents like Gap Analysis for the given application.
    
* We couldn't implement some of the existing functionalities like adding a logo to the image, shapes, and colors for the logo, etc.
    
* We focused only on the core functionality of generating and downloading the QR image, providing the simplest and most effective solution possible.
    
* It also took some time to get to know each other and plan the activities for the task, as the team members were new and unfamiliar with each other. We needed time to distribute the tasks and understand each other's roles in the session.
    
* Despite these issues, we used the given time to create a minimum viable working product for the jury.
    
* Other teams also worked very hard and gave great presentations. One team used Python for their app and built all the functionality. Another team generated detailed documentation but did not create a working solution. Each team had its own unique experience.
    

### Conclusion

It was great to be part of this Kata and collaborate with others to focus on a common goal, which helped us to expand our capabilities. Despite team constraints and limited time, we successfully created a modular, scalable application. We leveraged Generative AI and used prompt engineering, generated documentation, architecture diagrams, and test plans. Our solution focused on core functionality and effective collaboration, leading to our victory.