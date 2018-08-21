# refinery-stats
Altair code for Refinery Stats plots.


### Description
This repo contains the [Altair](https://altair-viz.github.io/index.html) scripts for generating visuals of Refinery usage. Altair (python library) is used to generate validated [Vega-Lite](https://vega.github.io/vega-lite/) schemas that can be embeded as HTML and reused. A working web example is located [here](https://beta.observablehq.com/@manzt/refinery-stats).

### Embeding the Vega-Lite chart
The output from Altair is a Vega-Lite schema (`chart.json`). This chart can be embeded directly into HTML using [vega-embed](https://github.com/vega/vega-embed): 

```html
<!<!DOCTYPE html>
<html>
<head>
  <!-- Import Vega 4 & Vega-Lite 2 -->
  <script src="https://cdn.jsdelivr.net/npm/vega@4"></script>
  <script src="https://cdn.jsdelivr.net/npm/vega-lite@2"></script>
  <!-- Import vega-embed -->
  <script src="https://cdn.jsdelivr.net/npm/vega-embed@3"></script>
</head>
<body>

<div id="vis"></div>

<script type="text/javascript">
  
  var spec = "chart.json";
  vegaEmbed('#vis', spec);
  
</script>
</body>
</html>
```

### Create a reusable schema
You can create a reusable schema by defining a REST API endpoint for `refinery-stats.csv` and running the Jupyter Notebook again. Just replace the URL:
```python
# add API endpoint below
url = "http://your-url/refinery-usage.csv"

data = alt.UrlData(url)

# define columns at endpoint as below, and normalize the time of each entry
columns = [
    "datasets_shared",
    "datasets_uploaded",
    "groups_created",
    "total_user_logins",
    "total_visualization_launches",
    "total_workflow_launches",
    "users_created",
    "unique_user_logins",
]
```
`chart.json` will now serve as a reusable schema. An example of a working example is located [here](https://beta.observablehq.com/@manzt/refinery-stats).
