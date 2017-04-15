---
layout: post
title:  "Blocks, Yield and Kittens"
date:   2017-04-15 13:02:39 -0400
---

![Kitten in a basket](http://i.imgur.com/BnLSI2w.jpg)

*Retrieved from [Pixabay](https://pixabay.com/en/kitten-cat-basket-cute-pet-feline-390447/), CC0 Public Domain*

This is my attempt to better understand Ruby blocks, and how to use `yield`. I'm sure that there are more efficient and elegant ways to approach the same problem, and I hope that in time, I'll know what they are! For now, I'm wading through beginner's Ruby.  [This](https://mixandgo.com/blog/mastering-ruby-blocks-in-less-than-5-minutes) is a good overview of how Ruby blocks work, and a more authoritative source than below.

## The Problem

I have a basket of kittens. Some have short hair, and some are fluffy. I want to find the fluffy kittens, put a ribbon on them, and add them to a special basket. The fluffy kittens have fancy names, longer than four characters. The function should `puts` which kittens are receiving a ribbon, and return the special basket with the fluffy kittens. 

Here is the array, representing the basket of kittens, which I'll pass as an argument to the method I'm creating:
```
basket_of_kittens = ["Prudence","Lucy","Sebastian","Sam","Max"]
```

I'll work through this first using `#each`, and then using `#select`.

<h2>Using each</h2>

First, I define a method, `find_fluffy_kittens` that takes an array, `basket_of_kittens`, as an argument.

```
def find_fluffy_kittens(basket_of_kittens)
  
end
```

I'll need a new box to put the fluffy kittens in, so I have defined that within the `find_fluffy_kittens` method, using the class constructor. If I put it outside the method, I receive an error, `undefined local variable or method` (to solve this, I could include two parameters in the method, the array of kittens and the special basket, passing the special_basket in as the second argument).

```
def find_fluffy_kittens(basket_of_kittens)

  special_basket = Array.new 
	
end
```

Next, I need to find the fluffy kittens. Using `#each`, I can iterate through the box_of_kittens array, and for each kitten, use an if statement to check if the kitten's name is longer than four characters (using `#length`).

```
def find_fluffy_kittens(basket_of_kittens)

  special_basket = Array.new
	
  basket_of_kittens.each do |kitten|
	
    if kitten.length > 4 # i.e. is fluffy
      
			# code to execute for fluffy kittens
			
    end 
		
  end
	
end
```

If I find a fluffy kitten, I'll use the `yield` keyword to pass it to the block which puts a ribbon on the kitten.

```
def find_fluffy_kittens(basket_of_kittens)

  special_basket = Array.new 
	
  basket_of_kittens.each do |kitten|
	
    if kitten.length > 4 
		
      yield kitten  # yielding the kitten to the block
			
    end 
		
  end
	
end
```

Here's the block that will run when we hit `yield`:

```
{|fluffy_kitten| puts "#{fluffy_kitten} gets a ribbon!"}
```

Here, `fluffy_kitten` refers to the element that we are yielding to the block; the kitten that has a name with more than four characters. This could be done within main method, but for the sake of demonstration, I'm adding the ribbon using `yield`.

To run the `find_fluffy_kittens` method, passing in the block:

`find_fluffy_kittens(basket_of_kittens){|fluffy_kitten| puts "#{fluffy_kitten} gets a ribbon!"}`

All this does is `puts` the following:
```
Prudence gets a ribbon!
Sebastian gets a ribbon!
```

Now that I have put ribbons on the fluffy kittens, I need to add them to the `special_basket`. One option is to do this within the if statement, which identifies the fluffy kittens, using the shovel operator. Then, in order to return the basket full of fluffy kittens, I have added `special_basket` just before the final `end` keyword.

```
def find_fluffy_kittens(basket_of_kittens)

  special_basket = Array.new 
	
  basket_of_kittens.each do |kitten|
	
    if kitten.length > 4 #i.e. is fluffy
		
      yield kitten
			
      special_basket << kitten  # adds fluffy kitten to the special basket
			
    end 
		
  end
	
  special_basket  # returns the basket of fluffy kittens
	
end
```

Now, when I run:
```
find_fluffy_kittens(basket_of_kittens) do |fluffy_kitten| 
    puts "#{fluffy_kitten} gets a ribbon!" 
  end
```

I see:
```
Prudence gets a ribbon!
Sebastian gets a ribbon!
=> ["Prudence", "Sebastian"]
```

Alternatively, I can return the `fluffy_kitten` within the block that we are yielding to, and then add the return value from the block to the `special_basket`:

```
def find_fluffy_kittens(basket_of_kittens)

  special_basket = Array.new 
	
  basket_of_kittens.each do |kitten|
	
    if kitten.length > 4
		
      special_basket << yield(kitten) # return value from the block added to special_basket
			
    end 
		
  end
	
  special_basket
	
end
```

As before, when I run:
```
find_fluffy_kittens(basket_of_kittens) do |fluffy_kitten| 
    puts "#{fluffy_kitten} gets a ribbon!" 
    fluffy_kitten #this returns the fluffy_kitten
  end
```

I see:
```
Prudence gets a ribbon!
Sebastian gets a ribbon!
=> ["Prudence", "Sebastian"]
```

<h2>Using select</h2>

I can shorten my code significantly by using `#select`. `#select` returns a new array with all of the elements that caused the block passed to `#select` to return a true value, so there is no need to create a separate basket for the fluffy kittens. 

Initially, I tried to use `#collect`, which returns a new array with the return values of each iteration of the block. This did not work, as for each iteration where the return value was not a fluffy kitten, it was "nil", and the function returned: `["Prudence", nil, "Sebastian", nil, nil]`

Using `#select`:

```
def find_fluffy_kittens(basket_of_kittens)

  basket_of_kittens.select do |kitten|
	
    if kitten.length > 4 # i.e. is fluffy
		
      yield kitten
			
    end
		
  end 
	
end
```

Running the function with the block:

```
find_fluffy_kittens(basket_of_kittens) do |fluffy_kitten| 
    puts "#{fluffy_kitten} gets a ribbon!" 
		fluffy_kitten
  end
```

The results:  
```
Prudence gets a ribbon!
Sebastian gets a ribbon!
=> ["Prudence", "Sebastian"]
```

Note that I needed to either return `fluffy_kitten` within the block above, or return `kitten` within the if statement of `find_fluffy_kittens`, otherwise I'd be returning `nil`, a falsey value, and would end up with an empty array.

