# Piazza FAQs

## Common questions  

**Why doesn't X method work on Y class?** 
* Be sure to check whether you're using a method that exists for the class at hand.

**Why is my function still running? / Why isn't rails grade able to complete?**
* This is likely due to a loop that isn't breaking—and thus is running indefinitely.
* Examine any loops in your code. 
* First, try articulating in plain English what the loop is meant to do. 
* Think about the points at which conditions are checked in the loop (for example, a while loop will continue to bounce back to its starting condition until the condition fails to be met. As your code is written, will the condition ever eventually fail to be met?). 

**Why is my calculation output not matching the rails grade expected output?**
* First, try printing the values of the individual variables used in the overall calculation, to figure out at what point the error is introduced. 
* Pay extra attention to any conversions you're performing on the variables (are you transforming a string into a number? are you formatting something as a percentage?).

**How do I get the value I want from a Hash?**
* When working with `Hashes`, it's important to think of the data as being layered. Visualizing the information can help you figure out the steps you need to take.
* Let's imagine I'm looking for a pair of shoes. Where are my Nike Blazers with the blue swoosh?! 
~~~~
storage_hash = {
   :closet => { 
      :organizer_bin => { 
         :name => "Shoes",
         "sneakers" => { 
            "Nike Blazers" => "blue swoosh",
            "Vans Old Skool" => "pink suede"
         }
      }
   }
}
~~~~

* Where could I have stored them? Let's use the `.keys` method to investigate.
~~~~
p storage_hash.keys
[:closet]
~~~~

* So, the only available storage option is the closet, as it's the only thing in the `Array` of keys that was returned. We can fetch its contents.
~~~~
p storage_hash.fetch(:closet)
{ 
  :organizer_bin => { 
     :name => "Shoes",
     :sneakers => { 
        "Nike Blazers" => "blue swoosh",
        "Vans Old Skool" => "pink suede"
     }
  }
}
~~~~

* This closet is extremely organized—looks like we're once again dealing with a `Hash` that contains another `Hash`! We can save this response in a new variable and then repeat what we did above, figuring out what keys exist:
~~~~
closet_contents = storage.hash.fetch(:closet)
p closet_contents.keys
[:organizer_bin]
~~~~

* Alrighty, we know where to look next:
~~~~
bin_contents = closet_contents.fetch(:organizer_bin)
p bin_contents.keys
[:name, :sneakers]
~~~~

* Finally, I think I know where my sneakers are. But are the ones I want there?
~~~~
sneaker_selection = bin_contents.fetch(:sneakers)
p sneaker_selection.keys
["Nike Blazers", "Vans Old Skool"]
~~~~

* Nice, I found some Nikes! But are they the right ones?
~~~~
nike_blazers = sneaker_selection.fetch("Nike Blazers")
p nike_blazers
"blue swoosh"
~~~~

**How do I approach the "count each letter" exercise?** (Jelani's answer to this student's question was really great, so I've copied it here)
* Assuming the word we want to count the letters for is "apple".
~~~~
word = "apple"
~~~~

* Our first step is to get an `Array` of letters. We can use the `.split` method to do that.
~~~~
word = "apple"letter_array = word.split("")
# => ["a", "p", "p", "l", "e"]
~~~~

* And if we wanted to count the number of times "a" appears in that `Array`, we can use the `.count` method
~~~~
word = "apple"
letter_array = word.split("")
p letter_array.count("a")
# => 1
~~~~

* But this only works for "a" and we want this program to work for any letter that exists in the word.
Is there a way we can figure out what the first letter in the word is, without knowing what the word is in advance?
Well, if we have the `Array` of all the letters in the word, we can write some Ruby to grab the first element from that `Array`
~~~~
word = "apple"
letter_array = word.split("")
first_letter = letter_array.at(0)
p first_letter
# => "a"
~~~~

* Now this is a little better! I can change word to be "hello" and my program will print "h" instead of "a".  I can even update this program to count how many times `first_letter` appears in `letter_array`.
~~~~
word = "apple"
letter_array = word.split("")
first_letter = letter_array.at(0)
p letter_array.count(first_letter)
# => 1
~~~~

* Now the only issue is that I don’t just want the count of the first letter, I want the count for each letter in the `Array`. This is where the loop comes in.
* The `.each` loop is particularly powerful when you have some code that you want to run on each element in an `Array`.
* The block variable will automatically have the value of an element in the `Array`. This means if I write a basic each loop over `letter_array` and print the block variable…
~~~~
word = "apple"
letter_array = word.split("")
letter_array.each do |some_letter|
  p some_letter
end
# "a"# "p"# "p"# "l"# "e"
~~~~
each letter of the word is printed one at a time.
* If you can use the block variable, you no longer need to create a variable, like `first_letter`, to figure out what letters exist in the word, because the block variable will be assigned the value of each letter one at a time. This can be used to count each of the letters in the `Array`.
