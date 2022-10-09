# Simulation

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.patches import Polygon

class Figure:
    def __init__(self):
        figure = self.ChooseFigure()
        data = self.GetSides(figure)

        if figure == 1:
            self.DrawTriangle(data)


        pass

    def DrawTriangle(self, data):
        domainForB = np.arange(0, data["B"][0] + 0.1, 0.1)
        domainForC = np.arange(0, data["C"][0] + 0.1, 0.1)
        domainForA = np.arange(data["B"][0], data["C"][0] + 0.1, 0.1)
        y1 = self.LeftTriangle(domainForB, data)
        if y1 == 0:
            plt.plot([0, 0], [0, data["B"][1]])
        else:
            plt.plot(domainForB, y1)
        y2 = self.RightTriangle(domainForA, data)

        plt.plot(domainForC, [0 for i in range(len(domainForC))])
        plt.plot(domainForA, y2)
        plt.xlim(-0.5, data["C"][0] + 0.5)
        plt.ylim(-0.5, data["B"][1] + 0.5)
        plt.show()
        


    def ChooseFigure(self):
        print("""Hello At First try to choose which figure you want to visualise
            1) Triangle
            2) Rectangle
            3) Polygon
            4) Circle
        """)
        figure = input("Please Enter The number: ")
        if (figure not in ["1","2","3"]):
            return self.ChooseFigure()
        return int(figure)

    def GetSides(self, figure):
        if figure == 1: #Triangle
            print("""Now Enter Side lengths of your triangle: """)
            A = input("First Side: ")
            B = input("Second Side: ")
            C = input("Third Side: ")
            if not (self.TriangleChecker(A, B, C)):
                return self.GetSides(figure)
            coord = self.GetCoordinateForTriangle(float(A),float(B),float(C))
            result = {
                "A": (0,0),
                "B": coord,
                "C": (float(A),0),
                "sides": [float(A),float(B),float(C)],
                "perimeter": float(A)+float(B)+float(C),
                "area": self.TriangleArea(float(A),float(B),float(C)),
            }
            return result
        elif figure == 2: #Rectangle
            pass
        else:
            pass


    def TriangleChecker(self, a, b, c):
        if not (a.isnumeric() or b.isnumeric() or c.isnumeric()): return False
        if (a =="" or b == "" or c ==""): return False
        smallest, medium, biggest = sorted([float(a),float(b),float(c)])
        return smallest + medium >= biggest and all(s > 0 for s in [float(a), float(b), float(c)])

    def GetCoordinateForTriangle(self, a, b, c):
        x = (np.power(a, 2) + np.power(b, 2) - np.power(c, 2)) / (2*a)
        y = np.sqrt(np.power(b, 2) - np.power(x, 2))
        return (x,y)

    def TriangleArea(self, a, b, c):
        s = (a+b+c)/2
        return np.sqrt(s* (s-a)*(s-b)*(s-c))

    def LeftTriangle(self, x, data):
        if (data["B"][0]!=0):
            k = data["B"][1] / data["B"][0]
        else:
            return 0
        return np.array(x)*k

    def RightTriangle(self, x, data):
        k = data["B"][1] / (data["B"][0] - data["C"][0])
        b = -data["C"][0]*k
        return np.array(x)*k + b


# polygon1 = Polygon([(0,5), (1,1), (3,0),])
#
# fig, ax = plt.subplots(1,1)
#
# ax.add_patch(polygon1)
# plt.ylim(0,6)
# plt.xlim(0,6)
# plt.show()
Figure()
```
