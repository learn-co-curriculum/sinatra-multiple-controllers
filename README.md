# Sinatra Multiple Controllers

## Objectives
1. Multi-Controller Application structure.
2. Mounting multiple controllers

## Intro

If you think about a typical e-commerce application, you would need at least two models: one for orders and one for products. The products model would require a series of routes all starting with `/products`, and the orders model routes would start with `/orders`.  `/products/1` would represent requesting information about the very first product, and `/orders/1` would represent requesting information about the first order.

When you think about all the CRUD actions, you would need a lot of controller actions for both of these models.

Products:

| Request | Route | CRUD Action |
|----------|------|-------------|
| GET      | '/products/:id' | Read |
| GET      | '/products/new'  | Create |
| POST     | '/products'   | Create |
| GET      | '/products/:id/edit'| Update|
| POST     | '/products/:id'     | Update |
| GET      | '/products'         | Read|
| POST     | '/products/:id/delete'| Delete|


Orders:

| Request | Route | CRUD Action |
|----------|------|-------------|
| GET      | '/orders/:id' | Read |
| GET      | '/orders/new'  | Create |
| POST     | '/orders'   | Create |
| GET      | '/orders/:id/edit'| Update|
| POST     | '/orders/:id'     | Update |
| GET      | '/orders'         | Read|
| POST     | '/orders/:id/delete'| Delete|


With GET and POST requests, you're looking at a really full and complex controller, with 7 controller actions for each model. That's 14 controller actions before you even add in users or shopping carts. With that much code in one file, it can gets hard to maneuver and find different pieces of the code you might need to edit or update.

Just like we separate out our different models into different files, we need to separate these domain concepts in our code into separate controllers. Every controller in our application should follow the Single Responsibility Principal, only encapsulating logic relating to a singular entity in our application domain. We need to separate out a Products Controller and an Orders Controller.

### Products Controller

You can imagine a controller `ProductsController`, defined in `app/controllers/products_controller.rb`.

```ruby
class ProductsController < Sinatra::Base

  get '/products' do
    "Product Index"
  end

  get '/products/:id' do
    "Product #{params[:id]} Show"
  end

end
```

### Orders Controller

Similarly, we'd have `OrdersController` defined in `app/controllers/orders_controller.rb`/

```ruby
class OrdersController < Sinatra::Base

  get '/orders' do
    "Order Index"
  end

  get '/orders/:id' do
    "Order #{params[:id]} Show"
  end

end
```

#### Mounting Multiple Controllers in `config.ru`.

Now that our application logic spans more than one controller class, our `config.ru` that starts our application becomes a bit more complicated.

First, our environment must load both controller files.

`config.ru`:
```ruby
require 'sinatra'

require_relative 'app/controllers/products_controller'
require_relative 'app/controllers/orders_controller'
```

Then, we must mount both classes. Only one class can be specified to be `run`. The other class must be loaded as Middleware. We won't get into MiddleWare now, suffice to say, you simply `use` it instead of `run`.

`config.ru`:
```ruby
require 'sinatra'

require_relative 'app/controllers/products_controller'
require_relative 'app/controllers/orders_controller'

use ProductsController
run OrdersController
```

Which classes you `use` or `run` matter, but we won't worry about that now, just make sure you only ever `run` one class and the rest our loaded via `use`.

You would start your server in the same way you would any Sinatra application. Both `shotgun` and `rackup` work just fine!

## Resources

* [Blake Mizerany - Ruby Learning Interview](http://rubylearning.com/blog/2009/08/11/blake-mizerany-how-do-i-learn-and-master-sinatra/)
* [Companies Using Sinatra](http://www.sinatrarb.com/wild.html)
<a href='https://learn.co/lessons/sinatra-multiple-controllers' data-visibility='hidden'>View this lesson on Learn.co</a>
