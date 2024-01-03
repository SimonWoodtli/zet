# JAMstack with Hugo, Tailwind and TinaCMS

* TODO: Create new project and go over these notes, it's a bit of a mess currently :)

## Install Dependencies

### Hugo

> 🧐 I personally prefer to use `asdf`. So as the projects gets older you still
> have a version that works. Because backward compatibility is not guaranteed,
> better safe than sorry.

1. via `npm`
1. via `go install`
1. via your package manager
1. via `asdf`

### Node

1. via [install script][node]
1. via your package manger
1. via `asdf`

## Actual setup

### Initialize Repo

> 🧐 Decide if you want to create a reusable hugo theme or not. If you don't
> want a reusable theme don't run the hugo theme cmd. Instead install tailwind
> in the root dir with the rest of your npm packages. If you are using a theme
> you would pretty much install all npm packages in the themes dir except
> TinaCMS and browser-sync.

1. init hugo and git

```
git init
asdf local gohugo extended_0.120.3
asdf local nodejs 20.9.0
hugo new site site-name
mv site-name/* ../ && rm -rf site-name
echo "node_modules" > .gitignore
npm init
npm install -D browser-sync
```

> 🧐 If you don't wanna use browser-sync you can also add a npm package script:
> `"watch": "find . -name '*.js' -o -name '*.md' -o -name '*.html' | grep -Ev
> '(node_modules|docs|assets)' | entr -ccdr npm run build"`
> But than stop using hugo to serve the site instead use `python -m http.server 1313`

2. Don't forget to commit often :)
3. Test if hugo works as intended

* <https://gohugo.io/getting-started/quick-start/>

### Setup some basic Hugo layout templates

> 🧐 Skip this chapter if you are using a theme. Except if you want to
> overwrite some of the defaults from the theme.

```
mkdir layouts/_default
touch layouts/_default/{baseof.html,single.html,list.html}
```

These are just some example templates to test and get you started.

1. edit **baseof.html**:

```
<!DOCTYPE html>
<html lang="{{ or .Site.LanguageCode .Site.Language.Lang }}">
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, minimum-scale=1.0, initial-scale=1.0">
		<title>{{ with .Title }}{{ printf "%s | " . }}{{ end }}{{ site.Title }}</title>
	</head>
	<body>
		<p>Top of baseof</p>
		<hr>
		{{ block "main" . }}{{ end }}
		<hr>
		<p>Bottom of baseof</p>
	</body>
</html>
```

2. edit **single.html**

```
{{ define "main" }}
  <h1>{{ .Title }}</h1>
	<p>This is the single template</p>
  {{ .Content }}
{{ end }}
```

3. edit **list.html**: added dark mode [snippet][s-darkmode] from Tailwind for testing darkmode

```
{{ define "main" }}
  <h1>{{ .Title }}</h1>
	<p>This is the list template</p>
  {{ .Content }}
  {{ range .Pages }}
    <h2><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
  {{ end }}
	<div class="bg-white dark:bg-slate-800 rounded-lg px-6 py-8 ring-1 ring-slate-900/5 shadow-xl">
		<div>
			<span class="inline-flex items-center justify-center p-2 bg-indigo-500 rounded-md shadow-lg">
				<svg class="h-6 w-6 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true"><!-- ... --></svg>
			</span>
		</div>
		<h3 class="text-slate-900 dark:text-white mt-5 text-base font-medium tracking-tight">Writes Upside-Down</h3>
		<p class="text-slate-500 dark:text-slate-400 mt-2 text-sm">
			The Zero Gravity Pen can be used to write in any orientation, including upside-down. It even works in outer space.
		</p>
	</div>
{{ end }}
```

4. create content and launch server and test it

```
hugo new a.md
hugo new dir1/b.md
hugo server -D
```

> Notice `http://localhost:1313/a` shows correct but `http://localhost:1313/` cuts off the tailwind part cause we need to set that up in the next chapter.

5.

### General Hugo Advice/Hints

