- Start Date: 2022/01/31
- Specification: [Step Indicator](https://wwnorton1.sharepoint.com/:w:/g/DP/products_and_projects/nds/EYaT6DU_L9dJgpDUAm0brj0BTyXTdFEpfNOhvmPsaYnK_A?e=mpeK98)
- Design: [Step Indicator](https://zpl.io/a7prL4q)
- RFC PR:

## Summary

The `StepIndicator` component is the combination of multiple `Step Markers` components that indicate to the user their progress through a multiple step process.

The information shared to the user includes:

- The current step that the user is in
- The completed/uncompleted states for every step
- A label to identify each step

- `<StepMarker>` - a visible step with a completed/uncompleted state

## Detailed design

## Drawbacks

Why should we not do this? Please consider:

- State management for the steps is not supported and is responsibility of the parent component
- Step navigation is not supported within this component, must be handled outside with external buttons for example
- Since we are displaying each step visually, the component will be significantly big. Meaning usage in mobile will be very limited to a small number of steps. Desktop will also be limited but given the average monitor screen we have enough room for at least 8 steps

## Alternatives

## Adoption strategy

## Unresolved questions
