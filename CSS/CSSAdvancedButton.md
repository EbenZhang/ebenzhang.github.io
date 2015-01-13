Using CSS properties alone, recreate this button:
![enter image description here](http://cdn.css-tricks.com/wp-content/uploads/2013/10/css-button.png)


----------


My solution
```css
a {
  border-radius: 4px;
  border-color : aliceblue;
  background-color: gray;
  padding: 5px;  
  display:inline;
  text-decoration: none;
  text-transform:uppercase;
  box-shadow:0px 0px 1px 4px red,
    0px 0px 1px 8px #000000;
  text-shadow:1px 1px #00ffff;
  font-weight: bold;
 
  background: linear-gradient(180deg, lightgray, gray);
}

a:before, a:after{
  content:'\2605';
}
```


The challenge/question was from http://css-tricks.com/interview-questions-css/
> Written with [StackEdit](https://stackedit.io/).