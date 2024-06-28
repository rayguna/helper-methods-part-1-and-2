# helper-methods-part-1-and-2

There is no target for this project.

Grading: https://grades.firstdraft.com/resources/177e71ec-b121-408a-bb32-581716ef6f50

Lesson: https://learn.firstdraft.com/lessons/160-helper-methods-part-1

Video: https://share.descript.com/view/bY2qZcJtJHl

***

### I. HELPER METHODS I

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



### C. Check Routes

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

### D. Benefits of Routes method

By defining methods to refer to routes: 
  - Calling the method will output the entire url, 
  - If we ever had to change the routes, you only have to change it in one place without breaking the application.

### E. Define custom methods to our routes

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

### F. Amazing! A Direct Application of the route method

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

If we just give movie_path an instance of ActiveRecord, then Rails will figure out the ID number for us by searching that record for the id column. Go back through the view templates and remove the .id for that helper method.

6. Made the changes within index.html.erb and show.html.erb.

**To do: Look over all of the html.erb files within the movies folder and omit the .id attributes accordingly.

### G. Redirect_to Refactoring

Within redirect_to() function, change the route to `movies_url`. Note that the methos is called as `movies_url`, rather than the usual `movies_path`.

Technically we should only use paths on the client facing view templates. But, when we’re sending responses back, we should use the fully qualified URL, including the domain name.

1. Make those changes within movies_controller.rb file.

```
class MoviesController < ApplicationController
  def new
    @the_movie = Movie.new

    render template: "movies/new"
  end

  def index
    matching_movies = Movie.all

    @list_of_movies = matching_movies.order({ :created_at => :desc })

    respond_to do |format|
      format.json do
        render json: @list_of_movies
      end

      format.html do
        render({ :template => "movies/index" })
      end
    end
  end

  def show
    the_id = params.fetch(:id)

    matching_movies = Movie.where({ :id => the_id })

    @the_movie = matching_movies.first

    render({ :template => "movies/show" })
  end

  def create
    @the_movie = Movie.new
    @the_movie.title = params.fetch("query_title")
    @the_movie.description = params.fetch("query_description")

    if @the_movie.valid?
      @the_movie.save
      
      #redirect_to("/movies", :notice => "Movie created successfully.")
      redirect_to(movies_url, :notice => "Movie created successfully.")
    else
      render template: "movies/new"
    end
  end

  def edit
    the_id = params.fetch(:id)

    matching_movies = Movie.where({ :id => the_id })

    @the_movie = matching_movies.first

    render({ :template => "movies/edit" })
  end

  def update
    the_id = params.fetch(:id)
    the_movie = Movie.where({ :id => the_id }).first

    the_movie.title = params.fetch("query_title")
    the_movie.description = params.fetch("query_description")

    if the_movie.valid?
      the_movie.save
      #redirect_to("/movies/#{the_movie.id}", :notice => "Movie updated successfully.")
      redirect_to(movie_path(the_movie), :notice => "Movie updated successfully.")
    else
      #redirect_to("/movies/#{the_movie.id}", :alert => "Movie failed to update successfully.")
      redirect_to(movie_path(the_movie), :alert => "Movie failed to update successfully.")
    end
  end

  def destroy
    the_id = params.fetch(:id)
    the_movie = Movie.where({ :id => the_id }).first

    the_movie.destroy

    #redirect_to("/movies", :notice => "Movie deleted successfully.")
    redirect_to(movies_url, :notice => "Movie deleted successfully.")
  end
end
```

Further simplify to:

