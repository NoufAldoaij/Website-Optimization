# Website Performance Optimization portfolio project

 Project #4 of the [Udacity Front-End Web Developer Nanodegree]

 (https://www.udacity.com/course/front-end-web-developer-nanodegree--nd001)

The goal was to optimize a provided website with a number of optimization- and
performance-related issues so that it achieves a target PageSpeed score of 90
and runs at 60 frames per second.

## Optimization Made to index.html to achieves a PageSpeed score of at least 90 for Mobile and Desktop.
1. To eliminate render-blocking JS & CSS:
  * I used a media query on the print.css.
  * added async to the JS script so it doesn't block rendering. 
  * moved the JS script to the bottom of the body so it doesn't
    block rendering.
  * Adding the CSS as internal style to the page to reomve the css rendering block.

  2. Optimize images:
  * The images were resized and compressed using (http://resizeimage.net)

## Optimization Made to main.js make to views/pizza.html render with a consistent frame-rate at 60fps when scrolling.

Here are Sample code of the original code and the  new code to achieve 60fps when scrolling.

### While scrolling the page 
The Old code 
```
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  for (var i = 0; i < items.length; i++) {
    // document.body.scrollTop is no longer supported in Chrome.
    var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
    var phase = Math.sin((scrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
```
The New Code 
```
function updatePositions() {
    frame++, window.performance.mark("mark_start_frame");
    var r = document.documentElement.scrollTop;
    for (var e = document.querySelectorAll(".mover"), a = 0; a < e.length; a++) {
            n = Math.sin(r / 1250 + a % 5);
        e[a].style.left = e[a].basicLeft + 100 * n + "px"
    }
    
    if (window.performance.mark("mark_end_frame"), window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame"), frame % 10 === 0) {
        var i = window.performance.getEntriesByName("measure_frame_duration");
        logAverageFrame(i)
    }
}
```

### Resizing 
The New Code 
```
  function changePizzaSizes(size) {
    var windowWidth = document.getElementById("randomPizzas").offsetWidth;
    var randomPizzas = document.getElementsByClassName("randomPizzaContainer");

    for (var i = 0; i < randomPizzas.length; i++) {
      randomPizzas[i].style.width = (sizeSwitcher(size) * windowWidth) + 'px';
    }
  }
```

The Old Code 
```
     var newSize = sizeSwitcher(size);
    var dx = (newSize - oldSize) * windowWidth;

    return dx;
  }

  // Iterates through pizza elements on the page and changes their widths
  function changePizzaSizes(size) {
    for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
      var dx = determineDx(document.querySelectorAll(".randomPizzaContainer")[i], size);
      var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
      document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
    }
  }
```