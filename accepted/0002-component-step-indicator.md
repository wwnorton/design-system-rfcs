- Start Date: 2022/01/31
- Specification: [ACs](https://wwnorton1.sharepoint.com/:w:/g/DP/products_and_projects/nds/EYaT6DU_L9dJgpDUAm0brj0BTyXTdFEpfNOhvmPsaYnK_A?e=mpeK98) / [Spec](https://docs.google.com/document/d/1Km1st0VA-rEBiSStirv7iwhyyJu8QiKz_3PwkfTwFMw)
- Design: [Step Indicator](https://zpl.io/a7prL4q)
- RFC PR:

## Summary

The `StepIndicator` component used to present to the user information on their progress through a process of discrete steps

Built with the combination of multiple `Step` marker components that indicate the state of each step

The information shared to the user includes:

- The current step that the user is in
- The total amount of steps (implicit)
- The completed/uncompleted states for every step
- A label to identify each step

### Sub components

- `<Step>` - a visible step with a completed/uncompleted state, a label and a current state

## Detailed design

A fully (externally) controlled and purely presentational component. Neither the `StepIndicator` nor the `Step` components handle any sort of state management or event dispatching. The parent component is responsible of keeping track of each individual steps. This component is solely responsibility for presenting said states and other information (labels, etc) to the user.

### StepIndicator

`<StepIndicator>` extends the `React.OlHTMLAttributes<HTMLOListElement>` without adding any new props, it acts as a simple container with accessibility attributes like `aria-label="Steps"`

### Step

`<Step>` extends the `React.HTMLAttributes<HTMLDivElement>` and adds the following props:

| Name          | Type      | Description                                          | Required | Default |
| ------------- | --------- | ---------------------------------------------------- | -------- | ------- |
| `isCurrent`   | `boolean` | Indicates whether the user is currently on this step | `false`  | `false` |
| `isCompleted` | `boolean` | Indicates whether the step is complete               | `false`  | `false` |

### Render examples

#### Simple usage

```tsx
<StepIndicator>
  <Step isCompleted>
    <strong>Order</strong> details
  </Step>
  <Step isCurrent>Payment</Step>
  <Step>Delivery</Step>
</StepIndicator>
```

#### With simple state management

```tsx
export const OrderWizard = () => {
  const [currentStep, setCurrentStep] = useState<number>(0);

  return (
    <div>
      <StepIndicator>
        {stepsData.map(({ label, isCompleted, ID }) => {
          return (
            <Step isComplete={isCompleted} isCurrent={ID === currentStep}>
              {label}
            </Step>
          );
        })}
      </StepIndicator>

      <StepBodyComponentExample
        data={stepsData.find(({ ID }) => currentStep === ID)}
      />

      <Button onClick={() => setCurrentStep((currentStep) => currentStep + 1)}>
        Continue
      </Button>
    </div>
  );
};
```

#### Simple example of final rendered HTML

```tsx
<ol aria-label="Steps">
  <li>
    <svg aria-label="completed" />
    Order details
  </li>
  <li aria-current="step">
    <svg aria-label="incomplete" />
    Payment
  </li>
  <li>
    <svg aria-label="incomplete" />
    Delivery
  </li>
</ol>
```

In reality, in order to add the connector lines some dummy `<div/>`'s might be necessary, for example:

```tsx
<ol aria-label="Steps">
  <li>
    <div>
      <div classname="connector" />
      <svg aria-label="completed" />
      <div classname="connector" />
    </div>
    <div>Order details</div>
  </li>
  <li>
    <div>
      <div classname="connector" />
      <svg aria-label="incomplete" />
      <div classname="connector" />
    </div>
    <div>Delivery</div>
  </li>
</ol>
```

First and last connectors can be easily hidden with some simple css child selectors:

```scss
.step {
  &:first-child {
    .connector:first-child {
      display: none;
    }
  }

  &:last-child {
    .connector:last-child {
      display: none;
    }
  }
}
```

### Design Tokens

To allow for customization to suit different project and context usages, the component will expose a series of scss tokens used to change the visuals

| Token Name                             | Description                                                                               | Default         |
| -------------------------------------- | ----------------------------------------------------------------------------------------- | --------------- |
| `--nds-stepindicator-primary-color`    | The color for the currently active step marker halo and the background of completed steps | `primary-color` |
| `--nds-stepindicator-base-color`       | The background base color for incomplete step markers and connector line                  | `base-color-90` |
| `--nds-stepindicator-show-connector`   | Boolean on whether or not to show a connector between the step markers                    | `true`          |
| `--nds-stepindicator-step-marker-size` | The width and height of the (non-current) step markers circle                             | `1.75rem`       |
| `--nds-stepindicator-max-step-width`   | Max-width for the specific steps, can be useful when the number of steps is dynamic       | `null`          |

## Drawbacks

- State management for the steps is not supported and is responsibility of the parent component
- Step navigation is not supported within this component, must be handled outside with external buttons for example
- Since we are displaying each step visually, the component will be significantly big. Meaning usage in mobile will be very limited to a small number of steps. Desktop will also be limited but given the average monitor screen we have enough room for at least 8 steps

## Alternatives

Other implementations (for example [ant design](https://ant.design/components/steps/)) of similar components offer a component with internal state management where the developer only provides initial values (`initialStep`, etc) and saves the developer from the trouble of writing state management logic for the step states (in some limited cases only)

Not only this only helps in a limited number of use cases at a cost of increasing the complexity significantly but it also doesn't make sense given the current set of features of the component being built. Since step navigation is not offered by the component, the component has no way of internally modifying its state so it must always be externally controlled

## Adoption strategy

Applications that desire a visual design different to the one we ship would need to make use of the exposed design tokens to suit the component to their needs