```
class MoviesController < ApplicationController
  def new
    @the_movie = Movie.new

    render template: "movies/new"
  end

  def index
    matching_movies = Movie.all

    @list_of_movies = matching_movies.order({ :created_at => :desc })

    respond_to do |format|
      format.json do
        render json: @list_of_movies
      end

      format.html do
        render({ :template => "movies/index" })
      end
    end
  end

  def show
    the_id = params.fetch(:id)

    matching_movies = Movie.where({ :id => the_id })

    @the_movie = matching_movies.first

    render({ :template => "movies/show" })
  end

  def create
    @the_movie = Movie.new
    @the_movie.title = params.fetch("query_title")
    @the_movie.description = params.fetch("query_description")

    if @the_movie.valid?
      @the_movie.save
      
      #redirect_to("/movies", :notice => "Movie created successfully.")
      #redirect_to(movies_url, :notice => "Movie created successfully.")
      redirect_to movies_url, notice: "Movie created successfully."
    else
      render template: "movies/new"
    end
  end

  def edit
    the_id = params.fetch(:id)

    matching_movies = Movie.where({ :id => the_id })

    @the_movie = matching_movies.first

    render({ :template => "movies/edit" })
  end

  def update
    the_id = params.fetch(:id)
    the_movie = Movie.where({ :id => the_id }).first

    the_movie.title = params.fetch("query_title")
    the_movie.description = params.fetch("query_description")

    if the_movie.valid?
      the_movie.save
      #redirect_to("/movies/#{the_movie.id}", :notice => "Movie updated successfully.")
      #redirect_to(movie_path(the_movie), :notice => "Movie updated successfully.")
      redirect_to movie_path(the_movie), notice: "Movie updated successfully."
    else
      #redirect_to("/movies/#{the_movie.id}", :alert => "Movie failed to update successfully.")
      #redirect_to(movie_path(the_movie), :alert => "Movie failed to update successfully.")
      redirect_to movie_path(the_movie), alert: "Movie failed to update successfully."
    end
  end

  def destroy
    the_id = params.fetch(:id)
    the_movie = Movie.where({ :id => the_id }).first

    the_movie.destroy

    #redirect_to("/movies", :notice => "Movie deleted successfully.")
    #redirect_to(movies_url, :notice => "Movie deleted successfully.")
    redirect_to movies_url, notice: "Movie deleted successfully."
  end
end
```

### H. Shorten template names

1. if the folder name that the view templates are located inside of matches the name of the controller, and if the action name matches the name of the template, we can just get rid of the whole string! (And the method!). The above script can be made shorter:

From

```
  def new
    @the_movie = Movie.new

    render "movies/new"
  end
```

Into

```
 def new
    @the_movie = Movie.new
  end
```

2. (32 min) Let's modify the movies_controller further by deleting the render statement. Try and go through the code in the controller and delete any render statements where the template matches the action name (only when it matches, Rails won’t figure out when it doesn’t). In short, RoR can recognize the html.erb file located within the views folder if the route and subroute match the class and method name, e.g., movies/new match class MoviesController < ApplicationController ... def new ... end. 

You can shorten

```
#render template: "movies/new"
```
into

```
render "new"
```

This is because the class name is equivalent to the route address, but the method is not, so you have to retain the method but you could drop the class name.

The final script becomes:

```
class MoviesController < ApplicationController
  def new
    @the_movie = Movie.new

    #render template: "movies/new"
  end

  def index
    matching_movies = Movie.all

    @list_of_movies = matching_movies.order({ :created_at => :desc })

    respond_to do |format|
      format.json do
        render json: @list_of_movies
      end

      format.html do
        #render({ :template => "movies/index" })
        #render template: "movies/index"
      end
    end
  end

  def show
    the_id = params.fetch(:id)

    matching_movies = Movie.where({ :id => the_id })

    @the_movie = matching_movies.first

    #render({ :template => "movies/show" })
  end

  def create
    @the_movie = Movie.new
    @the_movie.title = params.fetch("query_title")
    @the_movie.description = params.fetch("query_description")

    if @the_movie.valid?
      @the_movie.save
      
      #redirect_to("/movies", :notice => "Movie created successfully.")
      #redirect_to(movies_url, :notice => "Movie created successfully.")
      redirect_to movies_url, notice: "Movie created successfully."
    else
      #render template: "movies/new"
      render "new"
    end
  end

  def edit
    the_id = params.fetch(:id)

    matching_movies = Movie.where({ :id => the_id })

    @the_movie = matching_movies.first

    #render({ :template => "movies/edit" })
  end

  def update
    the_id = params.fetch(:id)
    the_movie = Movie.where({ :id => the_id }).first

    the_movie.title = params.fetch("query_title")
    the_movie.description = params.fetch("query_description")

    if the_movie.valid?
      the_movie.save
      #redirect_to("/movies/#{the_movie.id}", :notice => "Movie updated successfully.")
      #redirect_to(movie_path(the_movie), :notice => "Movie updated successfully.")
      redirect_to movie_path(the_movie), notice: "Movie updated successfully."
    else
      #redirect_to("/movies/#{the_movie.id}", :alert => "Movie failed to update successfully.")
      #redirect_to(movie_path(the_movie), :alert => "Movie failed to update successfully.")
      redirect_to movie_path(the_movie), alert: "Movie failed to update successfully."
    end
  end

  def destroy
    the_id = params.fetch(:id)
    the_movie = Movie.where({ :id => the_id }).first

    the_movie.destroy

    #redirect_to("/movies", :notice => "Movie deleted successfully.")
    #redirect_to(movies_url, :notice => "Movie deleted successfully.")
    redirect_to movies_url, notice: "Movie deleted successfully."
  end
end
```