* If you are using multiple frameworks and libraries with Hugo it's better to publish the site and use watch `browser-sync` (NOT SURE GOTTA TEST)
* By default a list page only gets created for children of the content folder e.g. **content/dir1/**
		* If you want a list page for grand children of content you need to add **content/dir1/dir2/\_index.md**
		* If you want to overwrite the default auto created list page you can also create **content/dir1/\_index.md**
* To create content: `hugo new foo.md` uses the default archetype and always works.
		* If you want to use a specific archetype like **content** you need to create a new archetype in **archetypes/** and to create content with it: `hugo new content foo.md`
* To get some barebone **template/layouts/** files generated just create a new theme within a hugo project `hugo new them foo`
* If you want to change the front matter to not use **archetypes/default.md**: `touch archetypes/dir1.md` this will then apply for new content: `hugo new dir1/c.md`
* Hugo shortcodes (go template lang extension) allow you to embed more complicated content within the **content/\*.md** files without using html in your markdown. E.g A link to a YT video `{{< youtube w7Ft2ymGmfc >}}`
		* Note that shortcodes will not work in template files. If you need the type of drop-in functionality that shortcodes provide but in a template, you most likely want a partial template instead.
		* You can also create custom shortcodes
		* https://gohugo.io/content-management/shortcodes/

* shortcode templates are the equivalent of what **layouts/partials** are for **layouts/\_default**. In other words you can access shortcodes in **content/** that are predefined in **layouts/shortcodes/foo.html**
		* to access them in **content/a.md** you can use `{{< foo >}}`
		* you can also access them and pass in variables into them: `{{< foo color="blue" >}}`.
		That way you could customize **layouts/shortcodes/foo.html** `<p style="color:{{ .Get \`color\`}};">This is foo</p>`. If you wanna access the var without it being in quotes its just `{{ .Get "color" }}`
		* If you just pass in a parameter but don't define a  var name like `{{< foo blue >}}` you access it with `{{ .Get 0 }}`
		* If you wanna use them on multiple lines like so: `{{< foo >}}\n blabla\n blabla\n{{< /foo >}}` you can access blabla with `{{ .Inner }}`
		* By default inside of shortcodes any markdown special chars do not get rendered. If you want them to get rendered use `{{% foo %}}**bold text**{{% /foo %}}`
* To change template of homepage edit: **layouts/index.html**
* To change list or single pages in **content/dir1** specifically use sections **layouts/dir1/{single.html,list.html}**
		* You can also do that by using type and layout vars in the front matter: https://gohugo.io/templates/lookup-order/#target-a-template
		* Both ways have there ups and downs, but in a nut shell sections are less granular but easier to maintain and front matter is more specific but also more cumbersome
		* https://discourse.gohugo.io/t/type-vs-sections-whats-the-difference/178/3
* To change template that every site uses: **layouts/\_default/baseof.html**
* To change template that list pages use: **layouts/\_default/list.html**
* To change template that single pages use: **layouts/\_default/single.html**
* Block constructs allow you to be more granular with your templates: If you define a block in **baseof.html** `{{ block "footer" .}} This is a footer{{end}}` but use the same block name again in **list.html** `{{define "footer"}} This overwrites the baseof footer{{end}}` on a list page it will render the content defined in **list.html** overwritting the content form **baseof.html**
* Use `{{ range .Pages }}` on list pages where you can iterate over multiple pages to get results from them.
* To use taxonomies and define them in the front matter like `tags: ["tag1", "tag2", "tag3"]` of a **content/a.md** file you also have to edit **hugo.toml** with `[taxonomies]\n  tag = "tags"` or if you use multiple taxonomies add them too.
* To show taxonomies `{{ if .Params.tags }}`, `{{range .Params.tags}}` or `<a href="{{ "/tags/" | relLangURL }}{{ . | urlize }}">{{ . }}</a>` are useful
* `{{.URL}}` is deprecated use `{{.Permalink}}` or `{{.RelPermalink}}`
* Partials are reusabale template snippets for baseof/single/list/index in **layouts/partials/foo.html**
		* use in another template(. means all variables): `{{ partial "foo" . }}`
		* if you use custom variables within a partial like `{{.myFoo}}` you can access them with dictionaries: `{{ partial "foo" (dict "myFoo") }}`
* Variables can only be used with templates in **layouts/**
		* Some important default/special Vars; front matter: `.Date`, `.Title` URLs: `.Permalink`, `.RelPermalink` all pages: `.Site.Pages` only pages of whatever template you write it in (and if list its children) `.Pages`, written content in **content/\*md** `.Content`
		* Custom front matter Vars are assigned in front matter and accessed with `{{ .Params.foo }}`
		* Custom vars within a template are like normal go template vars: `{{$foo := "string"}}` and access it with `{{ $foo }}`
		* https://gohugo.io/variables/
* Functions can only be used with templates in **layouts/**
		* used just like regular go template funcs e.g. f call: `{{ funcFoo para1 para2 }}`
		* https://gohugo.io/functions/
* Conditionals work the same as in go templatelang
* **data/** acts as mini database you can add json,yml,toml files in there
		* to access **data/foo.json**: `{{ range .Site.Data.foo }}{{.key}}{{end}}`
* some advanced features explained: https://www.stackbit.com/blog/advanced-hugo-templates
* Hugo related blog: https://www.regisphilibert.com/blog/

### Add and setup Tailwind

> 🧐 I prefer using the prettier plugin to sort the class names automatically.
> If you don't just install tailwind here.

1. install pkgs and init tailwind

```
npm install -D prettier prettier-plugin-tailwindcss tailwindcss @tailwindcss/typography
npx tailwindcss init
```

2. add tailwind-prettier plugin and typography to **prettier.config.js**

```
module.exports = {
 plugins: [
	'prettier-plugin-tailwindcss',
	require('@tailwindcss/typography'),
	],
}
```

3. add content and layout to **tailwind.config.js**

* TODO checkout if `content: ["./layouts/**/*.{html,js}"],` is enough

