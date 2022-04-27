- Start Date:
- Related documents: (fill me in with any links to related documents created by the Norton design system team. for example: the visual design, Jira ticket(s), or the working documentation)
- RFC PR: (leave this empty)

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


```ts
/**
 * If a default is not provided, generate an id.
 * @param defaultId - Default component id.
 */
export function useId(defaultId?: string): string {
  let isRendering = useRef(true);
  isRendering.current = true;
  let [value, setValue] = useState(defaultId);
  let nextId = useRef(null);

  let res = useSSRSafeId(value);

  // don't memo this, we want it new each render so that the Effects always run
  let updateValue = (val) => {
    if (!isRendering.current) {
      setValue(val);
    } else {
      nextId.current = val;
    }
  };

  idsUpdaterMap.set(res, updateValue);

  useLayoutEffect(() => {
    isRendering.current = false;
  }, [updateValue]);

  useLayoutEffect(() => {
    let r = res;
    return () => {
      idsUpdaterMap.delete(r);
    };
  }, [res]);

  useEffect(() => {
    let newId = nextId.current;
    if (newId) {
      setValue(newId);
      nextId.current = null;
    }
  }, [setValue, updateValue]);
  return res;
}

/**
 * Merges two ids.
 * Different ids will trigger a side-effect and re-render components hooked up with `useId`.
 */
export function mergeIds(idA: string, idB: string): string {
  if (idA === idB) {
    return idA;
  }

  let setIdA = idsUpdaterMap.get(idA);
  if (setIdA) {
    setIdA(idB);
    return idB;
  }

  let setIdB = idsUpdaterMap.get(idB);
  if (setIdB) {
    setIdB(idA);
    return idA;
  }

  return idB;
}

/**
 * Used to generate an id, and after render, check if that id is rendered so we know
 * if we can use it in places such as labelledby.
 * @param depArray - When to recalculate if the id is in the DOM.
 */
export function useSlotId(depArray: ReadonlyArray<any> = []): string {
  let id = useId();
  let [resolvedId, setResolvedId] = useValueEffect(id);
  let updateId = useCallback(() => {
    setResolvedId(function* () {
      yield id;

      yield document.getElementById(id) ? id : null;
    });
  }, [id, setResolvedId]);

  useLayoutEffect(updateId, [id, updateId, ...depArray]);

  return resolvedId;
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

1. [@react-aria/utils - useId](https://github.com/adobe/react-spectrum/blob/main/packages/%40react-aria/utils/src/useId.ts)
2. [@fluentui/react-hooks - useId](https://docs.microsoft.com/en-us/javascript/api/react-hooks?view=office-ui-fabric-react-latest#useId_prefix__providedId_)
3. [@accessible/use-id](https://github.com/reach/reach-ui/tree/develop/packages/auto-id)

## Adoption strategy

If we implement this proposal, how will existing applications adopt it?
Focus on how applications might use it or migrate to it rather than which applications might use it.

## Unresolved questions

Optional, but suggested for first drafts. What parts of the design are still TBD?
