- Start Date: 27/04/2022
- Related documents:
  - [Out-of-sync server/client ids](https://wwnorton.atlassian.net/browse/NDS-22)
  - [Out-of-sync server/client ids [NDS-22] - GitHub](https://github.com/wwnorton/design-system/issues/105)
- RFC PR: [useId technical design NDS-22](https://github.com/wwnorton/design-system-rfcs/pull/6)

## Summary

`useId` is an API for generating unique IDs on both the client and server, while avoiding hydration mismatches.

```jsx
  function Checkbox() {
    const id = useId();
    return (
      <>
        <label htmlFor={id}>Do you like React?</label>
        <input type="checkbox" name="react" id={id} />
      </>
    );
  );
```

## Detailed design

This solves an issue that already exists in React 17 and below, but it's even more important in React 18 because of how our streaming server renderer delivers HTML out-of-order. Solutions that may have worked previously, like using a counter to generate IDs, don't work in 18.0 because you can't rely on a consistent sequence.

### useIsomorphicLayoutEffect hook

The React documentation says about `useLayoutEffect`:

> The signature is identical to `useEffect`, but it fires synchronously after all DOM mutations.

That means this hook is a browser hook. But React code could be generated from the server without the Window API.
This hook fixes this problem by switching between `useEffect` and `useLayoutEffect` following the execution environment.

```ts
import { useEffect, useLayoutEffect as useReactLayoutEffect } from "react";
import { canUseDOM } from "../environment";

/** Use layout effect that is safe to use in SSR/isomorphic applications. */
export const useIsomorphicLayoutEffect = canUseDOM
  ? useReactLayoutEffect
  : useEffect;

// export as useLayoutEffect so eslint-plugin-react-hooks works with it.
export const useLayoutEffect = useIsomorphicLayoutEffect;
```

### useId Hook

```ts
import * as React from "react";
import { useLayoutEffect } from "./utilities";

let ID = 0;
const genId = (): number => ID++;
let serverHandoffComplete = false;

export interface BaseIdGeneratorProps {
  /**
   * Allows you to provide your own id as a fallback
   */
  providedId?: number | string | undefined | null;
}

/**
 * useId
 *
 * Autogenerate IDs to facilitate WAI-ARIA and server rendering.
 *
 * Note: The returned ID will initially be `null` and will update after a
 * component mounts. Users may need to supply their own ID if they need
 * consistent values for SSR.
 *
 */
function useId({ providedId, prefix }): BaseIdGeneratorProps {
  const [id, setId] = React.useState(
    providedId ?? (serverHandoffComplete ? genId() : null)
  );

  useLayoutEffect(() => {
    if (id === null) {
      setId(genId());
    }

    serverHandoffComplete = true;
  }, []);

  return providedId ?? id ?? undefined;
}

export { useId };
```

## Drawbacks

Why should we not do this? Please consider:

- Some APIs like `aria-labelledby` accept multiple ids via a space-separated list. If you attempt to do this today, React will warn when you concatenate the ids.
- you can't create new ids dynamically — they must obey the hook rules. This doesn't scale well in practice. For example, to implement a form, you'd need a separate `useOpaqueIdentifier` hook instance per distinct form field.
- We need supportting this solution as a long-term solution, at least until React v17 is deprecated

## Alternatives

What other designs have been considered?
Is there a way for developers to do this without this component?

Please focus on alternative ways to design the component, rather than alternative solutions that exist in other libraries.
There are two reasons you might mention another library:

1. [@react-aria/utils - useId](https://github.com/adobe/react-spectrum/blob/main/packages/%40react-aria/utils/src/useId.ts)
2. [@fluentui/react-hooks - useId](https://docs.microsoft.com/en-us/javascript/api/react-hooks?view=office-ui-fabric-react-latest#useId_prefix__providedId_)
3. [@accessible/use-id](https://github.com/reach/reach-ui/tree/develop/packages/auto-id)

## Adoption strategy

If this solution of creating a hook is implemented, applications using NDS should make use of this hook which is provided as a utility in the necessary components. It has no major impacts at the migration level, only a new implementation of ID generation for each component.

## Unresolved questions

Optional, but suggested for first drafts. What parts of the design are still TBD?
