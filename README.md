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
  
  var spec = "your-url/chart.json";
  vegaEmbed('#vis', spec);
  
</script>
</body>
</html>
```

### Reusing the Vega-Lite schema
The `chart.json` schema created with Altair contains a single dataset. You can create a reusable schema by defining a REST API endpoint containing `refinery-stats.csv` and specify this within the schema. Edit the `chart.json` by removing the top-level dataset and replacing each reference to this dataset with a url definition:
```javascript
{
  "$schema": "https://vega.github.io/schema/vega-lite/v2.6.0.json",
  "config": {"view": {"height": 300, "width": 400}},
  // Remove top-level dataset
  // "datasets": { 
      // "data-a85ba82af39c1fa11dd908a952905777": [
        // {
          //"datasets_shared": 1,
          // "datasets_uploaded": 1,
          // "groups_created": 0,
          // "run_date": "2018-08-16T00:00:00",
          // "total_user_logins": 5,
          // "total_visualization_launches": 2,
          // "total_workflow_launches": 0,
          // "unique_user_logins": 4,
          // "users_created": 18
        // }, 
        //...
      // ]
  //},
  ...
  {
  ...
    {
      // Remove reference to top-level dataset and replace with url
      // "data": {"name": "data-a85ba82af39c1fa11dd908a952905777"}, 
      "data": {"url": "api/refinery-usage.csv"},
      "encoding": {
        "x": {"field": "run_date", "title": null, "type": "temporal"},
        "y": {"field": "users_created", "title": null, "type": "quantitative"}
      },
     },
     .
     .
     .
     {
      // Remove reference to top-level dataset and replace with url
      // "data": {"name": "data-a85ba82af39c1fa11dd908a952905777"}, 
      "data": {"url": "api/refinery-usage.csv"},
      "encoding": {
        "x": {"field": "run_date", "title": null, "type": "temporal"},
        "y": {"field": "datasets_shared", "title": null, "type": "quantitative"}
      },
     },
     .
     .
     .
     // Repeat
}
```
An example of a working reusable schema is located [here](https://beta.observablehq.com/@manzt/refinery-stats).
