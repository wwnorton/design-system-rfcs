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

```typescript
interface TableProps extends React.TableHTMLAttributes<HTMLTableElement> {
  /**
   * Indicates whether the table header is sticky.
   */
  stickyHeader?: boolean;

  /**
   * Override or extend existing table style.
   */
  className?: string;

  /**
   * Indicates whether table with or without border.
   */
  hasBorder?: boolean;

  /**
   * Used to enable sorting in the table.
   * For Uncontrolled Sorting, this activates the internal sorting state of the Table component.
   * For Controlled Sorting, this just renders the buttons in the headers that will call `onSort`.
   */
  isSortable?: boolean;

  /**
   * Used for Controlled Sorting. When set, the Table component will not manage the sorting state.
   * `onSort` is called when the user clicks on one of the column sorting buttons. It receives
   * the `columnIndex` and the `direction` of the sort `asc`, `desc`, or `default`.
   */
  onSort?: (columnIndex: number, direction: "asc" | "desc" | undefined) => void;

  /**
   * Define header style ghost, outline and solid.
   *
   * @default 'solid'
   */
  variant?: "ghost" | "outline" | "solid";

  /**
   * The data to be rendered in the table.
   */
  data?: TableData;
}
```

The `TableData` object allows to define the data of the table where we don't care about its structure and the default structure can be used. This object is made of the following properties:

```typescript
interface TableData {
  headers: TableDataHeader[];
  rows: TableDataCell[][];
}

type SortableValue = string | number | boolean;

interface TableDataHeader {
  /**
   * The element to render inside the data cell
   */
  children: ReactNode;

  /**
   * Used for Uncontrolled Sorting, overrides the default sorting function for this column.
   */
  sorter?: (a: SortableValue, b: SortableValue) => void;
}

interface TableDataCell {
  /**
   * The value of the cell
   */
  value: SortableValue;

  /**
   * The react component used to wrap the value to render it
   */
  wrapper?: (value: SortableValue) => ReactNode;
}
```

#### Render Example

In its simplest form the **Table Component** can be rendered by feeding data in the `data` prop.

```js
import { Table } from "@wwnds/react";

function YearWrapper({ value }) {
  return <span>{value} years</span>;
}

const data = {
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
        wrapper: (value) => <YearWrapper value={value} />,
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
        wrapper: (value) => <YearWrapper value={value} />,
      },
    ],
  ],
};

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

```typescript
interface TableHeaderProps
  extends React.TableHTMLAttributes<HTMLTableSectionElement> {
  /**
   * Override or extend existing table style.
   */
  className?: string;
}
```

#### TableHeaderCell

```typescript
interface TableHeaderCellProps
  extends React.TableHTMLAttributes<HTMLTableHeaderCellElement> {
  /**
   * Override or extend existing table style.
   */
  className?: string;

  /**
   * Used for Uncontrolled Sorting, overrides the default sorting function for this column.
   */
  sorter?: (a: SortableValue, b: SortableValue) => void;

  /**
   * Used for Controlled Sorting, defines the current sort state of the column.
   */
  sorted?: "asc" | "desc" | undefined;
}
```

#### TableBody

```typescript
interface TableBodyProps
  extends React.TableHTMLAttributes<HTMLTableSectionElement> {
  /**
   * Override or extend existing table style.
   */
  className?: string;
}
```

#### TableRow

```typescript
interface TableRowProps extends React.TableHTMLAttributes<HTMLTableRowElement> {
  /**
   * Override or extend existing table style.
   */
  className?: string;
}
```

#### TableCell

```typescript
interface TableCellProps
  extends React.TableHTMLAttributes<HTMLTableCellElement> {
  /**
   * Override or extend existing table style.
   */
  className?: string;

  /**
   * The value of the cell.
   * In uncontrolled sorting the value is used to sort the column. If none is defined
   * the text content of the cell is used as value.
   */
  value?: SortableValue;
}
```

### Sorting

Sorting is enabled by setting the prop `sortable` to `true`.

By default, **Sorting is Uncontrolled** and the Table component will manage the internal state.

**Controlled Sorting** can be enabled by passing a callback as the `onSort` prop. This will disable the internal sorting state of the Table component. All sorting of the `data` will then be the responsibility of the application.

#### Default Sorting Function for Uncontrolled Sorting

It's defined as:

```js
function defaultSorter(a, b) {
  let direction;
  switch (sorted) {
    case "asc":
      direction = 1;
      break;
    case "desc":
      direction = -1;
    default:
      direction = 0;
  }

  if (!direction) {
    return 0;
  }

  if (a < b) {
    return -1 * direction;
  }
  if (a > b) {
    return 1 * direction;
  }

  return 0 * direction;
}
```

#### Uncontrolled Sorting using the `data` prop

The most basic example is as follows:

```js
import { Table } from "@wwnds/react";

const data = {
  // ... data is defined here
};

function TableExample() {
  return <Table isSortable data={data} />;
}
```

To override the Default Sorting Function for a column:

```js
import { Table } from "@wwnds/react";

const data = {
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
};

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
				<TableHeaderCell>Last Name</TableCell>
				<TableHeaderCell>Age</TableCell>
				<TableHeaderCell>Goals</TableCell>
			</TableHeader>
			<TableBody>
				<TableRow>
                                  <TableCell>Andrew</TableCell>
                                  <TableCell>Arnold</TableCell>
                                  <TableCell value={31}>31 Years</TableCell>
                                  <TableCell value={5}>5</TableCell>
				</TableRow>
			</TableBody>
		</Table>

	)
}
```

`TableCell` allows to define a `value` prop to be used by the internal sorting algorithm to the sort values in the column. This is not requried and if not passed, the component uses the text content of the cell. This means that if `value` is not defined, the type of the value will _ALWAYS_ be string.

To override the default sorting function for a column:

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

const data = {
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
};

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
		<Table isSortable onSort={(columnIndex, order) => { /* custom sorting logic */ }}>
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

- How to enforce typing of values in the same column? Should we enforce it? Can we provide a helper type for the data object?
