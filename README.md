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

1. You can write the following hash:

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

### B. Check Routes

1. To check the routes, type:

```
helper-methods-part-1-and-2 main % rails routes
                                  Prefix Verb   URI Pattern                                                                                       Controller#Action
                                    root GET    /                                                                                                 movies#index
                                  movies POST   /movies(.:format)                                                                                 movies#create
                              movies_new GET    /movies/new(.:format)                                                                             movies#new
                                         GET    /movies(.:format)                                                                                 movies#index
                                         GET    /movies/:id(.:format)                                                                             movies#show
                                         PATCH  /movies/:id(.:format)                                                                             movies#update
                                         GET    /movies/:id/edit(.:format)                                                                        movies#edit
                                         DELETE /movies/:id(.:format)                                                                             movies#destroy
                                rails_db        /rails/db                                                                                         RailsDb::Engine
        turbo_recede_historical_location GET    /recede_historical_location(.:format)                                                             turbo/native/navigation#recede
        turbo_resume_historical_location GET    /resume_historical_location(.:format)                                                             turbo/native/navigation#resume
       turbo_refresh_historical_location GET    /refresh_historical_location(.:format)                                                            turbo/native/navigation#refresh
           rails_postmark_inbound_emails POST   /rails/action_mailbox/postmark/inbound_emails(.:format)                                           action_mailbox/ingresses/postmark/inbound_emails#create
              rails_relay_inbound_emails POST   /rails/action_mailbox/relay/inbound_emails(.:format)                                              action_mailbox/ingresses/relay/inbound_emails#create
           rails_sendgrid_inbound_emails POST   /rails/action_mailbox/sendgrid/inbound_emails(.:format)                                           action_mailbox/ingresses/sendgrid/inbound_emails#create
     rails_mandrill_inbound_health_check GET    /rails/action_mailbox/mandrill/inbound_emails(.:format)                                           action_mailbox/ingresses/mandrill/inbound_emails#health_check
           rails_mandrill_inbound_emails POST   /rails/action_mailbox/mandrill/inbound_emails(.:format)                                           action_mailbox/ingresses/mandrill/inbound_emails#create
            rails_mailgun_inbound_emails POST   /rails/action_mailbox/mailgun/inbound_emails/mime(.:format)                                       action_mailbox/ingresses/mailgun/inbound_emails#create
          rails_conductor_inbound_emails GET    /rails/conductor/action_mailbox/inbound_emails(.:format)                                          rails/conductor/action_mailbox/inbound_emails#index
                                         POST   /rails/conductor/action_mailbox/inbound_emails(.:format)                                          rails/conductor/action_mailbox/inbound_emails#create
       new_rails_conductor_inbound_email GET    /rails/conductor/action_mailbox/inbound_emails/new(.:format)                                      rails/conductor/action_mailbox/inbound_emails#new
      edit_rails_conductor_inbound_email GET    /rails/conductor/action_mailbox/inbound_emails/:id/edit(.:format)                                 rails/conductor/action_mailbox/inbound_emails#edit
           rails_conductor_inbound_email GET    /rails/conductor/action_mailbox/inbound_emails/:id(.:format)                                      rails/conductor/action_mailbox/inbound_emails#show
                                         PATCH  /rails/conductor/action_mailbox/inbound_emails/:id(.:format)                                      rails/conductor/action_mailbox/inbound_emails#update
                                         PUT    /rails/conductor/action_mailbox/inbound_emails/:id(.:format)                                      rails/conductor/action_mailbox/inbound_emails#update
                                         DELETE /rails/conductor/action_mailbox/inbound_emails/:id(.:format)                                      rails/conductor/action_mailbox/inbound_emails#destroy
new_rails_conductor_inbound_email_source GET    /rails/conductor/action_mailbox/inbound_emails/sources/new(.:format)                              rails/conductor/action_mailbox/inbound_emails/sources#new
   rails_conductor_inbound_email_sources POST   /rails/conductor/action_mailbox/inbound_emails/sources(.:format)                                  rails/conductor/action_mailbox/inbound_emails/sources#create
   rails_conductor_inbound_email_reroute POST   /rails/conductor/action_mailbox/:inbound_email_id/reroute(.:format)                               rails/conductor/action_mailbox/reroutes#create
rails_conductor_inbound_email_incinerate POST   /rails/conductor/action_mailbox/:inbound_email_id/incinerate(.:format)                            rails/conductor/action_mailbox/incinerates#create
                      rails_service_blob GET    /rails/active_storage/blobs/redirect/:signed_id/*filename(.:format)                               active_storage/blobs/redirect#show
                rails_service_blob_proxy GET    /rails/active_storage/blobs/proxy/:signed_id/*filename(.:format)                                  active_storage/blobs/proxy#show
                                         GET    /rails/active_storage/blobs/:signed_id/*filename(.:format)                                        active_storage/blobs/redirect#show
               rails_blob_representation GET    /rails/active_storage/representations/redirect/:signed_blob_id/:variation_key/*filename(.:format) active_storage/representations/redirect#show
         rails_blob_representation_proxy GET    /rails/active_storage/representations/proxy/:signed_blob_id/:variation_key/*filename(.:format)    active_storage/representations/proxy#show
                                         GET    /rails/active_storage/representations/:signed_blob_id/:variation_key/*filename(.:format)          active_storage/representations/redirect#show
                      rails_disk_service GET    /rails/active_storage/disk/:encoded_key/*filename(.:format)                                       active_storage/disk#show
               update_rails_disk_service PUT    /rails/active_storage/disk/:encoded_token(.:format)                                               active_storage/disk#update
                    rails_direct_uploads POST   /rails/active_storage/direct_uploads(.:format)                                                    active_storage/direct_uploads#create

Routes for RailsDb::Engine:
            root GET  /                                    rails_db/dashboard#index
      table_data GET  /tables/:table_id/data(.:format)     rails_db/tables#data
       table_csv GET  /tables/:table_id/csv(.:format)      rails_db/tables#csv
  table_truncate GET  /tables/:table_id/truncate(.:format) rails_db/tables#truncate
   table_destroy GET  /tables/:table_id/destroy(.:format)  rails_db/tables#destroy
      table_edit GET  /tables/:table_id/edit(.:format)     rails_db/tables#edit
    table_update PUT  /tables/:table_id/update(.:format)   rails_db/tables#update
      table_xlsx GET  /tables/:table_id/xlsx(.:format)     rails_db/tables#xlsx
       table_new GET  /tables/:table_id/new(.:format)      rails_db/tables#new
    table_create POST /tables/:table_id/create(.:format)   rails_db/tables#create
          tables GET  /tables(.:format)                    rails_db/tables#index
           table GET  /tables/:id(.:format)                rails_db/tables#show
             sql GET  /sql(.:format)                       rails_db/sql#index
      sql_import GET  /import(.:format)                    rails_db/sql#import
     sql_execute POST /execute(.:format)                   rails_db/sql#execute
         sql_csv POST /sql-csv(.:format)                   rails_db/sql#csv
         sql_xls POST /sql-xls(.:format)                   rails_db/sql#xls
sql_start_import POST /import-start(.:format)              rails_db/sql#import_start
      data_table GET  /data-table(.:format)                rails_db/dashboard#data_table
      standalone GET  /standalone(.:format)                rails_db/dashboard#standalone
```
2. Visit: https://glorious-dollop-4gj565pwg4w3jqqp-3000.app.github.dev/rails/info/routes to view the list of routes.

