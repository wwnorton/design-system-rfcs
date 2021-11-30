- Start Date: 2021-10-01
- Specification: ([Table](https://docs.google.com/document/d/1YPO3KZNZkY3amEK2xtpSND4ajHXzms_FrTwZ3TQWnWw/edit))
- Design: ([Table](https://app.zeplin.io/project/5d66e28439bbe3139aa846ad/screen/6148d737ee5b5557ca49792c))
- RFC PR:

## Summary

Use a table to organize and display long lists of data or content, or to help users find a specific piece of information in a large data set. If a table has no content, it will display an empty state or will not display the table at all. A table's content may include Text, Numbers, Checkboxes or radio buttons or custom content, such as buttons.
Tables must have at least 2 columns.Column titles in the header row should be as short as possible, but can wrap to a 2nd row and then truncate if necessary. Table content will be ideally fit on one row but may wrap to multiple lines. Column titles will be accurately label the data within the column, and include units of measurement in the column title if applicable. Now a day most of the applications expecting data table, this table component also works as a manual table and data table. DataSource and DataColumn JSON array properties convert a table into the data table.

- `<Table>`        - A table allows users to view data organized in rows and columns and in some cases perform actions on it.
- `<TableHeader>`  - The first row of a table contains cells that act as labels for the columns..
- `<TableRow>`     - A horizontal slice of the table groups related cells.
- `<TableCell>`    - A vertical slice of the table groups cells related by the label in the header cell.

## Detailed design

### Table

`<Table>` extends the `React.TableHTMLAttributes<HTMLTableElement>` following are properties:

| Name     | Type | Description                              | Required | Default     |
| -------- | ----- | ----------------------------------- | -------- | ----------- |
| `tableHeader` | Node | Top row component. | `false`  | `undefined` |
| `tableRow` | Node | Element defines a row of cells | `false`  | `undefined` |
| `dataSource` | JSON | Indicates array of JSON formatted row data | `false`  | `undefined` |
| `dataColumn` | JSON | Indicates array of JSON formatted column data | `false`  | `undefined` |
| `className` | String | Override or extend existing table style.  | `false`  | `undefined` |
| `border` | Boolean | Indicates whether table with or without border. | `false`  | `undefined` |
| `sort` | Boolean | Sortable header maintain current sort state, which can be ascending, descending, or unsorted. | `false`  | `undefined` |

### JSX Example

```
<Table>
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

#### Render Example

```
<table>  
    <thead>    
        <tr>      
            <th>First Name</th>      
            <th>Last Name</th>    
        </tr>  
    </thead>  
    <tr>   
        <td>Marissa</td>    
        <td>Keep</td>  
    </tr>
    <tr>   
        <td>Andrew</td>    
        <td>Arnold</td>  
    </tr>
</table>
```

#### DataColumn JSON

RowKey is the mapping between DataSource and DataColumn based on this mapping table convert all the json data in to rows and columns format. A cellFormatter formate cell as per the requirment.

```
[
   {
    name: "First Name",
    rowKey: "firstName",
   },
   {
    name: "Last Name",
    rowKey: "lastName",
    cellFormatter: row => <LastNameFormatter row={row} />,
   }
]
```

#### DataSource JSON

```
[
    {
        firstName: "Marissa",
        lastName: "Keep",
    },
     {
        firstName: "Andrew",
        lastName: "Arnold",
    }
]
```

### TableHeader

`<TableHeader>` extends the `React.TableHTMLAttributes<HTMLTableSectionElement>` following are properties:

| Name     | Type | Description                              | Required | Default     |
| -------- |--| --------------------------------------- | -------- | ----------- |
| `tableCell` | Node | Element defines a cell of data in a table. | `false`  | `undefined` |
| `variant` | Variant|  Define header style Ghost, Outline and Solid. | `false`  | `Solid` |
| `stickyHeader` | Boolean| Indicates whether the table header is sticky. | `false`  | `undefined` |
| `className` |String | Override or extend existing table style. | `false`  | `undefined` |

#### Render Example

```
<thead>    
  <tr>      
   <th></th>      
   <th colspan="2"></th>    
  </tr>  
 </thead>  
```

### TableRow

`<TableRow>` extends the `React.TableHTMLAttributes<HTMLTableRowElement>` following are properties:

| Name     | Type | Description                              | Required | Default     |
| -------- | ---| ------------------------------------- | -------- | ----------- |
| `tableCell` | Node | Element defines a cell of data in a table. | `false`  | `undefined` |
| `className` | String | Override or extend existing table style. | `false`  | `undefined` |
| `dataColumn` | JSON | Indicates array of JSON formatted column data | `false`  | `undefined` |

#### Render Example

```
<tr>   
    <th scope="row"></th>    
    <td></td>    
    <td></td>  
</tr>
```

### TableCell

`<TableCell>` extends the `React.TableHTMLAttributes<HTMLTableColElement>` following are properties:

| Name     | Type | Description                              | Required | Default     |
| -------- | -- |---------------------------------------- | -------- | ----------- |
| `colSpan` | Number | Specifies the number of columns a cell should span. | `false`  | `undefined` |
| `header` | Boolean | Indicates a cell is header, header converts in `<th>` by default `<td>` | `false`  | `<td>` |
| `className` | String | Override or extend existing table style. | `false`  | `undefined` |
| `alignment` | Variant | Indicates cell alignment. | `false`  | `Left` |
| `type` | Variant | Indicates cell type whether `Text,Number,Date,Boolean,Custom` | `false`  | `Text`  |
| `cellFormatter` | Callback | Indicates React component or string. | `false`  | `undefined` |
| `data` | JSON | Indicates array of JSON formatted data | `false`  | `undefined` |

#### Render Example

```
    <td></td>   
```

Table cell use in header element.

```
    <th></th>   
```

## Drawbacks

Why should we not do this? Please consider:

- Pagination is not supported in this version may be in a future version we will support, but for the workaround developer can implement their own pagination as per the application design when uses data table.
- Column level filters are not supported but developer can write their own filters using cellFormatter.

## Alternatives

There are many alternatives avalible in the market.

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

    ```
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

    ```
        [
        {
            name: "First Name",
            rowKey: "firstName",
            sort: false
        },
        {
            name: "Last Name",
            rowKey: "lastName",
            cellFormatter: row => <LastNameFormatter row={row} />,
        }
        ]
    ```

- As per the application's requirements cell can be formatted using ```cellFormatter```
- By default, all the cells values are left align but as per the column type cell values can auto align.

    ```
        [
        {
            name: "User Name",
            rowKey: "userName",
        },
        {
            name: "Experience",
            rowKey: "exp",
            type: 'number' // This column is right align.
        }
        ]
    ```

- Easy to customize.

## Unresolved questions

Events are in TBD
