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

- `<Step>` - a visible step with a completed/uncompleted state, a label and an active/inactive state

## Detailed design

A fully (externally) controlled and purely presentational component. The neither the `StepIndicator` or `Step` components handle any sort of state management or event dispatching (other than those inherited from `<div>`'s like onClick). The parent component is responsible of keeping track of each individual steps, this component solely responsibility is to present said states and other information (labels, etc) to the user

### StepIndicator

`<StepIndicator>` extends the `React.HTMLAttributes<HTMLDivElement>` without adding any new props, it acts as a simple container

### Step

`<Step>` extends the `React.HTMLAttributes<HTMLDivElement>` and adds the following props:

| Name         | Type      | Description                                          | Required | Default     |
| ------------ | --------- | ---------------------------------------------------- | -------- | ----------- |
| `label`      | `string`  | The label that identifies the step                   | `false`  | `undefined` |
| `isActive`   | `boolean` | Indicates whether the user is currently on this step | `false`  | `false`     |
| `isComplete` | `boolean` | Indicates whether the step is complete               | `false`  | `false`     |

#### Render examples

##### Simple usage

```tsx
<StepIndicator>
  <Step label="Order details" isComplete />
  <Step label="Payment" isActive />
  <Step label="Delivery" />
</StepIndicator>
```

##### With simple state management

```tsx
export const OrderWizard = () => {
  const [currentStep, setCurrentStep] = useState<number>(0);

  return (
    <div>
      <StepIndicator>
        <Step label="Order details" isComplete={currentStep > 0} isActive={currentStep === 0} />
        <Step label="Payment" isComplete={currentStep > 1} isActive={currentStep === 1} />
        <Step label="Delivery" isComplete={currentStep > 2} isActive={currentStep === 2} />
      </StepIndicator>
      {currentStep === 0 && <div>Order details form...</div>}
      {currentStep === 1 && <div>Payment details form...</div>}
      {currentStep === 2 && <div>Delivery details form...</div>}

      <Button onClick={() => setCurrentStep((currentStep) => currentStep + 1)}>Continue</Button>
    </div>
  );
};
```

## Drawbacks

- State management for the steps is not supported and is responsibility of the parent component
- Step navigation is not supported within this component, must be handled outside with external buttons for example
- Since we are displaying each step visually, the component will be significantly big. Meaning usage in mobile will be very limited to a small number of steps. Desktop will also be limited but given the average monitor screen we have enough room for at least 8 steps

## Alternatives

Other implementations (for example [ant design](https://ant.design/components/steps/)) of similar components offer a component with internal state management where the developer only provides initial values (`initialStep`, etc) and saves the developer from the trouble of writing state management logic for the step states (in some limited cases only)

Not only this only helps in a limited number of use cases at a cost of increasing the complexity significantly but it also doesn't make sense given the current set of features of the component being built. Since step navigation is not offered by the component, the component has no way of internally modifying its state so it must always be externally controlled

## Adoption strategy

## Unresolved questions

warning when only 1 Step is found as children or more than suggested (8+)?