### Helper Routes Methods

1. By visiting: https://glorious-dollop-4gj565pwg4w3jqqp-3000.app.github.dev/rails/info/routes, you will see the helper commands which you can call from a html page with <%=helper%>. Such command will output the url path. You will see that only the sttaic paths are listed. 

2. To create the same methods for dynamic path, you need to manually add them using the additional command `as: :details`.

```
# config/routes.rb

# ...
  get "/movies" => "movies#index"
  get "/movies/:id" => "movies#show", as: :details
  # ...
end
```

3. When you visit https://glorious-dollop-4gj565pwg4w3jqqp-3000.app.github.dev/rails/info/routes again, you will see that a new helper method appears. It is called details_path. When you want to call this method on a html page, however, you need to remember that this is a dynamic route and you need to specify the dynamic variable. For example, use the following command to call this method:

```
<%= details_path(42) %>.
```

The above command will output: `"/movies/42"`.

If you instead use the command <%= details_path %>, you will get the following error.

```
No route matches {:action=>"show", :controller=>"movies"}, missing required keys: {:id}
```

### Benefits of Routes method

By defining methods to refer to routes: 
  - Calling the method will output the entire url, 
  - If we ever had to change the routes, you only have to change it in one place without breaking the application.

  ### Define custom methods to our routes

  Add route definition as discussed in the above as show here:

  ```
  # config/routes.rb

Rails.application.routes.draw do
  root "movies#index"

  # CREATE
  post "/movies" => "movies#create", as: :movies
  get "/movies/new" => "movies#new", as: :new_movie
          
  # READ
  get "/movies" => "movies#index"
  get "/movies/:id" => "movies#show", as: :movie
  
  # UPDATE
  patch "/movies/:id" => "movies#update"
  get "/movies/:id/edit" => "movies#edit", as: :edit_movie
  
  # DELETE
  delete "/movies/:id" => "movies#destroy"

  #------------------------------
end
  ```