### I. link_to helper method

RoR has a helper method (or shorthand notation for defining a url as well).

Here are the changes you can make:


1. From

```
  <a href="<%= new_movie_path %>">Add a new movie</a>
```

To
```
  <%= link_to "Add a new movie", new_movie_path %>
```


2. From
```
#views/index.html.erb

<a href="<%= movie_path(a_movie) %>">
  Show details
</a>
```

To
```
#views/index.html.erb

<%= link_to "Show details", movie_path(a_movie) %>
```

3. From

```
a href="<%= movie_path(@the_movie) %>" data-turbo-method="delete">
  Delete movie
</a>
```

To

```
<%= link_to "Delete movie", movie_path(@the_movie), data: {turbo_method: :delete} %>
```

or

```
<%= link_to "Delete movie", @the_movie, data: {turbo_method: :delete} %>
```

Note that: 
- in this case, there is an extra argument in the form of a hash.
- @movie and @the_movie both work. @the_movie is more correct, but RoR somehow is able to trace the object that contains the keyword @movie.

***

### II. HELPER METHODS 2

Further refactor what we did in the previous section.

Video: https://share.descript.com/view/TrSygtEDGsx

Lesson: https://learn.firstdraft.com/lessons/161-helper-methods-part-2


### A. form_with (3 min)

1. The form_with method has a closing tag like any other html tags.

2. This helper method automatically creates the authenticity token. So, we can omit the authenticity token and condense the code.

Change this

```
<!-- app/views/movies/new.html.erb -->


<form action="/movies" method="post" data-turbo="false">
  <input name="authenticity_token" value="<%= form_authenticity_token %>" type="hidden">
  ...
</form>
```

To this

```
<!-- app/views/movies/new.html.erb -->

<%= form_with(url: movies_path, data: { turbo: false }) do %>
  ...
<% end %>
```

Note that the closing </form> tag is replaced with <%end%>

### B. Label helper method for html forms (12 min)

new.html.erb form

1. Replace all html form elements with RoR helper methods for views/movies/new.html.erb.

Before:

```
<!-- app/views/movies/new.html.erb -->

<%= form_with(url: movies_path, data: { turbo: false }) do %>

  <div>
    <label for="title_box">
      Title
    </label>

    <input type="text" id="title_box" name="query_title" value="<%= @the_movie.title %>">
  </div>

  <div>
    <label for="description_box">
      Description
    </label>

    <textarea id="description_box" name="query_description" rows="3"><%= @the_movie.description %></textarea>
  </div>

  <button>
    Create Movie
  </button>

<% end %> 
```

After:

```
<!-- app/views/movies/new.html.erb -->

<%= form_with(url: movies_path, data: { turbo: false }) do %>
  <div>
    <%= label_tag :title_box, "Title" %>

    <%= text_field_tag :query_title, @the_movie.title, { id: "title_box" } %>
  </div>

  <div>
    <%= label_tag :description_box, "Description" %>

    <%= text_area_tag :query_description, @the_movie.description, { id: "description_box", rows: 3 } %>
  </div>

  <%= button_tag "Create Movie" %>
<% end %>
```

