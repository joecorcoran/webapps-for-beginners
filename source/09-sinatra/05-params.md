# Params

Now, sometimes the request path contains dynamic data.

For example, the path of the URL <a href="https://rubygems.org/gems/middleman">https://rubygems.org/gems/middleman</a>
is `/gems/middleman`. The path of the details page for the gem Rack on
RubyGems.org is `/gems/rack`, the path for Sinatra is `/gems/sinatra`, and so
on.

Obviously we don't want to hardcode ("write out literally") all these names in
our application code: We don't want to change our code for each and every new
gem that is added. Instead we want to be able to express *"a path that starts
with `/gems` followed by another, second segment"*.

In Sinatra we can do this by specifying pattern as a path. Sinatra will then
match the pattern against the path, and see if it applies.

Let's try that out.

Add the following route (request handler) to your program, at the end of the
file:

```ruby
get "/hello/:name" do
  "Hello #{params["name"]}!"
end
```

Restart your Sinatra application, and point your browser to
<a href="http://localhost:4567/hello/monstas">http://localhost:4567/hello/monstas</a>.

How does this work?

`params` is a hash that Sinatra makes available for you in your route blocks, and
it will automatically include relevant data from the request.

In our case our route specifies a path that is a pattern: the last part of the
path starts with a colon `:`. This tells Sinatra that we'd like to accept any
string here, and that we'd like to call this string `name`.

Sinatra therefore adds the key `"name"` to the `params` hash, and sets the
given string from the path (i.e. from the URL) to it.

When you point your browser to the URL
<a href="http://localhost:4567/hello/Elizabeth">http://localhost:4567/hello/Elizabeth</a>
your application will say *"Hello Elizabeth!"*, when you go to
<a href="http://localhost:4567/hello/Juliane">http://localhost:4567/hello/Juliane</a>
your application will say *"Hello Juliane!"*, and so on.

Let's inspect the params hash, and return this string as the response body:

```ruby
get "/hello/:name" do
  params.inspect
end
```

If you restart your application, and reload the page in your browser, then it
should display something like this:

```
{"splat"=>[], "captures"=>["monstas"], "name"=>"monstas"}
```

So this confirms that `params` is a hash, and the key `"name"` has the value
`"monstas"` set.  `splat` and `captures` are for building more complicated
routes, and we can ignore these for now.

This is pretty cool.

The `params` hash can contain more than matches from the URL. You'll later see
that it also contains any data sent from HTML forms as part of the HTTP
request. As well as any query params that can be part of the URL (separated
with a question mark `?`).

But for now it's good to know that Sinatra adds matches from the path pattern
to the `params` hash.


