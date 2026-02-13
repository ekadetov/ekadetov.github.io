# Hi, I'm Evgenii 👋

Welcome to my personal page!

## 🚀 GitHub Projects
* [Project Name](https://github.com/yourusername/project-link) - A brief description of what this does.
* [Another Project](https://github.com/yourusername/another-link) - Another cool tool.

## 📝 Recent Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a> 
      <span>({{ post.date | date: "%b %d, %Y" }})</span>
    </li>
  {% endfor %}
</ul>

## 📫 Contact Me
* [LinkedIn](https://www.linkedin.com/in/ekadetov/)

