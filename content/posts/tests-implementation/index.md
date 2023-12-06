---
title: "Breaking Barriers: A Practical Guide to Starting Unit Testing in Your JS/TS Development Team"
date: 2023-12-02T12:38:55+02:00
draft: false
---

In the fast-paced world of software development, the idea of implementing unit testing might seem like a daunting task,
especially when your codebase is already extensive. Many believe it's too late or too challenging to start, and the fear 
of disruption can be paralyzing. This article aims to dispel those concerns and serve as a practical guide for teams 
hesitant to embark on the journey of unit testing. Join us as we explore tangible steps to initiate unit testing, 
enhance code quality, and foster a culture of testing within your development team.

## My Own Thoughts about Testing
*Principle: Tests as the First Step*\
Tests, as the first step, assured developers that their code worked correctly. This shift in mindset marked the
beginning of a more structured and confident development process.\
*Principle: Tests as Documentation*\
Tests were recognized not only as a means of verification but also as documentation for complicated code.
This documentation aspect eased collaboration and knowledge transfer within the team.\
*Principle: Finding Bugs Before QA*\
The integration of mutation testing highlighted the ability to find and fix bugs before they reached the QA stage.
This proactive approach earned the development team a reputation for reliability.\
*Principle: Well-Designed Code through Testing*\
Fully testable code became synonymous with well-designed code. The comprehensive test coverage not only ensured
correctness but also exposed code smells, fostering a culture of clean and maintainable code.\
*Principle: Faster Feature Delivery*\
A good test coverage emerged as a catalyst for faster feature delivery.
The proactive approach to bug detection and the confidence in code quality allowed for more efficient development cycles.\
*Principle: Time Savings at Every Level*\
Each previous point in our journey demonstrated that time is saved at every level when developers write tests.
The investment in testing paid off in terms of enhanced productivity, reliability, and overall code quality.

## 1. The Pre-Testing Era:
### The Setting:
Imagine a colossal codebase where bugs hid in the shadows and had a frustrating tendency to resurface. 
The development team found themselves stuck in a cycle of déjà vu, fixing a bug in a complicated logic and adding a new feature, 
only to reencounter the same bug. The repetitive nature of these issues led to a significant drain on productivity and 
raised questions about the effectiveness of the development process.

Additionally, the software development life cycle was marred by a prolonged and often disheartening process: implement a feature, 
QA finds a bug, and fixes the bug, only to unintentionally break something else in the process. 
This cycle became a routine that sapped the team's time and energy, hindering the ability to confidently deliver features.

Examples of such challenges included fixing a critical bug in a complex algorithm, celebrating its resolution, 
and encountering the same bug after integrating a seemingly unrelated feature. This repetition not only led to 
frustration but also eroded trust in the stability of the codebase.

## 2. First Steps Towards Stability:
### Initiating Change:
The journey to stability began with the strategic adoption of Jest, a popular and powerful testing framework, as the 
primary testing tool for our React.JS frontend applications. Jest, known for its simplicity and versatility, 
became a cornerstone in our efforts to revolutionize the testing landscape.

#### Why Jest?
- **Community Support:** Jest boasts a vibrant and extensive community, providing frontend developers with a 
wealth of resources and support. This proved invaluable as we embarked on our testing journey, offering insights, 
solutions, and best practices.
- **Snapshot Testing:** Jest's snapshot testing functionality became a game-changer. It enabled us to capture 
the output of components and track changes over time, preventing unintended alterations and ensuring the stability
of critical UI elements.
- **Mocking Capabilities:** The built-in mocking capabilities of Jest facilitated the isolation of components and 
functions during testing. This not only accelerated the testing process but also improved the reliability of our tests.

#### Leveraging Testing-Library:
In conjunction with Jest, we embraced the Testing Library ecosystem—an essential toolkit for testing React applications. Testing Library encourages best practices and focuses on writing tests that simulate user behavior, resulting in tests that closely resemble how our applications are used in the real world.

- **User-Centric Testing:** Testing Library's philosophy centers around user-centric testing, 
encouraging developers to write tests that mirror how users interact with the application. 
This approach leads to more meaningful and reliable tests, capturing real-world scenarios.
- **Accessibility Focus:** Accessibility is a top priority for the Testing Library, 
aligning with our commitment to delivering inclusive and user-friendly applications. The library provides
utilities and guidance to ensure that our components are not only functional but also accessible to all users.

With Jest and Testing Library in our arsenal, we set out to create a shared and easy-to-use test configuration, 
simplifying the testing process for every team member. This marked the initial stride towards a more stable and reliable codebase.

### First Steps:
The journey to stability began with small but impactful steps. Recognizing that the majority of the team had limited 
experience or enthusiasm for writing tests, we prioritized making the initial testing process as accessible as possible. 
To achieve this, a shared, easy-to-use test configuration was introduced, making testing a breeze with a simple `yarn add -d`and importing the config.

#### Features of the Shared Test Configuration:
1. **Mocking Solutions:** Given the historical issue of significant coupling in our codebase, writing effective unit tests posed a challenge. The shared configuration addressed this by incorporating predefined mocking solutions. This not only streamlined the testing process but also provided a head start for developers less familiar with the intricacies of effective mocking.