edit.html.erb form - do the same as the above

1. Before:

```
<form action="/movies/<%= @the_movie.id %>" method="post" data-turbo="false">
  <input name="authenticity_token" value="<%= form_authenticity_token %>" type="hidden">
  
  <input name="_method" value="patch" type="hidden">

  <div>
    <label for="title_box">
      Title
    </label>

    <input type="text" id="title_box" name="query_title" value="<%= @the_movie.title %>">
  </div>

  <div>
    <label for="description_box">
      Description
    </label>

    <textarea id="description_box" name="query_description" rows="3"><%= @the_movie.description %></textarea>
  </div>

  <button>
    Update Movie
  </button>
</form>
```

After:

```
<%= form_with(url: movie_path(@the_movie), method: :patch, data: { turbo: false }) do %>
  <div>
    <%= label_tag :title_box, "Title" %>

    <%= text_field_tag :query_title, @the_movie.title, {id: "title_box" } %>
  </div>

  <div>
    <%= label_tag :description_box, "Description" %>

    <%= text_area_tag :query_description, @the_movie.description, {id: "description_box", rows: 3 } %>
  </div>

  <%= button_tag "Update Movie" %>
<% end %>
```
- The default method that HTML forms use is get. But Rails is smart, and it knows that many of our forms use post. So by default, the form_with method on our new.html.erb form added the method="post" attribute and the authenticity token.

- Similarly, we can tell form_with exactly which method (HTTP verb) we want to use. In the case of our edit.html.erb form, we actually want the method: :patch, which we pass in as an argument above.

- (16 min) RoR automatically takes into account a hidden authenticity toekn and the method patch.

### C. Refactoring controller since we are done with the views .html.erb files (17 min)

Let's start with `app/controllers/movies_controller.rb`.

1. Remove curly brackets.

From:

```
matching_movies = Movie.where({ :id => the_id })
```

To

```
matching_movies = Movie.where( :id => the_id )
```

An further to:

```
matching_movies = Movie.where( id: the_id )

```

2. Use methods chaining.

From

```
# app/controllers/movies_controller.rb

  # ...
  def show
    the_id = params.fetch(:id) # FIRST we get the ID

    matching_movies = Movie.where(id: the_id) # THEN we get the set of matching rows (ActiveRecord:Relation)

    @the_movie = matching_movies.first # FINALLY we get one instance of ActiveRecord, or one row
  end
  # ...
```

To

```
# ...
  def show
    @the_movie = Movie.where(id: params.fetch(:id)).first
  end
  # ...
```

Even more precise:

```
# ...
  def show
    @the_movie = Movie.find(id: params.fetch(:id)) 
  end
  # ...
```

As opposed to find_by, which returns nil if the record with the given ID doesn’t exist, find throws an error exception of class ActiveRecord::RecordNotFound.

Once I deploy my app, if I’m using the find method, then this error message shows up as a 404 page, which is the correct behavior when someone tries to visit a resource that doesn’t exist (like /movies/890909820, or some ID number we don’t have in our table). If we used find_by and the query returned a nil, then a 500 page would be shown (“Something went wrong”), which is not the experience we want for our users.

Also, rename @the_movie with just @movie. The variable name should match with the class name.

So, change the above to:

```
# ...
  def show
    @movie = Movie.find(params.fetch(:id))
  end
  # ...
```

3. Also, name @list_of_movies to just @movies

From
```
  # ...
  def index
    @list_of_movies = Movie.order(created_at: :desc)

    respond_to do |format|
      format.json do
        render json: @movies
      end

      format.html
    end
  end
  # ...
```

To
```
  # ...
  def index
    @movies = Movie.order(created_at: :desc)

    respond_to do |format|
      format.json do
        render json: @movies
      end

      format.html
    end
  end
  # ...
```

Do the same for the rest of the variables. Make them precise. Accordingly, change the variables within .html.erb forms to match. 

4. simplify variable names to match column names.

