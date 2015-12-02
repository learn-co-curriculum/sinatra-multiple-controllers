# Sinatra Multiple Controllers

1. Multi-Controller Application structure.
2. Mounting multiple controllers

Often applications need to respond to multiple URLs relating to a single concept in the application.

In an e-commerce application, the `/products` URL, would relate to requests to view products. `/products/1` would represent requesting information about the first product. However URLs around `/orders` would relate to requests to view a customer's order history. `/orders/1` would represent requesting information about the first order.

To separate these domain concepts in our code, we'd actually code the responses to such a grouping of URLs in separate controllers. Every controller in our application should follow the Single Responsibility Principal, only encapsulating logic relating to a singular entity in our application domain.

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

Don't worry about the specifics of how `get '/products/:id'` as a route functions, just know it provides the functionality of URLs that match the pattern `/products/1`, `/products/2`, `/products/10`, etc.

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

Then, we must mount both classes. Only one class can be specified to be `run`. The other class must be loaded as something called MiddleWare. We won't get into MiddleWare now, suffice to say, you simply `use` it instead of `run`.

`config.ru`:
```ruby
require 'sinatra'

require_relative 'app/controllers/products_controller'
require_relative 'app/controllers/orders_controller'

use ProductsController
run OrdersController
```

Which classes you `use` or `run` matter, but we won't worry about that now, just make sure you only ever `run` one class and the rest our loaded via `use`.

You can run this complex application with `rackup`. Once the application loads and is listening on port 9292, navigate to http://localhost:9292/products , http://localhost:9292/products/42, http://localhost:9292/orders , and http://localhost:9292/orders/2600 to see the application in action. Stop it with `CTRL+C`

## Resources

* [Blake Mizerany - Ruby Learning Interview](http://rubylearning.com/blog/2009/08/11/blake-mizerany-how-do-i-learn-and-master-sinatra/)
* [Companies Using Sinatra](http://www.sinatrarb.com/wild.html)
<a href='https://learn.co/lessons/sinatra-multiple-controllers' data-visibility='hidden'>View this lesson on Learn.co</a>