- We don’t need to define methods twice for paths that already have a method defined. 

- These methods just return a string with the path name, they ignore the HTTP verb, so there’s no difference between naming a route helper method for delete or patch above; both will return "/movies/ID". 

- We defined the method movie_path and movie_url for all three get, patch, and delete HTTP verbs for this route just by adding it once.

### Amazing! A Direct Application of the route method

Let's apply the route method by modifying:

```
<!-- app/views/movies/index.html.erb -->

<!-- ... -->
<div>
  <a href="/movies/new">Add a new movie</a>
</div>
<!-- ... -->
```

into

```
<!-- app/views/movies/index.html.erb -->

<!-- ... -->
<div>
  <a href="<%= new_movie_path %>">Add a new movie</a>
</div>
<!-- ... -->
```

**NEXT: Cleaning up the routes.**

1.

```
<!-- app/views/movies/index.html.erb -->

<!-- ... -->
<div>
  <a href="/movies/new">Add a new movie</a>
</div>
<!-- ... -->
```

to

```
<!-- app/views/movies/index.html.erb -->

<!-- ... -->
<div>
  <a href="<%= new_movie_path %>">Add a new movie</a>
</div>
<!-- ... -->
```

2. 
```
<!-- app/views/movies/index.html.erb -->

<!-- ... -->
    <td>
      <a href="/movies/<%= a_movie.id %>">
        Show details
      </a>
    </td>
<!-- ... -->
```

to

```
<a href="<%= movie_path(a_movie.id) %>">
```

3. 
```
<!-- app/views/movies/show.html.erb -->

<!-- ... -->
    <div>
      <div>
        <a href="/movies">
          Go back
        </a>
      </div>

      <div>
        <a href="/movies/<%= @the_movie.id %>/edit">
          Edit movie
        </a>
      </div>

      <div>
        <a href="/movies/<%= @the_movie.id %>" data-turbo-method="delete">
          Delete movie
        </a>
      </div>
    </div>
<!-- ... -->
```

to

```
<!-- app/views/movies/show.html.erb -->

<!-- ... -->
    <div>
      <div>
        <a href="<%= movies_path %>">
          Go back
        </a>
      </div>

      <div>
        <a href="<%= edit_movie_path(@the_movie.id) %>">
          Edit movie
        </a>
      </div>

      <div>
        <a href="<%= movie_path(@the_movie.id) %>" data-turbo-method="delete">
          Delete movie
        </a>
      </div>
    </div>
<!-- ... -->
```

4. We can also omit the attribute .id to shorten such commands. For example:

```
movie_path(@the_movie.id)
```

can be written as

```
movie_path(@the_movie)
```

Rails will be able to locate the id automatically.

**Todo: Look over all of the html.erb files within the movies folder and omit the .id attributes accordingly.**
