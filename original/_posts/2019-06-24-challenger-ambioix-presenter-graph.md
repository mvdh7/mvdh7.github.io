---
title: AMBIO IX conference co-authorship network
tags: [Conferences, Challenger Society, AMBIO IX]
style: outline
color: secondary
description: Abstract submissions reveal the interconnected nature of marine biogeochemical research in the UK.
comments: false
---

The [Challenger Society's](https://www.challenger-society.org.uk/) *Advances in Marine Biogeochemistry IX* conference kicks off here at UEA later today.

Although it's by no means a complete picture of the UK research community (e.g. biases towards England and ECRs), a quick graph analysis of the lists of authors for the abstracts submitted to this conference does reveal a fascinating network of collaborations in studying marine biogeochemistry across the different research institutes.

In this analysis, we say that there's a connection between two authors if their names appear on an abstract together. If person A and person B are co-authors on one abstract, and B is a co-author with C on another, then A and C are also connected via B. It's essentially a game of [six degrees of separation](https://en.wikipedia.org/wiki/Six_degrees_of_separation).

## 80% of authors are connected in a single group

That's four out of every five of the ~200 authors can be directly or indirectly connected to each via co-authorships on the submitted abstracts - a remarkable result. The remaining authors appear on independent abstracts that do not overlap with any others. The main group looks like this:

![Graph of AMBIO abstract author institutes](https://raw.githubusercontent.com/mvdh7/mvdh7.github.io/master/images/blog/AMBIOinstitutes.png "Graph of AMBIO abstract author institutes.")

*Above: connection graph for the biggest interconnected group. Each point represents a different author, coloured by their home institute, while the lines show the connections (i.e. co-authorships). City names refer to the corresponding university only, and NOC includes both centres (Southampton and Liverpool).*

There's strong representation from many of the usual suspects. Interestingly, it looks like some of these tend towards more internal collaborations, while others have scientists spread throughout the network, binding different sections of the community together. There are also lots of international connections, highlighting the global nature of the scientific questions that we investigate.

Wherever they come from, I'm looking forward to meeting everyone and learning about lots of exciting new science throughout the next few days. Let's see if we can draw some new lines on the graph!

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://mvdh.xyz/blog/challenger-ambioix-presenter-graph';  // Replace PAGE_URL with your page's canonical URL variable
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
