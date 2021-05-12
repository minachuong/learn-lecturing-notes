Cross Origin Resource Sharing

When you have two decoupled apps like we have, their origins are pretty much their servers. The React app has it’s own origin because it’s getting served from a different server than the Rails application.

When we’re dealing with communication between multiple origins, there are security concerns because how one application chooses to handle data differs from another application.

Browsers by default recognize when an application is making a request to a different server other than its own.
And it does that through CORS.

CORS allows you to establish policies/rules around which origins you want the application to interact with.

Because our frontend is requesting data from our backend Rails application, we have to tell our Rails application that it’s okay to take requests from our React application…we’ll actually go a step further and say, “lets take requests from anyone”. We’re generous like that.

So there are some specific steps to handle to do this:

ADD to app/controllers/application_controller.rb
skip_before_action :verify_authenticity_token

ADD to Gemfile
gem 'rack-cors', :require => 'rack/cors'

ADD file cors.rb to config/initializers



# Avoid CORS issues when API is called from the frontend app.
# Handle Cross-Origin Resource Sharing (CORS) in order to accept cross-origin AJAX requests.


# Read more: https://github.com/cyu/rack-cors


Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'  # <- change this to allow requests from any domain while in development.


    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end

$ bundle install
