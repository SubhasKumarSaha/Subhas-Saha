## Automated Accessibility Testing

This section outlines how to set up and run automated accessibility tests for the `Sheet` component using `jest-axe`.

### 1. Installation

First, install the necessary packages:
```
bash
npm install --save-dev jest-axe @axe-core/react
```
*   `jest-axe`: A Jest matcher for running accessibility tests.
*   `@axe-core/react`: React bindings for the `axe-core` accessibility testing engine.

### 2. Configuration

Configure Jest to use `jest-axe` by adding the following to your Jest configuration file (`jest.config.js` or similar):
```
javascript
// jest.config.js
module.exports = {
  // ... other config
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
};
```
Then, create a `jest.setup.js` file in your project root (if you don't already have one) and add:
```
javascript
// jest.setup.js
import { configure } from 'jest-axe';

expect.extend(configure({
    rules: {
      /* Add rules here to ignore from axe */
    },
}));
```
### 3. Writing Tests

Here's an example of how to write an accessibility test for the `Sheet` component:
```
typescript
// Sheet.test.tsx
import React from 'react';
import { render } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';
import { Sheet, SheetTrigger, SheetContent } from './sheet';

expect.extend(toHaveNoViolations);

describe('Sheet Accessibility', () => {
  it('should have no accessibility violations', async () => {
    const { container } = render(
      <Sheet>
        <SheetTrigger>Open</SheetTrigger>
        <SheetContent>Sheet Content</SheetContent>
      </Sheet>
    );
    const results = await axe(container);
    expect(results).toHaveNoViolations();
  });
});
```
This test renders a basic `Sheet` component and checks for accessibility violations.

### 4. Running Tests

Run your Jest tests using the following command:
```
bash
npm test
```
This will execute all your tests, including the accessibility tests.

### 5. Interpreting Results

If there are accessibility violations, the test will fail, and Jest will output details about the violations. Each violation will include:

*   **Description:** A description of the violation.
*   **Help URL:** A link to more information about the violation and how to fix it.
*   **Nodes:** The specific HTML elements that caused the violation.

Use this information to correct the accessibility issues in your component.

### 6. Best Practices

The `toHaveNoViolations()` matcher checks if the component has any accessibility violations. This is very useful to check if your components are accessible.