From

```
def create
    @movie = Movie.new
    @movie.title = params.fetch("query_title")
    @movie.description = params.fetch("query_description")
```

To
```
@movie.title = params.fetch("title")
@movie.description = params.fetch("description")
```

Even more to
```
@movie.title = params.fetch(:title)
@movie.description = params.fetch(:description)
```

In the params hash, we typically fetch on symbols, rather than strings. We can only use strings and symbols interchangeably in params because it is a special subclass of Hash. You are fetching the hash values directly. 

Change the relevant parameters in the corresponding new.html.erb files as follows.

```
<!-- app/views/movies/new.html.erb -->

<!-- ... -->
<%= form_with(url: movie_path(@movie), data: { turbo: false }) do %>
  <div>
    <%= label_tag :title, "Title" %>

    <%= text_field_tag :title, @movie.title, { id: "title" } %>
  </div>

  <div>
    <%= label_tag :description, "Description" %>

    <%= text_area_tag :description, @movie.description, { id: "description", rows: 3 } %>
  </div>
<!-- ... -->
```

Also, fix the corresponding names of the text fields in the corresponding .html.erb forms.

5. Fix more names.

- We don’t need the query_ and we don’t need the _box. Those are names we made up to help use keep track of things, but they aren’t used in a professional codebase.
- Make sure to make the form element names accordingly with the same names as those in the controller.

6. Simplify the script for form:

From
```
<!-- app/views/movies/new.html.erb -->

<%= form_with(url: movies_path, data: { turbo: false }) do %>

  <div>
    <%= label_tag :title, "Title" %>

    <%= text_field_tag :title, @the_movie.title, { id: "title" } %>
  </div>

  <div>
    <%= label_tag :description, "Description" %>

    <%= text_area_tag :description, @the_movie.description, { id: "description", rows: 3 } %>
  </div>

  <%= button_tag "Create Movie" %>

<% end %> 
```

To

```
<!-- app/views/movies/new.html.erb -->

<!-- ... -->
<%= form_with(url: movie_path(@movie), data: { turbo: false }) do %>
  <div>
    <%= label_tag :title %>

    <%= text_field_tag :title, @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>

    <%= text_area_tag :description, @movie.description, { rows: 3 } %>
  </div>
<!-- ... -->
```

**Amazing!** Let Rails automatically:

- set the label copy (by calling .capitalize on the :title or :description)

- and allow form_with to set the for="" and id="" attributes (which are just going to default to "title" and "description"):

Hence, you don't have to explicitly state the html form name and id.


### D. Refactor the data structure with mass assignment or nested hash

1. My goal with this form, is to have the params hash end up looking like:

```
{ :movie => { :title => "Some title", :description => "Some description" } }
```

Whereas, right now, it looks like:

```
{ :title => "Some title", :description => "Some description" }
```

2. Let's modify the script to make nested hash.

From:

```
<%= form_with(url: movies_path(@movie), data: { turbo: false }) do %>

  <div>
    <%= label_tag :title%>

    <%= text_field_tag :title, @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>

    <%= text_area_tag :description, @movie.description, { rows: 3 } %>
  </div>
```

To:

```
<!-- /movies/new -->

<%= form_with(url: movie_path(@movie), data: { turbo: false }) do %>
  <div>
    <%= label_tag :title %>

    <%= text_field_tag "movie[title]", @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>

    <%= text_area_tag "movie[description]", @movie.description, { rows: 3 } %>
  </div>
```

Also modify the script within movies_controller.rb

```
# app/controllers/movies_controller.rb

  # ...
  def create
    @movie = Movie.new
    @movie.title = params.fetch(:movie).fetch(:title)
    @movie.description = params.fetch(:movie).fetch(:description)
  # ...
```
### E. Refactor forms with mass assignment

1. (39 min) 

make nested hash in new.html.erb:

From
```
<% @movie.errors.full_messages.each do |message| %>
  <p style="color: red;"><%= message %></p>
<% end %>

<%= form_with(url: movies_path, data: { turbo: false }) do %>

  <div>
    <%= label_tag :title %>

    <%= text_field_tag :title, @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>

    <%= text_area_tag :description, @movie.description, { rows: 3 } %>
  </div>

  <%= button_tag "Create Movie" %>

<% end %> 
```

