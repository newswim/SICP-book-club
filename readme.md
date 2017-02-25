### A blog for SICP Book Club

---

## Contributing

Deploying to GH-Pages on a non-org repo is a little effed up. (Meaning I haven't
figured out how to resolve this issue). So for now, if you add or edit content
and then generate a new static site, you'll need to append the baseUrl to the
bottom of `index.html`, like so:


```html
<!-- before -->
 <script src="js/theme.min.js" type="text/javascript"></script>

<!-- after -->
<script src="https://newswim.github.io/SICP-book-club/js/theme.min.js" type="text/javascript"></script>

```
