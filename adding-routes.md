# Adding Routes

We're now able to set up database tables, write quite complicated queries, and encapsulate them within handy shortcut methods. Great! However, if we (the developers) the only ones that can _use_ these queries (through the `rails console`), then they aren't much use. It's time to start adding an **interface** on top of our database so that external users can interact with and benefit from our carefully designed domain models.

## Specs

For an application that transmits information across the internet (a.k.a. a web app), the **interface** consists of a set of URLs that a user can visit. Each URL will either

 - display a page with some information
 - trigger the storing of some information
 - trigger the updating of some information
 - trigger the deleting of some information
 - forward to another URL
 - or some combination of the above

Most obviously, the user might be visiting the URLs in their browser by typing into the address bar or clicking on links. Or, more and more commonly, users might be visiting from native iPhone or Android apps without even knowing that they are visiting URLs behind the scenes.

But make no mistake: if there is information being stored in a central database, then there's a web server running somewhere and URLs are being visited with each **action** a user takes.

You can fully _specify_ a web application by listing out the URLs that users can visit, and what happens when each URL is visited. For example, let's say we wanted to build an interactive game of Rock, Paper, Scissors. The complete specifications (or **specs**, for short) for this app might look like this:

 - `http://[OUR APP DOMAIN]/rock` — Should display "You played rock!", a random move by the computer, and the outcome
 - `http://[OUR APP DOMAIN]/paper` — Should display "You played paper!", a random move by the computer, and the outcome
 - `http://[OUR APP DOMAIN]/scissors` — Should display "You played scissors!", a random move by the computer, and the outcome 
 - `http://[OUR APP DOMAIN]/` — A welcome page that displays
    - "Happy Monday!" (or whatever day it is)
    - The rules of the game.
    - For example,
        
        ```
        Happy Tuesday!
        Rock beats Scissors, Paper beats Rock, Scissors beats Paper.
        Point your browser at /rock, /paper, or /scissors to play the game.
        ```

Now — how do we get our web server to perform the above tasks when users visit the above URLs? Right now, if we try typing any of these URLs into our browsers, we'll see a "No route matches error." Let's fix that.

## Routing

The key is: we need to connect _a visit to a URL_ by a user to _a Ruby method_ that will perform the desired work and send back a response. We do this by defining **routes** in a very special file, `config/routes.rb`, which is included in every Rails app.

Here's an example `routes.rb`:

```ruby
# /config/routes.rb

Rails.application.routes.draw do
  match("/rock", { :controller => "application", :action => "play_rock", :via => "get" })
  match("/paper", { :controller => "application", :action => "play_paper", :via => "get" })
  match("/scissors", { :controller => "application", :action => "play_scissors", :via => "get" })
  match("/", { :controller => "application", :action => "homepage", :via => "get" })
end
```

