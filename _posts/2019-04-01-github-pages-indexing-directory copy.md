---
layout: post
title: How to enable automatic directory indexing/listing on GitHub Pages
---
As you may know GitHub uses Jekyll for its free GitHub pages hosting.

If you have a bunch of html files in a repo, if would be useful if the index page display a list of html files contained inside the repository. So when user visit your repository home page, it will have a quick overview of the html files available.

However it is troublesome to edit home page (index.html) manually each time you make changes to one of the html files.

## Guide
So here is a guide to automate it. It uses existing Jekyll, no other dependencies.

<p>
1. Enable GitHub page and choose a theme. Doing so will generate a _config.yml on your directory.
</p>
<p>
2. Git pull it to sync the files. Now add the following code to index.md or index.html.
</p>
```xml
{% raw %}
   {% assign doclist = site.pages | sort: 'url'  %}
    <ul>
       {% for doc in doclist %}
            {% if doc.name contains '.md' or doc.name contains '.html' %}
                <li><a href="{{ site.baseurl }}{{ doc.url }}">{{ doc.url }}</a></li>
            {% endif %}
        {% endfor %}
    </ul>
{% endraw %}
```
<br/>
<p>
3. Commit and push it to GitHub, and you should see an updated directory listing on index page every time you commit.
</p>

## Notes
Two things to take note:

### HTML issue
It can't detect .html file if it doesn't have the [front matter](https://jekyllrb.com/docs/front-matter/) inside. There are two ways to remedy it:
- For each .html file, put `------` before the <html> tag.
  - Good: It retained html extension on the repository
  - Bad: Doesn't look good when you opened the html file directly, as you will see the dotted dash sticking out in the page when viewed in browser. 
  - Bad 2: The `------` will still appear in the repo.


- Change all .html file to .md extension so it become a markdown file. Next time when you git push the repo, GitHub's Jekyll builder will automatically convert all md file (except README.md) to html, and populate the list on the repo automatically. Yay!
  - Good: No `------` needed.
  - Bad: GitHub pages will automatically use `_layout/default.html` to wrap your file code, so your `<head>` information will go missing.
  - Bad 2: You have to use md extension.



### File types
You can't track other file format aside from markdown. I’m not sure about HTML and CSS though.

---

Useful reference: 
- [https://jekyllrb.com/docs/variables/](https://jekyllrb.com/docs/variables/)

