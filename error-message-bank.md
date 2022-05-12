# Common error messages

**`Undefined method` errors:** 
* Try printing the method in question! Is the method being called on a variable that's itself undefined or nil? Trace the steps of your RCAV from beginning.
* Is the method you're applying one that can be used on that particular class? (For example, `.each` is a method for `Arrays`; we can't apply `.each` to a `String`)

**`Unable to find field` errors:** 
* The text of your label might not *exactly* match the label in the target app.
* The label might be missing a `for=""` attribute that matches the unique `id=""` of the `<input>` element.

**`Uninitialized constant` errors:**
* Your code is referencing something that Ruby can't find; the error should tell you what that thing is. Is there a typo? Is something capitalized when it shouldn't be (remember that Ruby is case-sensitive: classes are capitalized, but methods and variables shouldn't be).

**`Template is missing` errors:**
* Look at the error details to see where it's trying to locate the template. Is that the correct path? Make sure that you didn't accidentally make a typo, or store your templates in the wrong folder. 

**`killed rails server` message in terminal:**
* Only one server can run at a time, so `bin/server` will "kill" any other server that's running in another tab. This error can be safely ignored!

**This rails grade test is failing, but my output looks the same as the expected output:**
* There's always a reason! You can try comparing your output vs the test's expected output in a diff checker.
* Sometimes, this might come down to a subtle difference in characters: as far as your computer is concerned, `'` is different from `‘`, for example!
