---
layout:     post
title:      Javascript notes branch
subtitle:   Mobilenet network
date:       2020-04-15
author:     Rain
header-img: img/clarkson.jpg
catalog: true
tags:    
 - javascript
---
### what is javascript promises?
https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261

### what is callbacks



### class

 - strict mode
 ?
 
 - constructor
 - 

```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  // Getter
  get area() {
    return this.calcArea();
  }
  // Method
  calcArea() {
    return this.height * this.width;
  }
}

const square = new Rectangle(10, 10);

console.log(square.area); // 100
```