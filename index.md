## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/yagizolmez/uiuc_covid_data_viz/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/yagizolmez/uiuc_covid_data_viz/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

<script src="https://d3js.org/d3.v4.js"></script>
<div id = "viz"></div>

<script>

var margin = {top: 10, right: 30, bottom: 30, left: 60},
    width = 460 - margin.left - margin.right,
    height = 400 - margin.top - margin.bottom;

var svg = d3.select('#viz').append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform",
          "translate(" + margin.left + "," + margin.top + ")");

data = d3.csv("https://github.com/yagizolmez/uiuc_covid_data_viz/blob/main/illinois_covid_data.csv",
  function(d){
    return {date: d3.timeParse('%c')(d._time),
            uCases: d.undergradCases,
            uPositivity: d.undergradCases/d.undergradTests*100};
  }
);

var fall2020end = d3.timeParse('%m-%d-%Y')('12-16-2020')

fall2020data = data.filter(function(d)
{return d.date < fall2020end}
)

var x = d3.scaleTime()
  .domain(d3.extent(fall2020data,function(d){return d.date;}))
  .range([0,width]);
svg.append("g")
  .attr("transform", "translate(0," + height + ")")
  .call(d3.axisBottom(x));

var y = d3.scaleLinear()
  .domain([0,d3.max(fall2020data, function(d) {return +d.value;})])
  .range([ height, 0 ]);
svg.append("g")
  .call(d3.axisLeft(y));

svg.append("path")
  .datum(fall2020data)
  .attr("fill", "none")
  .attr("stroke", "steelblue")
  .attr("stroke-width", 1.5)
  .attr("d", d3.line()
    .x(function(d) { return x(d.date) })
    .y(function(d) { return y(d.value) })
    )

</script>


