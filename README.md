# helper-methods-part-1-and-2

There is no target for this project.

Grading: https://grades.firstdraft.com/resources/177e71ec-b121-408a-bb32-581716ef6f50

Lesson: https://learn.firstdraft.com/lessons/160-helper-methods-part-1

Video: https://share.descript.com/view/bY2qZcJtJHl

***

### A. Starting

1. Type rake grade. All tests passed. 
2. We will be refactoring while making sure that all tests remain pass.
3. The test file is located in the `spec/features/1_basic_spec.rb` file.
3. Each function block in the file is a case test.

### B. Step-by-Step Refactoring

1. cleaning up config/routes.rb by implementing the new hash syntax: https://learn.firstdraft.com/lessons/116-optional-syntaxes-in-ruby#new-hash-syntax

- no parentheses qround arguments: When we call a method that has arguments, we put the arguments within parentheses (()) immediately next to the method name (with no space between the method name and the opening parenthesis): `pp "hello".gsub("l", "z")` can be written as `pp "hello".gsub "l", "z"`.
- Try to include the parenthesis as much as possible, however, to make your code more readable especially when you are chaining multiple methods.
- You may drop the curly braces around hash arguments ONLY if it is the last argunment. If it is the only argument, you can't drop the curly braces!

2. Refactoring the routing command:

a. For the `get` http verb for thei ndex page, you can change it to `root "class#method"`. You can only have one root http route defined and not more than one.

Most verbose
```
get ("/movies", controller: "movies", action: "create")
```

less verbose

```
get "/movies", controller: "movies", action: "create"
```

even less verbose

```
get "/movies" => "movies#create"
```

The least verbose and most precise.

```
root "movies#index"
```

The # symbol is used to indicate an Object#method, which, in this example, is MoviesController#create.

Make this change to refactor the route commands in `config/routes.rb`.

3. For the rest of the routes, you may shorten them into, e.g.,:

From:

```
get("/movies", controller: "movies", action: "create")
```

to

```
get "/movies" => "movies#create"
```

4. Here is the new routes.rb file:

```
Rails.application.routes.draw do
  #get("/", { :controller => "movies", :action => "index" })
  root "movies#index"

  # Routes for the Movie resource:

  # CREATE
  #post("/movies", { :controller => "movies", :action => "create" })
  post "/movies" => "movies#create"

  #get("/movies/new", { :controller => "movies", :action => "new" })
  get "/movies/new" => "movies#new"
  
  # READ
  #get("/movies", { :controller => "movies", :action => "index" })
  get "/movies" => "movies#index"

  #get("/movies/:id", { :controller => "movies", :action => "show" })
  get "/movies/:id" => "movies#show"

  # UPDATE
  #patch("/movies/:id", { :controller => "movies", :action => "update" })
  patch "/movies/:id" => "movies#update"

  get("/movies/:id/edit", { :controller => "movies", :action => "edit" })
  #get "/movies" => "movies#edit"

  # DELETE
  #delete("/movies/:id", { :controller => "movies", :action => "destroy" })
  delete "/movies/:id" => "movies#destroy"

  #------------------------------
end

```


### Appendix A: Ruby Styles

- You can write the following hash:

```
my_hash = { 1 => "pink", 2 => "cookies" }
```

as the following only if the keys are non-numeric. If the keys are numeric, it will throw an error.

```
my_hash = { a: "pink", b: "cookies" }
```
2. `:uniqueness => :scope => :year`, is equivalent to `scope: :year`. 

3. A one-liner:

The following 
```
numbers = [8, 3, 1, 4, 3, 9]

numbers.each do |num|
  pp num
end

```

is equivalent to

```
numbers.each do |num| pp num end
```

Simply translate a block to just one line.

4. Here is a one liner to make a dictionary:

```
numbers = [8, 3, 1, 4, 3, 9]

numbers.each { |num| pp num }
```

5. You can do method chaining with a one liner:

```
numbers.select { |num| num > 3 }.sort
```
