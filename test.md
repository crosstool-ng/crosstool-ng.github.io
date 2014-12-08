---
layout: page
title: Test page
permalink: /test/
---
# Testing markdown features 

Lorem ipsum. Will this be converted? – Yes, and ~~it’ll be served as `/test.html` (not `/test`)~~ actually, by setting `permalink` in the front matter appropriately, that’s possible too.

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

http://autolinking.com

| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |
