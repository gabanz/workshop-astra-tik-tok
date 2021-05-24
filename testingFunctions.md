## Serverless Functions - How they work

So how do these functions work with no back-end server?

The short answer is that Netlify is providing the back-end environment for us. All we have to do is tell Netlify where to find the functions. Netlify will do the rest.

### Configuration

Take a look at `netlify.toml`.

```
[build]
command = "npm run build"
functions = "functions"
publish = "build"
```

This is the configuration file we include in our codebase that tells Netlify how to build our app. In our case it's really simple. First we give the `build` command to build our app: `npm run build`. Then we tell Netlify where to find our serverless functions, and finally where to find the resulting app after build.

So Netlify will create endpoints for our serverless functions based on the files it finds in our functions folder.

For example, we have a function called `posts.js`. As we saw before, this function returns all of the current posts in our database. Netlify will see that file in our `functions` directory and dynamically create an endpoint at `/.netlify/functions/posts`.

We can see this in action by manually going to the endpoint on our Netlify site: `[your-site-url]/.netlify/functions/posts`.

We can also see the functions in our Netlify account.
- Go to netlify.com and sign in.
- Select your site from the list.
- Select the "Functions" tab at the top.

![netlify_functions](./tutorial/images/netlify_functions.png?raw=true)

From here we can see all our functions and get direct links as well as watch real time logs.