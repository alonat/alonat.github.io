---
title: "Embracing Confidence with Contract Testing: A Frontend Perspective"
date: 2023-12-15T17:00:00+02:00
draft: false
---

How often do you face the issue where the backend updates something and breaks a contract for the frontend? Or what about the frontend stopping the use of certain fields but not informing the backend team about the redundancy? Probably too often, right?

There’s no silver bullet for situations like these, but I do have a recommendation worth considering.

Have you heard that contract testing is the ultimate solution to fix all communication and collaboration problems between frontend and backend teams? But is that really true? Let’s weigh the pros and cons to find out.

## Contract Testing and Pact

The issue mentioned above became a stumbling block for us. With a lot of legacy code, we found ourselves stuck — no one could say for sure whether we still needed all those fields or what might break if we changed something. You might say, “Why not just refactor the entire system?” Sure, but let’s be honest — you don’t always have the time for that.

So, we decided to give Pact a try and use it to cover the crucial functionality with contract tests.

Pact, known for its simplicity and effectiveness, provided a structured framework for contract testing. It allowed us to define and verify the contracts between our services, helping us regain confidence in our codebase.

## The Frontend Angle in Contract Testing

### Why Contract Testing?

#### Pros:


- **Documentation for API Fields:**
Contract testing serves as living documentation, showcasing the fields and interactions that each service expects and provides. This helps maintain a clear understanding of the API structure. Plus, you’ll have documented API usage directly in the frontend application. Dreamy, right?

- **Identifying Potential Breakages:**
  By simulating real interactions, contract tests highlight potential problems that could occur with new changes, 
refactorings, or optimizations. This proactive approach prevents unintended failures in our apps.

- **Enhancing API Reliability:**
  Contracts act as a guarantee, adding an extra layer of reliability to our API. This not only prevents miscommunications 
between frontend and backend teams but also fosters a culture of trust in our development workflow.

- **Bug Identification Before QA:**
  Contract testing empowers developers to catch and fix bugs before they reach the QA stage. Proactive bug identification 
showcases the effectiveness of our development processes. Additionaly, you don't need to start the frontend app to catch such bugs and both sides may receive failures in theis PRs.

#### Cons:

- **Time Investment in New Features:**  
  Implementing contract tests might require extra time for new features. However, this investment pays off in the long run by preventing unforeseen issues.  

- **Handling Legacy API Calls:**  
  Don’t overlook the need to cover existing code. Strategic planning and gradual integration can help address this challenge effectively.  

- **Determining Test Scope:**  
  Identifying what to cover and what can be considered optional can be tricky. A well-defined testing strategy and strong collaboration between teams are essential.  

- **Duplication in Microservices:**  
  Teams may encounter duplication in test cases when the same endpoint is used in multiple places. A well-thought-out planning process is key to resolving this issue.

### Pact and Jest: A Powerful Duo for Frontend Developers

Pact works smoothly with Jest, one of the most popular testing frameworks for JavaScript. And honestly, that’s a game-changer for frontend teams already using Jest—no need to learn a whole new tool just to add contract testing.

By leveraging this setup, frontend devs can jump into writing contract tests without much friction, keeping the testing process unified and efficient across the stack.

Plus, contract testing lets you check components in isolation without spinning up full integration tests. That means faster feedback, fewer headaches, and a more streamlined dev experience. And you know what? Developers won't forget to run those tests as they are in your repo.

### Understanding Contract Tests

A contract test is a focused examination of the interactions between different components, ensuring that each service
adheres to the agreed-upon contracts. In the context of frontend development, a typical contract test involves specifying
the expected interactions between the frontend and backend services.

#### Required Fields for a Contract Test:
1. **Provider:**  
   The service providing the API, typically the backend service.

2. **Consumer:**  
   The service consuming the API, often the frontend application or some backend service that uses another API service as a consumer.

3. **Interaction:**  
   Describes a specific interaction between the consumer and provider. It includes the expected request, the response  
   the provider should give, and any associated metadata.

4. **State (optional):**  
   Describes the state of the provider before the interaction occurs. This is particularly useful for scenarios where  
   the outcome depends on the current state of the system.

