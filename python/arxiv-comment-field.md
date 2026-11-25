# The arXiv API has a comment field, and code links often live there

When you check whether a paper has public code, the abstract is not enough. Authors often put the repository link in the arXiv comment ("22 pages, code at github.com/..."), on their own page or in a post, and not in the abstract.

The export API returns the comment as `arxiv:comment`:

```python
import urllib.request
import xml.etree.ElementTree as ET

NS = {"a": "http://www.w3.org/2005/Atom", "ar": "http://arxiv.org/schemas/atom"}
url = "https://export.arxiv.org/api/query?id_list=2611.02269"
feed = ET.fromstring(urllib.request.urlopen(url, timeout=60).read())
for entry in feed.findall("a:entry", NS):
    print(entry.findtext("a:title", "", NS))
    print("comment:", entry.findtext("ar:comment", "", NS))
```

If neither the abstract nor the comment links code, search GitHub for the paper's title too, not only its arXiv id. A repo's README often never mentions the id.
