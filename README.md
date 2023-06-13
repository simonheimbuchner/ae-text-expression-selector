# After Effects Text Expression Selector

## Examples
Visit the [Examples](/Examples) page for some ready-to-use code and usage examples.

## How it works

#### Disclaimer
>This repo was created by trial and error. Since there is little information on the Text Expression Selector, some information might be incomplete or flat out wrong.

### Legend

| Name  | Description | Output |
| ------------- | ------------- | ------------- |
| Based On  |Determines the Unit of the Range | Characters / Characters Excluding Spaces / Words / Lines
| Effect |The Effect on selected parts of the text | / |
| Expression  | Determines the Range and Amount of the Effect | [x,y,z]
| Amount | If not overwritten by the Expression, controls the Amount of Effect applied | [x,y,z] |

![character selector_Character Selector How 2_2023-06-13_15 32 23](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/39feb909-bb52-4501-9462-06ca40ec0a00)

Instead of the Start and End-Sliders of the Range selector, an Expression is used to select a Range in the Source Text.

#### Note
> For simplicity's sake, I will only be using the _Based On_-Unit **Characters** in this explanation. Of course, the same applies for all other units.

### Unique Variables

There are three variables unique to the Expression Selector:
| Name  | Description |
| ------------- | ------------- |
| `textTotal` | the total number of Characters, starting at 1 |
| `textIndex` | the index of each Character, starting at 1 |
| `selectorValue` | the value of a Range Selector, if exists |

### Introduction
Let's just jump straight into a crude but useful example to illustrate how this works.

<sub>Text Layer > Expression Selector > Amount</sub>

```javascript
  if(textIndex==1) {
    0
  } else if(textIndex==2) {
    25
  } else if(textIndex==3) {
    50
  } else if(textIndex==4) {
    75
  } else if(textIndex==5) {
    100
  }

```

![character selector_Character Selector How_2023-06-13_13 57 12](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/9c1b2955-a0cb-44ed-90aa-c214d8c6fdef)
We used some If Statements to check for the textIndex. For each textIndex, we've returned a different number. This is the percentage of how much _Effect_ is applied to the specified range. For `textIndex==1`, it's 0% — hence the first character is completely unaffected. `textIndex==5` is 100% affected, so the full 90° Rotation and Green Color are applied. The states inbetween are interpolated.

#### Note
> Return `-100` to have the exact opposite effect applied.

Think of it this way: This whole block of code is executed for each Character in the Source Text. That's why we can ask for the value of `textIndex` five times and get different results for each Character.

![character selector_Character Selector How 3_2023-06-13_15 04 54](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/e373bdaf-5a07-4320-b2e9-0286d6f5c9e4)



In Javascript talk, this is how you can think of what's going on under the hood:
```Javascript
for(let textIndex=1;i<=textTotal;textIndex++) {

  // Expression Selector Code
  
}
```

### Simplify
To simplify the above code, we could do the following:

```javascript
(textIndex-1)*25;
```

And to make this fully dynamic, I'll divide `100%` by `textTotal` to get to the split percentages.

```javascript
(textIndex-1)*(100/(textTotal-1));
```


#### Note
>Since both `textIndex` and `totalText` start at `1`, I'm subtracting `1` to get the first Character to be `0%`.

Now, this code works with any amount of Characters.
![character selector_Character Selector How 4_2023-06-13_14 47 19](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/5f493986-6cc6-49c1-84d6-064f2b732e2f)



### Using the Amount Slider

If no expression overrides the value, the Expression will default to _Amount_.
If you do override it, you can access it using `value`.

This, for example, is how you would reverse the _Amount_-Slider:
```javascript
[100,100,100]-value;
```
You can also use this to your advantage by animating _Amount_ and then staggering the animation using `valueAtTime()`. You'll find this code in the [Examples](/Examples).

### Dimensions
_Amount_ is a three-dimensional property: `[x,y,z]`.

Effects by default have either one or two dimensions. `Position` has two (`[x,y]`), `Opacity` has one (`[x]`), and if you enable _Enable Per-character 3D_ in the Animator, all sorts of properties gain a third dimension (`[x,y,z]`), too. 

_Amount_ and the Effects both work with these dimensions. An amount of `[100,0,0]`, for example, will only affect every first dimension of the added Effects, leaving the `z` and `y`-Axis untouched.

![Character Selector Bsp9_1_1](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/7623aaff-a7b1-41bc-977d-5b463d21576f)

This is a difficult thing to wrap your head around, especially when dealing with Properties like Colors. It can be quite useful for more complex rigs, though.


