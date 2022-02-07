- Start Date: 2021-10-01
- Specification: ([Table](https://docs.google.com/document/d/1YPO3KZNZkY3amEK2xtpSND4ajHXzms_FrTwZ3TQWnWw/edit))
- Design: ([Table](https://app.zeplin.io/project/5d66e28439bbe3139aa846ad/screen/6148d737ee5b5557ca49792c))
- RFC PR: https://github.com/wwnorton/design-system-rfcs/pull/2

## Summary

Use a table to organize and display long lists of data or content, or to help users find a specific piece of information in a large data set. If a table has no content, it will display an empty state or will not display the table at all. A table's content may include Text, Numbers, Checkboxes or radio buttons or custom content, such as buttons.
Tables must have at least 2 columns.Column titles in the header row should be as short as possible, but can wrap to a 2nd row and then truncate if necessary. Table content will be ideally fit on one row but may wrap to multiple lines. Column titles will be accurately label the data within the column, and include units of measurement in the column title if applicable. Now a day most of the applications expecting data table, this table component also works as a manual table and data table. DataSource and DataColumn JSON array properties convert a table into the data table.

- `<Table>` - A table allows users to view data organized in rows and columns and in some cases perform actions on it.
- `<TableHeader>` - The first row of a table contains cells that act as labels for the columns..
- `<TableRow>` - A horizontal slice of the table groups related cells.
- `<TableCell>` - A vertical slice of the table groups cells related by the label in the header cell.
- `<DataTable>` - A table allow user to pass data into JSON format and render it into rows and column format.

## Detailed design

### Table

`<Table>` extends the `React.TableHTMLAttributes<HTMLTableElement>` following are properties:

| Name           | Type    | Description                                                                       | Required | Default     |
| -------------- | ------- | --------------------------------------------------------------------------------- | -------- | ----------- |
| `stickyHeader` | boolean | Indicates whether the table header is sticky.                                     | `false`  | `undefined` |
| `className`    | string  | Override or extend existing table style.                                          | `false`  | `undefined` |
| `border`       | boolean | Indicates whether table with or without border.                                   | `false`  | `undefined` |
| `sort`         | variant | Sortable header maintain current sort state, which can be asc, desc, or unsorted. | `false`  | `asc`       |

Table examples on line 133

### TableHeader

`<TableHeader>` extends the `React.TableHTMLAttributes<HTMLTableSectionElement>` following are properties:

| Name           | Type    | Description                                   | Required | Default     |
| -------------- | ------- | --------------------------------------------- | -------- | ----------- |
| `variant`      | variant | Define header style ghost, outline and solid. | `false`  | `solid`     |
| `stickyHeader` | boolean | Indicates whether the table header is sticky. | `false`  | `undefined` |
| `className`    | string  | Override or extend existing table style.      | `false`  | `undefined` |

#### Render Example

```js
//stickyHeader props pass on TableHeader or can pass on table level.
//Table component props carry forward to TableHeader.
import { Table } from "@wwnds/react";

const header = [
	{
		name: "First name",
		key: "first_name",
		sort: "ascending",
	},
	{
		name: "Last name",
		key: "last_name",
	},
];
const myStickyHeaderTable = ({ header }) => (
	<Table>
		<TableHeader stickyHeader solid dataColumn={header} />
	</Table>
);
```

### TableRow

`<TableRow>` extends the `React.TableHTMLAttributes<HTMLTableRowElement>` following are properties:

| Name        | Type   | Description                              | Required | Default     |
| ----------- | ------ | ---------------------------------------- | -------- | ----------- |
| `className` | string | Override or extend existing table style. | `false`  | `undefined` |

#### Render Example

```js
//datasource props can pass to Table component or TableRow component.
//Table component dataSource props carry forward to TableRow.
//Or can directly pass props to TableRow.
import { Table } from "@wwnds/react";
const rows = ["Marissa", "Andrew"];
const myTableWithDataColumn = ({ rows }) => (
	<Table>
		<TableHeader>
			<TableCell>First Name</TableCell>
		</TableHeader>
		<TableRow dataSource={rows} />
	</Table>
);
```

### TableCell

`<TableCell>` extends the `React.TableHTMLAttributes<HTMLTableColElement>` following are properties:

| Name            | Type     | Description                                                                        | Required | Default     |
| --------------- | -------- | ---------------------------------------------------------------------------------- | -------- | ----------- |
| `colSpan`       | number   | Specifies the number of columns a cell should span.                                | `false`  | `undefined` |
| `header`        | boolean  | Indicates a cell is header, header converts in `<th>` by default `<td>`            | `false`  | `<td>`      |
| `className`     | string   | Override or extend existing table style.                                           | `false`  | `undefined` |
| `alignment`     | variant  | Indicates cell alignment.                                                          | `false`  | `Left`      |
| `type`          | variant  | Indicates cell type whether `Text,Number,Date,Boolean,Custom`                      | `false`  | `Text`      |
| `cellFormatter` | Callback | Indicates React component or string.                                               | `false`  | `undefined` |
| `value`         | variant  | Indicates table cell value in any format whether `Text,Number,Date,Boolean,Custom` | `false`  | `string`    |

#### Render Example

```js
import { Table, TableHeader, TableRow, TableCell } from "@wwnds/react";

//Table with different types of cell values.
//Table cell inside TableHeader convert into th.
//Table cell inside TableRow convert into tr.
const myTableWithDifferentCellValues= () => (
  <Table>
    <TableHeader>
      <TableCell>First Name</TableCell>
      <TableCell value="Last Name"/>
    </TableHeader>
    <TableRow>
      <TableCell>Marissa</TableCell>
      <TableCell value="Keep"/>
    </TableRow>
     <TableRow>
      <TableCell>Andrew</TableCell>
      <TableCell><span<b>Arnold</b></span></TableCell>
    </TableRow>
  </Table>
);
```

### Composition Example

```js
import { Table, TableHeader, TableRow, TableCell, Button } from "@wwnds/react";

const employees = [
	{
		FirstName: "Marissa",
		LastName: "Keep",
	},
	{
		FirstName: "Andrew",
		LastName: "Arnold",
	},
];

const detailView = () => {
	return <Button>Click for info</Button>;
};

const myTable = ({ employees, ...options }) => (
	<Table {...options}>
		<TableHeader>
			<TableCell>First Name</TableCell>
			<TableCell>Last Name</TableCell>
			<TableCell>Info</TableCell>
		</TableHeader>
		employees.map((employee)=>
		<TableRow>
			<TableCell>{employee.FirstName}</TableCell>
			<TableCell>{employee.LastName}</TableCell>
			<TableCell cellFormatter={detailView} />
		</TableRow>
		)
	</Table>
);
```

#### Render Example

```js
<table>
	<thead>
		<tr>
			<th>First Name</th>
			<th>Last Name</th>
			<th>Info</th>
		</tr>
	</thead>
	<tr>
		<td>Marissa</td>
		<td>Keep</td>
		<td>
			<Button>Click for info</Button>
		</td>
	</tr>
	<tr>
		<td>Andrew</td>
		<td>Arnold</td>
		<td>
			<Button>Click for info</Button>
		</td>
	</tr>
</table>
```

Expected output

| First Name | Last Name | Info                                                                                                             |
| ---------- | --------- | ---------------------------------------------------------------------------------------------------------------- |
| Marissa    | Keep      | <Button name="button" onclick="https://wwnorton.github.io/design-system/docs/components">Click for info</Button> |
| Andrew     | Arnold    | <Button name="button" onclick="https://wwnorton.github.io/design-system/docs/components">Click for info</Button> |

### Data Table

`<DataTable>` Render JSON formatted rows and columns data internally this component is the wrapper on table component, following are properties:

| Name           | Type    | Description                                                                       | Required | Default     |
| -------------- | ------- | --------------------------------------------------------------------------------- | -------- | ----------- |
| `rows`         | array   | Indicates array of JSON formatted row data                                        | `true`   | []          |
| `header`       | array   | Indicates array of JSON formatted column data.                                    | `true`   | []          |
| `stickyHeader` | boolean | Indicates whether the table header is sticky.                                     | `false`  | `undefined` |
| `className`    | string  | Override or extend existing table style.                                          | `false`  | `undefined` |
| `border`       | boolean | Indicates whether table with or without border.                                   | `false`  | `undefined` |
| `sort`         | variant | Sortable header maintain current sort state, which can be asc, desc, or unsorted. | `false`  | `asc`       |

```js
import { DataTable } from "@wwnds/react";

const header = [
	/**
	 * Simple form: a string for the name. Internally, it's transformed
	 * into the { name, key, sort, etc. } form with all defaults and a
	 * key that's just the string in camelCase form.
	 */
	"First name",
	/**
	 * Complete form: a header cell can be a full object to customize
	 * things such as sorting or to use a custom key.
	 */
	{
		name: "Last name",
		key: "last_name",
		sort: "ascending",
	},
];

const rows = [
	/**
	 * Simplest form: a tuple that maps to the order of the header cells.
	 */
	["Marissa", "Keep"],
	/**
	 * Object form (simple): an unordered set of key: value pairs that
	 * correspond to the keys given in the header.
	 */
	{ firstName: "Andrew", last_name: "Arnold" },
	/**
	 * Complete form: a collection that allows you to customize some part
	 * of individual cells.
	 */
	[
		{ key: "firstName", value: "Andrew" },
		{
			// key: 'lastName', // if not specified, use the position in the array?
			value: "Arnold",
			// give Mr. Arnold that hat with a render function!
			cellFormatter: (cell: React.ReactNode) => <>{cell} 🎩</>,
		},
	],
];

// DataTable is separate component to handle data-releated functionalities.
// Pass header to dataColumn props and rows pass to datasource props and
// component convert dataColumn into header and dataSource into rows.
const DataTableExample = ({ header, rows, ...options }) => (
	<DataTable {...options} header={header} rows={rows}></DataTable>
);

/// Internal implementation of the dataTable.
const DataTable = ({ header, rows, ...options }) => (
	<Table {...options}>
		<TableHeader>{header.map(headerMappingFunction)}</TableHeader>
		{rows.map(rowMappingFunction)}
	</Table>
);
```

#### Output of Data driven example

| First Name | Last Name |
| ---------- | --------- |
| Marissa    | Keep      |
| Andrew     | Arnold    |
| Andrew     | Arnold 🎩 |

## Drawbacks

Why should we not do this? Please consider:

- Pagination is not supported in this version may be in a future version we will support, but for the workaround developer can implement their own pagination as per the application design when uses data table.
- Column level filters are not supported but developer can write their own filters using cellFormatter.

## Alternatives

There are many alternatives avalible but this component provide flixiblity to add rows or column without restricting any data type please check each of the examples.There is cell level customazation can format data as per the application requiments.

- User can create manual table using simple table html tags.
- [Ant design](https://ant.design/components/table/)
- [Shopify](https://polaris.shopify.com/components/lists-and-tables/data-table#navigation)
- [Chakra](https://chakra-ui.com/docs/data-display/table)

## Adoption strategy

This component considering all the existing application and trying to adopt common features of the table.
Following are the features application can adopt.

- Effortless style (zero CSS changes) for Norton applications.
- Existing style can be overridden using tokens.
- Sorting enabled without zero code work by default for all the columns.

  ```js
  <Table sort>
  	<TableHeader>
  		<TableCell>First Name</TableCell>
  		<TableCell>Last Name</TableCell>
  	</TableHeader>
  	<TableRow>
  		<TableCell>Marissa</TableCell>
  		<TableCell>Keep</TableCell>
  	</TableRow>
  	<TableRow>
  		<TableCell>Andrew</TableCell>
  		<TableCell>Arnold</TableCell>
  	</TableRow>
  </Table>
  ```

- Sorting can hide for specific column(s).

  ```js
  [
  	{
  		name: "First Name",
  		rowKey: "firstName",
  		sort: "ascending",
  	},
  	{
  		name: "Last Name",
  		rowKey: "lastName",
  		cellFormatter: (row) => <LastNameFormatter row={row} />,
  	},
  ];
  ```

- As per the application's requirements cell can be formatted using `cellFormatter`
- By default, all the cells values are left align but as per the column type cell values can auto align.

  ```js
  [
  	{
  		name: "User Name",
  		rowKey: "userName",
  	},
  	{
  		name: "Experience",
  		rowKey: "exp",
  		type: "number", // This column is right align.
  	},
  ];
  ```

- Easy to customize.

## Unresolved questions

Events are in TBD
