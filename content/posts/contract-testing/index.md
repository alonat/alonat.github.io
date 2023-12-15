---
title: "Embracing Confidence with Contract Testing: A Frontend Perspective"
date: 2023-12-15T17:00:00+02:00
draft: false
---

## Introduction

Ensuring seamless communication between
frontend and backend systems is paramount in the ever-evolving software development landscape. Unit tests fail to guarantee 
end-to-end correctness, and exhaustive end-to-end tests can be impractical. This is where contract testing steps in, offering a
balanced approach to enhance communication reliability between different components.

Contract testing reduces the risk of unexpected changes to an API causing unexpected behavior in downstream components.
These tests act as a safeguard, catching potential issues before they propagate through the system.

## Contract Testing and Pact

Contract testing, particularly with Pact, has become a cornerstone in our quest for robust and communicative systems.
Pact, known for its simplicity and effectiveness, provides a structured framework for contract testing, allowing us to
define and verify the contracts between our services.

## The Frontend Angle in Contract Testing

### Why Contract Testing?

#### Pros:


- **Documentation for API Fields:**
  Contract testing serves as living documentation, showcasing the fields and interactions that each service expects and provides. 
This aids in maintaining a clear understanding of the API structure.

- **Identifying Potential Breakages:**
  By simulating real interactions, contract tests illuminate potential breakages that could occur with new changes, 
refactorings, or optimizations. This proactive approach prevents unintended disruptions in our API ecosystem.

- **Enhancing API Reliability:**
  Contracts act as a guarantee, adding an extra layer of reliability to our API. This not only prevents miscommunications 
between frontend and backend teams but also fosters a culture of trust in our development workflow.

- **Bug Identification Before QA:**
  Contract testing empowers developers to catch and fix bugs before they reach the QA stage. Proactive bug identification 
showcases the effectiveness of our development processes.

#### Cons:

- **Time Investment in New Features:**
  Implementing contract tests may require additional time for new features. However, this investment pays off in the long
  run by preventing unforeseen issues.

- **Handling Legacy API Calls:**
  The challenge lies in covering old API calls with contracts. Strategic planning and gradual integration can address this concern.

- **Determining Test Scope:**
  Identifying what to cover and what is optional can be challenging. A well-defined testing strategy and collaboration
  between teams is crucial.

- **Duplication in Microservices:**
  The possibility of duplicating contracts across microservices can be managed with effective documentation and
  communication between development teams.

### Pact and Jest: A Powerful Duo for Frontend Developers

Pact integrates seamlessly with Jest, a popular testing framework for JavaScript. This compatibility is a
game-changer for frontend developers already familiar with Jest. Leveraging this familiarity, frontend teams
can transition smoothly into writing contract tests, ensuring a unified and efficient testing experience across the development stack.

Contract testing allows developers to test components in isolation, without requiring a full integration test.
This focused testing approach improves efficiency and enables quick feedback on potential issues.

### Understanding Contract Tests

A contract test is a focused examination of the interactions between different components, ensuring that each service
adheres to the agreed-upon contracts. In the context of frontend development, a typical contract test involves specifying
the expected interactions between the frontend and backend services.

#### Required Fields for a Contract Test:

1. **Provider:**
   The service providing the API, typically the backend service.

2. **Consumer:**
   The service consuming the API, often the frontend application.

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

## Conclusion

Contract testing, particularly from a frontend perspective, is not just a step towards more reliable communication but a
strategic investment in the longevity and resilience of our systems. The balance between pros and cons and an
easy-to-use CI workflow positions contract testing as an essential practice in our development journey.

As we embrace this testing paradigm, we pave the way for a future where frontend and backend communication is not just a
technical requirement but a seamless and confident collaboration.