To

```
<% @movie.errors.full_messages.each do |message| %>
  <p style="color: red;"><%= message %></p>
<% end %>

<%= form_with(url: movies_path) do %>

  <div>
    <%= label_tag :title %>

    <%= text_field_tag "movie[title]", @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>

    <%= text_area_tag "movie[description]", @movie.description, { rows: 3 } %>
  </div>

  <%= button_tag "Create Movie" %>

<% end %> 
```

The output in the terminal looks as expected: 
```
Parameters: {"authenticity_token"=>"[FILTERED]", "movie"=>{"title"=>"some movie", "description"=>"description"}, "button"=>""}
```

2. When running the grade command, I receive the error:

```
Unable to find field "Title" that is not disabled

Capybara::ElementNotFound
```

Probably because of the different data structure.

3. Further modify def create as follows:

From:

```
  def create
    @movie = Movie.new
    @movie.title = params.fetch(:movie).fetch(:title)
    @movie.description = params.fetch(:movie).fetch(:description)
```

To:

```
  def create
    movie_attributes = params.fetch(:movie)

    @movie = Movie.new(movie_attributes)
```

Got an error message:

```
ActiveModel::ForbiddenAttributesError at /movies
ActiveModel::ForbiddenAttributesError
```

4. (47 min) Mass attribute is received as an attack. Must specify which parameter is getting mass attribute.

Modify to:
```
  def create
    #movie_attributes = params.fetch(:movie)
    movie_attributes = params.require(:movie).permit(:title, :description)

    @movie = Movie.new(movie_attributes)
```

Note that all fields must be listed in the permit function to be checked for existence. An error message will be trigerred when any of the listed fields are missing. 

### F. Form builder with model

1.Refactor form url.

From:
```
<%= form_with(url: movies_path) do %>

  <div>
    <%= label_tag :title %>

    <%= text_field_tag "movie[title]", @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>

    <%= text_area_tag "movie[description]", @movie.description, { rows: 3 } %>
  </div>

  <%= button_tag "Create Movie" %>

<% end %> 

```

To:
```
<%= form_with(model: @movie) do %>

  <div>
    <%= label_tag :title %>

    <%= text_field_tag "movie[title]", @movie.title %>
  </div>

  <div>
    <%= label_tag :description %>

    <%= text_area_tag "movie[description]", @movie.description, { rows: 3 } %>
  </div>

  <%= button_tag "Create Movie" %>

<% end %> 
```

You can refer directly to the model/table object.

This shorthand command only works if you had set your route RESTfully within routes.rb:

Example:
```
  get "/movies/:id" => "movies#show", as: :movie
```

2. Now that you refer to the table as model object, you can then call the methods directly and refactor the above code even further as follows:

```
<!-- new.html.erb -->

<% @movie.errors.full_messages.each do |message| %>
  <p style="color: red;"><%= message %></p>
<% end %>

<%= form_with(model: @movie) do |form| %>

  <div>
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :description %>
    <%= form.text_area :description, rows: 3 %>
  </div>

  <%= form.button %>

<% end %> 
```

The form.button helper method is smart because if you had prefilled the forms, the button name will recognize it and automatically change to update. 

You can, however, override the names as follows:

```
  <%= form.button "howdy" %>
```

3. You can create default values directly by simply setting the parameters when instantiating the objects as follows.
```
#new.html.erb

def new
  @movie = Movie.new(title:"hi", description: "bye")
  @movie.save
``` 
4. Modify the app/models/movie.rb table to validate both title and description:

```
class Movie < ApplicationRecord
  validates :title, presence: true
  validates :description, presence: true
end
```

### G. Challenge

i. (1 h 2 min) Update the edit form in the same way.

