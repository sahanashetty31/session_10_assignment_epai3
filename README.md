# Session 10 - Sequence Types

## Assignment

1. A regular strictly convex polygon is a polygon that has the following characteristics:

    i. all interior angles are less than 180

    ii.all sides have equal length

2. For a regular strictly convex polygon with:
   1. n edges (=n vertices)
   2. R circumradius
   3. interiorAngle = (n-2)*180/n
   4. edgeLength, s = 2*R*sin(pi/n)
   5. apothem, a= R*cos(pi/n)
   6. area = 1/2*n*s*a
   7. perimeter = n*s
3. Objective 1 [pts:400]:
   1. Create a Polygon Class:
      1. where initializer takes in:
         1. number of edges/vertices
         2. circumradius
      2. that can provide these properties:
         1. edges
         2. vertices
         3. interior angle
         4. edge length
         5. apothem
         6. area
         7. perimeter
      3. that has these functionalities:
         1. a proper __repr__ function
         2. implements equality (==) based on # vertices and circumradius (__eq__)
         3. implements > based on number of vertices only (__gt__)
   
   2. Objective 2 [pts:600]:
      1. Implement a Custom Polygon sequence type:
         1. where initializer takes in:
            1. number of vertices for largest polygon in the sequence
            2. common circumradius for all polygons
         2. that can provide these properties:
            1. max efficiency polygon: returns the Polygon with the highest area: perimeter ratio
         3. that has these functionalities:
            1. functions as a sequence type (__getitem__)
            2. supports the len() function (__len__)
            3. has a proper representation (__repr__)
      2. Results:
         1. Implement these 2 classes as a separate module. Access these modules in a jupyter-notebook (or Google Colab or Deep Note)
         2. Run Objective 1 module to show that the functionalities are implemented properly
         3. Run Objective 2 module and show which polygon is efficient for n = 25
         4. You are submitting link to your GitHub repo, where we can find the 2 modules and your notebook in which you have called and tested them
         5. All your code must be publicly accessible (make sure to open all links in an incognito window before submitting)


# Solution:

 ## Objective 1

```python
import math

class Polygon:
    """ 
    A class which generates a regular strict convex polygon with desired vertex and circumradius
    """
    def __init__(self, vertices, circumradius):
        """
        A constructor to initialize the Polygon class attributes
        """
        self.vertices = vertices # Number of vertices of polygon
        self.circumradius = circumradius # Circumradius
  
    @property
    def vertices(self):
        """A function to calculate number of vertices"""
        return self.vertices


    def vertices(self, vertices):
        """A function to set the number of vertices of polygon"""
        if not isinstance(vertices, int):
            raise TypeError(f'Number of edges/vertices should be of type integer')
        if vertices < 3:
            raise ValueError(f'Number of edges/vertices should be greater than or equal to 3')

        self.vertices = vertices

    @property
    def circumradius(self):
        """A function to calculate circumradius"""
        return self.circumradius

    
    def circumradius(self, circumradius):
        """A function to set the circumradius of polygon"""
        if not isinstance(circumradius, int):
            raise TypeError(f'Circumradius should be of type integer')
        if radius < 0:
            raise ValueError(f'Circumradius should be greater than 0')

        self.circumradius = circumradius

    @property
    def num_of_edges(self):
        """A function to calculate number of edges"""
        return(self.vertices)

    @property
    def interior_angle(self):
        """A function to calculate interior angle """
        return((self.vertices - 2)*(180/self.vertices))

    @property
    def edge_len(self):
        """A function to calculate edge length """
        return(2 * self.circumradius * math.sin(math.pi / self.vertices))

    @property
    def apothem(self):
        """Get apothem value"""
        return(self.circumradius * math.cos(math.pi / self.vertices))

    @property
    def area(self):
        """A function to calculate area """
        return(0.5 * (self.vertices * self.edge_len * self.apothem))

    @property
    def perimeter(self):
        """A function to calculate perimeter """
        return(self.vertices * self.edge_len)

    def __repr__(self):
        """ A representation function """
        return (f'Polygon({self.vertices}, {self.circumradius})')

    def __eq__(self,other):
        """ Check for the equality of Polygon"""
        if isinstance(other, Polygon):
            return(self.vertices == other.vertices and self.circumradius == other.circumradius)
        else:
            raise NotImplementedError('Incorrect data type')

    def __gt__(self,other):
        """ Check for the greater than ineqaulity for Polygon"""
        if isinstance(other, Polygon):
            return(self.vertices > other.vertices)
        else:
            raise NotImplementedError('Incorrect data type')

```



## Objective 2

```python

from polygon import Polygon as poly 
from functools import lru_cache

class Polygon_sequence:
    """ 
    A class which generates a regular strict convex polygon with desired vertex and circumradius
    """
    def __init__(self, vertices, circumradius):
        """
        A constructor to initialize the Polygon class attributes
        """
        self.vertices = vertices # Number of vertices of polygon
        self.circumradius = circumradius # Circumradius

    @property
    def vertices(self):
        """A function to calculate number of vertices"""
        return self.vertices


    def vertices(self, vertices):
        """A function to set the number of vertices of polygon"""
        if not isinstance(vertices, int):
            raise TypeError(f'Number of edges/vertices should be of type integer')
        if vertices < 3:
            raise ValueError(f'Number of edges/vertices should be greater than or equal to 3')

        self.vertices = vertices

    @property
    def circumradius(self):
        """A function to calculate circumradius"""
        return self.circumradius

    
    def circumradius(self, circumradius):
        """A function to set the circumradius of polygon"""
        if not isinstance(circumradius, int):
            raise TypeError(f'Circumradius should be of type integer')
        if radius < 0:
            raise ValueError(f'Circumradius should be greater than 0')

        self.circumradius = circumradius

    @property
    def max_efficiency_polygon(self):
        """ A function to find the max efficiency polygon """
        max_ratio = -100
        for i in range(self.vertices-2):
            area_perimeter_ratio = self.__getitem__(i)[1]
            if area_perimeter_ratio > max_ratio:
                max_ratio = area_perimeter_ratio
                max_ratio_polygon = self.__getitem__(i)[0]
        return max_ratio_polygon

    def __getitem__(self, sequence):
        """A function to get next item in the sequence """
        if isinstance(sequence, int):
            sequence = sequence + 3
            if sequence - 3 < 0:
                sequence = self.vertices + sequence - 3
            if sequence  < 3 or sequence > self.vertices:
                raise IndexError
            else:
                if sequence >= 3:
                    return Polygon_sequence._sequence(sequence, self.circumradius)
        else:
            raise TypeError ('Please provide valid integer value')

 

    def __len__(self)->int:
        """
        This is a length function
        """
        return self.vertices - 2


 
    def __repr__(self):
        """ Representation function for Polygon Sequence"""
        return (f'Polygon Sequence of ({self.vertices}, {self.circumradius})')


    @staticmethod
    @lru_cache(2 ** 10)
    def _sequence(sequence,circumradius):
        if sequence < 3:
            return None
        else:
            poly1 = poly(sequence,circumradius)
            return poly1 , poly1.area/poly1.perimeter

```