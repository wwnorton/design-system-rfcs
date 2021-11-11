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

For instance: `<Combobox>` extends the `React.HTMLAttributes<HTMLDivElement>` and adds the following properties:

| Name     | Type      | Description                              | Required | Default     |
| -------- | --------- | ---------------------------------------- | -------- | ----------- |
| `allowClear` | `boolean` | If set, the `input` renders the clear button. | `false`  | `false` |
| `autoFocus` | `boolean` | If set, the focusable `input` should be focused when it is rendered. | `false`  | `false` |
| `defaultValue` | `string[]`, `number[]` | Initial selected option(s). | `false`  | `""` |
| `value` | `string[]`, `number[]` | Selected option(s). | `false`  | `""` |
| `multiselectable` | `boolean` | Indicates that more than one option can be selected. | `false`  | `false` |
| `variant` |  `string` | Indicates the type of list to render `listbox` or `checkboxes`, only works if the `multiselectable` attribute is active. | `false`  | `listbox` |
| `options` | `string[]`, `object`, `object[]` |  A list of options as either an array of `value` props. | `true`  | `[]` |
| `selected` | `string[]` |  The currently selected value(s). | `true`  | `[]` |
| `notFoundContent` | `string` |  Text displayed when no matches are found in search. | `true`  | `No results were found` |
| `isOpen` | `boolean` |  Indicates whether the `listbox` is rendered or not. | `false`  | `false` |
| `onSelect` | `function` | Called when a option is selected/unselected. | `false`  | `function(value, option)` |
| `onClear` |  `function` | Called when the `clearButton` is clicked. | `false`  | `() => {}` |

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
### TextField

For instance: `<TextField>` extends the `React.InputHTMLAttributes<HTMLInputElement>`, for the combobox component, we will use the current `TextFieldProps` API, but some properties that prevent direct use in the `TextField` component will be omitted.

#### Interface
```
Omit<TextFieldProps, 'type' | 'addonBefore' | 'addonAfter' | 'multiline' 'autoSize'>
```
### Listbox

For instance: `<Listbox>` extends the `React.HTMLAttributes<HTMLUListElement>`, for the combobox component, we will use the current `ListboxProps` API, but some properties that prevent direct use in the `Listbox` component will be omitted.

#### Interface
```
Omit<ListboxProps, 'multiselectable'>
```
### ChecboxGroup

For instance: `<CheckboxGroup>` extends the `React.InputHTMLAttributes<HTMLInputElement>`, for the combobox component, we will use the current `ChoiceFieldProps` API, but some properties that prevent direct use in the `CheckboxGroup` component will be omitted.

#### Interface
```
Omit<ChoiceFieldProps, 'multiple'>
```
### Datasource Structure

##### Strings Array

```
[ 'Cat', 'Dog', '🐠 Fish' ]
```
##### Object

```
{
	Dog: 'dog',
	Cat: 'cat',
	Hamster: 'hamster',
	Parrot: 'parrot',
	Spider: 'spider',
	Fish: 'fish'
}
```
##### Object Array

```
[
	{
		value: 'dog',
		label: '🐶 Dog'
	},
	{
		value: 'cat',
		label: '🐱 Cat'
	},
	{
		value: 'hamster',
		label: '🐹 Hamster'
	}
]
```

### Accessibility
#### List Autocomplete with Automatic Selection

List autocomplete with automatic selection means that:

1. When the listbox popup is displayed, it contains suggested values that complete or logically correspond to the characters typed in the textbox. In this implementation, the values in the listbox have names that start with the characters typed in the textbox.
2. The first suggestion is automatically highlighted as selected.
3. The automatically selected suggestion becomes the value of the textbox when the combobox loses focus unless the user chooses a different suggestion or changes the character string in the textbox.

#### Keyboard Support
##### Textbox

| Key | Function |
| --- | --- |
|` Down Arrow` | * If the listbox is displayed:<br/> * Example 1: Moves focus to the first suggested value.<br/>    * Examples 2 and 3: Moves focus to the second suggested value. Note that the first value is automatically selected.<br/>* If the listbox is not displayed, in example 3 only, opens the listbox and moves focus to the first value. |
| `Up Arrow` | * If the listbox is displayed, moves focus to the last suggested value.<br/>* If the listbox is not displayed, in example 3 only, opens the listbox and moves focus to the last value. |
| `Enter` | * Example 1: Does nothing.<br/>* Examples 2 and 3: If the listbox is displayed and the first option is automatically selected:<br/>    * Sets the textbox value to the content of the selected option.<br/>    * Closes the listbox. |
| `Escape` | * Clears the textbox<br/>* If the listbox is displayed, closes it. |
| `Standard single line text editing keys` | * Keys used for cursor movement and text manipulation, such as Delete and Shift + Right Arrow.<br/>* An HTML `input` with `type=text` is used for the textbox so the browser will provide platform-specific editing keys. |