1. From:
```
<%= form_with(url: movie_path(@movie), method: :patch, data: { turbo: false }) do %>
  <div>
    <%= label_tag :title, "Title" %>

    <%= text_field_tag :title, @movie.title, {id: "title" } %>
  </div>

  <div>
    <%= label_tag :description, "Description" %>

    <%= text_area_tag :description, @movie.description, {id: "description", rows: 3 } %>
  </div>

  <%= button_tag "Update Movie" %>
<% end %>
```

To:
```
<% @movie.errors.full_messages.each do |message| %>
  <p style="color: red;"><%= message %></p>
<% end %>

<%= form_with(model: @movie) do |form| %>
  <div>
    <%= form.label :title %>

    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :description %>

    <%= form.text_area :description, rows: 3 %>
  </div>

  <%= form.button %>
<% end %>
```
2. I realize that the values are not being updated because you have to modify update method differently from the new method. In the update method, you are passing new parameters to an existing object. In this case, you don't have to instantiate a new object. Here is the new code:
```
def update
    movie_attributes = params.require(:movie).permit(:title, :description)
    
    the_id = params.fetch(:id)
    the_movie = Movie.where( id: the_id).first

    the_movie.title = movie_attributes["title"] #params.fetch("title")
    the_movie.description = movie_attributes["description"] #params.fetch("description")

    if the_movie.valid?
      the_movie.save
      #redirect_to("/movies/#{the_movie.id}", :notice => "Movie updated successfully.")
      #redirect_to(movie_path(the_movie), :notice => "Movie updated successfully.")
      redirect_to movie_path(the_movie), notice: "Movie updated successfully."
    else
      #redirect_to("/movies/#{the_movie.id}", :alert => "Movie failed to update successfully.")
      #redirect_to(movie_path(the_movie), :alert => "Movie failed to update successfully.")
      redirect_to movie_path(the_movie), alert: "Movie failed to update successfully."
    end
  end
```

Note that when you applyrefactoring on one file, you have to follow through for the rest of the RCAV routes!!! 

ii. (1 h 3 min) Generate a new model from scratch.

1. Generate just a table: `rails generate model director name:string dob:date`. This will create just the database tables. 
```
helper-methods-part-1-and-2 rg_branch_before-hash-nesting % rails generate model director name:string dob:date
      invoke  active_record
      create    db/migrate/20240628164614_create_directors.rb
      create    app/models/director.rb
```

2. You also need to type `rails db:migrate`.

```
helper-methods-part-1-and-2 rg_branch_before-hash-nesting % rails db:migrate
== 20240628164614 CreateDirectors: migrating ==================================
-- create_table(:directors)
   -> 0.1120s
== 20240628164614 CreateDirectors: migrated (0.1122s) =========================

Annotated (1): app/models/director.rb
```

3. Build up the entire resource, all the routes, controller, and the seven golden actions: create - post and get, read - get and get, update - patch and get, and delete - delete (CRUD). Do it in the modern way as a practice.
4. Note that the routes command is very short for RESTful routes, it is just: `resources :directors`.
5. You can look at the built resources by visiting: `https://stunning-space-guide-4gj565pwgrjcgjw-3000.app.github.dev/rails/info/routes`.
6. Type in the search box, director to show the new routes.   
7. Make your own directors controller.
8. Implement your seven routes.
9. Build the view templates.
10. Use all helper methods you know for:
  - links
  - url
  - form elements

iii. TIPs

1. You can define the routes in a concise way in just one line:
```
resources :movies
```

2. You can generate the entire RCAV using the command `rails generate scaffold director name:string dob:date`.

Follow this command with: `rails db:migrate`.

Rather than generating just the table with `rails generate model director name:string dob:date`

3. To reverse table creation, type `rails db:rollback`. Then, type `rails destroy model director name:string dob:date`.

helper-methods-part-1-and-2 main % rails destroy model director name:string dob:date
      invoke  active_record
      remove    db/migrate/20240628164614_create_directors.rb
      remove    app/models/director.rb

ref.: https://www.learneroo.com/modules/137/nodes/769

4. Now, let's execute: `rails generate scaffold director name:string dob:date`. Review the lesson on getting started with scaffolds: https://github.com/rayguna/getting-started-with-scaffolds

