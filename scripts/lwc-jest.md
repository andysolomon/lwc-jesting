# The Comprehensive Guide to Jest Testing LWC

## Setup
### Node/NPM Config
### Mocking Config
### Imports
Import `createElement` and you component `c-test-component`
```
import { createElement } from "lwc";
import testComponent from "c/test-component";
```
We will use `createElement` to bootstrap our component and cache it across our test suite.

### Describe/It
describe() is a top level test method which used to organize jest test into suites.
In the describe its good to cache the component element and any other long existing data. This is where we initially cache our element so we can use it across all test in our suite.
```
let element
```

it() is a lower level test method where we typically perform actions and assertions

### Cleanup
#### beforeEach() & afterEach()
In our `beforeEach()` method we want to initialize our lwc component and mount it. Any other pre-setup will be done here. This is where we will use our `createElement` method to create our lwc component from the module import.
```
element = createElement('c-test-component', { is: testComponent});
```
Most notably its recommend to mount the component here using `appenchChild(element)`

In our `afterEach()` we want to gracefully remove our lwc component. Essentially unmounting our component.
```
afterEach(() => {
  // The jsdom instance is shared across test cases in a single file so reset the DOM
  while (document.body.firstChild) {
    document.body.removeChild(document.body.firstChild);
  }
});
```

## Selectors & Matchers
### shadowRoot.querySelector()
Quite simply a way to traverse and select elements in the dom. These will typcially be class name or data attributes. When we select elements in the dom we can then perform accessibility testing on these elements. For example we can use `element.shadowRoot.querySelector('[data-test-id="close-modal"]')`, to track the close modal button through out testing session. We execute our test and finally make our assertions we use these found elements.
### Jest Matchers
The final step in a jest test is the actual assertion. We can assert almost any code behavior like if a variable is a number, false, an object, etc.. And because of jsdom and the headless browser that jest provides we can also have assertions on the DOM. Jest Matchers allow us 

#### DOM Accessibility Testing
This is planting assertions on lwc dom selectors that we select using `shadowRoot.querySelector`. Jest has preserved the `expect()` keyword for assertions of a successful condition. Using `expect()` for DOM testing is as simple as placing your cached querySelector parameter to the `expect(modalButton)`
Here are some common DOM Jest Matchers.
1. .toBe()
2. .toMatchObject()
3. .toContain()

Here are some common DOM Jest Modifiers
1. .not()

#### Functional Unit Testing
Functional unit testing is the practice of testing functions in the LWC. The LWC is simply an es6 class composed of all functions all created by the author. We are not allowed to publicly access the elements in an LWC component so directly testing functions is not possible without using stubs and/or mocks

## Testing in VSCode

## Stubs/Mocks/Spies
### Stubs
Stubbing is the idea of having a fake/stub function that has the same public interface as the real function but doesn't actually execute real behavior.
Stubs return artificial and pre-programmed values or errors
### Mock/Spy
Mocks are differ from spies in the sense that mocks conceptually is really just an umbrella term to represt any replacement module. A mock may contain stubs or spies.
#### Mocks
#### Spies
Along with mocking data we also need to mock core lwc base components like the NavigationMixin and 
### jest.mock
Using mocks allow us to test our component without need for a an actual Saleforce backend. Everything that our LWC component relies on or is that is imported will most likely mocked.

## Data Mocking
### require(./data/testdata.json)

## Event Handling Mocks
When testing event handlers whats important during testing is that the event was called, and what parameters the event was called with. Afterwards we want to flushPromises and make assertions on those claims.
We create handler spies for our events like so
```javascript
const handler = jest.fn();
element.addEventListener(ShowToastEventName, handler)
```
This way whenever our event is called our handler will spy on all calls and we can traverse the calls. using the following syntax against the hanlder
```
handler.mock.calls[][].detail.title
```
This syntax represents the parameter that was called for a specific call to the event handler.
- toHaveBeenCalled()

## Self Mocks

## Wire Service
### Wired Apex
### Wired LDS

## Imperative Apex
### mockResolvedValue()
### mockRejectedValue()

## Time
### Asynchronous DOM Updates
When applying user interaction that will force any updates its best to use a resolved promise to allow for DOM updates to finish.

## Debugging in VSCode
### Run Time Debugging
In VS Code thanks to the Saleforce Extensions we have a "Run and Debug" option where we use this along with a Console Debug window to monitor and track our tests as the run in real time. You can also create a [launch.json](https://github.com/salesforce/sfdx-lwc-jest?tab=readme-ov-file#debugging-in-visual-studio-code) file for more control over run time debugging.
Breakpoints cannot be used in both component source and test files to pause debugger during run time. You must use the actual `debugger;` statement.

### Life Cycle Hooks
- Creation Hooks
- Mounting Hooks
- Updating Hooks
- Unmounting Hooks
- Error Handling Hooks
- Debugging Hooks
- Keep-Alive Component Hooks
- Server-Side Rendering Hooks