```
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["content/**/*.md", "layouts/**/*.html"],
  content: ["./layouts/**/*.{html,js}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

4. add **base.css** and **style.css**

```
mkdir -p assets/{js,css,images}
touch assets/css/{base.css,style.css}
```

5. edit **base.css**

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

6. edit **layouts/\_default/baseof.html** (or a head partial if you prefer)

```
{{ $style := resources.Get "css/style.css" }}
<link rel="stylesheet" href="{{ $style.RelPermalink }}" />
```

* TODO  check if this snippet works too  `<link rel="stylesheet" type="text/css" href="{{ "css/style.css" | relURL }}">`
* TODO play around with hugo built in minifier:

```
{{ $style := resources.Get "/stylesheet.scss" | toCSS | minify }}
<link rel="stylesheet" href="{{ $style.Permalink | relURL }}" />
```


7. add some `npm run` scripts to **package.json**

```
"build-style": "npx tailwindcss -i ./assets/css/base.css -o ./assets/css/style.css",
"build-hugo": "bash -c \"/var/home/xnasero/.asdf/bin/asdf exec hugo --printPathWarnings --printUnusedTemplates --cleanDestinationDir\"",
"build": "npm run build-style && npm run build-hugo",
"watch": "find . -name '*.js' -o -name '*.md' -o -name '*.html' | /bin/grep -Ev '(node_modules|public|assets)' | entr -ccdr npm run build",
"watchbg": "nohup bash -c 'find . -name \"*.js\" -o -name \"*.md\" -o -name \"*.html\" | /bin/grep -Ev \"(node_modules|public|assets)\" | entr -ccdr npm run build' > /tmp/build.output &",
"serverbg": "nohup bash -c 'browser-sync start --server public --port=1313 --files \"*.html, css/*.css, js/*.js\"' > /tmp/browsersync.output &",
"server": "browser-sync start --server public --port=1313 --files \"*.html, css/*.css, js/*.js\""
```

8. run watch and server in tmux split pane or bg
9. test site

* TODO nxt:
* test if prettier is already shuffling the classes, or add some steps on how to fix that


* <https://tailwindcss.com/docs/installation>

### Add and setup TinaCMS

```
npx @tinacms/cli@latest init
npx tinacms dev -c "hugo server -D -p 1313" **adjust server cmd use browser-sync
```

* <https://tina.io/docs/frameworks/hugo/>
* <https://github.com/tinacms/tina-hugo-starter/>


### Add pagefind search

1. Install and run it:

```
npm install -D pagefind
npx pagefind --site "public"
```

2. Add searchbar:

```
<link href="/pagefind/pagefind-ui.css" rel="stylesheet">
<script src="/pagefind/pagefind-ui.js"></script>
<div id="search"></div>
<script>
    window.addEventListener('DOMContentLoaded', (event) => {
        new PagefindUI({ element: "#search", showSubResults: true });
    });
</script>
```

3. Add some pagefind attributes to the relevant html tags to customize your results. E.g. if you only want to display single pages in the results use 'data-pagefind-body' on the article tag in single.html

https://pagefind.app/docs/metadata/

## Add Cactus Comments

See cactus docs pretty straight forward.

## Add some Vanilla JS

> 🧐 You can also add the assets files in static/ folder instead. Then you don't need to create the Hugo vars with resources.Get func and can use them directly.

```
mkdir assets/js
touch assets/js/main.js
```

In ***layouts/_default/baseof.html*** add:

```
{{ $js := resources.Get "js/main.js" }}
<script type="text/javascript" src="{{ $js.RelPermalink }}"></script>
```

## Random

`npx prettier --check .`
`npx prettier --write [dir name]`

`npm install -D --save-exact esbuild`

custom script in packages.json: `npm run bundle`

`hugo server --disableFastRender`

[node]: <https://github.com/SimonWoodtli/dotfiles/blob/main/install/install-node>

[s-darkmode]: <https://tailwindcss.com/docs/dark-mode>
