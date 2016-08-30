# Clustergrammer JSON Format
The visualization JSON format required for [clustergrammer](https://github.com/MaayanLab/clustergrammer) is described here (see example JSON here [clustergrammer_example.json](clustergrammer_example.json)). An overview of the format is shown below (note that the group arrays are not shown):
```
{
  "row_nodes":[
     {
      "name": "ATF7",
      "clust": 67,
      "rank": 66,
      "rankvar": 10,
      "group": []
    }
  ],
  "col_nodes":[
    {
      "name": "Col-0",
      "clust": 4,
      "rank": 10,
      "rankvar": 120,
      "group": []
    }
  ],
  "links":[
    {
      "source": 0,
      "target": 0,
      "value": 0.023
    }
  ]
}
```
See [example workflow](https://github.com/MaayanLab/clustergrammer#example-workflow) or [make_clustergrammer.py](https://github.com/MaayanLab/clustergrammer/blob/master/make_clustergrammer.py) for examples of how to use the python module to generate a visualization json from a matrix file.

Optional 'views' of the clustergram are encoded in the 'views' value at the base level of the visualization json. These views are used to store filtered versions of the matrix - the links are shared but the views have their own row_nodes/col_nodes. The view attributes are stored in the view object (e.g. N_row_sum).
```
"views":[
  {
    "N_row_sum": "all",
    "dist": "cos",
    "nodes":{
      "row_nodes": [],
      "col_nodes": []
    }
  }
```
Views are discussed in more detail the main repo [here](https://github.com/MaayanLab/clustergrammer#clustergrammer-python-module).

There are three required properties: ```row_nodes```, ```col_nodes```, and ```links```. Each of these properties is an array of objects and these objects are discussed below.

#### ```row_nodes``` and ```col_nodes``` properties

#### required properties: ```name```, ```clust```, ```rank```
row_node and col_node objects are required to have the three properties: ```name```, ```clust```, ```rank``` . ```name``` specifies the name given to the row or column. ```clust``` and ```rank``` give the ordering of the row or column in the clustergram.

#### optional properties: ```group```, ```value```
row_nodes and col_nodes have optional properties: ```group``` and ```value```. Group is an array that contains group-membership of the row/column at different dendrogram distance cutoffs. If ```row_nodes``` and ```col_nodes``` have the property ```group``` then a dendrogram will be added the clustergram.

If row_nodes or col_nodes have the property ```value```, then semi-transpaent bars will be made behind the labels that represent this value.

#### ```links``` properties

#### required properties: ```source```, ```target```, ```value```
Link objects are required to have three properties: ```source```, ```target```, ```value```. ```source``` and ```target``` give the integer value of the row and column of the tile in the visualization. ```value``` specifies the opacity and color of the tile, where positive/negative values result in red/blue tiles (tiles are not made for links with zero value).

#### optional properties: ```value_up```, ```value_dn```
Links also have the optional properties ```value_up``` and ```value_dn``` which allow the user to split a tile into up- and down-triangles if a link has both up- and down-values. If a link has only an up- or down-value then a normal square tile is shown.

