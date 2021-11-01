- Start Date: 2021-10-01
- Specification: ([Table](<https://wwnorton1.sharepoint.com/:w:/g/DP/products_and_projects/nds/EUXxmi6yZvBLiSLZLKo7M2cBuFiWRXtQ_lM1_OW0Smm5mQ?e=nutOb4>))
- Design: ([Table](https://app.zeplin.io/project/5d66e28439bbe3139aa846ad/screen/6148d737ee5b5557ca49792c))
- RFC PR:

## Summary

Table component is a web component that enables you to create rows and columns of known data. Table components consist of as many columns and rows you need, with the column being used to specify the type of values like text, number, date or custom and the rows being used to contain the values themselves. Table also have a requirement that you choose at least two columns in the table. Now a day most of the applications expecting data table, this table component alsow work as a manual table and data table. DataSource and DataColumn JSON array properties convert a table into the data table.

- `<Table>`        - A table with one header and simple data are fairly accessible out of the box and may not need additional accessibility updates.
- `<TableHeader>`  - Is a row at the top of a table contains column title, with sorting, title, sticky header properties.
- `<TableRow>`     - A table row can contain as many cells as required, each one defined in a different column. There must be at least one column defined.
- `<TableCell>`    - This should contain the data or information required for each relevant column and row.

## Detailed design

### Table

`<Table>` extends the `React.forwardRef<HTMLTableElement>` following are properties:

| Name     | Description                              | Required | Default     |
| -------- | ---------------------------------------- | -------- | ----------- |
| `TableHeader` | Top row component. | `false`  | `undefined` |
| `TableRow` | Element defines a row of cells | `false`  | `undefined` |
| `DataSource` | Indicates array of JSON formatted row data | `false`  | `undefined` |
| `DataColumn` | Indicates array of JSON formatted column data | `false`  | `undefined` |
| `ClassName` | Override or extend existing table style.  | `false`  | `undefined` |
| `Border` | Indicates whether table with or without border. | `false`  | `undefined` |
| `Sort` | Indicates support of sort by column. | `false`  | `undefined` |
| `NoDataLabel` | Display label when no data. | `false`  | `undefined` |

#### Render Example

```
<table>  
 <thead>    
  <tr>      
   <th></th>      
   <th colspan="2"></th>    
  </tr>  
 </thead>  
  <tr>   
    <th scope="row"></th>    
   <td></td>    
   <td></td>  
  </tr>
</table>
```

#### DataColumn JSON

RowKey is the mapping between DataSource and DataColumn based on this mapping table convert all the json data in to rows and columns format. A cellFormatter formate cell as per the requirment.

```
[
   {
    name: "User Name",
    rowKey: "userName",
   },
   {
    name: "Address",
    rowKey: "address",
    cellFormatter: row => <AddressFormate row={row} />,
   }
]
```

#### DataSource JSON

```
[
    {
        userName: "Marissa Keep",
        address: "306 Campbell Hall, Michigan State University",
    },
    {
        userName: "John Smith",
        dataColumn: "21 2nd Street, New York"
   }
]
```

### TableHeader

`<TableHeader>` extends the `React.forwardRef<HTMLTableRowElement>` following are properties:

| Name     | Description                              | Required | Default     |
| -------- | ---------------------------------------- | -------- | ----------- |
| `TableCell` | Element defines a cell of data in a table. | `false`  | `undefined` |
| `Variant` | Define header style Ghost, Outline and Solid. | `false`  | `Solid` |
| `StickyHeader` | Indicates whether the table header is sticky. | `false`  | `undefined` |
| `ClassName` | Override or extend existing table style. | `false`  | `undefined` |

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

`<TableRow>` extends the `React.forwardRef<HTMLTableRowElement>` following are properties:

| Name     | Description                              | Required | Default     |
| -------- | ---------------------------------------- | -------- | ----------- |
| `TableCell` | Element defines a cell of data in a table. | `false`  | `undefined` |
| `ClassName` | Override or extend existing table style. | `false`  | `undefined` |
| `DataColumn` | Indicates array of JSON formatted column data | `false`  | `undefined` |

#### Render Example

```
<tr>   
    <th scope="row"></th>    
    <td></td>    
    <td></td>  
</tr>
```

### TableCell

`<TableCell>` extends the `React.forwardRef<HTMLTableColElement>` following are properties:

| Name     | Description                              | Required | Default     |
| -------- | ---------------------------------------- | -------- | ----------- |
| `ColSpan` | Specifies the number of columns a cell should span. | `false`  | `undefined` |
| `Header` | Indicates a cell is header, header converts in `<th>` by default `<td>` | `false`  | `<td>` |
| `ClassName` | Override or extend existing table style. | `false`  | `undefined` |
| `Alignment` | Indicates cell alignment. | `false`  | `Left` |
| `Type` | Indicates cell type whether `Text,Number,Date,Boolean,Custom` | `false`  | `Text`  |
| `CellFormatter` | Indicates React component or string. | `false`  | `undefined` |
| `Data` | Indicates array of JSON formatted data | `false`  | `undefined` |

#### Render Example

```
    <td></td>   
```

Table cell use in header element.

```
    <th></th>   
```

## Drawbacks

- Not supporting pagging.
- Not supporting column filter but developer can write won filters using cellFormatter.
- Not supporting nestead table.
- Not supporting virtual scrolling.

## Alternatives

There are many alternatives avalible in the market.

- User can create manual table using simple table html tags.
- [Ant design](https://ant.design/components/table/)
- [Shopify](https://polaris.shopify.com/components/lists-and-tables/data-table#navigation)
- [Chakra](https://chakra-ui.com/docs/data-display/table)

## Adoption strategy

A purpose of this table maintains uniformity in Norton applications. Following are application where we can replace existing third party tables.

- SW5
- Helpdesk
- Testmaker

## Unresolved questions

What are the expected events.
