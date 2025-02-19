# Frontend Mentor - Expenses chart component solution

This is a solution to the [Expenses chart component challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/expenses-chart-component-e7yJBUdjwt). Frontend Mentor challenges help you improve your coding skills by building realistic projects. 

## Table of contents

- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
 


## Overview

### The challenge

Users should be able to:

- View the bar chart and hover over the individual bars to see the correct amounts for each day
- See the current day’s bar highlighted in a different colour to the other bars
- View the optimal layout for the content depending on their device’s screen size
- See hover states for all interactive elements on the page
- **Bonus**: Use the JSON data file provided to dynamically size the bars on the chart

### Screenshot


### Links

- Solution URL: [https://github.com/Heriniaina-30/Chart-expense]
- Live Site URL: [https://chart-expense.vercel.app/]

## My process

### Built with

- Semantic HTML5 markup
- CSS custom properties
- Flexbox
- CSS Grid
- VueJS 3
- Boostrap 5 
- Chart.js


### What I learned

Use this section to recap over some of your major learnings while working through this project. Writing these out and providing code samples of areas you want to highlight is a great way to reinforce your own knowledge.

To see how you can add code snippets, see below:

```js
const renderChart = (data) => {
      if (!chartCanvas.value) return;
    
      if (chartInstance) chartInstance.destroy(); // Éviter les doublons
    
      const maxAmount = Math.max(...data); // Valeur max
      
      chartInstance = new Chart(chartCanvas.value, {
        type: "bar",
        data: {
          labels: spendingData.value.map((item) => item.day),
          datasets: [{
            data: data,
            backgroundColor: data.map((amount) => amount === maxAmount ? "#76B5BC" : "#EC755D"),
            borderRadius: 3,
          }],
        },
        options: {
          responsive: true,
          plugins: { legend: { display: false } },
          scales: { y: { display: false }, x: { grid: { display: false } } },
        }
      });
    };
```

### Continued development

Use this section to outline areas that you want to continue focusing on in future projects. These could be concepts you're still not completely comfortable with or techniques you found useful that you want to refine and perfect.

I will focus on using bootstrap and VueJs


