[marked.tech](https://marked.tech/blog) influence by the medium of text, a regular thought accelerator but they scorn at the thought of using a monolothic CMS just to get their ideas to the world.

![caddy anner image](http://www.tecmint.com/wp-content/uploads/2015/05/caddy-web-server.jpg)

In comes their secret weapon: [Caddy.](https://github.com/mholt/caddy)

Caddy is an open source web server written in GO. The main feature of interest here is 'directives', a low level plugin system supported by many third party developers. [marked.tech](https://marked.tech) are followers of the markdown race, they enjoy many different flavours from Github to Ghost so what better choice is there for marking up their ideas?

## Templates
Form follows function, Content before code (or something along those lines) keep it simple they like to say. In comes [bettermotherfuckingwebsite.com](http://bettermotherfuckingwebsite.com), with an inoffensive name this might not stand out from the crowd  but with some minor adaptations marked have a theme that their readers love.

![image of marked.tech blog](https://i.gyazo.com/7762058fba812183c132b6f096a51dba.png)

Caddy has a set of predefined template tags from the go standard library [text/template](https://golang.org/pkg/text/template/) these allow us to insert useful information like a readers IP address, the current date, or a rendered markdown file.

```HTML
<!-- insert the rendered markdown here -->
{{.Doc.body}}
``` 

Getting more advanced, this allows us to create a contents page, linking to all of our posts --

```HTML
{{range .Files}}
    {{if ne .Name "index.md"}}
        <a href="{{$.StripExt .Name}}">
            <h3>{{$.StripExt .Name}}</h3>
        </a>
    {{end}}
{{end}}
```

Loop through each .md file -> ignore our index page -> strip out the .md extension -> link to the post.

Cool huh? We're just getting started. 

 
### The Caddyfile
A good Caddyfile is the backbone of any self respecting blog. They describe the directives we want to use, which files we want to serve, the domain we want our users to visit and much more. A basic Caddyfile looks something like this --
```
marked.tech
```

Yes that is a little contrived, marked have a slightly more complicated use case --

```
localhost:2017

# clean up our urls
ext .md

# logs help us with debugging later
errors logs/error.log
log logs/access.log

# serve our md files, using custom templates
markdown /blog {
        template /templates/post.html
        template index /templates/index.html
}
```

### The Index
There's one undocumented oddity in Caddy's markdown directive, the index.md post.

Index is considered a special post, it can be reached by visiting the root url of the blog, in our case [marked.tech/blog](https://marked.tech/blog). Alongside a custom template file that removes our '- return to home -' link, we're all wrapped up.

![image of marked.tech index page](https://i.gyazo.com/fd5cd354cf82ce55d154f0ffbb867786.png)

So now marked.tech can get back to disrupting technology as we know it, and you can get back to tanning behind your laptop screen at 3am.

Checkout the project at [marked.tech/blog](https://marked.tech/blog)

Checkout the source at [github/marked.tech](https://github.com/aranscope/marked.tech)

Checkout this post (meta) at [aran.site/marked-tech](https://blog.aran.site/marked-tech)

