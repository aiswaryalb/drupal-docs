# Basics

### let vs var
  - var - global scope, update value allowed, redeclare allowed
  - let - block scope, update value allowed, redeclare not allowed

```
document.getElementById('id').style.backgroundColor = 'blue';
document.body.style.backgroundColor = 'blue';
document.getElementsByTagName('body')[0].style.backgroundColor = 'blue';
```

` Sample Code `
```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<style>
  div.a {
    width: 100px;
    height: 100px;
  }

  div.row {
    display: flex;
    gap: 20px;
  }

  div.row :nth-child(1) {
    background-color: pink;
  }

  div.row :nth-child(2) {
    background-color: violet;
  }

  div.row :nth-child(3) {
    background-color: lavender;
  }

  div.row :nth-child(4) {
    background-color: purple;
  }
</style>

<body>
  <div class="row">
    <div class="a" data-color="pink"> </div>
    <div class="a" data-color="violet"> </div>
    <div  class="a" data-color="lavender"> </div>
    <div  class="a" data-color="purple"> </div>
  </div>
  <h1>Hello, Aiswarya</h1><button id="b1">Hide</button><button id = 'b2'>Show</button>

  <script>

    let elements = document.getElementsByClassName('a');
    for (let x of elements) {
      x.addEventListener("click", () => {
        document.body.style.backgroundColor = x.dataset.color;
      })
    }

    let button1 = document.getElementById('b1');
    button1.addEventListener('click',() => {
      let text = document.getElementsByTagName('h1')[0];
      text.style.display = 'none';
    })
    let button2 = document.getElementById('b2');
    button2.addEventListener('click',() => {
      let text = document.getElementsByTagName('h1')[0];
      text.style.display = 'block';
    })
  </script>

</body>

</html>
```


### To retrieve value in data-attribute eg data-color:
  `let x = document.getElementById('id').dataset.color`

### for of loop
  ```
  let elements = document.getElementsByClassName('a');
  for (let x of elements) {
    x.addEventListener("click", () => {
      document.body.style.backgroundColor = x.dataset.color;
    })
  }
  ```

### Add event listener, to trigger on click for example
  ```
  let elements = document.getElementsByClassName('a');
  for (let x of elements) {
    x.addEventListener("click", () => {
      document.body.style.backgroundColor = x.dataset.color;
    })
  }
  ```

### Show or hide a text on clicking a button

```
  let button1 = document.getElementById('b1');
    button1.addEventListener('click',() => {
      let text = document.getElementsByTagName('h1')[0];
      text.style.display = 'none';
    })
    let button2 = document.getElementById('b2');
    button2.addEventListener('click',() => {
      let text = document.getElementsByTagName('h1')[0];
      text.style.display = 'block';
    })
```