5. **Upon Receiving:**  
   A unique identifier for the interaction, often combined with the state to create a distinct value.  
   This uniqueness is crucial to avoid overwriting previous tests and ensures each contract test captures specific scenarios.

These key fields form the foundation of a contract test, providing a comprehensive definition of the expected behavior  
between frontend and backend services. The consumer's expectations are captured in the interaction, allowing for thorough  
validation and documentation.

### Small example
To start contract testing with Pact, make sure to set up a dedicated Jest configuration specifically for Pact. The only
change needed is in the testMatch parameter. To keep things organized and run Pact contract tests independently of
unit tests, use something like
```js
config.testMatch = ['**/*.pacttest.ts']
```
This separation allows for targeted testing of contracts without affecting the regular test suite.

To ensure the successful execution of the Pact tests, it's essential to install two dependencies,
namely `@pact-foundation/pact` and `jest-pact`. Additionally, in your `package.json`, incorporate a new script for
running Pact tests independently. Add 
```json
"test:pact": "yarn run jest --config ./pact.jest.config.js"
```
This script enables you to execute Pact tests easily using Jest with the specified configuration file, keeping your
contract testing process streamlined.

Upon executing the earlier provided command for running the Pact test suite, which generates contracts stored in the
`/pact/pacts` directory, the subsequent command handles the publication to the broker. Use the following command for
publishing (using GitHub Actions syntax):
```shell
./node_modules/.bin/pact-broker publish ./pact/pacts \
    --consumer-app-version=${GITHUB_SHA} \
    --branch ${GITHUB_REF:11} \
    --broker-base-url=${{ vars.PACT_BROKER_HOST }} \
    -u ${{ secrets.PACT_BROKER_USERNAME }} -p ${{ secrets.PACT_BROKER_PASSWORD }}
```

Here's a concise example of a Pact test for the User API and frontend app Users List:
```tsx
import { pactWith } from 'jest-pact';
import { Matchers } from '@pact-foundation/pact';
import api from 'users-api-client';
import { render, screen } from '@testing-library/react';
import UsersList from './UsersList';

pactWith({ consumer: 'front-users-list', provider: 'users-api' }, provider => {
    let client;

    beforeEach(() => {
        client = api(provider.mockService.baseUrl)
    });

    describe('users list endpoint', () => {
        beforeEach(() => {
            provider.addInteraction({
                state: "users exist",
                uponReceiving: 'A request for users list',
                willRespondWith: {
                    status: 200,
                    body: Matchers.eachLike({
                        name: string('John')
                    }),
                },
                withRequest: {
                    method: 'GET',
                    path: '/users',
                },
            });
        });

        it('returns users list', async () => {
            let users = await client.list();
            render(<UsersList users={users} />);
            
            await screen.findByText('John');
        });
    });
});
```

### Reusable CI Workflow and Documentation

Adopting the same principles we explored with unit tests, our contract testing workflow is designed to be easy and reusable. 
As part of our Continuous Integration (CI) process, we've streamlined the generation and publishing of new contracts.

1. **Generating New Contracts:**
   For each branch or feature development, our CI setup automates the process of generating new contracts. 
This ensures that every new feature or change comes with updated contracts that accurately reflect the expected interactions.

2. **Publishing Contracts with Versioning:**
   Once changes are merged into the master branch, the contracts are published with a new version tag. 
This versioning approach allows us to track changes and maintain a clear history of our API contracts.

By defining clear contracts, contract testing helps teams to communicate and collaborate more effectively. 
This structured approach promotes a shared understanding of API expectations, reducing the likelihood of miscommunications.

## Conclusion & feedback from developers

Contract testing, particularly from a frontend perspective, is not just a step towards more reliable communication but a  
strategic investment in the longevity and resilience of our systems. The balance between pros and cons and an  
easy-to-use CI workflow positions contract testing as an essential practice in our development journey.

Seems like such a cool technology and cure for a lot of "deseases", right? But let's also go through the feedback of those, who have been using this setup for more than a year.
