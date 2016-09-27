---
layout: post
title: Getting all the links with Wget
author: James Hook
tags: linux bash
---

How to download all the links on a page..... use ```wget```!!!

```
wget -r -np -l 1 -A zip http://example.com/download/
```

where 

```
-r,  --recursive          specify recursive download.
-np, --no-parent          don't ascend to the parent directory.
-l,  --level=NUMBER       maximum recursion depth (inf or 0 for infinite).
-A,  --accept=LIST        comma-separated list of accepted extensions.

```

also

```
-nd don't create subdirectories - all downloads to go in root folder
```

[Credit][credit] where credit's due..

[credit]: http://stackoverflow.com/questions/13533217/how-to-download-all-links-to-zip-files-on-a-given-web-page-using-wget-curl

