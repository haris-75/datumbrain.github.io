# HOW-TO-POST MANUAL

Get the latest from master
```
git pull origin master
```

Create a new branch with the following format.

```
post/whatever-post-title-is
```

## Creating new post

New posts go in <code>_posts</code> directory. Every single post is actually a file of the format<br><br> <code>YYYY-MM-DD-whatever-post-title.md</code>

Write your post in markdown syntax.

## Testing locally

To test what you have created locally. Run <br>

```
bundle exec jekyll serve
```

The application is accessible at <code>http://localhost:4000/</code>

## Publish

Once everything went as expected, push your branch and create a pull request. Feel free to add a reviewer.

## More ...

To learn more what goes in a post. Please refer to **[jekyll docs](https://jekyllrb.com/docs/)**
