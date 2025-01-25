# Vue Calculator

This repository serves as a foundation for building a user-friendly calculator application utilizing the Vue.js framework for efficient and reactive front-end development.

<hr><br>

## Purpose of This Repository

This repository contains the source code for a basic calculator application built with Vue.js. The project demonstrates fundamental front-end development principles and provides a solid foundation for further exploration of Vue.js capabilities.

<hr><br>

## Demo

Check out the live demo of the Vue Calculator [here](https://vue-calculator.guanshiyinnn.com/).

### Program Function Demonstration

Here is a demonstration of the basic arithmetic operations provided by the calculator:

```javascript
// filepath: /home/guan/Documents/Code/Vue-Calculator/src/components/Calculator.vue
methods: {
  add(a, b) {
    return a + b;
  },
  subtract(a, b) {
    return a - b;
  },
  multiply(a, b) {
    return a * b;
  },
  divide(a, b) {
    if (b === 0) {
      return 'Error: Division by zero';
    }
    return a / b;
  }
}
```

<hr><br>

## Features

- Basic arithmetic operations (addition, subtraction, multiplication, division)
- Responsive design
- Clear and user-friendly interface
- Real-time calculation updates

<hr><br>

## Technologies Used

- Vue.js
- HTML5
- CSS3
- JavaScript

<hr><br>

## Project Setup

1. **Install Node.js**
2. **Clone this repository**

```bash
git clone https://github.com/guanshiyin28/Vue-Calculator.git
```

3. **Direct to directory**

```bash
cd Vue-Calculator
```

4. **Install npm**

```bash
npm install
```

<hr><br>

## Steps to Run

1. **Compiles and hot-reloads for development**

```bash
npm run serve
```

2. **Compiles and minifies for production**

```bash
npm run build
```

3. **Lints and fixes files**

```bash
npm run lint
```

<hr><br>

## License

This project is licensed under the Apache-2.0 License. See the [LICENSE](LICENSE) file for details.

<hr><br>

<div align="center">
   <a href="https://www.instagram.com/guanshiyin_/">
      <img src="https://capsule-render.vercel.app/api?type=waving&height=200&color=100:393E46,20:F7F7F7&section=footer&reversal=false&textBg=false&fontAlignY=50&descAlign=48&descAlignY=59"/>
   </a>
</div>
