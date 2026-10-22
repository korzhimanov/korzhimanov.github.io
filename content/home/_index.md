+++
title = "Главная"

# This section only holds blocks for the homepage (about, teaching).
# Neither the section nor its pages get their own URLs, RSS feed or sitemap entries,
# but the pages stay in site.RegularPages so that layouts/index.html can render them.
[build]
  render = "never"
  list = "never"

[cascade.build]
  render = "never"
  list = "always"
+++
