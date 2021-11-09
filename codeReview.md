### Selecting the elements
```js
    const squares = document.querySelectorAll('.grid div')
    const scoreDisplay = document.querySelector('span')
    const startBtn = document.querySelector('.start')
```
Select the appropriate elements and contain them in variables for later use
### Create and set default variables and values for game 
```js
    const width = 10 // grid is 10x10
    let currentIndex = 0 // so first div in our grid
    let appleIndex = 0 //so first div in our grid
    let currentSnake = [2,1,0] // so the div is our grid beign 2 (or the HEAD), and 0 being the end (TAIL, with all 1's being the body from now on)
    let direction = 1
    let score = 0
    let speed = 0.9
    let intervalTime = 0
    let interval = 0
```
Create appropriate variable with default values for later use.
 - In this example the `width` value is set to 10 for sizing.  
 - Another example `currentIndex` value is set a 0, the value will change later on.
 - `currentSnake` value is set to `[2, 1, 0]` which will display the starting location of the snake on the grid
 - 

### To start, and restart the game
```js
    function startGame() {
    // clear/reset everything
    currentSnake.forEach(index => squares[index].classList.remove('snake'))
    squares[appleIndex].classList.remove('apple') // removes apple
    clearInterval(interval) // clears any intervals running  
    score = 0; // resets score to 0

    //starts game 
    randomApple() // call function to generate random apple 
    direction = 1
    scoreDisplay.innerText = score
    intervalTime = 1000
    currentSnake = [2, 1, 0]
    currentIndex = 0;
    // add snake class to the index number to draw the snake 
    currentSnake.forEach(index => squares[index].classList.add('snake'))
    // `setInterval()`
    interval = setInterval(moveOutcomes, intervalTime)
}
```
Create a function to start and restart the game. Update default values to start new game 
- To clear the board and reset we call `forEach` on `currentSnake` to run through all items in the `squares` variable and removes the class snake 🤔 This means that loop through the div to check for the snake and remove it.
- `randomApple()` is a function that will be made in the future
- `direction` value set to 1 so that the snake move one block per interval 
- 


### Function that deals with ALL the other outcomes of the Snake
```js
function moveOutcomes() {
    // deals with snake hitting border and snake hitting self
    if(
        //currentSnake = [2, 1, 0] + 10 >= 100 && 1 === 10
        (currentSnake[0] + width >= (width * width) && direction === width) || // if snake hits bottom
        (currentSnake[0] % width === width - 1 && direction === 1 ) || //if snake hits right wall
        (currentSnake[0] % width === 0 && direction === -1) || //if snake hits left wall
        (currentSnake[0] - width < 0 && direction === -width) || //if snake hits the top
        squares[currentSnake[0] + direction].classList.contains('snake') //if snake goes into  itself 
    ) {
        return clearInterval(interval) //this will clear the interval if any of the above happen
    }
    
    const tail = currentSnake.pop() //removes last ite of the array and shows its
    squares[tail].classList.remove('snake') // removes class of snake from the TAIL
    currentSnake.unshift(currentSnake[0] + direction) // gives direction to the head of the array 
    
    // deals with snake getting apple
    if(squares[currentSnake[0]].classList.contains('apple')) {
       squares[currentSnake[0]].classList.remove('apple')
       squares[tail].classList.add('snake')
       currentSnake.push(tail)
       randomApple()
       score++
       scoreDisplay.textContent = score
       clearInterval(interval)
       intervalTime = intervalTime * speed
       interval = setInterval(moveOutcomes, intervalTime) 
    }
    squares[currentSnake[0]].classList.add('snake')
}

```
### Deals with hitting the border and hitting itself 
 ```js
        // deals with snake hitting border and snake hitting self
    if(
        // locate the head of the snake `currentSnake[0] and use width to locate if the head reaches the end of edges of the grid`
        (currentSnake[0] + width >= (width * width) && direction === width) || // if snake hits bottom
        (currentSnake[0] % width === width - 1 && direction === 1 ) || //if snake hits right wall
        (currentSnake[0] % width === 0 && direction === -1) || //if snake hits left wall
        (currentSnake[0] - width < 0 && direction === -width) || //if snake hits the top
        squares[currentSnake[0] + direction].classList.contains('snake') //if snake goes into  itself 
    ) {
        return clearInterval(interval) //this will clear the interval if any of the above happen
    }
 ```