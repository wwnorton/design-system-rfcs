- Start Date: 2024-08-09
- Working documentation: [Google Docs - Table](https://docs.google.com/document/d/1YPO3KZNZkY3amEK2xtpSND4ajHXzms_FrTwZ3TQWnWw/edit)
- Visual design: [Zeplin - Table](https://app.zeplin.io/project/5d66e28439bbe3139aa846ad/screen/6148d737ee5b5557ca49792c)
- RFC PR:

## Summary

Use a table to organize and display long lists of data or content, or to help users find a specific piece of information in a large data set. If a table has no content, it will display an empty state or will not display the table at all. A table's content may include Text, Numbers, Checkboxes or radio buttons or custom content, such as buttons.
Tables must have at least 2 columns.Column titles in the header row should be as short as possible, but can wrap to a 2nd row and then truncate if necessary. Table content will be ideally fit on one row but may wrap to multiple lines. Column titles will be accurately label the data within the column, and include units of measurement in the column title if applicable. Now a day most of the applications expecting data table, this table component also works as a manual table and data table. DataSource and DataColumn JSON array properties convert a table into the data table.

- `<Table>` - A table allows users to view data organized in rows and columns and in some cases perform actions on it.
- `<TableHeader>` - The first row of a table contains cells that act as labels for the columns.
- `<TableHeaderCell>` - A cell in the header row of a table that contains a label for a column.
- `<TableRow>` - A horizontal slice of the table groups related cells.
- `<TableCell>` - A vertical slice of the table groups cells related by the label in the header cell.

## Detailed design

### Table

`<Table>` extends the `React.TableHTMLAttributes<HTMLTableElement>` following are properties:

| Name           | Type      | Description                                                                       | Required | Default     |
| -------------- | --------- | --------------------------------------------------------------------------------- | -------- | ----------- |
| `stickyHeader` | boolean   | Indicates whether the table header is sticky.                                     | `false`  | `undefined` |
| `className`    | string    | Override or extend existing table style.                                          | `false`  | `undefined` |
| `border`       | boolean   | Indicates whether table with or without border.                                   | `false`  | `undefined` |
| `sortable`     | boolean   | Sortable header maintain current sort state, which can be asc, desc, or unsorted. | `false`  | `undefined` |
| `variant`      | variant   | Define header style ghost, outline and solid.                                     | `false`  | `solid`     |
| `data`         | TableData | Data to be rendered in the table.                                                 | `false`  | `undefined` |

The `TableData` object allows to define the data of the table where we don't care about its structure and the default structure can be used. This object is made of the following properties:

| Name      | Type                        | Description                       | Required | Default |
| --------- | --------------------------- | --------------------------------- | -------- | ------- |
| `headers` | Array<TableHeaderData>      | The data that defined the headers | `true`   | ---     |
| `rows`    | Array<Array<TableCellData>> | The data that defined the rows    | `true`   | ---     |

`TableHeaderData` object properties:

| Name      | Type      | Description                                                                            | Required | Default     |
| --------- | --------- | -------------------------------------------------------------------------------------- | -------- | ----------- |
| `element` | ReactNode | The element to render inside the data cell                                             | `true`   | ---         |
| `sorter`  | Function  | Used for Uncontrolled Sorting, overrides the default sorting function for this column. | `false`  | `undefined` |

`TableCellData` object properties:

| Name      | Type                    | Description                                             | Required | Default     |
| --------- | ----------------------- | ------------------------------------------------------- | -------- | ----------- |
| `value`   | string, boolean, number | The value of the cell                                   | `true`   | ---         |
| `wrapper` | ReactComponent          | The react component used to wrap the value to render it | `false`  | `undefined` |

#### Render Example

In its simplest form the **Table Component** can be rendered by feeding data in the `data` prop.

```js
import { Table } from "@wwnds/react";

function YearWrapper({ value }) {
  return <span>{value} years</span>;
}

const data = [
  {
    headers: [
      {
        element: "First Name",
      },
      {
        element: "Last Name",
      },
      {
        element: "Age",
      },
    ],
    rows: [
      [
        {
          value: "Marissa",
        },
        {
          value: "Keep",
        },
        {
          value: 25,
          wrapper: YearWrapper,
        },
      ],
      [
        {
          value: "Andrew",
        },
        {
          value: "Arnold",
        },
        {
          value: 31,
          wrapper: YearWrapper,
        },
      ],
    ],
  },
];

function TableExample() {
  return <Table data={data} />;
}
```

You can also render the table using composition, by passing children to the Table component. This allows for more flexibility.

```js
import { Table, TableHeader, TableBody, TableCell } from "@wwnds/react";

function TableExample() {
	return (
		<Table>
			<TableHeader>
				<TableHeaderCell>First Name</TableCell>
				<TableHeaderCell>Last Name</TableCell>
				<TableHeaderCell>Age</TableCell>
			</TableHeader>
			<TableBody>
				<TableRow>
					<TableCell>Marissa</TableCell>
					<TableCell>Keep</TableCell>
					<TableCell>25 years</TableCell>
				</TableRow>
				<TableRow>
					<TableCell>Andrew</TableCell>
					<TableCell>Arnold</TableCell>
					<TableCell>31 years</TableCell>
				</TableRow>
			</TableBody>
		</Table>

	)
}
```

### Subcomponents

#### TableHeader

`<TableHeader>` extends the `React.TableHTMLAttributes<HTMLTableSectionElement>` following are properties:

| Name        | Type   | Description                              | Required | Default     |
| ----------- | ------ | ---------------------------------------- | -------- | ----------- |
| `className` | string | Override or extend existing table style. | `false`  | `undefined` |

#### TableHeaderCell

`<TableHeaderCell>` extends the `React.TableHTMLAttributes<HTMLTableHeaderCellElement>` following are properties:

| Name        | Type                           | Description                                                                            | Required | Default     |
| ----------- | ------------------------------ | -------------------------------------------------------------------------------------- | -------- | ----------- |
| `className` | string                         | Override or extend existing table style.                                               | `false`  | `undefined` |
| `sorter`    | Function                       | Used for Uncontrolled Sorting, overrides the default sorting function for this column. | `false`  | `undefined` |
| `sorted`    | `'asc'`, `'desc'`, `undefined` | Used for Controlled Sorting, defines the current sort state of the column.             | `false`  | `undefined` |

#### TableBody

`<TableBody>` extends the `React.TableHTMLAttributes<HTMLTableSectionElement>` following are properties:

| Name        | Type   | Description                              | Required | Default     |
| ----------- | ------ | ---------------------------------------- | -------- | ----------- |
| `className` | string | Override or extend existing table style. | `false`  | `undefined` |

#### TableRow

`<TableRow>` extends the `React.TableHTMLAttributes<HTMLTableRowElement>` following are properties:

| Name        | Type   | Description                              | Required | Default     |
| ----------- | ------ | ---------------------------------------- | -------- | ----------- |
| `className` | string | Override or extend existing table style. | `false`  | `undefined` |

#### TableCell

`<TableCell>` extends the `React.TableHTMLAttributes<HTMLTableCellElement>` following are properties:

| Name        | Type   | Description                              | Required | Default     |
| ----------- | ------ | ---------------------------------------- | -------- | ----------- |
| `className` | string | Override or extend existing table style. | `false`  | `undefined` |

### Sorting

Sorting is enabled by setting the prop `sortable` to `true`. More controls are given to the sorting functionality and they are described in the sections below.

#### Uncontrolled Sorting using the `data` prop

The most basic example is as follows:

```js
import { Table } from "@wwnds/react";

const data = [
  // ... data is defined here
];

function TableExample() {
  return <Table sortable data={data} />;
}
```

The **default** sorting function uses JS comparison operators, so the [coercion rules for comparison](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality#description) apply when 2 values aren't of the same type, all values in a column **SHOULD** be of the same type. To override the sorting function for a column, define the `sorter` property in the `TableHeaderData` object:

```js
import { Table } from "@wwnds/react";

const data = [
  {
    headers: [
      {
        element: "First Name",
        sorter: (a, b) => {
          // ... custom sorting logic
        },
      },
      // ... other headers
    ],
    rows: [
      // ... rows are defined here
    ],
  },
];

function TableExample() {
  return <Table data={data} />;
}
```

#### Uncontrolled Sorting using the composition pattern

The most basic example is as follows:

```js
import { Table } from "@wwnds/react";

function TableExample() {
	return (
		<Table sortable>
			<TableHeader>
				<TableHeaderCell>First Name</TableCell>
				{/* ... other header cells */}
			</TableHeader>
			<TableBody>
                                {/* ... rows */}
			</TableBody>
		</Table>

	)
}
```

The **default** sorting function uses JS comparison operators, so the [coercion rules for comparison](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality#description) apply when 2 values aren't of the same type, all values in a column **SHOULD** be of the same type. To override the sorting function for a column, define the `sorter` prop in the `TableHeaderCell` component:

```js
import { Table } from "@wwnds/react";

function TableExample() {
	return (
		<Table sortable>
			<TableHeader>
				<TableHeaderCell sorter={(a, b) => { /* ... custom sorting */ }}>First Name</TableCell>
				{/* ... other header cells */}
			</TableHeader>
			<TableBody>
                                {/* ... rows */}
			</TableBody>
		</Table>

	)
}
```

#### Controlled Sorting using the `data` prop

The most basic example is as follows:

```js
import { Table } from "@wwnds/react";

const data = [
    headers: [
      {
        element: "First Name",
        sorted: "asc",
      },
      {
        element: "Last Name",
        sorted: "desc",
      },
      {
        element: "Age",
      },
    ],
    // rows
];

function TableExample() {
  return (
    <Table
      sortable
      onSort={(columnIndex, direction) => {
        /* your custom logic */
      }}
      data={data}
    />
  );
}
```

In `data.headers` the `sorted` property specifies the 3 possible states for the column: _ascending_, _descending_, or _default_ sorting. This will affect the way the sort indicator (button) is displayed.

Since this is the controlled approach, the sorting of the rows is 100% responsibility of the application.

The `onSort` callback is called when the user clicks on a sortable header. The callback receives the `columnIndex` as defined in `data.headers` and the `direction` of the sort `asc`, `desc`, or `default`.

#### Controlled Sorting using the composition pattern

The most basic example is as follows:

```js
import { Table } from "@wwnds/react";

function TableExample() {
	return (
		<Table sortable onSort={(columnIndex, order) => { /* custom sorting logic */ }}>
			<TableHeader>
				<TableHeaderCell sorted="asc">First Name</TableCell>
				{/* ... other header cells */}
			</TableHeader>
			<TableBody>
                                {/* ... rows */}
			</TableBody>
		</Table>

	)
}
```

In `TableHeaderCell` the `sorted` property specifies the 3 possible states for the column: _ascending_, _descending_, or _default_ sorting. This will affect the way the sort indicator (button) is displayed.

Since this is the controlled approach, the sorting of the rows is 100% responsibility of the application.

The `onSort` callback is called when the user clicks on a sortable header. The callback receives the `columnIndex` as defined in `data.headers` and the `direction` of the sort `asc`, `desc`, or `default`.

## Drawbacks

## Alternatives

## Adoption strategy

## Unresolved questions
