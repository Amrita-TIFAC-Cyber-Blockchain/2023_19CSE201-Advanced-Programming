
## Class Definition

### C++
```
class rebind {
  public:
      string classadvisor;
      int strength;
      int bStrength;
      int gStrenght;
  protected:
  private:
      float avgCGPA;
 rebind(string ca) {
   classadvisor = ca;
 }
  void calculateAvgCGPA() {
  }
}
```
### Python
```
class rebind:
  def __init__(self, strength, bStrength, gStrength, CGPA):
    self.strength = strength
    self._bStrength = bStrength
    self._gStrength = gStrength  # _ is protected
    self.__avgCGPA  = CGPA     # __ is private

  def calculateAvgCGPA(self):
      
r = rebind(72, 53, 19, 7.2)
r.calculateAvgCGPA()

```





  
