# ZTable Jquery Plugin For Bootstrap

Use ZTable for Show data from database in the browser

![Alt text](https://s3.amazonaws.com/f.cl.ly/items/111g2O0M0s1p412L383r/Screen%20Shot%202015-09-09%20at%2012.51.09%20PM.png "Optional title")

## Usage

```html
<!--Import Table -->
<script type="text/javascript" src="/lib/ztable/ztable.min.js"></script>
<link rel="stylesheet" href="/lib/ztable/ztable.min.css">

```

```js

// Callg the plugin 
 $('#theTable').Table({
  	source   :'http://localhost:1337/products/table',
  	method   :'GET',
  	type     :'table',
  	rows     :10,
  	sortable :true,
  	checkbox :true,
  	headers  :{
      "sku"			:{name:'SKU',width:'100px', link:true, label :'danger', title:"Click to view details"},
      "description" :{name:'Descripción',width:'300px', link:true, title:"Click to view details"},
      "price"		:{name:'Precio',width:'100px',type:'money', align:'right'},
      "qty"			:{name:'Cantidad',align:'center',width:'100px', value:function(i,o){
      		return o.qty+' '+o.unity;
      }}, 	
      "category"    :{name:'Categoría',width:'100px', align:'left', label:'success'}, 
      "id"	:{name:'Eliminar',width:'100px', align:'center', sort:false, value:function(i,o) {
      		return '<span class="glyphicon glyphicon-remove text-danger ztable-cursor" data-value="'+o.sku+'" ></span>';
      }}
    }, 
    onLink: function(e){
    	console.log(e);
    },
    onCheckBox: function(check){
    	console.log('is checked',check);
    },
    onCheckBoxMain : function(checked){
    	console.log(checked)
    },
    onCompleteRequest:function(){
    	$('#theTable .glyphicon-remove').click(function(){		    		
    		if( confirm('are you sure to delete item: '+  $(this).attr('data-value') )  ) {

    		}
    	});
    	 $('#theTable  [data-toggle="tooltip"]').tooltip()
    }
  })
```

## Json Structure
This is how you have build your data from server.
```js
{
  "data": [
    {
      "sku": "24092561400398601",
      "description": "Automotivo Esmalte rojos y marrones 1/4",
      "cost": 0,
      "price": 79,
      "qty": 250,
      "unity": "ML",
      "category": "automotivas",
      "keywords": "automotivas,esmaltes",
      "createdAt": "2015-01-24 03:46:12",
      "updatedAt": "2015-01-24 03:46:12",
      "id": 305
    },
    {
      "sku": "24092561400398559",
      "description": "Alambre",
      "cost": 0,
      "price": 25,
      "qty": 1,
      "unity": "ML",
      "category": "complementos",
      "keywords": "",
      "createdAt": "2015-01-24 03:46:12",
      "updatedAt": "2015-01-24 03:46:12",
      "id": 304
    }
  ],
  "pages": {
    "current_page": 1,
    "last_page": 31,
    "rows": 10,
    "count": 303
  }
}

```
## API Description

| Parameter       | Description      |
| -------------   | --------------------- | 
| source          | Url source json from server         |
| params          | Parameters for the source url ex.  {from:2015-06-01, to: 2015-06-31}      |
| method          | Request Method for data  _POST, _GET. Default is GET    |
| allowAbort      | Allow abort previous ajax request to server. Default is false  |
| primary         | ID key in the collection. Default is the {id} column in the json object  |
| auto            | Table load at start: Boolean : true | false  |
| headers         | Define colums headers, view section Headers Configuration for details |
| showHeaders     | Table draw the headers. Default is true |
| rows            | Max rows in request |
| page            | Define Page to start table |
| mpv             | Max draw pages in the view. Default is 5 |
| navigation      | Table draw navigation section. Default is true |
| searcher        | Table draw Search Section enable search action using source, parameter {keyword}  is sending in the ajax request  |
| sortable        | Sortable columns , see section Headers Configuration for details, each column can be overrided for custom controll  |
| checkbox        | Set checkbox column   |
| searchtext        | Set placeholder text in the seach input.  Default is 'Enter keyword to find, press enter'  |

## Supported Handlers Events 

| Parameter       | Description      |
| -------------   | --------------------- | 
| onBeforeRequest          | Called before ajax request is started. Return  full table object
| onRequestFail            | Called when ajax request is faild. Return XHR object
| onCompleteRequest        | Called when request is completed and drawed table with the data. Return data object request and full table object
| onBeforeCreateRow        | Called before the data row is drawed, return row object
| onCheckBoxMain           | Called when checkallInput is checked return all primary values selected
| onCheckBox               | Called when a checkbox in row is checked
| onBeforeCreateCell       | Called before create data cell
| onLink                   | Called when link is clicked


## Headers Configuration

// Object Structure Key is the name in your database or unique identifier

| Parameter       | Description      |
| -------------   | --------------------- | 
| name                     | is the display in the column
| width                    | width of the column 
| link                     | ZTable can be display a value with link the action return entire row object. Default is false 
| label                    | ZTable can be draw value inside a boostrap label, use this to enable, Default is false ![Alt text](https://s3.amazonaws.com/f.cl.ly/items/2d2E3R2h1v0v3C0g0Y2o/Image%202015-09-09%20at%202.17.37%20PM.png "Label")
| title                    | ZTable can be draw value with boostrap tooltip capabilities you need enable onComplete request, enable this passing the text to display in the tooltip
| align                    | Column content aling
| sort                   | Disable or enable sort in this column. Default is true
| value                    | Is a function receive object row html and object data row, you need return anything content you want to display in this column. 
```js
value:function(i,o){
  return o.qty+' '+o.unity;
} 
``` 

```js
{
"sku"     :{name:'SKU',width:'100px', link:true, label :'danger', title:"Click to view details", hide:true},
"description" :{name:'Descripción',width:'300px', link:true, title:"Click to view details"},
"price"   :{name:'Precio',width:'100px',type:'money', align:'right'},
"qty"     :{name:'Cantidad',align:'center',width:'100px', value:function(i,o){
	return o.qty+' '+o.unity;
}},   
"category"    :{name:'Categoría',width:'100px', align:'left', label:'success'}, 
"id"  :{name:'Eliminar',width:'100px', align:'center', sort:false, value:function(i,o) {
	return '<span class="glyphicon glyphicon-remove text-danger ztable-cursor" data-value="'+o.sku+'" data-toggle="tooltip" data-placement="left" title="Click to delete this item"></span>';
}

```
