---
title: Network graphs in MATLAB
tags: [MATLAB, Dataviz]
style: outline
color: danger
description: An introduction to visualising connections within a network using MATLAB.
comments: true
---

Intro paragraph

## Prepare your data



## Create an adjacency matrix



## Generate the graph

Once we have created an adjacency matrix it's trivial to create the graph itself:

```matlab
datagraph = graph(adjmatrix, data.name);
```

MATLAB includes a few in-built functions for analysing the graph. For example, to get a list of which group each node belongs to:

```matlab
data.group = conncomp(datagraph)';
```

## Visualise the network



{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://mvdh.xyz/blog/matlab-network-graphs';  // Replace PAGE_URL with your page's canonical URL variable
// this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://mvdh7.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}
