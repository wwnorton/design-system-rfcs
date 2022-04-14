- Start Date: 2022/04/11
- Related documents:
  - [acceptance criteria](https://wwnorton.atlassian.net/browse/NDS-235)
  - [documentation draft](https://docs.google.com/document/d/1D0MjvCYdCaaHDJGYcPk8u9pSREkVpAac8JqNYzviHc0)
  - [visual design](@TODO)
  - [names, states, and properties](@TODO)
- RFC PR: @TODO

## Detailed design

### API option 1: tab content as children and reflected upwards the tree using portals

#### Internally controlled (Uncontrolled)

```tsx
<Tabs initialValue="cats">
  <Tab value="cats" label="Cats">
    Cats content
  </Tab>
  <Tab value="dogs" label="Dogs">
    Dogs content
  </Tab>
  <Tab value="horses" label="Horses">
    Horses content
  </Tab>
</Tabs>
```

#### Externally controlled

```tsx
<Tabs value={value} onChange={handleActiveTabChange}>
  <Tab value="cats" label="Cats">
    Cats content
  </Tab>
  <Tab value="dogs" label="Dogs">
    Dogs content
  </Tab>
  <Tab value="horses" label="Horses">
    Horses content
  </Tab>
</Tabs>
```

- Simpler API
- More complex implementation? The panel content is passed as children to the Tab component but it can be placed at a higher level in the DOM tree using Portals
- Used by ant-design https://codesandbox.io/s/dmkw8l
- Since the label is a string prop, adding icons is not as intuitive and flexible as having the label be the children. We could use the same API used in the button component for handling icon and icon position (`icon`, `iconRight`, `iconOnly` as props for the `Tab` component)

### API option 2: a specific "TabPanel" component, only related to the Tab component by an ID

```tsx
<>
  <Tabs value={value} onChange={handleActiveTabChange}>
    <Tab value="cats">Cats</Tab>
    <Tab value="dogs">Dogs</Tab>
    <Tab value="horses">Horses</Tab>
  </Tabs>

  <TabPanel value={value}>Cats content</TabPanel>
  <TabPanel value={value}>Dogs content</TabPanel>
  <TabPanel value={value}>Horses content</TabPanel>
</>
```

- One more component TabPanel
- I can't see how an uncontrolled/unmanaged version could be easily implemented without mayor changes, current active tab state must be handled by the parent. Could be done by adding a wrapper component that could distribute the state data downwards maybe using the context API or by using cloneElement to inject state props (I prefer staying away from cloneElement if possible)
- Mui uses this API and they don't offer an uncontrolled version (they offer uncontrolled versions for most of their components)
- More flexible API, the Tabs component is so detached that could be used differently like having the tab selector below the content

### Simple example of final rendered HTML

```tsx
<div role="tablist" aria-label="animals">
  <button
    type="button"
    role="tab"
    aria-selected="true"
    aria-controls="cats-tab"
    id="cats"
  >
    Cats
  </button>
  <button
    type="button"
    role="tab"
    aria-selected="false"
    aria-controls="dogs-tab"
    id="dogs"
  >
    Dogs
  </button>
  <button
    type="button"
    role="tab"
    aria-selected="false"
    aria-controls="horses-tab"
    id="horses"
  >
    Horses
  </button>

  <div className="panel-container">
    <div role="tabpanel" aria-labelledby="cats">
      Cats content
    </div>
    <div role="tabpanel" aria-labelledby="dogs" hidden="">
      Dogs content
    </div>
    <div role="tabpanel" aria-labelledby="horses" hidden="">
      Horses content
    </div>
  </div>
</div>
```
