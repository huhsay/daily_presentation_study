# 객체지향 프로그래밍

## 객체지향 5대 원칙 (SOLID)

### LISKOV Substitution Principle

> 서브타입은 기반타입에 대해 대체 가능해야 한다.

```java
Class rectangle {
  private int x, y, width, height;
  
  public Rectangle(int x, int y, int width, int height) {
    this.x = x;
    this,y = y;
    this.width = width;
    this.height = height;
  }
  
  ~
    
}

Class Square extends Rectangle {
  public Square(int x, int y, int size) {
    super(x, y, size, size);
  }
  
  @Overide
  public void setWidht(int width) {
    super.setWidth(width);
    super.swetHeight(width);
  }
  
  @Overide
  public void setHeight(int height) {
    super.setWidth(height);
    super.swetHeight(height);
  }
}

public void resize(Rectangle rectangle, int width, int height) {
  rectangle.setWidth(width);
  rectangle.setHeight(height);
  assert rectangle.getWidth() == width && rectangle.getHeight() == height;
}
```

출처 : 오브젝트 코드로 이해하는 객체지향 설계 / 조영호 / It leaders