```
helper-methods-part-1-and-2 main % rails generate scaffold director name:string dob:date
      invoke  active_record
      create    db/migrate/20240628180729_create_directors.rb
      create    app/models/director.rb
      invoke  resource_route
       route    resources :directors
      invoke  scaffold_controller
      create    app/controllers/directors_controller.rb
      invoke    erb
      create      app/views/directors
      create      app/views/directors/index.html.erb
      create      app/views/directors/edit.html.erb
      create      app/views/directors/show.html.erb
      create      app/views/directors/new.html.erb
      create      app/views/directors/_form.html.erb
      create      app/views/directors/_director.html.erb
      invoke    resource_route
      invoke    jbuilder
      create      app/views/directors/index.json.jbuilder
      create      app/views/directors/show.json.jbuilder
      create      app/views/directors/_director.json.jbuilder
```

5. Follow up with: `rails db:migrate`. If you don't, you will receive error when you visit ../rails/db.

- Edit form does not return to main page after the refactoring. -> Look into the def update function within movies_controller. -> problem is fixed.
- When either title or description is omitted, the error message is not shown on the edit page.
  - The issue was traced with the form command both for def edit and def new. You need to specify the method type. The default method is post. For patch, you have to explicitly state it. However, for post you need to also specify data: {turbo: false}.

  ```
  <%= form_with(model: @movie, data: {turbo: false}) do |form| %>

  <div>
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :description %>
    <%= form.text_area :description, rows: 3 %>
  </div>

  <%= form.button %>

<% end %> 
  ``` 

Also, visit: https://stunning-space-guide-4gj565pwgrjcgjw-3000.app.github.dev/rails/info/routes to check to see that the directors routes are indeed created.

6. Note that the command automatically generates for you the `resources :directors` command in the routes.db file. YOu just need to tailor the RCAV files to your own project. 

7. In the auto-generated html files, I see that the concise command called link_to is used.

```
<h1>New director</h1>

<%= render "form", director: @director %>

<br>

<div>
  <%= link_to "Back to directors", directors_path %>
</div>
```

### Issues 

- Need advise on best practice on git version tracking. 
  - If you encounter any issues, go back to the version where you haven't seen the erorr and create a branch with: git -b branch <version id> to directly create and switch to a new branch. If you only want to create a branch, type git branch <version id>. Then, type git checkout new_branch to switch.
  - To merge the new branch with the main, you must first create a pull request.
  - Select your own version, not the appdev version from which you forked repository from.
  - If there are conflicts, type in the terminal git merge --no-ff branch name.
  - Review the tabs and resolve conflicts. Checkmark on the version you want to incorporate. Finally, type merge conflicts.
  - Resolve all conflicts. You have to resolve all conflicts before you can merge. 


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

### Appendix B. Extracting data from a form - Bundled sub-hashes in params

1. Getting a dictionary from a form

```
<!-- app/views/movies/index.html.erb -->

<h1>
  List of all movies
</h1>

<form>
  <input name="zebra">
  <button>Submit</button>
</form>
<!-- ... -->
```

The output is: Parameters: {"zebra"=>"hi"}

2. Getting an array from a form

```
<form>
  <input name="zebra[]">
  <button>Submit</button>
</form>
```

The output is: Parameters: {"zebra"=>["hi"]}

3. Getting an array of multiple elements:

```
<form>
  <input name="zebra[]">
  <input name="zebra[]">
  <button>Submit</button>
</form>
```

The output is: Parameters: {"zebra"=>["hi", "there"]}

4. Getting a nested dictonary from a form:

```
<form>
  <input name="zebra[giraffe]">
  <input name="zebra[elephant]">
  <button>Submit</button>
</form>
```

The output is: Parameters: {"zebra"=>{"giraffe"=>"hi", "elephant"=>"there"}}

### C. Branching and merging

1. Use gitlens to make a branch from the previous version. call it rg_branch_before-hash-nesting

2. Switch to the newly created branch with `git checkout rg_branch_before-hash-nesting`.

3. Run grade to see if all tests passed.

4. Compare codes with the head.
