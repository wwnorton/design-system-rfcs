- Start Date: (fill me in with today's date, YYYY-MM-DD)
- Specification: (fill me in with a link to the document where the component's name, description, and anatomy are defined)
- RFC PR: (leave this empty)

## Summary

One paragraph explanation of the new component.

If the component contains new sub-components that are part of the content model of this component, also list them here.

- `<ComponentOne>` - a brief description of the component.
- `<ComponentTwo>` - a brief description of the component.

## Detailed design

This is the bulk of the RFC.
Explain the design in enough detail for somebody familiar with the Norton Design System to understand it, and for somebody familiar with the implementation to implement.
This should get into specifics and corner-cases, and include examples of how the component is used.
Any new terminology should be defined here.

A good approach is to explain the proposal as if it was already part of the Norton Design System and you were teaching it to another developer.
That generally means:

- Introducing any new components and props.
  - Note: names may undergo many changes ([naming is hard](https://www.karlton.org/2017/12/naming-things-hard/)), but the description of functionality should be stable.
- Explaining the component largely in terms of examples of how someone would use it (not its internals).
- Explaining how developers would use the component to compose more complex interfaces, and how it should impact the way they think about composition.
- If applicable, provide sample error messages, deprecation warnings, or migration guidance.
- If applicable, describe the differences between teaching this to experienced developers and completely new developers.

**Avoid the impulse to write actual implementations or proofs of concept**.
If you're uncertain whether something is possible to implement or how it would be implemented, say that and move on.

If your component contains more than one component, list each as sub-heads with the following structure:

### ComponentOne

Component API designs should include at least:

1. Any interface(s) that it extends, and an brief explanation of why. If it doesn't extend any interfaces, state that explicitly.
   - For example, "`<ComponentOne>` extends the `React.InputHTMLAttributes<HTMLInputElement>` interface."
1. All props that are not part of the extended interface.
   - If an inherited prop is customized or changed in some way, that must be included here.
   - Mention if the semantics of a prop are similar to other concepts or props used elsewhere in the design system.
1. Required or relevant HTML or ARIA attributes such as [`disabled`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/disabled) or [`role`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles).
   - Tip: closely review related [ARIA Authoring Practices](https://w3c.github.io/aria-practices/) patterns to help identify relevant semantics.
1. Example(s) of what HTML should render. The user experiences the HTML, not the React code, so include

For instance:

`<ComponentOne>` extends the `React.InputHTMLAttributes<HTMLInputElement>` and adds the following properties:

| Name     | Type      | Description                              | Required | Default     |
| -------- | --------- | ---------------------------------------- | -------- | ----------- |
| `isOpen` | `boolean` | Indicates whether the component is open. | `false`  | `undefined` |

Alternatively, you could use TypeScript to capture all the same information in an interface or type:

```tsx
interface ComponentOneProps extends React.InputHTMLAttributes<HTMLInputElement> {
    /** Indicates whether the component is open. */
    isOpen?: boolean;
}
```

## Drawbacks

Why should we not do this? Please consider:

- Implementation cost in terms of size, complexity, and maintenance.
- How it will impact other teams outside of engineering.
- The cost of migrating applications to the proposed solution.

There are tradeoffs to choosing any path, please attempt to identify them here.

## Alternatives

What other designs have been considered?
Is there a way for developers to do this without this component?

Please focus on alternative ways to design the component, rather than alternative solutions that exist in other libraries.
There are two reasons you might mention another library:

1. Extending another library is a viable or expected alternative. If that's the case, explain why it wasn't pursued. Don't just list it as an alternative.
2. Another library uses an idiomatic design that was considered but ultimately not used. If that's the case, focus your explanation on why the design wasn't chosen and reference the library, but don't focus on it.

## Adoption strategy

If we implement this proposal, how will existing applications adopt it?
Focus on how applications might use it or migrate to it rather than which applications might use it.

## Unresolved questions

Optional, but suggested for first drafts. What parts of the design are still TBD?
