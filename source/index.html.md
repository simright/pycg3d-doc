---
title: pycg3d documentation

toc_footers:
  - <a href='https://github.com/simright/pycg3d'>pycg3d website</a>

includes:
  - changelog

search: true
---

# Introduction

**pycg3d** - a pure **Py**thon package for **C**omputational **G**eometry in **3D**. 

- **Requirements**:
  - CPython 2.7+, CPython 3.3+ or pypy 4.0+

- **Highlights**:
    - Pure Python
    - Minimal dependency
    - Friendly operators for building expressions(e.g. '+', '-', '*' and '^' operators for vectors)
    - No graphics

- **Features**:
    - Classes for modeling geometrical/mathematical objects like [*Point*](#Point), [*Vector*](#Vector), [*Line*](#Line), [*Plane*](#Plane) in 3D space;
    - Built-in functions and operators: e.g. *addition*, *subtraction*, *dot product*, *cross product* for [*Vector*](#Vector);
    - Classes for modeling transformations like [translation](#Translation), [rotation](#Rotation), [reflection](#Reflection) etc, which could be chained and applied to [*Point*](#Point)
    - [Utility functions](#utility-functions")
    
- **Installation**:

    `$ git clone https://github.com/simright/pycg3d.git`  
    `$ cd pycg3d`  
    `$ python setup.py install`
  
     or

    `$ pip install pycg3d`

# <a name="user-guide"></a>User Guide

**pycg3d** provides classes for modeling [*Point*](#Point), [*Vector*](#Vector), [*Line*](#Line), [*Plane*](#Plane) and [*Transformation*](#Transformation) in 3D space, and all functions are provided via the instances of these classes.


## <a name="Vector"></a>Vector
> Create a vector and access its components:

```python
    from pycg3d.cg3d_vector import CG3dVector
    
    v1 = CG3dVector(1.0, 0.0, 0.0)
    print v1[0], v1[1], v1[2]  # 1.0 0.0 0.0
    v1[1]=2.0
    print v1  # [1.0, 2.0, 0.0]
```

> Use operators '+'/'-'  for component-wise addition/subtraction of vectors:

```python
    from pycg3d.cg3d_vector import CG3dVector
    
    v1 = CG3dVector(1.0, 2.0, 3.0)
    v2 = CG3dVector(1.0, 0.0, 2.0)
    v3 = v1 + v2  # v3 == CG3dVector(2.0, 2.0, 5.0)
    v4 = v1 - v2  # v4 == CG3dVector(0.0, 2.0, 1.0)
```

> Use operator '*' for dot product of vectors:

```python
    from pycg3d.cg3d_vector import CG3dVector
    
    va = CG3dVector(1.0, 0.0, 0.0)
    vb = CG3dVector(2.0, 2.0, 0.0)
    dp = va*vb  # 2.0
```

> Use operator '^' for cross product of vectors

```python
    from pycg3d.cg3d_vector import CG3dVector
    
    vx = CG3dVector(1.0, 0.0, 0.0)
    vy = CG3dVector(0.0, 1.0, 0.0)
    vz = vx^vy  # vz == CG3dVector(0.0, 0.0, 1.0)
```

Class [**CG3dVector**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_vector.py) defines a vector in 3D space.

## <a name="Point"></a>Point
> Class [**CG3dPoint**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_point.py) is actually an alias of class [**CG3dVector**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_vector.py).

```python
    from pycg3d.cg3d_vector import CG3dVector
    from pycg3d.cg3d_point import CG3dPoint
    
    p1 = CG3dPoint(1.0, 0.0, 0.0)
    print isinstance(p1, CG3dVector) # True
```

Class [**CG3dPoint**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_point.py) defines a point in 3D space, and it is actually an alias of class [**CG3dVector**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_vector.py).

## <a name="Line"></a>Line
> Class [**CG3dLine2P**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_line.py) defines a line with two points:

```python
    from pycg3d.cg3d_point import CG3dPoint
    from pycg3d.cg3d_line import CG3dLine2P
    
    p1 = CG3dPoint(1.0, 0.0, 0.0)
    p2 = CG3dPoint(0.0, 1.0, 0.0)
    line = CG3dLine2P(p1, p2)
```

- Class [**CG3dLine**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_line.py) is the super/base class for any class that defines a line in 3D space. 
- **pycg3d** provides various subclasses for defining [*Line*](#Line) in different ways, e.g. 
  - Class [**CG3dLine2P**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_line.py) : define a Line by two points
- The instances of these subclasses could be used equally wherever requires a [*Line*](#Line) as input.

## <a name="Plane"></a>Plane
> Define a plane by a point and normal vector:

```python
   from pycg3d.cg3d_point import CG3dPoint
   from pycg3d.cg3d_point import CG3dVector
   from pycg3d.cg3d_plane import CG3dPlanePN
   
   yz_plane = CG3dPlanePN(CG3dPoint(0.0, 0.0, 0.0), CG3dVector(1.0, 0.0, 0.0))
```

> Define a plane by three points:

```python
   from pycg3d.cg3d_point import CG3dPoint
   from pycg3d.cg3d_plane import CG3dPlane3P
   
   xy_plane = CG3dPlane3P(
       CG3dPoint(0.0, 0.0, 0.0), 
       CG3dPoint(1.0, 0.0, 0.0), 
       CG3dPoint(0.0, 1.0, 0.0)
   )
```

- Class [**CG3dPlane**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_plane.py) is the super/base class for any class that defines a *Plane* in 3D space.
- **pycg3d** provides various subclasses for defining a *Plane* in different ways, e.g. 
  - Class [**CG3dPlane3P**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_plane.py): define a plane with 3 points in the plane
  - Class [**CG3dPlanePN**](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_plane.py): defines a plane with a point in the plane and a normal vector
- The instances of these subclasses could be used equally wherever requires a *Plane* as input.
  
## <a name="Transformation"></a>Transformation

- *Transformation* (translation, rotation, reflection etc) can be applied to a [*Point*](#Point) in 3D space, and returns a new [*Point*](#Point);
- Class [**CG3dTransformer**](https://github.com/simright/pycg3d/blob/master/pycg3d/transform/cg3d_transform.py) is the super/base class for any class that defines a *Transformation* in 3D space.
- **pycg3d** provides various subclasses for defining *Transformation* in different ways. The instances of these subclasses could be used wherever requires a *Transformation* as input.

### <a name="Translation"></a>Translation
> Translation:

```python
      from pycg3d.cg3d_point import CG3dPoint
      from pycg3d.transform.cg3d_translate import CG3dXtranslateTF, CG3dYtranslateTF
      
      p1 = CG3dPoint(0.0, 0.0, 0.0)
      tf1 = CG3dXtranslateTF(1.0)  # translate along X-axis by 1.0
      p2 = p1.transform(tf1)  #  p2 == CG3dPoint(1.0, 0.0, 0.0)
      tf2 = CG3dYtranslateTF(2.0)  # translate along Y-axis by 1.0
      p3 = p2.transform(tf2)  # p3 == CG3dPoint(1.0, 2.0, 0.0)
      p4 = p1.transform(tf1).transform(tf2)  # chained transformations, p4 == CG3dPoint(1.0, 2.0, 0.0)
```
- Class [**CG3dTranslateTF**](https://github.com/simright/pycg3d/blob/master/pycg3d/transform/cg3d_translate.py) : Translation alone any direction
- Class [**CG3dX(Y/Z)translateTF**](https://github.com/simright/pycg3d/blob/master/pycg3d/transform/cg3d_translate.py) : Translation alone X/Y/Z-axis

### <a name="Rotation"></a>Rotation

- Class [**CG3dRotateTF**](https://github.com/simright/pycg3d/blob/master/pycg3d/transform/cg3d_rotate.py) : Rotation about an arbitrary axis
- Class [**CG3dX(Y/Z)rotateTF**](https://github.com/simright/pycg3d/blob/master/pycg3d/transform/cg3d_rotate.py) : Rotation about X/Y/Z axis

### <a name="Reflection"></a>Reflection
> Mirror point(-1.0, 0.0, 0.0) to Y-Z Plane:

```python
   from pycg3d.cg3d_point import CG3dPoint
   from pycg3d.cg3d_point import CG3dVector
   from pycg3d.cg3d_plane import CG3dPlanePN
   from pycg3d.transform.cg3d_reflect import CG3dPlaneMirrorTF
   
   p1 = CG3dPoint(-1.0, 0.0, 0.0)
   yz_plane = CG3dPlanePN(CG3dPoint(0.0, 0.0, 0.0), CG3dVector(1.0, 0.0, 0.0))
   mirror = CG3dPlaneMirrorTF(yz_plane)
   p2 = p1.transform(mirror) # p2 == CG3dPoint(1.0, 0.0, 0.0)
```
- Class [**CG3dPlaneMirrorTF**](https://github.com/simright/pycg3d/blob/master/pycg3d/transform/cg3d_reflect.py): Reflection by mirroring to a [*Plane*](#Plane)

##<a name="utility-functions"></a>Utility functions
> Use utility functions:

```python
from pycg3d import utils
from pycg3d.cg3d_point import CG3dPoint

utils.distance([0.0, 0.0, 0.0], [1.0, 0.0, 0.0])  # 1.0
utils.distance(CG3dPoint(0.0, 0.0, 0.0), CG3dPoint(1.0, 0.0, 0.0))  # 1.0
```

- **distance**(*p1*, *p2*):
  - compute distance between two points
  - **param** *p1*: point 1 (a list with 3 floats or [CG3dPoint](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_point.py))
  - **param** *p2*: point 2 (a list with 3 floats or [CG3dPoint](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_point.py))
  - **return**: float

- **dot_product**(*v1*, *v2*):
  - compute dot production of two vectors
  - **param** *v1*: vector 1 (a list with 3 floats or [CG3dVector](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_vector.py))
  - **param** *v2*: vector 2 (a list with 3 floats or [CG3dVector](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_vector.py))
  - **return**: float

- **cross_product**(*v1*, *v2*):
  - compute cross production of two vectors
  - **param** *v1*: vector 1 (a list with 3 floats or [CG3dVector](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_vector.py))
  - **param** *v2*: vector 2 (a list with 3 floats or [CG3dVector](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_vector.py))
  - **return**: [CG3dVector](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_vector.py)

- **mirror_point_to_plane**(*point*, *plane*):
  - compute mirrored point to a plane
  - **param** *point*: input point ([CG3dPoint](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_point.py))
  - **param** *plane*: mirror plane ([CG3dPlane](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_plane.py))
  - **return**: [CG3dPoint](https://github.com/simright/pycg3d/blob/master/pycg3d/cg3d_point.py)
  