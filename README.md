# Clustergrammer JSON Format
The visualization

Your visualization JSON (referred to as network_data) must be in the following format (group arrays are not shown):

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
See [example workflow](#example-workflow) or [make_clustergrammer.py](make_clustergrammer.py) for examples of how to use the python module to generate a visualization json from a matrix file.

Optional 'views' of the clustergram are encoded in the 'views' value at the base level of the visualization json. These views are used to store filtered versions of the matrix - the links are shared but the views have their own row_nodes/col_nodes. The view attributes are stored in the view object (e.g. N_row_sum). Views are discussed in the python module [section](#clustergrammer-python-module).

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


# Optional Arguments

These arguments can also be passsed to Clustergrammer as part of the args object.

```row_label``` and ```col_label```: Pass strings that will be used as 'super-labels' for the rows and columns.

```row_label_scale``` and ```col_label_scale```: A number that will be used as a scaling factor that increases or decreases the size of row and column labels (as well as the font-size of the text).

```super_label_scale```: A number that will be used a a scaling factor that increases or decreases the size of the 'super-labels'.

```ini_expand```: Initialize the visualization in 'expanded' mode so that the sidebar controls are not visible.

```opacity_scale```: This defines the function that will map values in your matrix to opacities of cells in the visualization. The default is 'linear', but 'log' is also available.

```input_domain```: This defines the maximum (absolute) value from your input matrix that corresponds to an opacity of 1. The default is defined based on the maximum absolute value of any cell in your matrix. Lowering this value will increase the opacity of the overall visualization and effectively cutoff the visualization opacity at the value you choose.

```do_zoom```: This determines whether zooming will be available in the visualization. The default is set to true.

```tile_colors```: This determines the colors that indicate positive and negative values, respectively, in the visualization. The default are red and blue. The input for this is an array of hexcode or color names, e.g. ['#ED9124','#1C86EE'].

```row_order``` and ```col_order```: This sets the initial ordering of rows and columns. The default is clust. The options are
  * alpha: ordering based on names of rows or columns
  * clust: ordering based on clustering (covered [here](clustergrammer-python-module))
  * rank: ordering based on the sum of the values in row/column
  * rank_var: ordering based on the variance of the values in the row/column

```ini_view```: This defines the initial view of the clustergram. A clutergram can have many views available (discussed [here](#making-additional-views)) and these views generally consist of filtered versions of the clustergram.

```sidebar_width```: The width, in pixels, of the sidebar. The default is 150px.

```sidebar_icons```: This determines whether the sidebar will have icons for help, share, and screenshot. The default is true.

```max_allow_fs```: This sets the maximum allowed font-size. The default is set to 16px.

Clustergrammer was developed by Nick Fernandez at Icahn School of Medicine at Mount Sinai.