### Route

 - All of our routes must be contained within the block following `Rails.application.routes.draw`. A new Rails app will already come with this code pre-written in `routes.rb`.
 - Each route is comprised of the `match` method and its two arguments:
    - The first argument to `match` is a `String`: the _path_ that we want users to be able to visit (the path is the portion of the URL that comes after the domain name).
    - The second argument to `match` is a `Hash`: this is where we tell Rails which method to call when a user visits the path in the first argument. (We'll have to actually write this method in the next step, after we write the route.)

        The `Hash` must have three key/value pairs:

        - `:controller`: The value for this key is what we're going to name the _class_ that contains the method we want Rails to call when the user visits the path.
        - `:action`: The value for this key is the what we're going to name the method itself. "Action" is the term used to refer to Ruby methods that are triggered by users visiting URLs.
        - `:via`: The value for this key must be either `"post"`, `"get"`, `"patch"`, or `"delete"`.

            In HTTP (the protocol for exchanging resources over the internet), these are the names for CREATE, READ, UPDATE, and DELETE, respectively; and are known as "HTTP verbs".

            We use the `:via` key to specify which CRUD operation best describes the work we're going to perform in the method. It turns out that _most_ requests involve _reading_ info, rather than creating/updating/deleting it; so we'll use `"get"` most often. When in doubt, just default to `"get"`.

### Action

If we now type in `http://[OUR APP DOMAIN]/rock` into the browser, the error we'll see is:

```ruby
The action 'play_rock' could not be found for ApplicationController
```

This is good! That means we defined the route correctly. If you still see a "No route matches" error, then double-check that and get that error to go away before you proceed further.

Now we have to define a method called `play_rock` within a class called `ApplicationController`, since that's what we specified as the value for the `:action` key back in our route. You can find the class in the `app/controllers/` folder, and add the method within:

```ruby
class ApplicationController < ActionController::Base
  def play_rock
  end
end
```

At the end of the day, the job of an _action_ is to send back a response to the user. A response can be either:

 - **Rendering** some data, in any one of many formats:
  - Plain text.
  - JSON for a native app to consume — we've seen JSON before (in APIs), and consumed it ourselves with our Ruby scripts.
  - HTML for their browser to draw — we'll learn this soon.
  - Less commonly, any other format: CSV, PDF, XML, etc.
 - Or, the action can forward the user to _another_ URL. This is known as **redirecting**.
 
We get a method for each of these two: `render` for the first, and `redirect_to` for the second.

#### redirect_to

Let's try `redirect_to` first:

```ruby
class ApplicationController < ActionController::Base
  def play_rock
    redirect_to("https://www.google.com")
  end
end
```

As you can see, the argument to `redirect_to` is a `String` which contains some URL that you want the user to simply be forwarded to. This will come in handy later when, for example, we want to send the user directly back to a list of all photos after they've deleted a photo.

#### render

More commonly, however, we'll use `render` to send some data to the user's browser for display:

```ruby
class ApplicationController < ActionController::Base
  def play_rock
    render({ :plain => "Hello, world!" })
  end
end
```

The argument to `render` is a `Hash` that specifies how we want to send the data back. We have a lot of options to choose from, corresponding to how many different formats there are in use for transmitting data. `:plain` is just going to send back plain text, which is enough for us for now.

Try visiting `/rock` in your browser and you should now see a response rather than an error message. Congratulations! You've wired up your very first route; prepare to do it a million more times, because all developers do is pick a **spec** from the Next Up list, wire up the route for the URL so that a user can visit it, and then implement the logic to send back the correct information.

Let's go ahead and implement the logic for `/rock`. You can write as much Ruby as you need to in the method prior to the `render`:

```ruby
class ApplicationController < ActionController::Base
  def play_rock
    moves = ["rock", "paper", "scissors"]

    computer_move = moves.sample
    
    if computer_move == "rock"
      outcome = "tied"
    elsif computer_move == "paper"
      outcome = "lost"
    elsif computer_move == "scissors"
      outcome = "won"
    end

    full_message = "You played rock. They played " + computer_move + ". You " + outcome + "!"

    render({ :plain =>  full_message })
  end
end
```

Now, every time you visit `/rock` (or refresh the page), you should see a dynamically generated page. Yay! 🎉

Get some practice at wiring up routes by implementing `/paper` and `/scissors`, similarly.

Then, implement the homepage by defining the route for just plain `/`. Once you've implemented all four routes in the **specs** above, then your job is done!

### Addendum: Custom Controller Files

We don't have to put all of our actions within the default `ApplicationController` file that comes included with any Rails app; we can add our own controllers, if we want to organize things a bit more. Suppose changed our route for `/rock` to this:

```ruby
match("/rock", { :controller => "game", :action => "play_rock", :via => "get" })
```

Now when a user visits `/rock`, they will see an error `uninitialized constant GameController`.

As we know, when Ruby says "uninitialized constant" it means "I can't find that class".

So, what's going on here? When we said `:controller => "game"` in the route, we _told_ Rails to look for a class called `GameController` when someone visits `/rock`.

 - All of the controller class names will end in `...Controller`, and they will begin with whatever value we provided for the key `:controller` in the route.
 - Like all Ruby classes, the name must be `CamelCase` (not `snake_case` or `Some_Hybrid`). So in this case, it will be `GameController`.
 - The class must be defined in a Ruby file that is the `snake_cased` version of its name. Rails will itself use the `.underscore` method to figure out the name; we can try it ourselves in `rails console`:

    ```ruby
    [2] pry(main)> "GameController".underscore
    => "game_controller"
    ```
 - The Ruby file must be placed within the `app/controllers/` folder. So, in this case, we create a file called `app/controllers/game_controller.rb` (don't forget the `.rb` file extension).
 - Finally, within this file, we define the class:

    ```ruby
    class GameController < ApplicationController
    end
    ```
 - We inherit from a Rails base class called `ApplicationController`, which in turn inherits from `ActionController::Base`; much like our models inherited from `ActiveRecord::Base` via `ApplicationRecord`.
 
    Our models inherited `.save`, `.where`, and a bunch of other awesome database-related methods from `ActiveRecord::Base`; whereas our controllers are going to inherit a bunch of methods like `render`, `redirect_to`, and a bunch of other awesome interface-related methods from `ActionController::Base`.
 - Don't forget the `end` that goes with the `class`; type it before you forget it.
 - Now, when a user visits the path `/rock`, the "uninitialized constant" error should go away. Progress!

    If you still see the "unitialized constant" error, then:

    - You named your class wrong; it must exactly match the value in `routes.rb`, followed by `Controller` (singular), and `CamelCase`.
    - You named the file wrong. Try doing `.underscore` on a string containing the class name in `rails console` to figure out the correct filename.
    - You put the file in the wrong folder. It has to be within `app/controllers/`. Not within, for example, `app/` or `app/controllers/concerns/`.
    - You forgot the `.rb` file extension.
    - If you can't find which of the above it is, try deleting what you did and paving over your work again from scratch. Sometimes you just can't spot your own typos, and paving over is the best approach.