2. **Common Examples for Codebase:**
    - **Hooks:** Shared examples for testing hooks allowed developers to understand and implement tests for these crucial components.
    - **Components:** Examples for testing components ensured consistency and reliability across our user interface elements.
    - **Intricate Reducers:** Testing reducers, often at the heart of state management, became more straightforward with predefined examples.
    - **Utilities:** Common utilities were accompanied by test examples, promoting good testing practices across the codebase.

#### Seamless CI Setup:
Recognizing the need for a frictionless testing experience, the CI setup was designed to be straightforward.
The shared configuration seamlessly integrated with our continuous integration pipeline, ensuring that every code
change was automatically tested. This simplified CI setup was a deliberate choice to encourage developers to embrace
testing without the burden of complex setup processes.

#### Convincing Key Stakeholders:
Convincing key stakeholders, of the necessity for tests was a critical step in the transformation. 
The task extended beyond the development team and aimed to demonstrate to the business why investing in testing was essential. 
QA's research findings played a pivotal role in this endeavor, revealing that a significant portion of bugs was attributable 
to developer mistakes.

Knowledge-sharing sessions and live-coding demonstrations were conducted not only to showcase the technical benefits of testing, 
such as bug prevention and improved code quality, but also to underline the tangible business advantages. 
Emphasizing the connection between writing tests and reducing the overall cost of bug fixing and maintenance resonated 
with stakeholders, laying the groundwork for a shift in priorities and resource allocation.

As a result, the initial steps towards testing were met with a more receptive and engaged development team, laying 
the groundwork for a culture shift towards a more stable and reliable codebase.

### Setting Goals:
A detailed plan was formulated and presented to the team. This plan outlined the first steps towards a comprehensive 
testing strategy, laying the groundwork for a more reliable and efficient development process.
## 3. Gathering Feedback and Iterating:
### Feedback Loop:
The initial steps were challenging. Gathering feedback from the team and architecture proved invaluable. 
It became apparent that reviewing every new test was impractical. 
This realization led to the need for a scalable testing approach.
## 4. Evolving Testing Methods:
### Exploring Mutation Testing:
Mutation testing is a powerful method that involves introducing small, intentional changes, or mutants, 
into your production code to evaluate the effectiveness of your unit tests. The principle is simple: for each mutant, 
your tests are executed. If the tests fail, the mutant is considered "killed"; if they pass, the mutant survives. 
The effectiveness of your tests is measured by the percentage of mutants killed.

#### Why Mutation Testing with Stryker?
- **Diverse Mutations:** Stryker controls over 30 supported mutations, allowing for a comprehensive assessment of 
your test suite. This diversity ensures that various aspects of your code are rigorously tested, providing a more 
thorough evaluation of your test quality.

- **Speed and Efficiency:** Leveraging advanced code analysis techniques and a parallel test runner, Stryker 
optimizes the mutation testing process for speed and efficiency. This enables quick feedback on the quality of your tests, 
promoting a faster and more agile development cycle.

- **Test Runner Agnosticism:** Stryker is test runner agnostic, allowing you to use your preferred test runner seamlessly. 
Whether you favor Jest, Mocha, or another test framework, Stryker integrates effortlessly into your existing testing ecosystem.
So, we can use it with Jest.

- **Open Source Community Support:** Stryker is an open-source tool that reflects the spirit of collaborative development.
Maintained by the open-source community on GitHub, it's free to use and benefits from developers' collective expertise 
and contributions worldwide.

Mutation testing, through tools like Stryker, serves a dual purpose—designing new software tests and evaluating the 
quality of existing ones. By introducing intentional mutations, we can ensure that our unit tests remain valuable and 
that our time is spent on tests that truly contribute to the robustness of our codebase.

## 5. Tracking Progress:
### Integrating Sonarqube and Grafana:
To provide transparency and track progress, Sonarqube was added to the continuous integration process. 
Coupled with Stryker Mutator, this integration allowed us to gather statistics and visualize our progress in Grafana. 
The development team could now witness the tangible impact of testing on the product level.
## 6. Bridging the Communication Gap:
### Enter Contract Testing:
While effective unit testing is crucial for frontend components, ensuring seamless communication between 
frontend and backend introduces another layer of complexity. To address this, we will delve into the topic of 
contract testing in a dedicated article. Contract testing offers a systematic approach to validating the interactions 
between different components, providing essential insights into potential issues that may arise during integration.

Stay tuned for our upcoming article, where we'll explore the principles, benefits, and practical implementation 
of contract testing to enhance the collaboration between frontend and backend systems.

## Conclusion:
As we wrap up this exploration into the realm of unit testing, the overarching message is clear: 
it's never too late or too challenging to begin the journey of writing tests. 
The initial steps outlined in this article are designed to be approachable, breaking down perceived barriers 
and providing a roadmap for teams ready to embrace the transformative power of unit testing. 

By fostering a culture of testing, we not only elevate the stability of our code but also empower our teams 
to overcome challenges and deliver more resilient and impactful software.

The journey begins with a single test, 
and with each step, we contribute to a future where testing is not just a practice but an integral part of 
how we build and maintain high-quality software.
