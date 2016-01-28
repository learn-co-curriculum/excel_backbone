# Backbone Spreadsheets

![spreadsheet](https://web-dev-readme-photos.s3.amazonaws.com/js/spreadsheet.png)

## Objective
We're going to build a clone of Microsoft Excel / Google Spreadsheets.
You can see an example of what we're aiming to build [here](http://learn-co-curriculum.github.io/excel_backbone/).


## Functionality
#### Enter Data
Click on a cell and enter numbers. Then hit enter or tab to store the data. (The data never gets stored unless you use one of these two keys.)

#### Edit formulas
Double click on a cell to edit its formula. Formulas must be of the form `=A1 + B1` (whitespace ignored), but you can't use parenthesis. You can use as many operators as you want, ie. `=A1 + B1 + C1 * B2`, and you can use formulas that specify cells that themselves have formulas (recursion!).

#### Recalculating values
If you click on a cell and hit enter, it makes that cell recalculate. It will not recalculate all dependencies of that cell automatically, though. Once you've played around with the live version it's time to start building your own.

## About
If you start thinking in Backbone terms, there are a bunch of different ways we could do this. 

#### Using collections? Maybe not.
The first thing that came to my mind was to represent the board as a collection. Each cell would be an individual model and a board would hold all the cells. However, this didn't really make that much sense as the best usage of a collection is to automatically emit add/remove events when its models are added or removed. In this case, the number of cells on the board is fixed and they'll never change, so I'm not sure how much good a collection is going to do for us. 

#### How about a POJO!
We are going to need some object that manages the cells, and we can use a POJO (Plain Old JavaScript Object) for this that keeps an internal data structure that represents the cells. I'd love it to have a function that can take coordinates like "C4" and retrieve the actual model object for me. We could either represent the board as a hash, aka an object in JavaScript terms, where the keys are the cell locations, like "A1", and the values are the model objects, or we can use an array of arrays. I'm partial to an array of arrays because you have positional data encoded in the indices of the arrays. That takes care of the data layer, but what about the user interface?

#### Views
I like to have an object that can draw the whole board -- some kind of `boardView` object -- and then individual views which represent each piece of the larger whole. So a `boardView` and a cellView.  The `boardView` essentially iterates through the cells and asks each cell to render itself and just assembles all the cells on the board.

#### Doing the Math
The last major piece we're going to need is something that evaluates the formulas. Let's write a Class (Function) that knows how to parse formulas in the form `=A1 + B2` and puts them into arrays of Cell coordinates and operators. Then we can just leverage JavaScript's `eval` function to ask it to add or multiply things for us.

#### Storing the Formula and Its Output
Another tricky piece is how to store both the formula and the evaluated formula's output. We could store a `data` attribute on the cell model, which is just whatever the user has input. We could also store a `viewData` attribute, which would be what we want to render to the user. If the data the user enters begins with an equals sign, we know we're going to need to evaluate it and store that in `viewData`, otherwise `viewData == data`.

## Instructions
Build out your own Backbone Spreadsheets by following along with the tests in the `spec` folder. The CSS has already been included for you.

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/excel_backbone' title='Backbone Spreadsheets'>Backbone Spreadsheets</a> on Learn.co and start learning to code for free.</p>
