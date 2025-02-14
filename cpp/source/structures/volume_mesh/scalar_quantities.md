Visualize scalar (real or integer)-valued data at the elements of a volume mesh.

![volume mesh scalar values]([[url.prefix]]/media/volume_scalar.jpg)

**Example**: showing a scalar on vertices (here just one of the spatial coordinate functions)
```cpp
/* ... initialization, create mesh ... */ 

// Register the volume mesh with Polyscope
polyscope::registerTetMesh("my mesh", verts, tets);

// Add a scalar quantity
size_t nVerts = verts.rows();
std::vector<double> scalarV(nVerts);
for (size_t i = 0; i < nVerts; i++) {
  // use the x-coordinate of vertex position as a test function
  scalarV[i] = V(i,0);
}
auto scalarQ = polyscope::getVolumeMesh("my mesh")->addVertexScalarQuantity("scalar Q", scalarV);

// Set some options
scalarQ->setEnabled(true);       // initially enabled
scalarQ->setMapRange({-1., 1.}); // colormap from [-1,1]
scalarQ->setColorMap("blues");   // use a blue colormap

// Show the GUI
polyscope::show();
```


### Add scalars to elements


???+ func "`#!cpp VolumeMesh::addVertexScalarQuantity(std::string name, const T& data, DataType type = DataType::STANDARD)`"

    Add a scalar quantity defined at the vertices of the mesh.

    - `data` is the array of scalars at vertices. The type should be [adaptable]([[url.prefix]]/data_adaptors) to a `float` scalar array; this includes may common types like `std::vector<float>` and `Eigen::VectorXd`. The length should be the number of vertices in the mesh.


??? func "`#!cpp VolumeMesh::addCellScalarQuantity(std::string name, const T& data, DataType type = DataType::STANDARD)`"

    Add a scalar quantity defined at the cells of the mesh.

    - `data` is the array of scalars, with one value per cell. The type should be [adaptable]([[url.prefix]]/data_adaptors) to a `float` scalar array; this includes may common types like `std::vector<float>` and `Eigen::VectorXd`. The length should be the number of cell in the mesh.



### Options

**Parameter** | **Meaning** | **Getter** | **Setter** | **Persistent?**
--- | --- | --- | --- | ---
enabled | is the quantity enabled? | `#!cpp bool isEnabled()` | `#!cpp setEnabled(bool newVal)` | [yes]([[url.prefix]]/basics/parameters/#persistent-values)
color map | the [color map]([[url.prefix]]/features/color_maps) to use | `#!cpp std::string getColorMap()` | `#!cpp setColorMap(std::string newMap)` | [yes]([[url.prefix]]/basics/parameters/#persistent-values)
map range | the lower and upper limits used when mapping the data in to the color map| `#!cpp std::pair<double,double> getMapRange()` | `#!cpp setMapRange(std::pair<double,double>)` and `#!cpp resetMapRange()`| no

_(all setters return `this` to support chaining. setEnabled() returns generic quantity, so chain it last)_

