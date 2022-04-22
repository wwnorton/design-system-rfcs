- Start Date: 2022/04/11
- Related documents:
  - [acceptance criteria](https://wwnorton.atlassian.net/browse/NDS-235)
  - [documentation draft](https://docs.google.com/document/d/1D0MjvCYdCaaHDJGYcPk8u9pSREkVpAac8JqNYzviHc0)
  - [visual design](@TODO)
- [RFC PR](https://github.com/wwnorton/design-system-rfcs/pull/5)

## Summary

`Tabs` allow the user to select layered sections of content to display within the same space.

A list of tab headers is displayed using the `TabList` and `Tab` components, allowing the user to interact and select the tab content they want to see. The contents of each tab is contained in a `TabPanel` component, all wrapped within a `TabPanels` container.

## Definitions

- `Controlled/uncontrolled or managed/unmanaged state`: The only state this component is interested in is which tab is the currently selected one. The controlled/uncontrolled distinction is about _who_ is responsible for managing this state.
  - Controlled means that the parent (the component invoking the `Tabs` component) is responsible of managing this state, this way the user can **control the state however they want**. In order to use the component in controlled mode, the user must provide `selectedIndex` and `onChange` props to the `Tabs` component.
  - Uncontrolled means that the state is managed internally by the `Tabs` component, the parent does not need to provide any specific props and **can't control the state**. In uncontrolled mode, the only thing that the parent can control is the default state for the initial render, this is done through the optional `defaultSelectedIndex` prop.
- `Position index`: this refers to the position of a component relative to their parent. If they are the first child their position index would be 0, if they are the second it would be 1 and so on.

### Sub components

- `<Tabs>` - The highest-order wrapper that contains all the other sub-components. Handles data management and distribution across it's children
- `<TabList>` - Container for the `<Tab>` components
- `<Tab>` - An interactive button within the `TabList`, allows the user to navigate between the different sections of content
- `<TabPanels>` - Container for the `<TabPanel>` components
- `<TabPanel>` - Container for the content of each section, contained within `TabPanels`

## Detailed design

A stateful component with both uncontrolled and controlled versions (state managed internally / state managed externally).

The only state needed by this component is the currently active section of content, identified by its index. `Tab` components must be in order with its corresponding `TabPanel`s in order to assure that the `Tab` buttons activate the correct section of content

### Tabs

`<Tabs>` extends the `React.ComponentPropsWithRef<'div'>` interface and adds the following props:

| Name                   | Type                              | Description                                                                                                          | Required | Default     |
| ---------------------- | --------------------------------- | -------------------------------------------------------------------------------------------------------------------- | -------- | ----------- |
| `selectedIndex`        | `number`                          | The currently active tab, for controlled use only                                                                    | `false`  | `undefined` |
| `onChange`             | `(selectedIndex: number) => void` | Callback for when the user interacts with one of the `Tab` buttons, will pass the position index of the selected tab | `false`  | `undefined` |
| `defaultSelectedIndex` | `number`                          | Sets the default active tab, for uncontrolled use only. Will be ignored if the `selectedIndex` prop is defined       | `false`  | 0           |

### TabList

`<TabList>` extends the `React.ComponentPropsWithRef<'div'>` interface with a `role="tablist"` attribute. A simple wrapper for the `Tab` components.

### Tab

`<Tab>` extends the `React.ComponentPropsWithRef<'button'>` interface.

The `Tab` component is responsible for rendering a `button` with the following (auto generated, the user does not need to provide these through props) attributes:

- `type="button"`
- `role="tab"`
- `aria-selected`: `true` if this `Tab` refers to the currently active tab panel, else `false`
- `aria-controls`: The value must be the `id` of the corresponding `TabPanel` this `Tab` controls. For example `tab-0` if this is the `Tab` with position index 0
- `id`: The value must be unique and will be referred to in the corresponding `TabPanel` through the `aria-labelledby` attribute. For example `tab-header-0` if this is the `Tab` with position index 0

### TabPanels

`<TabPanels>` extends the `React.ComponentPropsWithRef<'div'>` interface. A simple wrapper for the `TabPanel` components.

### TabPanel

`<TabPanel>` extends the `React.ComponentPropsWithRef<'div'>` interface with `role="tabpanel"` and an autogenerated `id` attributes.

The `TabPanel` component is responsible for rendering a `div` with the following (auto generated, the user does not need to provide these through props) attributes:

- `role="tabpanel"`
- `aria-labelledby`: The value must be the `id` of the corresponding `Tab` this `TabPanel` is controlled by. For example `tab-header-0` if this is the `TabPanel` with position index 0
- `id`: The value must be unique and will be referred to in the corresponding `Tab` through the `aria-controls` attribute. For example `tab-0` if this is the `TabPanel` with position index 0

### Simple Usage - Uncontrolled/State managed internally

```tsx
<Tabs defaultSelectedIndex={2}>
  <TabList>
    <Tab>Cats</Tab>
    <Tab>Dogs</Tab>
    <Tab>Horses</Tab>
  </TabList>
  <TabPanels>
    <TabPanel>Cats content</TabPanel>
    <TabPanel>Dogs content</TabPanel>
    <TabPanel>Horses content</TabPanel>
  </TabPanels>
</Tabs>
```

### Simple Usage - Controlled/State managed externally

```tsx
<Tabs selectedIndex={selectedTabIndex} onChange={handleSelectedTabIndexChange}>
  <TabList>
    <Tab>Cats</Tab>
    <Tab>Dogs</Tab>
    <Tab>Horses</Tab>
  </TabList>
  <TabPanels>
    <TabPanel>Cats content</TabPanel>
    <TabPanel>Dogs content</TabPanel>
    <TabPanel>Horses content</TabPanel>
  </TabPanels>
</Tabs>
```

### Simple example of final rendered HTML

```tsx
<div role="tablist">
  <button
    type="button"
    role="tab"
    aria-selected="true"
    aria-controls="tab-0"
    id="tab-header-0"
  >
    Cats
  </button>
  <button
    type="button"
    role="tab"
    aria-selected="false"
    aria-controls="tab-1"
    id="tab-header-1"
  >
    Dogs
  </button>
  <button
    type="button"
    role="tab"
    aria-selected="false"
    aria-controls="tab-2"
    id="tab-header-2"
  >
    Horses
  </button>

  <div className="panel-container">
    <div id="tab-0" role="tabpanel" aria-labelledby="tab-header-0">
      Cats content
    </div>
    <div id="tab-1" role="tabpanel" aria-labelledby="tab-header-1" hidden="">
      Dogs content
    </div>
    <div id="tab-2" role="tabpanel" aria-labelledby="tab-header-2" hidden="">
      Horses content
    </div>
  </div>
</div>
```

## Alternatives

Ant-design uses a simpler and more minimalistic API, which combines the `Tab` and `TabPanel` components into one and only a single `Tabs` wrapper. A clear disadvantage of this API is that it doesn't reflect the structure of the actual rendered HTML. Also it requires a more complex implementation due to the need to use Portals in order to elevate the tab content above its container in the final DOM.

```tsx
<Tabs defaultActiveKey="1" onChange={callback}>
  <TabPane tab="Tab 1" key="1">
    Content of Tab Pane 1
  </TabPane>
  <TabPane tab="Tab 2" key="2">
    Content of Tab Pane 2
  </TabPane>
  <TabPane tab="Tab 3" key="3">
    Content of Tab Pane 3
  </TabPane>
</Tabs>
```

## Unresolved questions

- We need a way to identify each `Tab` and relate it to its corresponding `TabPanel`. This is needed in order to calculate the `aria-controls`, `aria-labelledby` and `id` attributes for the `Tab` and `TabPanel` components
- Calculate these `id`s in a way that multiple tabs can be rendered in the same page at the same time and ensure the `id`s remain unique

We can:

1. Using the solution from NDS-22, generate an UUID to use as prefix for every `id`, that way we ensure that they are unique. For example a `Tab` would generate an `id="123autogenerated-tab-header-0"` for position index 0 and its corresponding `TabPanel` would have `id="123autogenerated-tab-0`, `aria-labelledby="123autogenerated-tab-header-0"`
2. Don't use autogenerated UUIDs (in order to not depend on NDS-22) and ask the developer to manually give the parent `Tabs` component an unique `id` and use that as prefix. For example `<Tabs id="myAnimalsTabbedPanel" />` would generate `id="myAnimalsTabbedPanel-tab-header-0"`. As long as the developer doesn't give multiple `Tabs` the same `id` we can be sure that the generated `id`s will be unique
