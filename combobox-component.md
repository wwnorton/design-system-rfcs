- Start Date: 2021-11-01
- Specification: [Combobox](https://www.google.com/url?q=https://docs.google.com/document/d/1mvNFw9C3r49jqplWD7ftpH0hG5Oc6jlLvpgz9CeHkUc/edit?usp%3Dsharing&sa=D&source=hangouts&ust=1636054364167000&usg=AOvVaw2QHNBAaKj72ettcN18MRLE)
- Design: [Combobox](https://zpl.io/bLAy0rd)
- RFC PR:

## Summary

The `Combobox` component is the combination of an input element with a drop-down list. You can use it to select and/or edit strings or objects from lists. The control automatically completes entries as the user types, and allows users to show a drop-down list with the items available.

The `Combobox` component contains new sub-components that are part of the content model of this component, also list them here.

- `<TextField>` - a text field allows the user to enter and edit text, and is intended to filter results.
- `<Listbox>` - display all information from a data set and allows the user to select one or more items from a list contained within a static, multiple line text box
- `<Option>` - a selectable item in a `<Listbox>`.
- `<CheckboxGroup>` - display all information from a data set and allows the user to select one or more items.
- `<Checkbox>` - a selectable item in a `<CheckboxGroup>`.
- `<Tag>` - allows the user to interact with or dismiss a status or classification.

## Detailed design

### Combobox

For instance: `<Combobox>` extends the `React.DivHTMLAttributes<HTMLDivElement>` and adds the following properties:

| Name     | Description                              | Required | Default     |
| -------- | ---------------------------------------- | -------- | ----------- |
| `allowClear` | If set, the `input` renders the clear button. | `false`  | `false` |
| `autoFocus` | If set, the focusable `input` should be focused when it is rendered. | `false`  | `false` |
| `defaultValue` | Initial selected option(s). | `false`  | `""` |
| `value` | Selected option(s). | `false`  | `""` |
| `multiselectable` | Indicates that more than one option can be selected. | `false`  | `false` |
| `variant` | Indicates the type of list to render `listbox` or `checkboxes`, only works if the `multiselectable` attribute is active. | `false`  | `listbox` |
| `options` | A list of options as either an array of `value` props. | `true`  | `[]` |
| `selected` | The currently selected value(s). | `true`  | `[]` |
| `notFoundContent` | Text displayed when no matches are found in search. | `true`  | `No results were found` |
| `isOpen` | Indicates whether the `listbox` is rendered or not. | `false`  | `false` |
| `onSelect` | Called when a option is selected/unselected. | `false`  | `function(value, option)` |
| `onClear` | Called when the `clearButton` is clicked. | `false`  | `() => {}` |

#### Render Examples
##### Single selection

```
<Combobox>  
	<TextField>Select your favorite pet:</TextField>  
	<Listbox>
		<Option value="dog">🐶 Dog</Option>
		<Option value="cat">🐱 Cat</Option>
		<Option value="hamster">🐹 Hamster</Option>
	</Listbox>
</Combobox>
```

##### multi-selection listbox

```
<Combobox
	multiselectable
	variant="listbox"
>  
	<TextField>Select your favorite pets:</TextField>  
	<Listbox>
		<Option value="dog">🐶 Dog</Option>
		<Option value="cat">🐱 Cat</Option>
		<Option value="hamster">🐹 Hamster</Option>
	</Listbox>
</Combobox>
```

##### multi-selection checkboxes

```
<Combobox
	multiselectable
	variant="checkbox"
>  
	<TextField>Select your favorite pets:</TextField>  
	<CheckboxGroup>
		<Checkbox value="dog">🐶 Dog</Checkbox>
		<Checkbox value="cat">🐱 Cat</Checkbox>
		<Checkbox value="hamster">🐹 Hamster</Checkbox>
	</CheckboxGroup>
</Combobox>
```

## Drawbacks

Why should we not do this? Please consider:

- Custom items are not supported.
- Loading state for asynchronous calls is not supported in this version.

## Alternatives

There are many alternatives available in the market.

- User can create a combobox from scratch using based on [W3C Combobox Specs](https://www.w3.org/TR/wai-aria-practices-1.2/#combobox)
- [Ant design - Autocomplete](https://ant.design/components/auto-complete/)
- [Polaris - Combobox](https://polaris.shopify.com/components/forms/combobox/)
- [Carbon - Combobox](https://carbondesignsystem.com/components/dropdown/code/)
- [React Select](https://react-select.com/home)
- [Adobe Spectrum - Combobox](https://opensource.adobe.com/spectrum-css/combobox.html)

## Adoption strategy

If we implement this proposal, how will existing applications adopt it?

## Unresolved questions

Optional, but suggested for first drafts. What parts of the design are still TBD?