#### Listbox Popup

| Key | Function |
| --- | --- |
| `Enter` | * Sets the textbox value to the content of the focused option in the listbox.<br/>* Closes the listbox.<br/>* Sets focus on the textbox. |
| `Escape` | * Closes the listbox.<br/>* Sets focus on the textbox.<br/>* Clears the textbox. |
| `Down Arrow` | * Moves focus to the next option.<br/>* If focus is on the last option, moves focus to the first option.<br/>* Note: This wrapping behavior is useful when Home and End move the editing cursor as described below. |
| `Up Arrow` | * Moves focus to the previous option.<br/>* If focus is on the first option, moves focus to the last option.<br/>* Note: This wrapping behavior is useful when Home and End move the editing cursor as described below. |
| `Right Arrow` | Moves focus to the textbox and moves the editing cursor one character to the right. |
| `Left Arrow` | Moves focus to the textbox and moves the editing cursor one character to the left. |
| `Home` | Moves focus to the textbox and places the editing cursor at the beginning of the field. |
| `End` | Moves focus to the textbox and places the editing cursor at the end of the field. |
| `Printable Characters `| * Moves focus to the textbox.<br/>* Types the character in the textbox. |

#### Role, Property, State, and Tabindex Attributes

##### Combobox

| Role | Attribute | Element | Usage |
| --- | --- | --- | --- |
| `combobox` |     | `div` | * Identifies the element as a combobox.<br/>* Note:<br/>    * Unlike this ARIA 1.1 combobox, an ARIA 1.0 pattern would have the `combobox` role on the textbox input instead of a parent container of the textbox.<br/>    * The ARIA 1.1 pattern demonstrated on this page enables screen reader users to perceive both the input and the popup when using reading mode (see note below about aria-owns). |
|     | `aria-haspopup=listbox` | `div` | * Indicates that the combobox can popup a `listbox` to suggest values.<br/>* This is the default value for elements with the `combobox` role. |
|     | `aria-owns=IDREF` | `div` | * Refers to the element that serves as the listbox popup.<br/>* Tells browsers to arrange the screen reader reading order so the listbox, when it is visible, immediately follows the other elements inside the combobox, regardless of where the listbox element is in the DOM. |
|     | `aria-expanded=false` | `div` | Indicates that the popup element **is not** displayed. |
|     | `aria-expanded=true` | `div` | Indicates that the popup element **is** displayed. |

##### Textbox
| Role | Attribute | Element | Usage |
| --- | --- | --- | --- |
|     | `id="string"` | `input[type="text"]` | * Referenced by `for` attribute of `label` element to provide an accessible label.<br/>* Recommended labeling method for HTML input elements so clicking label focuses input. |
|     | `aria-autocomplete=list` | `input[type="text"]` | Examples 1 and 2: Indicates that the autocomplete behavior of the text input is to suggest a list of possible values in a popup and that the suggestions are related to the string that is present in the textbox. |
|     | `aria-autocomplete=both` | `input[type="text"]` | Example 3: Indicates that the autocomplete behavior of the text input is to both show an inline completion string and suggest a list of possible values in a popup where the suggestions are related to the string that is present in the textbox. |
|     | `aria-controls=IDREF` | `input[type="text"]` | * Identifies the popup element that lists suggested values.<br/>* Note:<br/>    * In the ARIA 1.0 combobox pattern, the textbox has `aria-owns` instead of `aria-controls`.<br/>    * In this ARIA 1.1 pattern, `aria-owns` is instead on the parent container so the popup element is a sibling of the textbox instead of a child of the textbox, making it perceivable by screen reader users as an element adjacent to the textbox when using a reading cursor or touch interface. |
|     | `aria-activedescendant=IDREF` | `input[type="text"]` | * When an option in the listbox is visually indicated as having keyboard focus, refers to that option.<br/>* When navigation keys, such as Down Arrow, are pressed, the JavaScript changes the value.<br/>* Enables assistive technologies to know which element the application regards as focused while DOM focus remains on the `input` element.<br/>* For more information about this focus management technique, see [Using aria-activedescendant to Manage Focus.](../../../#kbd_focus_activedescendant) |

##### Listbox popup

| Role | Attribute | Element | Usage |
| --- | --- | --- | --- |
| `listbox` |     | `ul` | Identifies the `ul` element as a `listbox`. |
|     | `aria-labelledby=IDREF` | `ul` | Provides a label for the `listbox` element of the combobox. |
| `option` |     | `li` | * Identifies the element as a `listbox` `option`.<br/>* The text content of the element provides the accessible name of the `option`. |
|     | `aria-selected=true` | `li` | * Specified on an option in the listbox when it is visually highlighted as selected.<br/>* In example 1, occurs only when an option in the list is referenced by `aria-activedescendant`.<br/>* In examples 2 and 3, also occurs when focus is in the textbox and the first option is automatically selected. |
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
