---
title: 'Assignment 3: ggvis'
author: "Doug Raffle"
date: "April 17, 2015"
output: html_document
---

Graphically analyze using `ggvis` the 1970 Boston housing dataset. This dataset consists of 14 variables with 506 observations. The target variable, `medv`, is the median owner-occupied home value in 1000's of dollars. The Boston dataset is found in the `MASS` package. 

```r
library(MASS)
data(Boston)
Boston$chas <-factor(Boston$chas, levels = c(0,1),
                     labels = c("Not on River", "On River"))
Boston$rad <-factor(Boston$rad)
head(Boston)
```

```
##      crim zn indus         chas   nox    rm  age    dis rad tax ptratio
## 1 0.00632 18  2.31 Not on River 0.538 6.575 65.2 4.0900   1 296    15.3
## 2 0.02731  0  7.07 Not on River 0.469 6.421 78.9 4.9671   2 242    17.8
## 3 0.02729  0  7.07 Not on River 0.469 7.185 61.1 4.9671   2 242    17.8
## 4 0.03237  0  2.18 Not on River 0.458 6.998 45.8 6.0622   3 222    18.7
## 5 0.06905  0  2.18 Not on River 0.458 7.147 54.2 6.0622   3 222    18.7
## 6 0.02985  0  2.18 Not on River 0.458 6.430 58.7 6.0622   3 222    18.7
##    black lstat medv
## 1 396.90  4.98 24.0
## 2 396.90  9.14 21.6
## 3 392.83  4.03 34.7
## 4 394.63  2.94 33.4
## 5 396.90  5.33 36.2
## 6 394.12  5.21 28.7
```

The fit of a linear model to `log(medv)` using select predictors is:

```r
attach(Boston)
```

```
## The following objects are masked from Boston (pos = 6):
## 
##     age, black, chas, crim, dis, indus, lstat, medv, nox, ptratio,
##     rad, rm, tax, zn
```

```r
Boston.lm <- lm(log(medv) ~ crim + chas + rad + lstat)
summary(Boston.lm)
```

```
## 
## Call:
## lm(formula = log(medv) ~ crim + chas + rad + lstat)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.64352 -0.13733 -0.02115  0.10461  0.87218 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   3.428419   0.051028  67.187  < 2e-16 ***
## crim         -0.008905   0.001515  -5.876 7.73e-09 ***
## chasOn River  0.169177   0.039400   4.294 2.11e-05 ***
## rad2          0.214589   0.067157   3.195 0.001486 ** 
## rad3          0.204713   0.061187   3.346 0.000883 ***
## rad4          0.056412   0.054380   1.037 0.300072    
## rad5          0.167914   0.053908   3.115 0.001948 ** 
## rad6          0.091301   0.066334   1.376 0.169330    
## rad7          0.158928   0.073020   2.177 0.029990 *  
## rad8          0.216818   0.067290   3.222 0.001356 ** 
## rad24         0.094849   0.058213   1.629 0.103879    
## lstat        -0.039079   0.001667 -23.443  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.2213 on 494 degrees of freedom
## Multiple R-squared:  0.7134,	Adjusted R-squared:  0.707 
## F-statistic: 111.8 on 11 and 494 DF,  p-value: < 2.2e-16
```
All variables are highly significant.

See the variable descriptions using `help` in the `MASS` package.

In each of the following plots, **label the axes appropriately**.

1. Plot the boxplot distributions of the log of the median home values (`log_medv = log(medv)`) by the Charles River (`chas`) and separately by radial highway locations (`rad`). Draw horizontal lines at `mean(log(medv))` in each plot. Discuss each plot.


```r
library(dplyr, warn.conflicts = FALSE)
library(ggvis)
```

```r
Boston %>% ggvis(~chas, ~log(medv)) %>% layer_boxplots(fill = ~chas)
```

<!--html_preserve--><div id="plot_id755997269-container" class="ggvis-output-container">
<div id="plot_id755997269" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id755997269_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id755997269" data-renderer="svg">SVG</a>
 | 
<a id="plot_id755997269_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id755997269" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id755997269_download" class="ggvis-download" data-plot-id="plot_id755997269">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id755997269_spec = {
    "data": [
        {
            "name": ".0/group_by1/boxplot2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "min_": "number",
                    "max_": "number",
                    "lower_": "number",
                    "upper_": "number",
                    "median_": "number"
                }
            },
            "values": "\"chas\",\"min_\",\"max_\",\"lower_\",\"upper_\",\"median_\"\n\"Not on River\",2.2512917986065,3.80220813942094,2.8094026953625,3.21084365317094,3.03974915897077\n\"On River\",2.59525470695687,3.91202300542815,3.04909331770247,3.50104157915273,3.14845336057165"
        },
        {
            "name": ".0/group_by1/boxplot2",
            "source": ".0/group_by1/boxplot2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.chas"
                    ]
                }
            ]
        },
        {
            "name": ".0/group_by1/boxplot2/boxplot_outliers3_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "value_": "number"
                }
            },
            "values": "\"value_\",\"chas\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.88156379794344,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.84374416467485,\"Not on River\"\n3.87743156065853,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.8877303128591,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.8155121050473,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n2.17475172148416,\"Not on River\"\n1.97408102602201,\"Not on River\"\n2.00148000021012,\"Not on River\"\n2.14006616349627,\"Not on River\"\n1.6094379124341,\"Not on River\"\n1.84054963339749,\"Not on River\"\n1.7227665977411,\"Not on River\"\n1.97408102602201,\"Not on River\"\n2.11625551480255,\"Not on River\"\n2.14006616349627,\"Not on River\"\n1.6094379124341,\"Not on River\"\n1.94591014905531,\"Not on River\"\n1.97408102602201,\"Not on River\"\n2.01490302054226,\"Not on River\"\n2.17475172148416,\"Not on River\"\n2.12823170584927,\"Not on River\"\n2.11625551480255,\"Not on River\"\n2.16332302566054,\"Not on River\"\n2.12823170584927,\"Not on River\"\n1.94591014905531,\"Not on River\"\n2.09186406167839,\"Not on River\""
        },
        {
            "name": ".0/group_by1/boxplot2/boxplot_outliers3",
            "source": ".0/group_by1/boxplot2/boxplot_outliers3_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.chas"
                    ]
                }
            ]
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/xcenter",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n1.4943086577844\n4.02715226007785"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "padding": 0.1,
            "type": "ordinal",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "name": "x",
            "points": false,
            "sort": false,
            "range": "width"
        },
        {
            "points": true,
            "padding": 1.1,
            "name": "xcenter",
            "type": "ordinal",
            "domain": {
                "data": "scale/xcenter",
                "field": "data.domain"
            },
            "sort": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.chas"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.min_"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.max_"
                            },
                            "x": {
                                "scale": "xcenter",
                                "field": "data.chas"
                            },
                            "width": {
                                "value": 0.5
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.chas"
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.chas"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.lower_"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.upper_"
                            },
                            "width": {
                                "scale": "x",
                                "band": true
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.chas"
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.chas"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.median_"
                            },
                            "width": {
                                "scale": "x",
                                "band": true
                            },
                            "height": {
                                "value": 1
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2/boxplot_outliers3"
            },
            "marks": [
                {
                    "type": "symbol",
                    "properties": {
                        "update": {
                            "size": {
                                "value": 50
                            },
                            "fill": {
                                "value": "black"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.value_"
                            },
                            "x": {
                                "scale": "xcenter",
                                "field": "data.chas"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2/boxplot_outliers3"
                            }
                        }
                    }
                }
            ]
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "chas"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "chas"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "log(medv)"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id755997269").parseSpec(plot_id755997269_spec);
</script><!--/html_preserve-->

```r
mean.coords <- data.frame(y=mean(log(medv)), x=c(levels(Boston$chas)))

Boston %>% ggvis(~chas, ~log(medv)) %>% layer_boxplots(fill = ~chas) %>%
  layer_paths(data=mean.coords, x = ~x, y = ~y, stroke := "red", strokeWidth := 3) %>%
  scale_ordinal("x", domain = levels(chas))
```

<!--html_preserve--><div id="plot_id271446731-container" class="ggvis-output-container">
<div id="plot_id271446731" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id271446731_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id271446731" data-renderer="svg">SVG</a>
 | 
<a id="plot_id271446731_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id271446731" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id271446731_download" class="ggvis-download" data-plot-id="plot_id271446731">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id271446731_spec = {
    "data": [
        {
            "name": ".0/group_by1/boxplot2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "min_": "number",
                    "max_": "number",
                    "lower_": "number",
                    "upper_": "number",
                    "median_": "number"
                }
            },
            "values": "\"chas\",\"min_\",\"max_\",\"lower_\",\"upper_\",\"median_\"\n\"Not on River\",2.2512917986065,3.80220813942094,2.8094026953625,3.21084365317094,3.03974915897077\n\"On River\",2.59525470695687,3.91202300542815,3.04909331770247,3.50104157915273,3.14845336057165"
        },
        {
            "name": ".0/group_by1/boxplot2",
            "source": ".0/group_by1/boxplot2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.chas"
                    ]
                }
            ]
        },
        {
            "name": ".0/group_by1/boxplot2/boxplot_outliers3_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "value_": "number"
                }
            },
            "values": "\"value_\",\"chas\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.88156379794344,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.84374416467485,\"Not on River\"\n3.87743156065853,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.8877303128591,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.8155121050473,\"Not on River\"\n3.91202300542815,\"Not on River\"\n3.91202300542815,\"Not on River\"\n2.17475172148416,\"Not on River\"\n1.97408102602201,\"Not on River\"\n2.00148000021012,\"Not on River\"\n2.14006616349627,\"Not on River\"\n1.6094379124341,\"Not on River\"\n1.84054963339749,\"Not on River\"\n1.7227665977411,\"Not on River\"\n1.97408102602201,\"Not on River\"\n2.11625551480255,\"Not on River\"\n2.14006616349627,\"Not on River\"\n1.6094379124341,\"Not on River\"\n1.94591014905531,\"Not on River\"\n1.97408102602201,\"Not on River\"\n2.01490302054226,\"Not on River\"\n2.17475172148416,\"Not on River\"\n2.12823170584927,\"Not on River\"\n2.11625551480255,\"Not on River\"\n2.16332302566054,\"Not on River\"\n2.12823170584927,\"Not on River\"\n1.94591014905531,\"Not on River\"\n2.09186406167839,\"Not on River\""
        },
        {
            "name": ".0/group_by1/boxplot2/boxplot_outliers3",
            "source": ".0/group_by1/boxplot2/boxplot_outliers3_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.chas"
                    ]
                }
            ]
        },
        {
            "name": "mean.coords4",
            "format": {
                "type": "csv",
                "parse": {
                    "y": "number"
                }
            },
            "values": "\"x\",\"y\"\n\"Not on River\",3.03451287441466\n\"On River\",3.03451287441466"
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/xcenter",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n1.4943086577844\n4.02715226007785"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "padding": 0.1,
            "points": false,
            "name": "x",
            "type": "ordinal",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "sort": false,
            "range": "width"
        },
        {
            "points": true,
            "padding": 1.1,
            "name": "xcenter",
            "type": "ordinal",
            "domain": {
                "data": "scale/xcenter",
                "field": "data.domain"
            },
            "sort": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.chas"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.min_"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.max_"
                            },
                            "x": {
                                "scale": "xcenter",
                                "field": "data.chas"
                            },
                            "width": {
                                "value": 0.5
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.chas"
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.chas"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.lower_"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.upper_"
                            },
                            "width": {
                                "scale": "x",
                                "band": true
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.chas"
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.chas"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.median_"
                            },
                            "width": {
                                "scale": "x",
                                "band": true
                            },
                            "height": {
                                "value": 1
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2/boxplot_outliers3"
            },
            "marks": [
                {
                    "type": "symbol",
                    "properties": {
                        "update": {
                            "size": {
                                "value": 50
                            },
                            "fill": {
                                "value": "black"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.value_"
                            },
                            "x": {
                                "scale": "xcenter",
                                "field": "data.chas"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2/boxplot_outliers3"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "line",
            "properties": {
                "update": {
                    "x": {
                        "scale": "x",
                        "field": "data.x"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.y"
                    },
                    "stroke": {
                        "value": "red"
                    },
                    "strokeWidth": {
                        "value": 3
                    }
                },
                "ggvis": {
                    "data": {
                        "value": "mean.coords4"
                    }
                }
            },
            "from": {
                "data": "mean.coords4"
            }
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "chas"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "x"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "y"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id271446731").parseSpec(plot_id271446731_spec);
</script><!--/html_preserve-->

The distribution of the log-transformed median prices for riverfront homes is higher
than that of other properties and skewed slightly to the right.

The non-riverfront properties have more outliers, especially on the
low side, giving us a slight left skew.

Note that the functionality to add horizontal lines to boxplots
doesn't seem to be fully implemented yet.


```r
Boston %>% ggvis(~rad, ~log(medv)) %>% layer_boxplots(fill = ~rad)
```

<!--html_preserve--><div id="plot_id960040177-container" class="ggvis-output-container">
<div id="plot_id960040177" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id960040177_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id960040177" data-renderer="svg">SVG</a>
 | 
<a id="plot_id960040177_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id960040177" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id960040177_download" class="ggvis-download" data-plot-id="plot_id960040177">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id960040177_spec = {
    "data": [
        {
            "name": ".0/group_by1/boxplot2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "min_": "number",
                    "max_": "number",
                    "lower_": "number",
                    "upper_": "number",
                    "median_": "number"
                }
            },
            "values": "\"rad\",\"min_\",\"max_\",\"lower_\",\"upper_\",\"median_\"\n\"1\",2.80336038090653,3.48737507790321,3.01914826061291,3.3033679553851,3.10005170610965\n\"2\",2.75366071235426,3.7796338173824,3.06339092202781,3.50330175618843,3.17172927626409\n\"3\",2.66722820658195,3.91202300542815,3.05045507578101,3.54150409690195,3.27713761296996\n\"4\",2.54160199346455,3.50254987592244,2.86645027502161,3.16335639139334,3.01797989349727\n\"5\",2.46809953147162,3.91202300542815,2.9704144655697,3.40119182607574,3.13549421592915\n\"6\",2.82137888640921,3.21084365317094,2.93912022213536,3.13656459713591,3.05400118167797\n\"7\",2.91777073208428,3.58629286533884,3.1904763503465,3.38777436133001,3.26575941076705\n\"8\",2.77258872223978,3.91202300542815,3.17065371290387,3.49898852326139,3.3407409173295\n\"24\",1.6094379124341,3.39450839351136,2.41807586248831,2.99071973173045,2.6672040933462"
        },
        {
            "name": ".0/group_by1/boxplot2",
            "source": ".0/group_by1/boxplot2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.rad"
                    ]
                }
            ]
        },
        {
            "name": ".0/group_by1/boxplot2/boxplot_outliers3_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "value_": "number"
                }
            },
            "values": "\"value_\",\"rad\"\n3.91202300542815,\"1\"\n2.47653840011748,\"1\"\n3.91202300542815,\"4\"\n3.88156379794344,\"4\"\n3.91202300542815,\"4\"\n3.61899332664977,\"4\"\n1.94591014905531,\"4\"\n2.09186406167839,\"4\"\n2.86789890204411,\"7\"\n3.75653810258775,\"7\"\n3.91202300542815,\"24\"\n3.91202300542815,\"24\"\n3.91202300542815,\"24\"\n3.91202300542815,\"24\"\n3.91202300542815,\"24\""
        },
        {
            "name": ".0/group_by1/boxplot2/boxplot_outliers3",
            "source": ".0/group_by1/boxplot2/boxplot_outliers3_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.rad"
                    ]
                }
            ]
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"1\"\n\"2\"\n\"3\"\n\"4\"\n\"5\"\n\"6\"\n\"7\"\n\"8\"\n\"24\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"1\"\n\"2\"\n\"3\"\n\"4\"\n\"5\"\n\"6\"\n\"7\"\n\"8\"\n\"24\""
        },
        {
            "name": "scale/xcenter",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"1\"\n\"2\"\n\"3\"\n\"4\"\n\"5\"\n\"6\"\n\"7\"\n\"8\"\n\"24\""
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n1.4943086577844\n4.02715226007785"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "padding": 0.1,
            "type": "ordinal",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "name": "x",
            "points": false,
            "sort": false,
            "range": "width"
        },
        {
            "points": true,
            "padding": 1.1,
            "name": "xcenter",
            "type": "ordinal",
            "domain": {
                "data": "scale/xcenter",
                "field": "data.domain"
            },
            "sort": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.rad"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.min_"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.max_"
                            },
                            "x": {
                                "scale": "xcenter",
                                "field": "data.rad"
                            },
                            "width": {
                                "value": 0.5
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.rad"
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.rad"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.lower_"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.upper_"
                            },
                            "width": {
                                "scale": "x",
                                "band": true
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2"
            },
            "marks": [
                {
                    "type": "rect",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.rad"
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.rad"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.median_"
                            },
                            "width": {
                                "scale": "x",
                                "band": true
                            },
                            "height": {
                                "value": 1
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/boxplot2/boxplot_outliers3"
            },
            "marks": [
                {
                    "type": "symbol",
                    "properties": {
                        "update": {
                            "size": {
                                "value": 50
                            },
                            "fill": {
                                "value": "black"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.value_"
                            },
                            "x": {
                                "scale": "xcenter",
                                "field": "data.rad"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/boxplot2/boxplot_outliers3"
                            }
                        }
                    }
                }
            ]
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "rad"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "rad"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "log(medv)"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id960040177").parseSpec(plot_id960040177_spec);
</script><!--/html_preserve-->

Increasing the radial highway index
generally increases the median home value.  However, the
boxplots now reveal a more subtle pattern -- there are two distinct
increasing trends: from 1 to 3 and from 4 to 8. If the two groups are
considered as a whole, we see no overall trend. 

We also see that the distribution of median home values is lowest for
homes with an index of 24.  This group also has the most variability.
Again, this may point to median house prices in towns in a city center
being confounded with income.

2. Explore graphically the relationship of log crime (`log_crim = log(crim)`) to the log of the median home values (`log_medv`). Add the linear model fit and a smoothed fit with their standard error bands. Do you see any difficulties in using `log_crim` as a predictor? 

```r
Boston <- Boston %>%
    transform(log_crim = log(crim)) %>%
    transform(log_medv = log(medv))
```

```r
Boston %>% ggvis(~log_crim, ~log_medv) %>%
    layer_smooths(se=TRUE, stroke := "blue") %>%
    layer_model_predictions(model="lm", se=TRUE, stroke := "red") %>%
    layer_points()
```

```
## Guessing formula = log_medv ~ log_crim
```

<!--html_preserve--><div id="plot_id469169672-container" class="ggvis-output-container">
<div id="plot_id469169672" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id469169672_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id469169672" data-renderer="svg">SVG</a>
 | 
<a id="plot_id469169672_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id469169672" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id469169672_download" class="ggvis-download" data-plot-id="plot_id469169672">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id469169672_spec = {
    "data": [
        {
            "name": ".0",
            "format": {
                "type": "csv",
                "parse": {
                    "log_crim": "number",
                    "log_medv": "number"
                }
            },
            "values": "\"log_crim\",\"log_medv\"\n-5.06403607082337,3.17805383034795\n-3.60050234349652,3.07269331469012\n-3.60123494426189,3.54673968695281\n-3.43052321104398,3.50855589998265\n-2.67292439912684,3.58905911883173\n-3.51157043914353,3.35689712276558\n-2.42712842806865,3.13113691056019\n-1.93412981051978,3.29953372788566\n-1.5547603511434,2.80336038090653\n-1.77172157549155,2.9391619220656\n-1.49214388521174,2.70805020110221\n-2.14157229714634,2.9391619220656\n-2.3668036653207,3.07731226054641\n-0.462416484558303,3.01553490085017\n-0.449479693527584,2.90142159408275\n-0.46618692214761,2.99071973173045\n0.0525260342514466,3.13983261752775\n-0.243091189123906,2.86220088092947\n-0.219761775967802,3.00568260440716\n-0.320480784203167,2.90142159408275\n0.224574526979991,2.61006979274201\n-0.160121804898053,2.97552956623647\n0.209020285867676,2.72129542785223\n-0.0116374532441208,2.67414864942653\n-0.287335465860119,2.74727091425549\n-0.17371073654606,2.63188884013665\n-0.397630875999648,2.8094026953625\n-0.0452379806501943,2.69462718077007\n-0.257489167089002,2.91235066461494\n0.00244700364305185,3.04452243772342\n0.122934190094978,2.54160199346455\n0.30359479091183,2.67414864942653\n0.327856657447708,2.58021682959233\n0.141256477175355,2.57261223020711\n0.477984199611674,2.60268968544438\n-2.74621946721971,2.9391619220656\n-2.32851847502055,2.99573227355399\n-2.52398017377414,3.04452243772342\n-1.74268363158146,3.20680324363393\n-3.58885314004681,3.42751468997953\n-3.39352687535794,3.55248682920838\n-2.06010961338034,3.28091121578765\n-1.95545556189884,3.23080439573347\n-1.83658948514585,3.20680324363393\n-2.09809443017905,3.05400118167797\n-1.7636385935114,2.96010509591084\n-1.66940025360678,2.99573227355399\n-1.47285493064757,2.8094026953625\n-1.37093295400719,2.66722820658195\n-1.51517373404402,2.96527306606928\n-2.42215722813489,2.98061863574394\n-3.13798732113486,3.02042488614436\n-2.92620621090538,3.2188758248682\n-2.99953951189695,3.15273602236366\n-4.29768548624013,2.9391619220656\n-4.33437998120653,3.56671182013973\n-3.88489433803989,3.20680324363393\n-4.24609811744964,3.45315712059287\n-1.86788485961755,3.14845336057165\n-2.27031153244375,2.97552956623647\n-1.90166362493575,2.92852352386054\n-1.76194827165643,2.77258872223978\n-2.20482337521155,3.10009228887823\n-2.06751297081456,3.2188758248682\n-3.93682812434712,3.49650756146648\n-3.32869069087541,3.15700042115011\n-3.12834979816883,2.96527306606928\n-2.84921062089122,3.09104245335832\n-1.99848847927417,2.85647020622048\n-2.05447579566209,3.03974915897077\n-2.42746827514074,3.18635263316264\n-1.84036165106727,3.07731226054641\n-2.38988742139684,3.1267605359604\n-1.63275771775572,3.15273602236366\n-2.53881388385691,3.18221184049661\n-2.35261602659961,3.06339092202781\n-2.28740095766901,2.99573227355399\n-2.44104288614161,3.03495298670727\n-2.87422285615679,3.05400118167797\n-2.47848729798582,3.01062088604774\n-3.1910174967398,3.3322045101752\n-3.10957308997775,3.17387845893747\n-3.30798029995102,3.21084365317094\n-3.33794093202714,3.13113691056019\n-2.98400135067829,3.17387845893747\n-2.85858243540676,3.28091121578765\n-2.95882191953389,3.11351530921037\n-2.63791797892183,3.10009228887823\n-2.871746293773,3.16124671203156\n-2.93708607812126,3.35689712276558\n-3.06101774025262,3.11794990627824\n-3.23602198370317,3.09104245335832\n-3.16937162996511,3.13113691056019\n-3.54911751173878,3.2188758248682\n-3.1479514865315,3.02529107579554\n-2.10340641913367,3.34638914516716\n-2.1624753850094,3.06339092202781\n-2.11337067994292,3.65583960003574\n-2.50262265559378,3.7796338173824\n-2.6794627442503,3.50254987592244\n-1.90609345968499,3.31418600467253\n-2.16875374536053,3.27714473299218\n-1.47508185993502,2.92316158071916\n-1.55301032113546,2.96010509591084\n-1.96897408865386,3.00071981506503\n-2.02026738304142,2.9704144655697\n-1.7649228152745,2.9704144655697\n-2.03126108715508,3.01553490085017\n-2.05556877726828,2.98568193770049\n-1.3332086740283,2.96527306606928\n-2.22627241014488,3.07731226054641\n-2.29422017666242,3.1267605359604\n-2.09321597510168,2.9338568698359\n-1.50453750260873,2.92852352386054\n-1.94974750228657,2.91777073208428\n-1.76410539244624,2.90690105984738\n-2.02814024732429,3.05400118167797\n-1.89060790127066,2.95491027903374\n-2.03576921322365,3.01553490085017\n-1.9326780802866,2.96010509591084\n-2.67379371242412,3.09104245335832\n-2.63596212470796,3.01062088604774\n-2.37526331849203,3.02042488614436\n-1.89458985503226,2.85070650150373\n-2.31780025880053,2.9338568698359\n-1.7777382278658,3.06339092202781\n-0.948426601904225,2.75366071235426\n-1.35034823434642,2.78501124223834\n-1.12260789422433,2.89037175789616\n-0.126413924855659,2.66025953726586\n-1.0786332063528,2.95491027903374\n0.176420848472986,2.97552956623647\n-0.527547999910379,3.13549421592915\n-1.10920822788151,2.91235066461494\n-0.0241185274088078,2.74727091425549\n-0.583790659576773,2.89591193827178\n-1.13121812841702,2.85647020622048\n-1.0431870425627,2.83907846350861\n-1.38709468129066,2.58776403522771\n-0.607850606337865,2.87919845729804\n-1.23477571348098,2.63905732961526\n0.487745310721893,2.66722820658195\n1.20028099798739,2.59525470695687\n1.41035262621296,2.74727091425549\n1.02235739814894,2.46809953147162\n0.866823138301229,2.62466859216316\n0.767813925142701,2.74727091425549\n0.862307507076077,2.68102152871429\n0.846293070040128,2.87919845729804\n1.00575476530812,2.73436750941958\n0.504767309182027,3.06805293513362\n0.403008760421457,2.97552956623647\n0.119186494791163,2.72785282839839\n0.76508637404103,2.96527306606928\n0.346316479810509,2.83321334405622\n1.26271612819885,2.74727091425549\n0.894732003534746,2.57261223020711\n0.201780987950174,3.72086249996699\n0.294786774181713,3.1904763503465\n0.354185848709842,3.14845336057165\n0.241737605442712,3.29583686600433\n0.380735161487553,3.91202300542815\n0.606373957027711,3.91202300542815\n0.418065390083903,3.91202300542815\n0.807528882678661,3.12236492448736\n1.07295254188753,3.2188758248682\n0.698229244966739,3.91202300542815\n0.587942208360164,3.16968558067743\n0.833083020857462,3.16968558067743\n0.895896169418922,3.10458667846607\n0.188485851761799,2.85647020622048\n0.838934412625926,2.94968833505258\n-1.97227465848664,3.13983261752775\n-2.38836087001545,3.16124671203156\n-2.47135883724273,3.11794990627824\n-2.70845028112355,3.38099467434464\n-2.65612210824185,3.14415227867226\n-2.91415228656157,3.20274644293832\n-2.71175706303354,3.39785848039664\n-2.85076650330381,3.6163087612791\n-2.7199203736727,3.68386691229039\n-2.67538941886266,3.58905911883173\n-2.39656615646494,3.63495111208838\n-2.30178541282348,3.48124008933569\n-2.48795127997423,3.27336401015227\n-2.80560790469702,3.38777436133001\n-2.88204650915017,3.91202300542815\n-2.54147700127639,3.46573590279973\n-2.07314142913136,3.39450839351136\n-2.48051630148671,3.55248682920838\n-2.40041845334281,3.61091791264422\n-2.67205584087883,3.41772668361337\n-2.4459936762894,3.5945687746427\n-3.82263944429346,3.43720781918519\n-4.24122175808286,3.37073817417745\n-4.28236231156094,3.91202300542815\n-3.21612959920018,3.5055573969864\n-3.06486801238885,3.41114771251532\n-3.27862582927397,3.54385368206368\n-3.45776773315055,3.55248682920838\n-4.02968104889638,3.49347265777133\n-3.36824628152247,3.18221184049661\n-3.82722240373568,3.74478708605223\n-3.34955414851032,3.88156379794344\n-3.90753310015529,3.91202300542815\n-1.99201691675556,3.11794990627824\n-1.47102470528047,3.19458313229916\n-1.37836587479777,3.11351530921037\n-1.9960567327459,3.19458313229916\n-0.830778394549941,2.99573227355399\n-1.74605978997706,3.07731226054641\n-0.978751413216761,2.96010509591084\n-1.52698273249791,3.10906095886099\n-1.96240545158451,3.3357695763397\n-1.23942728531034,3.16547504814109\n-1.61938724328777,3.2188758248682\n-3.0878475624618,3.14845336057165\n-2.65740461643332,3.35689712276558\n-2.2010217775846,3.06805293513362\n-2.16936624920782,3.13549421592915\n-1.02697092752823,3.2846635654062\n-0.897199141618634,3.07731226054641\n-0.472310287537657,3.31418600467253\n-0.486620935069173,3.40452517175483\n-1.15413556947876,3.80220813942094\n-0.640687566587583,3.91202300542815\n-0.961968245370808,3.62700405039585\n-0.885810024620568,3.45315712059287\n-1.21002441175437,3.84374416467485\n-0.816943258373457,3.44998754583159\n-0.621757184473272,3.1904763503465\n-0.770114621716554,3.45631668088323\n-0.552881017499318,3.73050112880476\n-1.10421797118894,3.87743156065853\n-0.803162959605968,3.36729582998647\n-1.10729991706568,3.17805383034795\n-0.652811704370542,3.22286784613774\n-0.669762740327209,3.44998754583159\n-2.49568452295988,3.16547504814109\n-2.3803304416189,3.14845336057165\n-2.17780437609674,3.09104245335832\n-2.2431847497126,3.00071981506503\n-2.27399763614213,3.10009228887823\n-2.05909004543194,3.16547504814109\n-1.57949083606615,2.86789890204411\n-1.65375559308523,2.91777073208428\n-1.07930978641361,3.1904763503465\n-1.62673667701243,3.02042488614436\n-1.80551362546072,3.19867311755068\n-1.6568964635938,3.26575941076705\n-1.96397229187372,3.19458313229916\n-1.54135879162351,3.21084365317094\n-2.4984783298181,3.38777436133001\n-0.997121249788704,3.75653810258775\n-3.0326037483299,3.08648663682246\n-3.33878612154076,3.03974915897077\n-4.17468731490464,3.78418963391826\n-0.49177491307519,3.91202300542815\n-0.410211353733397,3.58351893845611\n-0.420604126950968,3.40452517175483\n-0.615982456464896,3.52046080248897\n-0.627134746166374,3.7635229971097\n-0.653657272873533,3.8877303128591\n-0.192056790782112,3.43398720448515\n-0.597709736126834,3.59731226058845\n-0.272307535345581,3.1267605359604\n-0.241180238800361,3.42426265459315\n-0.547593347958205,3.91202300542815\n-0.615260641902874,3.77276093809464\n-2.40074934178128,3.03013370027132\n-1.20677673165867,3.04927304048202\n-1.81948016182866,3.22684399451738\n-2.1663074747015,3.19458313229916\n-1.5056185837951,3.56104608260405\n-2.87457715199752,3.47815842279828\n-2.34299050762908,3.46573590279973\n-2.25675167665087,3.50254987592244\n-2.79246495224457,3.49953328238302\n-2.52848243250488,3.37073817417745\n-1.55883985967101,3.55820113047182\n-3.33036620090156,3.8155121050473\n-3.29548692724004,3.56671182013973\n-2.7921385814845,3.8286413964891\n-4.19903863333677,3.91202300542815\n-4.70388615892725,3.47196645255036\n-4.51350299746227,3.09104245335832\n-3.92967794066687,3.00071981506503\n-3.25165731439258,3.14415227867226\n-3.08129016191564,3.10458667846607\n-3.14725308119523,3.21084365317094\n-3.35183595212443,3.3499040872746\n-2.54008115053266,3.61899332664977\n-3.32007833037736,3.32862668882732\n-2.4931404547151,3.17387845893747\n-2.50115799037405,3.07731226054641\n-2.0454653261249,3.35340671782581\n-2.9239699073271,3.29953372788566\n-1.95878264527799,3.01062088604774\n-2.73861250668485,3.11351530921037\n-2.88939223778266,3.36729582998647\n-3.1197094533737,3.21084365317094\n-3.34189127576475,3.09104245335832\n-2.37881839899363,3.27336401015227\n-2.30258509299405,3.49953328238302\n-2.89769853328263,3.58629286533884\n-2.90424758343181,3.34638914516716\n-2.5898672454245,3.50855589998265\n-3.00942560068599,3.33932197794407\n-0.707286673713667,3.1267605359604\n-1.05153788128218,3.01062088604774\n0.969065328591483,2.77881927199042\n-0.23520348080665,3.09557760852371\n-1.3405946818689,2.96527306606928\n-1.31163225681147,3.07269331469012\n-0.99641677635344,3.16968558067743\n-1.37215479756617,2.78501124223834\n-1.14485519984285,2.87919845729804\n-1.4055995121779,2.98568193770049\n-0.911253440356887,3.13983261752775\n-0.743451490469693,3.04452243772342\n-1.78617509093415,3.16968558067743\n-1.70600388041043,3.13983261752775\n-1.04657027464107,3.01553490085017\n-1.2590627706439,2.91777073208428\n-1.07560890690315,3.2188758248682\n-1.65098933959234,3.20274644293832\n-1.19247252015573,3.13549421592915\n-1.42283387191084,3.10009228887823\n-2.71552809095817,2.96010509591084\n-2.69948697044172,3.11794990627824\n-3.09136250456924,2.98568193770049\n-2.99114282122018,2.83907846350861\n-3.36216899469468,2.96527306606928\n-2.97926854752333,3.10009228887823\n-3.28661947695472,3.03013370027132\n-3.22867366734831,3.04927304048202\n-3.37348494309562,2.9704144655697\n-3.49298377729286,2.91777073208428\n-3.40943118658926,3.02529107579554\n-2.90096769710957,2.94443897916644\n-2.78855551576186,2.92852352386054\n-4.34203698645772,3.48737507790321\n-3.68967977428471,2.80336038090653\n-3.67182569954811,3.17387845893747\n-3.49035651798197,3.44041809481544\n-3.46958329452862,2.86220088092947\n-2.78676878581361,2.84490938381941\n-3.9792317551216,3.13983261752775\n-4.19903863333677,3.19867311755068\n-3.54080433604857,3.28091121578765\n-2.77884827241093,3.13113691056019\n-2.53199825732185,3.18221184049661\n-2.62499664596692,2.92316158071916\n-4.06926178185464,3.40452517175483\n-3.14632263218649,2.90142159408275\n-2.23876558028109,3.02529107579554\n2.19532944938176,2.87919845729804\n1.34799522318569,3.07731226054641\n1.64899895228507,3.12236492448736\n1.44957662474632,3.11794990627824\n1.51334983014208,3.2188758248682\n1.34464911115118,2.99071973173045\n1.30242893951019,3.03495298670727\n1.44040131844278,2.82137888640921\n1.24538726330446,3.08648663682246\n1.51641651137626,3.31418600467253\n1.30750815538373,3.08648663682246\n2.60433277927054,3.13983261752775\n1.58887187381363,3.91202300542815\n1.73518559039658,3.91202300542815\n1.87774754504581,3.91202300542815\n2.22270820490502,3.91202300542815\n2.1123019265294,3.91202300542815\n2.40767457192474,2.62466859216316\n2.91767343005329,2.62466859216316\n2.97599374420349,2.70805020110221\n2.72706820693797,2.63188884013665\n2.28477645637644,2.58776403522771\n3.16328700210486,2.57261223020711\n2.88293864507855,2.32238772029023\n4.48836891823984,2.34180580614733\n2.76470774878891,2.3887627892351\n2.21779161827618,2.42480272571829\n2.07850109960278,2.50959926237837\n2.99996828895892,2.17475172148416\n2.82208102080754,1.97408102602201\n3.19432900165004,2.35137525716348\n3.11782157946064,2.00148000021012\n2.6626134080936,2.32238772029023\n2.09823140139806,2.4423470353692\n1.94048833469004,2.71469474382088\n1.66639463926993,3.14415227867226\n2.44909810854921,2.27212588550934\n2.15695335703792,2.62466859216316\n2.59225019793657,2.54160199346455\n2.16524646202657,2.57261223020711\n1.77020380626234,2.52572864430826\n2.03757994445991,2.14006616349627\n3.64680146282653,1.6094379124341\n2.29420507854844,1.84054963339749\n3.22071812678739,1.7227665977411\n2.6557880164394,1.97408102602201\n2.2613161235954,2.4932054526027\n3.2109121992087,2.11625551480255\n3.72639679427388,2.14006616349627\n4.21834232049674,1.6094379124341\n3.03091600288847,2.47653840011748\n2.48082332435036,3.32862668882732\n2.00200553776674,2.84490938381941\n2.66988439800228,3.31418600467253\n3.93448483899724,2.70805020110221\n2.64267221660273,2.84490938381941\n2.93444180511088,2.88480071284671\n3.35535586595285,2.79116510781272\n3.82310654218625,1.94591014905531\n2.8950607473823,1.97408102602201\n2.38270779746775,2.01490302054226\n3.25580930892149,2.34180580614733\n4.29774924420755,2.17475172148416\n2.4691413614596,2.12823170584927\n2.4058093284293,2.81540871942271\n1.94913209586287,2.65324196460721\n2.48891527118536,3.03495298670727\n1.95308718951777,2.59525470695687\n2.17385586577979,2.45958884180371\n2.76381913153852,2.11625551480255\n2.50529733943573,2.32238772029023\n3.62864897336371,2.3887627892351\n1.99702549904027,2.39789527279837\n2.2341874014952,2.2512917986065\n2.13913985224951,2.67414864942653\n2.30879576677076,2.64617479738412\n1.86315722444043,2.77881927199042\n1.71938051428274,2.66025953726586\n2.63285240453631,2.45958884180371\n2.41237179860475,2.59525470695687\n2.66867160882001,2.26176309847379\n2.71979430172596,2.16332302566054\n2.61579601365956,2.12823170584927\n2.23971238362132,2.54944517092557\n3.09336248726987,2.35137525716348\n2.27461556718342,2.83907846350861\n1.73454870107647,2.91235066461494\n2.29923348261767,2.73436750941958\n2.54962484228371,2.37954613413017\n2.36760474836797,2.46809953147162\n1.83865418738046,2.70136121295141\n2.29504171310892,2.53369681395743\n2.23313747526855,2.64617479738412\n2.01836502089744,2.56494935746154\n1.90474881125035,2.59525470695687\n1.693988597737,2.72129542785223\n1.62731122882592,2.77881927199042\n2.1099816583913,2.87919845729804\n2.25272550719709,2.70136121295141\n1.55864344098212,2.64617479738412\n1.54090850495968,2.54160199346455\n2.10420488347616,2.60268968544438\n2.04798054391097,2.70136121295141\n1.91709465620517,2.99573227355399\n1.57113981354136,2.79728133483015\n1.30646892150861,2.87356463957978\n1.89535643073801,2.9704144655697\n1.76149783672584,3.00568260440716\n2.05915209590677,3.06339092202781\n1.15171061966313,2.99071973173045\n1.32839508467371,2.94443897916644\n1.48665540019546,2.94968833505258\n2.7457120074838,2.94968833505258\n2.5707096581053,3.00071981506503\n1.46989764548713,2.99071973173045\n1.39585105014985,2.97552956623647\n1.27219577951878,3.14415227867226\n1.53619817863696,3.39450839351136\n2.08639108754919,2.62466859216316\n1.85522241213869,2.58776403522771\n1.58338342291018,2.81540871942271\n2.7096089855662,2.484906649788\n2.32561779210462,2.68102152871429\n2.6626134080936,3.06339092202781\n1.76198902792588,3.13549421592915\n1.74190023380554,3.16547504814109\n1.74591795351875,3.2188758248682\n1.0361622517949,3.08190996979504\n0.866499466770358,3.02529107579554\n1.30119116239956,3.05400118167797\n1.73901775796999,2.94968833505258\n1.57601969221081,3.02529107579554\n-1.89140302455665,2.72129542785223\n-1.69624930942106,1.94591014905531\n-1.57281672897846,2.09186406167839\n-2.24677202817483,2.61006979274201\n-2.19534634232445,3.00071981506503\n-1.75267338052085,3.08190996979504\n-1.27450257051646,3.19867311755068\n-1.72042534062373,3.13983261752775\n-1.23925461847058,2.98061863574394\n-1.31535139230933,2.90690105984738\n-1.43078976100645,3.05400118167797\n-1.72692724122657,2.86220088092947\n-1.49441423586532,2.82137888640921\n-2.77051088244482,3.10906095886099\n-3.09511071753427,3.02529107579554\n-2.80082360125457,3.17387845893747\n-2.21100914950684,3.09104245335832\n-3.04892210206811,2.47653840011748"
        },
        {
            "name": ".0/model_prediction1",
            "format": {
                "type": "csv",
                "parse": {
                    "resp_upr_": "number",
                    "pred_": "number",
                    "resp_lwr_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"resp_upr_\",\"pred_\",\"resp_lwr_\",\"resp_\"\n3.7806064356125,-5.06403607082337,3.35422754951318,3.56741699256284\n3.73707568297206,-4.94311955197447,3.34483891598169,3.54095729947688\n3.6951144662983,-4.82220303312557,3.33548230817185,3.51529838723508\n3.65473276750402,-4.70128651427667,3.32612889563811,3.49043083157107\n3.61594252143202,-4.58036999542777,3.31674789500498,3.4663452082185\n3.5787579789929,-4.45945347657886,3.3073062068291,3.443032092911\n3.54319610150206,-4.33853695772996,3.29776802126239,3.42048206138222\n3.50927693457432,-4.21762043888106,3.28809444415727,3.3986856893658\n3.47702384759146,-4.09670392003216,3.27824325759927,3.37763355259536\n3.44646341538854,-3.97578740118326,3.26816903822058,3.35731622680456\n3.4176245456191,-3.85487088233436,3.25782402983496,3.33772428772703\n3.39053622454593,-3.73395436348546,3.2471603976469,3.31884831109642\n3.36522305031127,-3.61303784463655,3.23613469498143,3.30067887264635\n3.34169779766001,-3.49212132578765,3.22471529856094,3.28320654811047\n3.31995106586949,-3.37120480693875,3.21289276057537,3.26642191322243\n3.29993998152583,-3.25028828808985,3.20069110590587,3.25031554371585\n3.28158045052604,-3.12937176924095,3.18817558012272,3.23487801532438\n3.26479887743561,-3.00845525039205,3.17551340222973,3.22015613983267\n3.24955101764312,-2.88753873154315,3.16294092608628,3.2062459718647\n3.23571118134822,-2.76662221269424,3.15058008703311,3.19314563419066\n3.22315520203014,-2.64570569384534,3.13854703369618,3.18085111786316\n3.21176260614214,-2.52478917499644,3.12695422172744,3.16935841393479\n3.20149474134077,-2.40387265614754,3.11611708854844,3.15880591494461\n3.19245194127243,-2.28295613729864,3.10629277542598,3.14937235834921\n3.18455691876805,-2.16203961844974,3.09717770395878,3.14086731136341\n3.17749563078607,-2.04112309960084,3.08868575099371,3.13309069088989\n3.17121817975461,-1.92020658075193,3.0813027912737,3.12626048551416\n3.16706977669528,-1.79929006190303,3.0758760230854,3.12147289989034\n3.16486948749359,-1.67837354305413,3.07131735668558,3.11809342208959\n3.16378723701879,-1.55745702420523,3.06686508494175,3.11532616098027\n3.16239762810727,-1.43654050535633,3.06235282275422,3.11237522543075\n3.15958547280816,-1.31562398650743,3.05833065823049,3.10895806551932\n3.15855876338465,-1.19470746765853,3.05727294544773,3.10791585441619\n3.15994420218618,-1.07379094880962,3.05822160524576,3.10908290371597\n3.16343196814615,-0.952874429960723,3.05970096002661,3.11156646408638\n3.16832242686941,-0.831957911111821,3.06062514552082,3.11447378619512\n3.17344536472427,-0.71104139226292,3.06037887669553,3.1169121207099\n3.17727714489066,-0.590124873414019,3.05870029170621,3.11798871829843\n3.17814407688572,-0.469208354565117,3.05547758237113,3.11681082962842\n3.17481107972506,-0.348291835716215,3.05079264909213,3.1128018644086\n3.16987370459483,-0.227375316867314,3.04634038054425,3.10810704256954\n3.16408775113584,-0.106458798018413,3.04217421762866,3.10313098438225\n3.157456291145,0.0144577208304888,3.0378479434403,3.09765211729265\n3.14998624041825,0.135374239679391,3.03291149707502,3.09144886874664\n3.14166631579242,0.256290758528292,3.02693301658782,3.08429966619012\n3.13244227150965,0.377207277377193,3.01952360262836,3.07598293706901\n3.12219268014869,0.498123796226094,3.01036153750972,3.06627710882921\n3.11071207230203,0.619040315074996,2.9992091455312,3.05496060891662\n3.09770819096471,0.739956833923897,2.98591553858961,3.04181186477716\n3.08281538562388,0.860873352772798,2.97040322208956,3.02660930385672\n3.06562004003501,0.9817898716217,2.95264266716745,3.00913135360123\n3.04569132358063,1.1027063904706,2.93262155933253,2.98915644145658\n3.02261310172505,1.2236229093195,2.91031288801231,2.96646299486868\n2.99611832007812,1.3445394281684,2.8856749213591,2.94089662071861\n2.96728327295379,1.46545594701731,2.8590415275451,2.91316240024945\n2.93671860534424,1.58637246586621,2.83057363656758,2.88364612095591\n2.90471344316316,1.70728898471511,2.80033984532247,2.85252664424282\n2.87153512084552,1.82820550356401,2.76843054218442,2.81998283151497\n2.83742745780775,1.94912202241291,2.73495963054664,2.78619354417719\n2.80261895846508,2.07003854126181,2.7000563288035,2.75133764363429\n2.7673415973823,2.19095506011071,2.66384638519984,2.71559399129107\n2.73175412604387,2.31187157895962,2.62627754074142,2.67901583339264\n2.69571803291769,2.43278809780852,2.58681010613231,2.641264069525\n2.65931838638151,2.55370461665742,2.54526904883151,2.60229371760651\n2.62263534459268,2.67462113550632,2.50154858314564,2.56209196386916\n2.5857103442649,2.79553765435522,2.45558164482497,2.52064599454494\n2.54854735288822,2.91645417320412,2.40733863884341,2.47794299586582\n2.51112076047946,3.03737069205302,2.3568195476481,2.43397015406378\n2.47338574850068,3.15828721090193,2.30404356224095,2.38871465537081\n2.43528769819324,3.27920372975083,2.24903967384454,2.34216368601889\n2.39676903366314,3.40012024859973,2.19183983081686,2.29430443224\n2.3577733981212,3.52103676744863,2.13247476241106,2.24512408026613\n2.31824777096378,3.64195328629753,2.07097186169471,2.19460981632925\n2.27814325136098,3.76286980514643,2.0073544019617,2.14274882666134\n2.23741508940702,3.88378632399533,1.94164150558177,2.08952829749439\n2.19602235098094,4.00470284284424,1.87384847913983,2.03493541506039\n2.15392744356047,4.12561936169314,1.80398728762213,1.9789573655913\n2.11109562362209,4.24653588054204,1.73206704701615,1.92158133531912\n2.0674945423718,4.36745239939094,1.65809447857984,1.86279451047582\n2.02309385131839,4.48836891823984,1.58207430326839,1.80258407729339"
        },
        {
            "name": ".0/model_prediction2",
            "format": {
                "type": "csv",
                "parse": {
                    "resp_upr_": "number",
                    "pred_": "number",
                    "resp_lwr_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"resp_upr_\",\"pred_\",\"resp_lwr_\",\"resp_\"\n3.559265990255,-5.06403607082337,3.42852942216199,3.4938977062085\n3.54483171773819,-4.94311955197447,3.41702886548998,3.48093029161409\n3.53040646075323,-4.82220303312557,3.40551929328613,3.46796287701968\n3.51599086574034,-4.70128651427667,3.39400005911022,3.45499546242528\n3.50158563751454,-4.58036999542777,3.38247045814719,3.44202804783087\n3.48719154531924,-4.45945347657886,3.37092972115369,3.42906063323646\n3.47280942953017,-4.33853695772996,3.35937700775394,3.41609321864205\n3.45844020906768,-4.21762043888106,3.34781139902762,3.40312580404765\n3.44408488957348,-4.09670392003216,3.336231889333,3.39015838945324\n3.42974457240458,-3.97578740118326,3.32463737731309,3.37719097485883\n3.41542046448789,-3.85487088233436,3.31302665604096,3.36422356026443\n3.40111388906477,-3.73395436348546,3.30139840227527,3.35125614567002\n3.38682629733157,-3.61303784463655,3.28975116481966,3.33828873107561\n3.37255928094803,-3.49212132578765,3.27808335201438,3.32532131648121\n3.35831458533659,-3.37120480693875,3.26639321843701,3.3123539018868\n3.34409412362734,-3.25028828808985,3.25467885095744,3.29938648729239\n3.32989999101198,-3.12937176924095,3.24293815438399,3.28641907269799\n3.3157344791481,-3.00845525039205,3.23116883705906,3.27345165810358\n3.30160009009928,-2.88753873154315,3.21936839691907,3.26048424350917\n3.28749954910041,-2.76662221269424,3.20753410872912,3.24751682891476\n3.27343581519971,-2.64570569384534,3.195663013441,3.23454941432036\n3.25941208855139,-2.52478917499644,3.18375191090052,3.22158199972595\n3.24543181282477,-2.40387265614754,3.17179735743832,3.20861458513154\n3.23149867087915,-2.28295613729864,3.15979567019512,3.19564717053714\n3.21761657156488,-2.16203961844974,3.14774294032058,3.18267975594273\n3.20378962530908,-2.04112309960084,3.13563505738756,3.16971234134832\n3.19002210610783,-1.92020658075193,3.1234677474,3.15674492675392\n3.17631839777511,-1.79929006190303,3.11123662654391,3.14377751215951\n3.16268292289995,-1.67837354305413,3.09893727223025,3.1308100975651\n3.14912005402851,-1.55745702420523,3.08656531191288,3.1178426829707\n3.13563400815923,-1.43654050535633,3.07411652859334,3.10487526837629\n3.12222872766549,-1.31562398650743,3.06158697989827,3.09190785378188\n3.10890775305409,-1.19470746765853,3.04897312532086,3.07894043918747\n3.09567409519631,-1.07379094880962,3.03627195398983,3.06597302459307\n3.08253011637661,-0.952874429960723,3.02348110362071,3.05300560999866\n3.06947743021136,-0.831957911111821,3.01059896059715,3.04003819540425\n3.05651682982289,-0.71104139226292,2.9976247317968,3.02707078080985\n3.04364825149553,-0.590124873414019,2.98455848093535,3.01410336621544\n3.03087077762073,-0.469208354565117,2.97140112562134,3.00113595162103\n3.01818267861913,-0.348291835716215,2.95815439543412,2.98816853702663\n3.00558148946624,-0.227375316867314,2.9448207553982,2.97520112243222\n2.99306411318695,-0.106458798018413,2.93140330248867,2.96223370783781\n2.98062694174032,0.0144577208304888,2.91790564474649,2.94926629324341\n2.9682659842669,0.135374239679391,2.9043317730311,2.936298878649\n2.95597699355418,0.256290758528292,2.890685934555,2.92333146405459\n2.94375558339194,0.377207277377193,2.87697251552843,2.91036404946018\n2.93159733175351,0.498123796226094,2.86319593797805,2.89739663486578\n2.91949786701146,0.619040315074996,2.84936057353128,2.88442922027137\n2.90745293636273,0.739956833923897,2.8354706749912,2.87146180567696\n2.89545845713572,0.860873352772798,2.82153032502939,2.85849439108256\n2.8835105526443,0.9817898716217,2.80754340033201,2.84552697648815\n2.87160557479289,1.1027063904706,2.79351354899459,2.83255956189374\n2.85974011581974,1.2236229093195,2.77944417877893,2.81959214729934\n2.84791101149818,1.3445394281684,2.76533845391167,2.80662473270493\n2.83611533789663,1.46545594701731,2.75119929832442,2.79365731811052\n2.82435040350185,1.58637246586621,2.73702940353038,2.78068990351612\n2.81261373819298,1.70728898471511,2.72283123965044,2.76772248892171\n2.80090308024901,1.82820550356401,2.70860706840559,2.7547550743273\n2.78921636230053,1.94912202241291,2.69435895716526,2.74178765973289\n2.77755169690459,2.07003854126181,2.68008879337238,2.72882024513849\n2.76590736223202,2.19095506011071,2.66579829885614,2.71585283054408\n2.75428178820541,2.31187157895962,2.65148904369394,2.70288541594967\n2.74267354330951,2.43278809780852,2.63716245940103,2.68991800135527\n2.7310813222076,2.55370461665742,2.62281985131413,2.67695058676086\n2.71950393423262,2.67462113550632,2.60846241010029,2.66398317216645\n2.7079402927752,2.79553765435522,2.5940912223689,2.65101575757205\n2.69638940555852,2.91645417320412,2.57970728039676,2.63804834297764\n2.68485036576824,3.03737069205302,2.56531149099822,2.62508092838323\n2.67332234399216,3.15828721090193,2.5509046835855,2.61211351378883\n2.66180458091624,3.27920372975083,2.5364876174726,2.59914609919442\n2.65029638072043,3.40012024859973,2.52206098847959,2.58617868460001\n2.63879710511641,3.52103676744863,2.5076254348948,2.57321127000561\n2.62730616797091,3.64195328629753,2.49318154285148,2.5602438554112\n2.61582303046074,3.76286980514643,2.47872985117284,2.54727644081679\n2.60434719670862,3.88378632399533,2.46427085573615,2.53430902622238\n2.59287820985285,4.00470284284424,2.44980501340311,2.52134161162798\n2.58141564850736,4.12561936169314,2.43533274555978,2.50837419703357\n2.56995912357271,4.24653588054204,2.42085444130561,2.49540678243916\n2.55850827536204,4.36745239939094,2.40637046032748,2.48243936784476\n2.54706277100951,4.48836891823984,2.39188113549119,2.46947195325035"
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-5.54165632027653\n4.965989167693"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n1.4655768681604\n4.02852044053613"
        }
    ],
    "scales": [
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "area",
            "properties": {
                "update": {
                    "fill": {
                        "value": "#333333"
                    },
                    "y2": {
                        "scale": "y",
                        "field": "data.resp_upr_"
                    },
                    "fillOpacity": {
                        "value": 0.2
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.pred_"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.resp_lwr_"
                    },
                    "stroke": {
                        "value": "transparent"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0/model_prediction1"
                    }
                }
            },
            "from": {
                "data": ".0/model_prediction1"
            }
        },
        {
            "type": "line",
            "properties": {
                "update": {
                    "strokeWidth": {
                        "value": 2
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.pred_"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.resp_"
                    },
                    "stroke": {
                        "value": "blue"
                    },
                    "fill": {
                        "value": "transparent"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0/model_prediction1"
                    }
                }
            },
            "from": {
                "data": ".0/model_prediction1"
            }
        },
        {
            "type": "area",
            "properties": {
                "update": {
                    "fill": {
                        "value": "#333333"
                    },
                    "y2": {
                        "scale": "y",
                        "field": "data.resp_upr_"
                    },
                    "fillOpacity": {
                        "value": 0.2
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.pred_"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.resp_lwr_"
                    },
                    "stroke": {
                        "value": "transparent"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0/model_prediction2"
                    }
                }
            },
            "from": {
                "data": ".0/model_prediction2"
            }
        },
        {
            "type": "line",
            "properties": {
                "update": {
                    "strokeWidth": {
                        "value": 2
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.pred_"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.resp_"
                    },
                    "stroke": {
                        "value": "red"
                    },
                    "fill": {
                        "value": "transparent"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0/model_prediction2"
                    }
                }
            },
            "from": {
                "data": ".0/model_prediction2"
            }
        },
        {
            "type": "symbol",
            "properties": {
                "update": {
                    "fill": {
                        "value": "#000000"
                    },
                    "size": {
                        "value": 50
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.log_crim"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.log_medv"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0"
                    }
                }
            },
            "from": {
                "data": ".0"
            }
        }
    ],
    "legends": [

    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "log_crim"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "log_medv"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id469169672").parseSpec(plot_id469169672_spec);
</script><!--/html_preserve-->
The biggest apparent issue with regressing `log_medv` on `log_crim` is
the non-linear relationship.  When `log_crim` $<1$, we have decent
linear relationships, albeit with a fair few outliers.  Once we hit
the subset of `log_crime` $ > 1$, however, the relationship
dramatically changes and the slope becomes much steeper.  This may
suggest a confounding variable, or a quadratic relationship overall
and warrants further investigation.

3. Explore graphically the relationship of log crime (`log_crim`) to the log of the median home values (`log_medv`) as in 2.
a) Fill (color) the points according to the radial highway locations (`rad`). Group by `rad` and fit linear models. Does there appear to be an interaction between `rad` and `log_crim`? Discuss.

```r
Boston %>% ggvis(~log_crim, ~log_medv) %>%
    layer_points(fill = ~rad) %>%
    group_by(rad) %>%
    layer_model_predictions(model="lm", se=TRUE, stroke = ~rad)
```

```
## Guessing formula = log_medv ~ log_crim
```

<!--html_preserve--><div id="plot_id694102749-container" class="ggvis-output-container">
<div id="plot_id694102749" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id694102749_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id694102749" data-renderer="svg">SVG</a>
 | 
<a id="plot_id694102749_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id694102749" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id694102749_download" class="ggvis-download" data-plot-id="plot_id694102749">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id694102749_spec = {
    "data": [
        {
            "name": ".0",
            "format": {
                "type": "csv",
                "parse": {
                    "log_crim": "number",
                    "log_medv": "number"
                }
            },
            "values": "\"log_crim\",\"log_medv\",\"rad\"\n-5.06403607082337,3.17805383034795,\"1\"\n-3.60050234349652,3.07269331469012,\"2\"\n-3.60123494426189,3.54673968695281,\"2\"\n-3.43052321104398,3.50855589998265,\"3\"\n-2.67292439912684,3.58905911883173,\"3\"\n-3.51157043914353,3.35689712276558,\"3\"\n-2.42712842806865,3.13113691056019,\"5\"\n-1.93412981051978,3.29953372788566,\"5\"\n-1.5547603511434,2.80336038090653,\"5\"\n-1.77172157549155,2.9391619220656,\"5\"\n-1.49214388521174,2.70805020110221,\"5\"\n-2.14157229714634,2.9391619220656,\"5\"\n-2.3668036653207,3.07731226054641,\"5\"\n-0.462416484558303,3.01553490085017,\"4\"\n-0.449479693527584,2.90142159408275,\"4\"\n-0.46618692214761,2.99071973173045,\"4\"\n0.0525260342514466,3.13983261752775,\"4\"\n-0.243091189123906,2.86220088092947,\"4\"\n-0.219761775967802,3.00568260440716,\"4\"\n-0.320480784203167,2.90142159408275,\"4\"\n0.224574526979991,2.61006979274201,\"4\"\n-0.160121804898053,2.97552956623647,\"4\"\n0.209020285867676,2.72129542785223,\"4\"\n-0.0116374532441208,2.67414864942653,\"4\"\n-0.287335465860119,2.74727091425549,\"4\"\n-0.17371073654606,2.63188884013665,\"4\"\n-0.397630875999648,2.8094026953625,\"4\"\n-0.0452379806501943,2.69462718077007,\"4\"\n-0.257489167089002,2.91235066461494,\"4\"\n0.00244700364305185,3.04452243772342,\"4\"\n0.122934190094978,2.54160199346455,\"4\"\n0.30359479091183,2.67414864942653,\"4\"\n0.327856657447708,2.58021682959233,\"4\"\n0.141256477175355,2.57261223020711,\"4\"\n0.477984199611674,2.60268968544438,\"4\"\n-2.74621946721971,2.9391619220656,\"5\"\n-2.32851847502055,2.99573227355399,\"5\"\n-2.52398017377414,3.04452243772342,\"5\"\n-1.74268363158146,3.20680324363393,\"5\"\n-3.58885314004681,3.42751468997953,\"3\"\n-3.39352687535794,3.55248682920838,\"3\"\n-2.06010961338034,3.28091121578765,\"3\"\n-1.95545556189884,3.23080439573347,\"3\"\n-1.83658948514585,3.20680324363393,\"3\"\n-2.09809443017905,3.05400118167797,\"3\"\n-1.7636385935114,2.96010509591084,\"3\"\n-1.66940025360678,2.99573227355399,\"3\"\n-1.47285493064757,2.8094026953625,\"3\"\n-1.37093295400719,2.66722820658195,\"3\"\n-1.51517373404402,2.96527306606928,\"3\"\n-2.42215722813489,2.98061863574394,\"4\"\n-3.13798732113486,3.02042488614436,\"4\"\n-2.92620621090538,3.2188758248682,\"4\"\n-2.99953951189695,3.15273602236366,\"4\"\n-4.29768548624013,2.9391619220656,\"3\"\n-4.33437998120653,3.56671182013973,\"5\"\n-3.88489433803989,3.20680324363393,\"2\"\n-4.24609811744964,3.45315712059287,\"5\"\n-1.86788485961755,3.14845336057165,\"8\"\n-2.27031153244375,2.97552956623647,\"8\"\n-1.90166362493575,2.92852352386054,\"8\"\n-1.76194827165643,2.77258872223978,\"8\"\n-2.20482337521155,3.10009228887823,\"8\"\n-2.06751297081456,3.2188758248682,\"8\"\n-3.93682812434712,3.49650756146648,\"3\"\n-3.32869069087541,3.15700042115011,\"4\"\n-3.12834979816883,2.96527306606928,\"4\"\n-2.84921062089122,3.09104245335832,\"4\"\n-1.99848847927417,2.85647020622048,\"4\"\n-2.05447579566209,3.03974915897077,\"4\"\n-2.42746827514074,3.18635263316264,\"4\"\n-1.84036165106727,3.07731226054641,\"4\"\n-2.38988742139684,3.1267605359604,\"4\"\n-1.63275771775572,3.15273602236366,\"4\"\n-2.53881388385691,3.18221184049661,\"5\"\n-2.35261602659961,3.06339092202781,\"5\"\n-2.28740095766901,2.99573227355399,\"5\"\n-2.44104288614161,3.03495298670727,\"5\"\n-2.87422285615679,3.05400118167797,\"5\"\n-2.47848729798582,3.01062088604774,\"5\"\n-3.1910174967398,3.3322045101752,\"4\"\n-3.10957308997775,3.17387845893747,\"4\"\n-3.30798029995102,3.21084365317094,\"4\"\n-3.33794093202714,3.13113691056019,\"4\"\n-2.98400135067829,3.17387845893747,\"3\"\n-2.85858243540676,3.28091121578765,\"3\"\n-2.95882191953389,3.11351530921037,\"3\"\n-2.63791797892183,3.10009228887823,\"3\"\n-2.871746293773,3.16124671203156,\"2\"\n-2.93708607812126,3.35689712276558,\"2\"\n-3.06101774025262,3.11794990627824,\"2\"\n-3.23602198370317,3.09104245335832,\"2\"\n-3.16937162996511,3.13113691056019,\"4\"\n-3.54911751173878,3.2188758248682,\"4\"\n-3.1479514865315,3.02529107579554,\"4\"\n-2.10340641913367,3.34638914516716,\"2\"\n-2.1624753850094,3.06339092202781,\"2\"\n-2.11337067994292,3.65583960003574,\"2\"\n-2.50262265559378,3.7796338173824,\"2\"\n-2.6794627442503,3.50254987592244,\"2\"\n-1.90609345968499,3.31418600467253,\"5\"\n-2.16875374536053,3.27714473299218,\"5\"\n-1.47508185993502,2.92316158071916,\"5\"\n-1.55301032113546,2.96010509591084,\"5\"\n-1.96897408865386,3.00071981506503,\"5\"\n-2.02026738304142,2.9704144655697,\"5\"\n-1.7649228152745,2.9704144655697,\"5\"\n-2.03126108715508,3.01553490085017,\"5\"\n-2.05556877726828,2.98568193770049,\"5\"\n-1.3332086740283,2.96527306606928,\"5\"\n-2.22627241014488,3.07731226054641,\"5\"\n-2.29422017666242,3.1267605359604,\"6\"\n-2.09321597510168,2.9338568698359,\"6\"\n-1.50453750260873,2.92852352386054,\"6\"\n-1.94974750228657,2.91777073208428,\"6\"\n-1.76410539244624,2.90690105984738,\"6\"\n-2.02814024732429,3.05400118167797,\"6\"\n-1.89060790127066,2.95491027903374,\"6\"\n-2.03576921322365,3.01553490085017,\"6\"\n-1.9326780802866,2.96010509591084,\"6\"\n-2.67379371242412,3.09104245335832,\"2\"\n-2.63596212470796,3.01062088604774,\"2\"\n-2.37526331849203,3.02042488614436,\"2\"\n-1.89458985503226,2.85070650150373,\"2\"\n-2.31780025880053,2.9338568698359,\"2\"\n-1.7777382278658,3.06339092202781,\"2\"\n-0.948426601904225,2.75366071235426,\"2\"\n-1.35034823434642,2.78501124223834,\"4\"\n-1.12260789422433,2.89037175789616,\"4\"\n-0.126413924855659,2.66025953726586,\"4\"\n-1.0786332063528,2.95491027903374,\"4\"\n0.176420848472986,2.97552956623647,\"4\"\n-0.527547999910379,3.13549421592915,\"4\"\n-1.10920822788151,2.91235066461494,\"4\"\n-0.0241185274088078,2.74727091425549,\"4\"\n-0.583790659576773,2.89591193827178,\"4\"\n-1.13121812841702,2.85647020622048,\"4\"\n-1.0431870425627,2.83907846350861,\"4\"\n-1.38709468129066,2.58776403522771,\"4\"\n-0.607850606337865,2.87919845729804,\"4\"\n-1.23477571348098,2.63905732961526,\"4\"\n0.487745310721893,2.66722820658195,\"4\"\n1.20028099798739,2.59525470695687,\"5\"\n1.41035262621296,2.74727091425549,\"5\"\n1.02235739814894,2.46809953147162,\"5\"\n0.866823138301229,2.62466859216316,\"5\"\n0.767813925142701,2.74727091425549,\"5\"\n0.862307507076077,2.68102152871429,\"5\"\n0.846293070040128,2.87919845729804,\"5\"\n1.00575476530812,2.73436750941958,\"5\"\n0.504767309182027,3.06805293513362,\"5\"\n0.403008760421457,2.97552956623647,\"5\"\n0.119186494791163,2.72785282839839,\"5\"\n0.76508637404103,2.96527306606928,\"5\"\n0.346316479810509,2.83321334405622,\"5\"\n1.26271612819885,2.74727091425549,\"5\"\n0.894732003534746,2.57261223020711,\"5\"\n0.201780987950174,3.72086249996699,\"5\"\n0.294786774181713,3.1904763503465,\"5\"\n0.354185848709842,3.14845336057165,\"5\"\n0.241737605442712,3.29583686600433,\"5\"\n0.380735161487553,3.91202300542815,\"5\"\n0.606373957027711,3.91202300542815,\"5\"\n0.418065390083903,3.91202300542815,\"5\"\n0.807528882678661,3.12236492448736,\"5\"\n1.07295254188753,3.2188758248682,\"5\"\n0.698229244966739,3.91202300542815,\"5\"\n0.587942208360164,3.16968558067743,\"5\"\n0.833083020857462,3.16968558067743,\"5\"\n0.895896169418922,3.10458667846607,\"5\"\n0.188485851761799,2.85647020622048,\"5\"\n0.838934412625926,2.94968833505258,\"5\"\n-1.97227465848664,3.13983261752775,\"5\"\n-2.38836087001545,3.16124671203156,\"5\"\n-2.47135883724273,3.11794990627824,\"5\"\n-2.70845028112355,3.38099467434464,\"5\"\n-2.65612210824185,3.14415227867226,\"5\"\n-2.91415228656157,3.20274644293832,\"5\"\n-2.71175706303354,3.39785848039664,\"5\"\n-2.85076650330381,3.6163087612791,\"3\"\n-2.7199203736727,3.68386691229039,\"3\"\n-2.67538941886266,3.58905911883173,\"3\"\n-2.39656615646494,3.63495111208838,\"3\"\n-2.30178541282348,3.48124008933569,\"3\"\n-2.48795127997423,3.27336401015227,\"3\"\n-2.80560790469702,3.38777436133001,\"3\"\n-2.88204650915017,3.91202300542815,\"3\"\n-2.54147700127639,3.46573590279973,\"5\"\n-2.07314142913136,3.39450839351136,\"5\"\n-2.48051630148671,3.55248682920838,\"5\"\n-2.40041845334281,3.61091791264422,\"5\"\n-2.67205584087883,3.41772668361337,\"5\"\n-2.4459936762894,3.5945687746427,\"5\"\n-3.82263944429346,3.43720781918519,\"1\"\n-4.24122175808286,3.37073817417745,\"1\"\n-4.28236231156094,3.91202300542815,\"4\"\n-3.21612959920018,3.5055573969864,\"2\"\n-3.06486801238885,3.41114771251532,\"2\"\n-3.27862582927397,3.54385368206368,\"2\"\n-3.45776773315055,3.55248682920838,\"3\"\n-4.02968104889638,3.49347265777133,\"3\"\n-3.36824628152247,3.18221184049661,\"2\"\n-3.82722240373568,3.74478708605223,\"2\"\n-3.34955414851032,3.88156379794344,\"4\"\n-3.90753310015529,3.91202300542815,\"4\"\n-1.99201691675556,3.11794990627824,\"4\"\n-1.47102470528047,3.19458313229916,\"4\"\n-1.37836587479777,3.11351530921037,\"4\"\n-1.9960567327459,3.19458313229916,\"4\"\n-0.830778394549941,2.99573227355399,\"4\"\n-1.74605978997706,3.07731226054641,\"4\"\n-0.978751413216761,2.96010509591084,\"4\"\n-1.52698273249791,3.10906095886099,\"4\"\n-1.96240545158451,3.3357695763397,\"4\"\n-1.23942728531034,3.16547504814109,\"4\"\n-1.61938724328777,3.2188758248682,\"4\"\n-3.0878475624618,3.14845336057165,\"5\"\n-2.65740461643332,3.35689712276558,\"5\"\n-2.2010217775846,3.06805293513362,\"5\"\n-2.16936624920782,3.13549421592915,\"5\"\n-1.02697092752823,3.2846635654062,\"8\"\n-0.897199141618634,3.07731226054641,\"8\"\n-0.472310287537657,3.31418600467253,\"8\"\n-0.486620935069173,3.40452517175483,\"8\"\n-1.15413556947876,3.80220813942094,\"8\"\n-0.640687566587583,3.91202300542815,\"8\"\n-0.961968245370808,3.62700405039585,\"8\"\n-0.885810024620568,3.45315712059287,\"8\"\n-1.21002441175437,3.84374416467485,\"8\"\n-0.816943258373457,3.44998754583159,\"8\"\n-0.621757184473272,3.1904763503465,\"8\"\n-0.770114621716554,3.45631668088323,\"8\"\n-0.552881017499318,3.73050112880476,\"8\"\n-1.10421797118894,3.87743156065853,\"8\"\n-0.803162959605968,3.36729582998647,\"8\"\n-1.10729991706568,3.17805383034795,\"8\"\n-0.652811704370542,3.22286784613774,\"8\"\n-0.669762740327209,3.44998754583159,\"8\"\n-2.49568452295988,3.16547504814109,\"6\"\n-2.3803304416189,3.14845336057165,\"6\"\n-2.17780437609674,3.09104245335832,\"6\"\n-2.2431847497126,3.00071981506503,\"6\"\n-2.27399763614213,3.10009228887823,\"6\"\n-2.05909004543194,3.16547504814109,\"6\"\n-1.57949083606615,2.86789890204411,\"7\"\n-1.65375559308523,2.91777073208428,\"7\"\n-1.07930978641361,3.1904763503465,\"7\"\n-1.62673667701243,3.02042488614436,\"7\"\n-1.80551362546072,3.19867311755068,\"7\"\n-1.6568964635938,3.26575941076705,\"7\"\n-1.96397229187372,3.19458313229916,\"7\"\n-1.54135879162351,3.21084365317094,\"7\"\n-2.4984783298181,3.38777436133001,\"7\"\n-0.997121249788704,3.75653810258775,\"7\"\n-3.0326037483299,3.08648663682246,\"1\"\n-3.33878612154076,3.03974915897077,\"1\"\n-4.17468731490464,3.78418963391826,\"3\"\n-0.49177491307519,3.91202300542815,\"5\"\n-0.410211353733397,3.58351893845611,\"5\"\n-0.420604126950968,3.40452517175483,\"5\"\n-0.615982456464896,3.52046080248897,\"5\"\n-0.627134746166374,3.7635229971097,\"5\"\n-0.653657272873533,3.8877303128591,\"5\"\n-0.192056790782112,3.43398720448515,\"5\"\n-0.597709736126834,3.59731226058845,\"5\"\n-0.272307535345581,3.1267605359604,\"5\"\n-0.241180238800361,3.42426265459315,\"5\"\n-0.547593347958205,3.91202300542815,\"5\"\n-0.615260641902874,3.77276093809464,\"5\"\n-2.40074934178128,3.03013370027132,\"3\"\n-1.20677673165867,3.04927304048202,\"3\"\n-1.81948016182866,3.22684399451738,\"3\"\n-2.1663074747015,3.19458313229916,\"3\"\n-1.5056185837951,3.56104608260405,\"3\"\n-2.87457715199752,3.47815842279828,\"4\"\n-2.34299050762908,3.46573590279973,\"4\"\n-2.25675167665087,3.50254987592244,\"4\"\n-2.79246495224457,3.49953328238302,\"4\"\n-2.52848243250488,3.37073817417745,\"4\"\n-1.55883985967101,3.55820113047182,\"5\"\n-3.33036620090156,3.8155121050473,\"5\"\n-3.29548692724004,3.56671182013973,\"5\"\n-2.7921385814845,3.8286413964891,\"5\"\n-4.19903863333677,3.91202300542815,\"1\"\n-4.70388615892725,3.47196645255036,\"1\"\n-4.51350299746227,3.09104245335832,\"1\"\n-3.92967794066687,3.00071981506503,\"1\"\n-3.25165731439258,3.14415227867226,\"6\"\n-3.08129016191564,3.10458667846607,\"6\"\n-3.14725308119523,3.21084365317094,\"6\"\n-3.35183595212443,3.3499040872746,\"4\"\n-2.54008115053266,3.61899332664977,\"4\"\n-3.32007833037736,3.32862668882732,\"4\"\n-2.4931404547151,3.17387845893747,\"4\"\n-2.50115799037405,3.07731226054641,\"4\"\n-2.0454653261249,3.35340671782581,\"4\"\n-2.9239699073271,3.29953372788566,\"4\"\n-1.95878264527799,3.01062088604774,\"4\"\n-2.73861250668485,3.11351530921037,\"5\"\n-2.88939223778266,3.36729582998647,\"5\"\n-3.1197094533737,3.21084365317094,\"5\"\n-3.34189127576475,3.09104245335832,\"7\"\n-2.37881839899363,3.27336401015227,\"7\"\n-2.30258509299405,3.49953328238302,\"7\"\n-2.89769853328263,3.58629286533884,\"7\"\n-2.90424758343181,3.34638914516716,\"7\"\n-2.5898672454245,3.50855589998265,\"7\"\n-3.00942560068599,3.33932197794407,\"7\"\n-0.707286673713667,3.1267605359604,\"4\"\n-1.05153788128218,3.01062088604774,\"4\"\n0.969065328591483,2.77881927199042,\"4\"\n-0.23520348080665,3.09557760852371,\"4\"\n-1.3405946818689,2.96527306606928,\"4\"\n-1.31163225681147,3.07269331469012,\"4\"\n-0.99641677635344,3.16968558067743,\"4\"\n-1.37215479756617,2.78501124223834,\"4\"\n-1.14485519984285,2.87919845729804,\"4\"\n-1.4055995121779,2.98568193770049,\"4\"\n-0.911253440356887,3.13983261752775,\"4\"\n-0.743451490469693,3.04452243772342,\"4\"\n-1.78617509093415,3.16968558067743,\"5\"\n-1.70600388041043,3.13983261752775,\"5\"\n-1.04657027464107,3.01553490085017,\"5\"\n-1.2590627706439,2.91777073208428,\"5\"\n-1.07560890690315,3.2188758248682,\"5\"\n-1.65098933959234,3.20274644293832,\"5\"\n-1.19247252015573,3.13549421592915,\"5\"\n-1.42283387191084,3.10009228887823,\"5\"\n-2.71552809095817,2.96010509591084,\"4\"\n-2.69948697044172,3.11794990627824,\"4\"\n-3.09136250456924,2.98568193770049,\"4\"\n-2.99114282122018,2.83907846350861,\"1\"\n-3.36216899469468,2.96527306606928,\"1\"\n-2.97926854752333,3.10009228887823,\"5\"\n-3.28661947695472,3.03013370027132,\"5\"\n-3.22867366734831,3.04927304048202,\"5\"\n-3.37348494309562,2.9704144655697,\"5\"\n-3.49298377729286,2.91777073208428,\"5\"\n-3.40943118658926,3.02529107579554,\"5\"\n-2.90096769710957,2.94443897916644,\"5\"\n-2.78855551576186,2.92852352386054,\"5\"\n-4.34203698645772,3.48737507790321,\"1\"\n-3.68967977428471,2.80336038090653,\"1\"\n-3.67182569954811,3.17387845893747,\"5\"\n-3.49035651798197,3.44041809481544,\"5\"\n-3.46958329452862,2.86220088092947,\"3\"\n-2.78676878581361,2.84490938381941,\"3\"\n-3.9792317551216,3.13983261752775,\"4\"\n-4.19903863333677,3.19867311755068,\"4\"\n-3.54080433604857,3.28091121578765,\"1\"\n-2.77884827241093,3.13113691056019,\"1\"\n-2.53199825732185,3.18221184049661,\"4\"\n-2.62499664596692,2.92316158071916,\"4\"\n-4.06926178185464,3.40452517175483,\"5\"\n-3.14632263218649,2.90142159408275,\"4\"\n-2.23876558028109,3.02529107579554,\"4\"\n2.19532944938176,2.87919845729804,\"24\"\n1.34799522318569,3.07731226054641,\"24\"\n1.64899895228507,3.12236492448736,\"24\"\n1.44957662474632,3.11794990627824,\"24\"\n1.51334983014208,3.2188758248682,\"24\"\n1.34464911115118,2.99071973173045,\"24\"\n1.30242893951019,3.03495298670727,\"24\"\n1.44040131844278,2.82137888640921,\"24\"\n1.24538726330446,3.08648663682246,\"24\"\n1.51641651137626,3.31418600467253,\"24\"\n1.30750815538373,3.08648663682246,\"24\"\n2.60433277927054,3.13983261752775,\"24\"\n1.58887187381363,3.91202300542815,\"24\"\n1.73518559039658,3.91202300542815,\"24\"\n1.87774754504581,3.91202300542815,\"24\"\n2.22270820490502,3.91202300542815,\"24\"\n2.1123019265294,3.91202300542815,\"24\"\n2.40767457192474,2.62466859216316,\"24\"\n2.91767343005329,2.62466859216316,\"24\"\n2.97599374420349,2.70805020110221,\"24\"\n2.72706820693797,2.63188884013665,\"24\"\n2.28477645637644,2.58776403522771,\"24\"\n3.16328700210486,2.57261223020711,\"24\"\n2.88293864507855,2.32238772029023,\"24\"\n4.48836891823984,2.34180580614733,\"24\"\n2.76470774878891,2.3887627892351,\"24\"\n2.21779161827618,2.42480272571829,\"24\"\n2.07850109960278,2.50959926237837,\"24\"\n2.99996828895892,2.17475172148416,\"24\"\n2.82208102080754,1.97408102602201,\"24\"\n3.19432900165004,2.35137525716348,\"24\"\n3.11782157946064,2.00148000021012,\"24\"\n2.6626134080936,2.32238772029023,\"24\"\n2.09823140139806,2.4423470353692,\"24\"\n1.94048833469004,2.71469474382088,\"24\"\n1.66639463926993,3.14415227867226,\"24\"\n2.44909810854921,2.27212588550934,\"24\"\n2.15695335703792,2.62466859216316,\"24\"\n2.59225019793657,2.54160199346455,\"24\"\n2.16524646202657,2.57261223020711,\"24\"\n1.77020380626234,2.52572864430826,\"24\"\n2.03757994445991,2.14006616349627,\"24\"\n3.64680146282653,1.6094379124341,\"24\"\n2.29420507854844,1.84054963339749,\"24\"\n3.22071812678739,1.7227665977411,\"24\"\n2.6557880164394,1.97408102602201,\"24\"\n2.2613161235954,2.4932054526027,\"24\"\n3.2109121992087,2.11625551480255,\"24\"\n3.72639679427388,2.14006616349627,\"24\"\n4.21834232049674,1.6094379124341,\"24\"\n3.03091600288847,2.47653840011748,\"24\"\n2.48082332435036,3.32862668882732,\"24\"\n2.00200553776674,2.84490938381941,\"24\"\n2.66988439800228,3.31418600467253,\"24\"\n3.93448483899724,2.70805020110221,\"24\"\n2.64267221660273,2.84490938381941,\"24\"\n2.93444180511088,2.88480071284671,\"24\"\n3.35535586595285,2.79116510781272,\"24\"\n3.82310654218625,1.94591014905531,\"24\"\n2.8950607473823,1.97408102602201,\"24\"\n2.38270779746775,2.01490302054226,\"24\"\n3.25580930892149,2.34180580614733,\"24\"\n4.29774924420755,2.17475172148416,\"24\"\n2.4691413614596,2.12823170584927,\"24\"\n2.4058093284293,2.81540871942271,\"24\"\n1.94913209586287,2.65324196460721,\"24\"\n2.48891527118536,3.03495298670727,\"24\"\n1.95308718951777,2.59525470695687,\"24\"\n2.17385586577979,2.45958884180371,\"24\"\n2.76381913153852,2.11625551480255,\"24\"\n2.50529733943573,2.32238772029023,\"24\"\n3.62864897336371,2.3887627892351,\"24\"\n1.99702549904027,2.39789527279837,\"24\"\n2.2341874014952,2.2512917986065,\"24\"\n2.13913985224951,2.67414864942653,\"24\"\n2.30879576677076,2.64617479738412,\"24\"\n1.86315722444043,2.77881927199042,\"24\"\n1.71938051428274,2.66025953726586,\"24\"\n2.63285240453631,2.45958884180371,\"24\"\n2.41237179860475,2.59525470695687,\"24\"\n2.66867160882001,2.26176309847379,\"24\"\n2.71979430172596,2.16332302566054,\"24\"\n2.61579601365956,2.12823170584927,\"24\"\n2.23971238362132,2.54944517092557,\"24\"\n3.09336248726987,2.35137525716348,\"24\"\n2.27461556718342,2.83907846350861,\"24\"\n1.73454870107647,2.91235066461494,\"24\"\n2.29923348261767,2.73436750941958,\"24\"\n2.54962484228371,2.37954613413017,\"24\"\n2.36760474836797,2.46809953147162,\"24\"\n1.83865418738046,2.70136121295141,\"24\"\n2.29504171310892,2.53369681395743,\"24\"\n2.23313747526855,2.64617479738412,\"24\"\n2.01836502089744,2.56494935746154,\"24\"\n1.90474881125035,2.59525470695687,\"24\"\n1.693988597737,2.72129542785223,\"24\"\n1.62731122882592,2.77881927199042,\"24\"\n2.1099816583913,2.87919845729804,\"24\"\n2.25272550719709,2.70136121295141,\"24\"\n1.55864344098212,2.64617479738412,\"24\"\n1.54090850495968,2.54160199346455,\"24\"\n2.10420488347616,2.60268968544438,\"24\"\n2.04798054391097,2.70136121295141,\"24\"\n1.91709465620517,2.99573227355399,\"24\"\n1.57113981354136,2.79728133483015,\"24\"\n1.30646892150861,2.87356463957978,\"24\"\n1.89535643073801,2.9704144655697,\"24\"\n1.76149783672584,3.00568260440716,\"24\"\n2.05915209590677,3.06339092202781,\"24\"\n1.15171061966313,2.99071973173045,\"24\"\n1.32839508467371,2.94443897916644,\"24\"\n1.48665540019546,2.94968833505258,\"24\"\n2.7457120074838,2.94968833505258,\"24\"\n2.5707096581053,3.00071981506503,\"24\"\n1.46989764548713,2.99071973173045,\"24\"\n1.39585105014985,2.97552956623647,\"24\"\n1.27219577951878,3.14415227867226,\"24\"\n1.53619817863696,3.39450839351136,\"24\"\n2.08639108754919,2.62466859216316,\"24\"\n1.85522241213869,2.58776403522771,\"24\"\n1.58338342291018,2.81540871942271,\"24\"\n2.7096089855662,2.484906649788,\"24\"\n2.32561779210462,2.68102152871429,\"24\"\n2.6626134080936,3.06339092202781,\"24\"\n1.76198902792588,3.13549421592915,\"24\"\n1.74190023380554,3.16547504814109,\"24\"\n1.74591795351875,3.2188758248682,\"24\"\n1.0361622517949,3.08190996979504,\"24\"\n0.866499466770358,3.02529107579554,\"24\"\n1.30119116239956,3.05400118167797,\"24\"\n1.73901775796999,2.94968833505258,\"24\"\n1.57601969221081,3.02529107579554,\"24\"\n-1.89140302455665,2.72129542785223,\"4\"\n-1.69624930942106,1.94591014905531,\"4\"\n-1.57281672897846,2.09186406167839,\"4\"\n-2.24677202817483,2.61006979274201,\"4\"\n-2.19534634232445,3.00071981506503,\"4\"\n-1.75267338052085,3.08190996979504,\"6\"\n-1.27450257051646,3.19867311755068,\"6\"\n-1.72042534062373,3.13983261752775,\"6\"\n-1.23925461847058,2.98061863574394,\"6\"\n-1.31535139230933,2.90690105984738,\"6\"\n-1.43078976100645,3.05400118167797,\"6\"\n-1.72692724122657,2.86220088092947,\"6\"\n-1.49441423586532,2.82137888640921,\"6\"\n-2.77051088244482,3.10906095886099,\"1\"\n-3.09511071753427,3.02529107579554,\"1\"\n-2.80082360125457,3.17387845893747,\"1\"\n-2.21100914950684,3.09104245335832,\"1\"\n-3.04892210206811,2.47653840011748,\"1\""
        },
        {
            "name": ".0/group_by1/model_prediction2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "resp_upr_": "number",
                    "pred_": "number",
                    "resp_lwr_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"resp_upr_\",\"pred_\",\"resp_lwr_\",\"resp_\",\"rad\"\n3.71010838533241,-5.06403607082337,3.13275586713896,3.42143212623568,\"1\"\n3.69789970637506,-5.02792180599658,3.13173817739358,3.41481894188432,\"1\"\n3.68571961077605,-4.99180754116979,3.13069190428986,3.40820575753295,\"1\"\n3.67356985369888,-4.955693276343,3.12961529266429,3.40159257318159,\"1\"\n3.6614523262027,-4.9195790115162,3.12850645145774,3.39497938883022,\"1\"\n3.6493690673843,-4.88346474668941,3.12736334157341,3.38836620447885,\"1\"\n3.63732227766122,-4.84735048186262,3.12618376259376,3.38175302012749,\"1\"\n3.62531433329021,-4.81123621703583,3.12496533826203,3.37513983577612,\"1\"\n3.61334780221672,-4.77512195220904,3.1237055006328,3.36852665142476,\"1\"\n3.60142546135,-4.73900768738225,3.12240147279679,3.36191346707339,\"1\"\n3.58955031535424,-4.70289342255546,3.12105025008981,3.35530028272203,\"1\"\n3.57772561703645,-4.66677915772866,3.11964857970487,3.34868709837066,\"1\"\n3.56595488939601,-4.63066489290187,3.11819293864259,3.3420739140193,\"1\"\n3.55424194937661,-4.59455062807508,3.11667950995925,3.33546072966793,\"1\"\n3.54259093332517,-4.55843636324829,3.11510415730796,3.32884754531656,\"1\"\n3.53100632411197,-4.5223220984215,3.11346239781843,3.3222343609652,\"1\"\n3.51949297979784,-4.48620783359471,3.11174937342982,3.31562117661383,\"1\"\n3.50805616364173,-4.45009356876791,3.10995982088321,3.30900799226247,\"1\"\n3.49670157512197,-4.41397930394112,3.10808804070023,3.3023948079111,\"1\"\n3.48543538148991,-4.37786503911433,3.10612786562956,3.29578162355974,\"1\"\n3.47426424917909,-4.34175077428754,3.10407262923765,3.28916843920837,\"1\"\n3.4631953741522,-4.30563650946075,3.10191513556181,3.28255525485701,\"1\"\n3.45223650997504,-4.26952224463396,3.09964763103624,3.27594207050564,\"1\"\n3.44139599206142,-4.23340797980716,3.09726178024713,3.26932888615427,\"1\"\n3.43068275613583,-4.19729371498037,3.09474864746999,3.26271570180291,\"1\"\n3.42010634852228,-4.16117945015358,3.09209868638081,3.25610251745154,\"1\"\n3.40967692540702,-4.12506518532679,3.08930174079333,3.24948933310018,\"1\"\n3.39940523777329,-4.0889509205,3.08634705972434,3.24287614874881,\"1\"\n3.38930259832012,-4.05283665567321,3.08322333047478,3.23626296439745,\"1\"\n3.37938082642632,-4.01672239084642,3.07991873366584,3.22964978004608,\"1\"\n3.36965216719939,-3.98060812601962,3.07642102419004,3.22303659569472,\"1\"\n3.36012918096917,-3.94449386119283,3.07271764171754,3.21642341134335,\"1\"\n3.35082460036731,-3.90837959636604,3.06879585361665,3.20981022699198,\"1\"\n3.3417511534844,-3.87226533153925,3.06464293179684,3.20319704264062,\"1\"\n3.33292135358518,-3.83615106671246,3.06024636299333,3.19658385828925,\"1\"\n3.32434725848299,-3.80003680188567,3.05559408939278,3.18997067393789,\"1\"\n3.3160402058047,-3.76392253705887,3.05067477336835,3.18335748958652,\"1\"\n3.30801053375812,-3.72780827223208,3.0454780767122,3.17674430523516,\"1\"\n3.30026730023771,-3.69169400740529,3.03999494152987,3.17013112088379,\"1\"\n3.29281801565697,-3.6555797425785,3.03421785740788,3.16351793653243,\"1\"\n3.28566840623365,-3.61946547775171,3.02814109812847,3.15690475218106,\"1\"\n3.27882222412196,-3.58335121292492,3.02176091153743,3.15029156782969,\"1\"\n3.27228111854819,-3.54723694809812,3.01507564840847,3.14367838347833,\"1\"\n3.26604457804686,-3.51112268327133,3.00808582020707,3.13706519912696,\"1\"\n3.26010994844824,-3.47500841844454,3.00079408110295,3.1304520147756,\"1\"\n3.25447252516086,-3.43889415361775,2.99320513568761,3.12383883042423,\"1\"\n3.24912571239357,-3.40277988879096,2.98532557975216,3.11722564607287,\"1\"\n3.24406123709564,-3.36666562396417,2.97716368634736,3.1106124617215,\"1\"\n3.23926940216083,-3.33055135913738,2.96872915257944,3.10399927737014,\"1\"\n3.23473936211532,-3.29443709431058,2.96003282392222,3.09738609301877,\"1\"\n3.23045940501086,-3.25832282948379,2.95108641232395,3.0907729086674,\"1\"\n3.2264172262219,-3.222208564657,2.94190222241018,3.08415972431604,\"1\"\n3.222600182783,-3.18609429983021,2.93249289714635,3.07754653996467,\"1\"\n3.21899552025985,-3.14998003500342,2.92287119096676,3.07093335561331,\"1\"\n3.21559056745154,-3.11386577017663,2.91304977507234,3.06432017126194,\"1\"\n3.21237289713478,-3.07775150534983,2.90304107668638,3.05770698691058,\"1\"\n3.20933045339013,-3.04163724052304,2.89285715172829,3.05109380255921,\"1\"\n3.20645164773312,-3.00552297569625,2.88250958868257,3.04448061820785,\"1\"\n3.20372542734293,-2.96940871086946,2.87200944037003,3.03786743385648,\"1\"\n3.2011413192303,-2.93329444604267,2.86136717977993,3.03125424950511,\"1\"\n3.19868945433001,-2.89718018121588,2.85059267597749,3.02464106515375,\"1\"\n3.19636057536083,-2.86106591638908,2.83969518624393,3.01802788080238,\"1\"\n3.19414603197038,-2.82495165156229,2.82868336093166,3.01141469645102,\"1\"\n3.19203776625862,-2.7888373867355,2.81756525794068,3.00480151209965,\"1\"\n3.19002829131391,-2.75272312190871,2.80634836418266,2.99818832774829,\"1\"\n3.18811066494156,-2.71660885708192,2.79503962185228,2.99157514339692,\"1\"\n3.1862784603448,-2.68049459225513,2.78364545774631,2.98496195904556,\"1\"\n3.18452573514443,-2.64438032742834,2.77217181424395,2.97834877469419,\"1\"\n3.18284699980289,-2.60826606260154,2.76062418088276,2.97173559034282,\"1\"\n3.18123718625073,-2.57215179777475,2.74900762573218,2.96512240599146,\"1\"\n3.17969161729444,-2.53603753294796,2.73732682598574,2.95850922164009,\"1\"\n3.17820597720932,-2.49992326812117,2.72558609736813,2.95189603728873,\"1\"\n3.1767762837835,-2.46380900329438,2.71378942209123,2.94528285293736,\"1\"\n3.17539886197277,-2.42769473846759,2.70194047519923,2.938669668586,\"1\"\n3.17407031924571,-2.39158047364079,2.69004264922355,2.93205648423463,\"1\"\n3.17278752263887,-2.355466208814,2.67809907712767,2.92544329988327,\"1\"\n3.17154757749874,-2.31935194398721,2.66611265356506,2.9188301155319,\"1\"\n3.17034780785733,-2.28323767916042,2.65408605450374,2.91221693118053,\"1\"\n3.16918573836783,-2.24712341433363,2.64202175529051,2.90560374682917,\"1\"\n3.1680590777144,-2.21100914950684,2.62992204724121,2.8989905624778,\"1\"\n3.65803056174109,-3.88489433803989,3.24236624036713,3.45019840105411,\"2\"\n3.64653990844176,-3.84772386036729,3.24071295032729,3.44362642938452,\"2\"\n3.63509778500177,-3.81055338269469,3.23901113042812,3.43705445771494,\"2\"\n3.62370786097058,-3.77338290502208,3.23725711112014,3.43048248604536,\"2\"\n3.61237413972448,-3.73621242734948,3.23544688902707,3.42391051437577,\"2\"\n3.60110099038414,-3.69904194967688,3.23357609502824,3.41733854270619,\"2\"\n3.58989318214182,-3.66187147200427,3.23163995993139,3.41076657103661,\"2\"\n3.57875592087873,-3.62470099433167,3.22963327785531,3.40419459936702,\"2\"\n3.56769488781713,-3.58753051665907,3.22755036757775,3.39762262769744,\"2\"\n3.55671627976723,-3.55036003898646,3.22538503228848,3.39105065602786,\"2\"\n3.54582685028564,-3.51318956131386,3.22313051843091,3.38447868435827,\"2\"\n3.53503395074665,-3.47601908364126,3.22077947463073,3.37790671268869,\"2\"\n3.52434556992918,-3.43884860596865,3.21832391210903,3.3713347410191,\"2\"\n3.51377037023017,-3.40167812829605,3.21575516846887,3.36476276934952,\"2\"\n3.50331771802588,-3.36450765062345,3.213063877334,3.35819079767994,\"2\"\n3.49299770502138,-3.32733717295084,3.21023994699933,3.35161882601035,\"2\"\n3.48282115667674,-3.29016669527824,3.2072725520048,3.34504685434077,\"2\"\n3.47279962302211,-3.25299621760564,3.20415014232026,3.33847488267119,\"2\"\n3.4629453464527,-3.21582573993303,3.20086047555051,3.3319029110016,\"2\"\n3.45327120054792,-3.17865526226043,3.19739067811612,3.32533093933202,\"2\"\n3.44379059375545,-3.14148478458783,3.19372734156942,3.31875896766244,\"2\"\n3.43451733212974,-3.10431430691522,3.18985665985596,3.31218699599285,\"2\"\n3.42546543645312,-3.06714382924262,3.18576461219341,3.30561502432327,\"2\"\n3.41664891121718,-3.02997335157002,3.18143719409019,3.29904305265369,\"2\"\n3.40808146625138,-2.99280287389741,3.17686069571682,3.2924710809841,\"2\"\n3.3997761962428,-2.95563239622481,3.17202202238624,3.28589910931452,\"2\"\n3.3917452287394,-2.91846191855221,3.16690904655047,3.27932713764493,\"2\"\n3.38399935690596,-2.8812914408796,3.16151097504474,3.27275516597535,\"2\"\n3.37654767844229,-2.844120963207,3.15581871016925,3.26618319430577,\"2\"\n3.36939726562009,-2.8069504855344,3.14982517965228,3.25961122263618,\"2\"\n3.36255289231706,-2.76978000786179,3.14352560961614,3.2530392509666,\"2\"\n3.3560168415145,-2.73260953018919,3.13691771707954,3.24646727929702,\"2\"\n3.34978881086887,-2.69543905251659,3.130001804386,3.23989530762743,\"2\"\n3.34386592530576,-2.65826857484398,3.12278074660994,3.23332333595785,\"2\"\n3.33824285543102,-2.62109809717138,3.11525987314551,3.22675136428827,\"2\"\n3.33291203060415,-2.58392761949878,3.10744675463322,3.22017939261868,\"2\"\n3.3278639274138,-2.54675714182617,3.0993509144844,3.2136074209491,\"2\"\n3.32308740921257,-2.50958666415357,3.09098348934646,3.20703544927952,\"2\"\n3.31857009076737,-2.47241618648096,3.0823568644525,3.20046347760993,\"2\"\n3.31429870369516,-2.43524570880836,3.07348430818554,3.19389150594035,\"2\"\n3.31025944236917,-2.39807523113576,3.06437962617236,3.18731953427076,\"2\"\n3.30643827533586,-2.36090475346315,3.0550568498665,3.18074756260118,\"2\"\n3.30282121295073,-2.32373427579055,3.04552996891247,3.1741755909316,\"2\"\n3.29939452712009,-2.28656379811795,3.03581271140393,3.16760361926201,\"2\"\n3.29614492323867,-2.24939332044534,3.0259183719462,3.16103164759243,\"2\"\n3.29305966744552,-2.21222284277274,3.01585968440018,3.15445967592285,\"2\"\n3.2901266742184,-2.17505236510014,3.00564873428813,3.14788770425326,\"2\"\n3.28733456025954,-2.13788188742753,2.99529690490782,3.14131573258368,\"2\"\n3.28467267082594,-2.10071140975493,2.98481485100226,3.1347437609141,\"2\"\n3.28213108435625,-2.06354093208233,2.97421249413278,3.12817178924451,\"2\"\n3.27970060064599,-2.02637045440972,2.96349903450387,3.12159981757493,\"2\"\n3.27737271707932,-1.98919997673712,2.95268297473137,3.11502784590535,\"2\"\n3.27513959664961,-1.95202949906452,2.94177215182192,3.10845587423576,\"2\"\n3.27299403076263,-1.91485902139191,2.93077377436973,3.10188390256618,\"2\"\n3.27092939915505,-1.87768854371931,2.91969446263814,3.0953119308966,\"2\"\n3.26893962869408,-1.84051806604671,2.90854028975994,3.08873995922701,\"2\"\n3.26701915235496,-1.8033475883741,2.8973168227599,3.08216798755743,\"2\"\n3.26516286929422,-1.7661771107015,2.88602916248146,3.07559601588784,\"2\"\n3.2633661066397,-1.7290066330289,2.87468198179682,3.06902404421826,\"2\"\n3.26162458338909,-1.69183615535629,2.86327956170827,3.06245207254868,\"2\"\n3.25993437663718,-1.65466567768369,2.85182582512101,3.05588010087909,\"2\"\n3.25829189022522,-1.61749520001109,2.8403243681938,3.04930812920951,\"2\"\n3.25669382581525,-1.58032472233848,2.8287784892646,3.04273615753993,\"2\"\n3.25513715632969,-1.54315424466588,2.817191215411,3.03616418587034,\"2\"\n3.25361910165446,-1.50598376699328,2.80556532674705,3.02959221420076,\"2\"\n3.25213710647844,-1.46881328932067,2.79390337858391,3.02302024253118,\"2\"\n3.25068882012729,-1.43164281164807,2.78220772159589,3.01644827086159,\"2\"\n3.24927207824417,-1.39447233397547,2.77048052013985,3.00987629919201,\"2\"\n3.24788488616951,-1.35730185630286,2.75872376887534,3.00330432752243,\"2\"\n3.24652540387595,-1.32013137863026,2.74693930782973,2.99673235585284,\"2\"\n3.24519193232125,-1.28296090095766,2.73512883604526,2.99016038418326,\"2\"\n3.24388290108975,-1.24579042328505,2.7232939239376,2.98358841251367,\"2\"\n3.24259685720216,-1.20861994561245,2.71143602448602,2.97701644084409,\"2\"\n3.24133245498267,-1.17144946793985,2.69955648336634,2.97044446917451,\"2\"\n3.24008844688134,-1.13427899026724,2.68765654812851,2.96387249750492,\"2\"\n3.23886367515899,-1.09710851259464,2.67573737651169,2.95730052583534,\"2\"\n3.23765706435013,-1.05993803492204,2.66380004398139,2.95072855416576,\"2\"\n3.23646761442727,-1.02276755724943,2.65184555056508,2.94415658249617,\"2\"\n3.23529439459772,-0.985597079576829,2.63987482705546,2.93758461082659,\"2\"\n3.23413653767032,-0.948426601904225,2.62788874064369,2.93101263915701,\"2\"\n3.74210049754368,-4.29768548624013,3.33236707016828,3.53723378385598,\"3\"\n3.73232840679936,-4.25856005896695,3.3303344472327,3.53133142701603,\"3\"\n3.72257523956684,-4.21943463169376,3.32828290078533,3.52542907017609,\"3\"\n3.71284212670891,-4.18030920442058,3.32621129996338,3.51952671333614,\"3\"\n3.70313028439325,-4.1411837771474,3.32411842859915,3.5136243564962,\"3\"\n3.69344102153437,-4.10205834987422,3.32200297777813,3.50772199965625,\"3\"\n3.68377574792195,-4.06293292260103,3.31986353771066,3.50181964281631,\"3\"\n3.67413598309221,-4.02380749532785,3.31769858886051,3.49591728597636,\"3\"\n3.66452336600046,-3.98468206805467,3.31550649227237,3.49001492913642,\"3\"\n3.65493966555277,-3.94555664078148,3.31328547904017,3.48411257229647,\"3\"\n3.64538679205345,-3.9064312135083,3.3110336388596,3.47821021545652,\"3\"\n3.63586680962063,-3.86730578623512,3.30874890761253,3.47230785861658,\"3\"\n3.62638194961515,-3.82818035896193,3.30642905393812,3.46640550177663,\"3\"\n3.61693462511608,-3.78905493168875,3.3040716647573,3.46050314493669,\"3\"\n3.60752744645888,-3.74992950441557,3.30167412973461,3.45460078809674,\"3\"\n3.59816323782713,-3.71080407714238,3.29923362468647,3.4486984312568,\"3\"\n3.58884505485496,-3.6716786498692,3.29674709397875,3.44279607441685,\"3\"\n3.57957620315071,-3.63255322259602,3.2942112320031,3.43689371757691,\"3\"\n3.57036025759216,-3.59342779532284,3.29162246388177,3.43099136073696,\"3\"\n3.56120108216472,-3.55430236804965,3.28897692562931,3.42508900389702,\"3\"\n3.55210285001472,-3.51517694077647,3.28627044409942,3.41918664705707,\"3\"\n3.54307006326491,-3.47605151350329,3.28349851716934,3.41328429021713,\"3\"\n3.53410757198732,-3.4369260862301,3.28065629476705,3.40738193337718,\"3\"\n3.52522059154555,-3.39780065895692,3.27773856152892,3.40147957653724,\"3\"\n3.51641471730476,-3.35867523168374,3.27473972208982,3.39557721969729,\"3\"\n3.50769593546469,-3.31954980441055,3.27165379025,3.38967486285734,\"3\"\n3.49907062850578,-3.28042437713737,3.26847438352902,3.3837725060174,\"3\"\n3.49054557346291,-3.24129894986419,3.26519472489199,3.37787014917745,\"3\"\n3.48212793097702,-3.20217352259101,3.26180765369799,3.37196779233751,\"3\"\n3.47382522285282,-3.16304809531782,3.2583056481423,3.36606543549756,\"3\"\n3.46564529571448,-3.12392266804464,3.25468086160076,3.36016307865762,\"3\"\n3.45759626835523,-3.08479724077146,3.25092517528012,3.35426072181767,\"3\"\n3.44968646058702,-3.04567181349827,3.24703026936844,3.34835836497773,\"3\"\n3.44192430188007,-3.00654638622509,3.2429877143955,3.34245600813778,\"3\"\n3.43431821890186,-2.96742095895191,3.23878908369381,3.33655365129784,\"3\"\n3.42687650225818,-2.92829553167872,3.2344260866576,3.33065129445789,\"3\"\n3.41960715430064,-2.88917010440554,3.22989072093525,3.32474893761795,\"3\"\n3.4125177217278,-2.85004467713236,3.2251754398282,3.318846580778,\"3\"\n3.40561511872202,-2.81091924985917,3.22027332915409,3.31294422393806,\"3\"\n3.39890544830355,-2.77179382258599,3.21517828589267,3.30704186709811,\"3\"\n3.39239383115489,-2.73266839531281,3.20988518936144,3.30113951025816,\"3\"\n3.38608425205852,-2.69354296803963,3.20439005477792,3.29523715341822,\"3\"\n3.37997943403138,-2.65441754076644,3.19869015912517,3.28933479657827,\"3\"\n3.37408074907311,-2.61529211349326,3.19278413040354,3.28343243973833,\"3\"\n3.36838817219213,-2.57616668622008,3.18667199360464,3.27753008289838,\"3\"\n3.36290028224906,-2.53704125894689,3.18035516986782,3.27162772605844,\"3\"\n3.35761430955212,-2.49791583167371,3.17383642888487,3.26572536921849,\"3\"\n3.35252622654239,-2.45879040440053,3.1671197982147,3.25982301237855,\"3\"\n3.34763087480835,-2.41966497712734,3.16021043626885,3.2539206555386,\"3\"\n3.34292211945139,-2.38053954985416,3.15311447794593,3.24801829869866,\"3\"\n3.33839302069866,-2.34141412258098,3.14583886301876,3.24211594185871,\"3\"\n3.3340360126366,-2.3022886953078,3.13839115740093,3.23621358501877,\"3\"\n3.32984307985829,-2.26316326803461,3.13077937649935,3.23031122817882,\"3\"\n3.32580592440773,-2.22403784076143,3.12301181827002,3.22440887133888,\"3\"\n3.32191611735068,-2.18491241348825,3.11509691164718,3.21850651449893,\"3\"\n3.31816523131529,-2.14578698621506,3.10704308400268,3.21260415765898,\"3\"\n3.31454495219914,-2.10666155894188,3.09885864943893,3.20670180081904,\"3\"\n3.31104716978928,-2.0675361316687,3.09055171816891,3.20079944397909,\"3\"\n3.30766404822105,-2.02841070439551,3.08213012605724,3.19489708713915,\"3\"\n3.30438807800861,-1.98928527712233,3.0736013825898,3.1889947302992,\"3\"\n3.30121211185266,-1.95015984984915,3.06497263506586,3.18309237345926,\"3\"\n3.29812938663285,-1.91103442257596,3.05625064660577,3.17719001661931,\"3\"\n3.29513353399012,-1.87190899530278,3.04744178556862,3.17128765977937,\"3\"\n3.29221858176396,-1.8327835680296,3.03855202411489,3.16538530293942,\"3\"\n3.28937894832566,-1.79365814075642,3.02958694387329,3.15948294609948,\"3\"\n3.28660943158297,-1.75453271348323,3.02055174693609,3.15358058925953,\"3\"\n3.28390519415656,-1.71540728621005,3.01145127068261,3.14767823241959,\"3\"\n3.28126174596395,-1.67628185893687,3.00229000519533,3.14177587557964,\"3\"\n3.27867492520424,-1.63715643166368,2.99307211227515,3.13587351873969,\"3\"\n3.27614087852478,-1.5980310043905,2.98380144527472,3.12997116189975,\"3\"\n3.27365604096861,-1.55890557711732,2.97448156915099,3.1240688050598,\"3\"\n3.27121711615058,-1.51978014984413,2.96511578028914,3.11816644821986,\"3\"\n3.26882105698619,-1.48065472257095,2.95570712577364,3.11226409137991,\"3\"\n3.26646504719857,-1.44152929529777,2.94625842188137,3.10636173453997,\"3\"\n3.26414648375079,-1.40240386802459,2.93677227164926,3.10045937770002,\"3\"\n3.2618629602911,-1.3632784407514,2.92725108142906,3.09455702086008,\"3\"\n3.25961225165263,-1.32415301347822,2.91769707638763,3.08865466402013,\"3\"\n3.25739229941552,-1.28502758620504,2.90811231494485,3.08275230718019,\"3\"\n3.25520119851473,-1.24590215893185,2.89849870216576,3.07684995034024,\"3\"\n3.25303718485972,-1.20677673165867,2.88885800214088,3.0709475935003,\"3\"\n3.50573523968259,-4.28236231156094,3.28738641176898,3.39656082572578,\"4\"\n3.49405126467221,-4.21588854396407,3.28015478082122,3.38710302274671,\"4\"\n3.48237791240067,-4.1494147763672,3.27291252713462,3.37764521976765,\"4\"\n3.47071587151428,-4.08294100877034,3.26565896206287,3.36818741678858,\"4\"\n3.45906588749848,-4.01646724117347,3.25839334012054,3.35872961380951,\"4\"\n3.44742876817021,-3.94999347357661,3.25111485349068,3.34927181083044,\"4\"\n3.43580538974414,-3.88351970597974,3.24382262595861,3.33981400785137,\"4\"\n3.42419670352951,-3.81704593838287,3.23651570621511,3.33035620487231,\"4\"\n3.41260374331809,-3.75057217078601,3.22919306046838,3.32089840189324,\"4\"\n3.40102763352596,-3.68409840318914,3.22185356430237,3.31144059891417,\"4\"\n3.38946959815286,-3.61762463559228,3.21449599371735,3.3019827959351,\"4\"\n3.37793097062218,-3.55115086799541,3.20711901528989,3.29252499295603,\"4\"\n3.36641320456074,-3.48467710039854,3.19972117539319,3.28306718997696,\"4\"\n3.35491788556943,-3.41820333280168,3.19230088842637,3.2736093869979,\"4\"\n3.34344674402233,-3.35172956520481,3.18485642401532,3.26415158401883,\"4\"\n3.33200166891076,-3.28525579760795,3.17738589316876,3.25469378103976,\"4\"\n3.3205847227167,-3.21878203001108,3.16988723340468,3.24523597806069,\"4\"\n3.30919815725586,-3.15230826241421,3.16235819290739,3.23577817508162,\"4\"\n3.29784443036802,-3.08583449481735,3.15479631383709,3.22632037210255,\"4\"\n3.28652622324906,-3.01936072722048,3.14719891499792,3.21686256912349,\"4\"\n3.27524645810858,-2.95288695962361,3.13956307418026,3.20740476614442,\"4\"\n3.26400831569511,-2.88641319202675,3.13188561063559,3.19794696316535,\"4\"\n3.25281525205132,-2.81993942442988,3.12416306832124,3.18848916018628,\"4\"\n3.24167101364199,-2.75346565683302,3.11639170077244,3.17903135720721,\"4\"\n3.23057964973483,-2.68699188923615,3.10856745872146,3.16957355422815,\"4\"\n3.2195455206138,-2.62051812163928,3.10068598188436,3.16011575124908,\"4\"\n3.20857329987547,-2.55404435404242,3.09274259666455,3.15065794827001,\"4\"\n3.197667968726,-2.48757058644555,3.08473232185588,3.14120014529094,\"4\"\n3.18683479989428,-2.42109681884869,3.07664988472947,3.13174234231187,\"4\"\n3.17607932856479,-2.35462305125182,3.06848975010082,3.1222845393328,\"4\"\n3.16540730768599,-2.28814928365495,3.06024616502148,3.11282673635374,\"4\"\n3.15482464522288,-2.22167551605809,3.05191322152645,3.10336893337467,\"4\"\n3.14433732149759,-2.15520174846122,3.04348493929361,3.0939111303956,\"4\"\n3.13395128579231,-2.08872798086436,3.03495536904075,3.08445332741653,\"4\"\n3.12367233292729,-2.02225421326749,3.02631871594764,3.07499552443746,\"4\"\n3.11350596254775,-1.95578044567062,3.01756948036903,3.06553772145839,\"4\"\n3.10345722621971,-1.88930667807376,3.00870261073894,3.05607991847933,\"4\"\n3.09353056986954,-1.82283291047689,2.99971366113098,3.04662211550026,\"4\"\n3.08372968120835,-1.75635914288003,2.99059894383403,3.03716431252119,\"4\"\n3.07405735309102,-1.68988537528316,2.98135566599322,3.02770650954212,\"4\"\n3.06451537384617,-1.62341160768629,2.97198203927994,3.01824870656305,\"4\"\n3.05510445421818,-1.55693784008943,2.96247735294979,3.00879090358398,\"4\"\n3.04582419771258,-1.49046407249256,2.95284200349725,2.99933310060492,\"4\"\n3.03667311718264,-1.4239903048957,2.94307747806906,2.98987529762585,\"4\"\n3.0276486960642,-1.35751653729883,2.93318629322936,2.98041749464678,\"4\"\n3.01874748850398,-1.29104276970196,2.92317189483144,2.97095969166771,\"4\"\n3.00996524941264,-1.2245690021051,2.91303852796465,2.96150188868864,\"4\"\n3.00129708364809,-1.15809523450823,2.90279108777106,2.95204408570958,\"4\"\n2.99273760321429,-1.09162146691137,2.89243496224672,2.94258628273051,\"4\"\n2.9842810823604,-1.0251476993145,2.88197587714248,2.93312847975144,\"4\"\n2.97592160239232,-0.958673931717633,2.87141975115242,2.92367067677237,\"4\"\n2.96765318039498,-0.892200164120767,2.86077256719163,2.9142128737933,\"4\"\n2.95946987848353,-0.825726396523901,2.85004026314494,2.90475507081423,\"4\"\n2.95136589234088,-0.759252628927035,2.83922864332945,2.89529726783517,\"4\"\n2.94333561948148,-0.692778861330169,2.82834331023071,2.8858394648561,\"4\"\n2.93537370885387,-0.626305093733303,2.81738961490019,2.87638166187703,\"4\"\n2.92747509409095,-0.559831326136437,2.80637262370498,2.86692385889796,\"4\"\n2.91963501302244,-0.493357558539571,2.79529709881534,2.85746605591889,\"4\"\n2.9118490160803,-0.426883790942705,2.78416748979935,2.84800825293982,\"4\"\n2.90411296605372,-0.360410023345838,2.77298793386779,2.83855044996076,\"4\"\n2.89642303136787,-0.293936255748973,2.7617622625955,2.82909264698169,\"4\"\n2.888775674731,-0.227462488152106,2.75049401327424,2.81963484400262,\"4\"\n2.88116763866223,-0.16098872055524,2.73918644338487,2.81017704102355,\"4\"\n2.87359592910171,-0.0945149529583746,2.72784254698725,2.80071923804448,\"4\"\n2.86605779803096,-0.0280411853615083,2.71646507209987,2.79126143506542,\"4\"\n2.85855072579916,0.0384325822353579,2.70505653837353,2.78180363208635,\"4\"\n2.85107240366074,0.104906349832223,2.69361925455381,2.77234582910728,\"4\"\n2.84362071687706,0.17138011742909,2.68215533537937,2.76288802612821,\"4\"\n2.83619372861649,0.237853885025956,2.6706667176818,2.75343022314914,\"4\"\n2.82878966479642,0.304327652622822,2.65915517554373,2.74397242017007,\"4\"\n2.8214068999429,0.370801420219688,2.64762233443911,2.73451461719101,\"4\"\n2.81404394409449,0.437275187816554,2.63606968432939,2.72505681421194,\"4\"\n2.80669943074178,0.50374895541342,2.62449859172396,2.71559901123287,\"4\"\n2.79937210577031,0.570222723010287,2.61291031073729,2.7061412082538,\"4\"\n2.7920608173587,0.636696490607152,2.60130599319076,2.69668340527473,\"4\"\n2.78476450677488,0.703170258204018,2.58968669781645,2.68722560229566,\"4\"\n2.77748220000799,0.769644025800885,2.57805339862521,2.6777677993166,\"4\"\n2.7702130001723,0.836117793397751,2.56640699250276,2.66830999633753,\"4\"\n2.7629560806198,0.902591560994616,2.55474830609711,2.65885219335846,\"4\"\n2.75571067870044,0.969065328591483,2.54307810205834,2.64939439037939,\"4\"\n3.41534718377044,-4.33437998120653,3.15091544588873,3.28313131482958,\"5\"\n3.41043749883871,-4.2616618469354,3.15118906043807,3.28081327963839,\"5\"\n3.40554219854053,-4.18894371266426,3.15144829035386,3.2784952444472,\"5\"\n3.40066217631263,-4.11622557839313,3.15169224219938,3.27617720925601,\"5\"\n3.39579839515213,-4.043507444122,3.1519199529775,3.27385917406481,\"5\"\n3.39095189380495,-3.97078930985086,3.1521303839423,3.27154113887362,\"5\"\n3.38612379352107,-3.89807117557973,3.15232241384379,3.26922310368243,\"5\"\n3.38131530541883,-3.8253530413086,3.15249483156364,3.26690506849124,\"5\"\n3.37652773849927,-3.75263490703747,3.15264632810082,3.26458703330005,\"5\"\n3.37176250834927,-3.67991677276633,3.15277548786844,3.26226899810885,\"5\"\n3.36702114656666,-3.6071986384952,3.15288077926866,3.25995096291766,\"5\"\n3.36230531093278,-3.53448050422407,3.15296054452016,3.25763292772647,\"5\"\n3.35761679634523,-3.46176236995294,3.15301298872532,3.25531489253528,\"5\"\n3.35295754650632,-3.3890442356818,3.15303616818185,3.25299685734409,\"5\"\n3.34832966633784,-3.31632610141067,3.15302797796795,3.2506788221529,\"5\"\n3.34373543505998,-3.24360796713954,3.15298613886342,3.2483607869617,\"5\"\n3.33917731982826,-3.1708898328684,3.15290818371276,3.24604275177051,\"5\"\n3.33465798976539,-3.09817169859727,3.15279144339325,3.24372471657932,\"5\"\n3.33018033015259,-3.02545356432614,3.15263303262366,3.24140668138813,\"5\"\n3.32574745645403,-2.95273543005501,3.15242983593984,3.23908864619694,\"5\"\n3.3213627277365,-2.88001729578387,3.15217849427499,3.23677061100574,\"5\"\n3.31702975891254,-2.80729916151274,3.15187539271657,3.23445257581455,\"5\"\n3.31275243107829,-2.73458102724161,3.15151665016843,3.23213454062336,\"5\"\n3.30853489903876,-2.66186289297047,3.15109811182558,3.22981650543217,\"5\"\n3.30438159491861,-2.58914475869934,3.15061534556334,3.22749847024098,\"5\"\n3.30029722655466,-2.51642662442821,3.15006364354492,3.22518043504979,\"5\"\n3.29628676917404,-2.44370849015708,3.14943803054315,3.22286239985859,\"5\"\n3.29235544870288,-2.37099035588594,3.14873328063192,3.2205443646674,\"5\"\n3.2885087149574,-2.29827222161481,3.14794394399502,3.21822632947621,\"5\"\n3.28475220298504,-2.22555408734368,3.147064385585,3.21590829428502,\"5\"\n3.28109168099634,-2.15283595307254,3.14608883719131,3.21359025909383,\"5\"\n3.27753298370979,-2.08011781880141,3.14501146409548,3.21127222390264,\"5\"\n3.2740819305668,-2.00739968453028,3.14382644685609,3.20895418871144,\"5\"\n3.27074422918727,-1.93468155025915,3.14252807785323,3.20663615352025,\"5\"\n3.26752536561823,-1.86196341598801,3.14111087103989,3.20431811832906,\"5\"\n3.26443048431882,-1.78924528171688,3.13956968195691,3.20200008313787,\"5\"\n3.26146426230446,-1.71652714744575,3.13789983358889,3.19968204794668,\"5\"\n3.25863078325861,-1.64380901317462,3.13609724225236,3.19736401275548,\"5\"\n3.25593341848941,-1.57109087890348,3.13415853663918,3.19504597756429,\"5\"\n3.2533747221271,-1.49837274463235,3.1320811626191,3.1927279423731,\"5\"\n3.25095634773994,-1.42565461036122,3.12986346662388,3.19040990718191,\"5\"\n3.24867899249961,-1.35293647609008,3.12750475148182,3.18809187199072,\"5\"\n3.24654237320173,-1.28021834181895,3.12500530039732,3.18577383679953,\"5\"\n3.24454523603826,-1.20750020754782,3.12236636717841,3.18345580160833,\"5\"\n3.24268539934535,-1.13478207327669,3.11959013348893,3.18113776641714,\"5\"\n3.24095982598576,-1.06206393900555,3.11667963646614,3.17881973122595,\"5\"\n3.23936471992039,-0.989345804734421,3.11363867214913,3.17650169603476,\"5\"\n3.23789564013261,-0.916627670463288,3.11047168155452,3.17418366084357,\"5\"\n3.23654762450372,-0.843909536192155,3.10718362680103,3.17186562565237,\"5\"\n3.23531531646322,-0.771191401921022,3.10377986445915,3.16954759046118,\"5\"\n3.23419308810053,-0.698473267649889,3.10026602243945,3.16722955526999,\"5\"\n3.23317515470522,-0.625755133378757,3.09664788545238,3.1649115200788,\"5\"\n3.2322556771677,-0.553036999107624,3.09293129260752,3.16259348488761,\"5\"\n3.23142885011917,-0.480318864836491,3.08912204927366,3.16027544969642,\"5\"\n3.23068897497088,-0.407600730565358,3.08522585403956,3.15795741450522,\"5\"\n3.23003051804314,-0.334882596294225,3.08124824058492,3.15563937931403,\"5\"\n3.22944815472628,-0.262164462023093,3.0771945335194,3.15332134412284,\"5\"\n3.22893680109956,-0.18944632775196,3.07306981676373,3.15100330893165,\"5\"\n3.22849163468942,-0.116728193480827,3.0688789127915,3.14868527374046,\"5\"\n3.22810810612443,-0.0440100592096941,3.0646263709741,3.14636723854926,\"5\"\n3.22778194339182,0.0287080750614388,3.06031646332432,3.14404920335807,\"5\"\n3.2275091502647,0.101426209332572,3.05595318606906,3.14173116816688,\"5\"\n3.22728600028741,0.174144343603704,3.05154026566397,3.13941313297569,\"5\"\n3.22710902750645,0.246862477874837,3.04708116806255,3.1370950977845,\"5\"\n3.22697501493493,0.31958061214597,3.04257911025168,3.13477706259331,\"5\"\n3.22688098155253,0.392298746417103,3.0380370732517,3.13245902740211,\"5\"\n3.22682416847636,0.465016880688236,3.03345781594549,3.13014099221092,\"5\"\n3.22680202479448,0.537735014959368,3.02884388924498,3.12782295701973,\"5\"\n3.226812193433,0.610453149230501,3.02419765022408,3.12550492182854,\"5\"\n3.22685249732824,0.683171283501633,3.01952127594646,3.12318688663735,\"5\"\n3.22692092609576,0.755889417772766,3.01481677679655,3.12086885144616,\"5\"\n3.22701562332459,0.828607552043899,3.01008600918534,3.11855081625496,\"5\"\n3.22713487457608,0.901325686315032,3.00533068755146,3.11623278106377,\"5\"\n3.22727709612948,0.974043820586164,3.00055239561568,3.11391474587258,\"5\"\n3.22744082448835,1.0467619548573,2.99575259687443,3.11159671068139,\"5\"\n3.2276247066415,1.11948008912843,2.99093264433889,3.1092786754902,\"5\"\n3.22782749105804,1.19219822339956,2.98609378953996,3.106960640299,\"5\"\n3.22804801938589,1.2649163576707,2.98123719082973,3.10464260510781,\"5\"\n3.22828521881729,1.33763449194183,2.97636392101596,3.10232456991662,\"5\"\n3.22853809508087,1.41035262621296,2.97147497436999,3.10000653472543,\"5\"\n3.26696068718529,-3.25165731439258,3.06826111486821,3.16761090102675,\"6\"\n3.26253706526695,-3.22618386254547,3.06729598312178,3.16491652419436,\"6\"\n3.25811941910998,-3.20071041069835,3.06632487561398,3.16222214736198,\"6\"\n3.25370807674041,-3.17523695885124,3.06534746431878,3.1595277705296,\"6\"\n3.24930338930155,-3.14976350700412,3.06436339809288,3.15683339369721,\"6\"\n3.24490573298929,-3.12429005515701,3.06337230074038,3.15413901686483,\"6\"\n3.24051551116784,-3.0988166033099,3.06237376889705,3.15144464003245,\"6\"\n3.2361331566831,-3.07334315146278,3.06136736971703,3.14875026320007,\"6\"\n3.23175913439227,-3.04786969961567,3.06035263834309,3.14605588636768,\"6\"\n3.22739394393007,-3.02239624776855,3.05932907514053,3.1433615095353,\"6\"\n3.22303812273282,-2.99692279592144,3.05829614267301,3.14066713270292,\"6\"\n3.21869224934361,-2.97144934407433,3.05725326239745,3.13797275587053,\"6\"\n3.21435694702242,-2.94597589222721,3.05619981105388,3.13527837903815,\"6\"\n3.21003288768635,-2.9205024403801,3.05513511672518,3.13258400220577,\"6\"\n3.20572079620554,-2.89502898853298,3.05405845454123,3.12988962537338,\"6\"\n3.2014214550803,-2.86955553668587,3.0529690420017,3.127195248541,\"6\"\n3.19713570952424,-2.84408208483876,3.05186603389299,3.12450087170862,\"6\"\n3.19286447297609,-2.81860863299164,3.05074851677638,3.12180649487623,\"6\"\n3.18860873305943,-2.79313518114453,3.04961550302827,3.11911211804385,\"6\"\n3.18436955800439,-2.76766172929741,3.04846592441855,3.11641774121147,\"6\"\n3.18014810353658,-2.7421882774503,3.04729862522159,3.11372336437908,\"6\"\n3.17594562022796,-2.71671482560319,3.04611235486544,3.1110289875467,\"6\"\n3.17176346128765,-2.69124137375607,3.04490576014099,3.10833461071432,\"6\"\n3.16760309075025,-2.66576792190896,3.04367737701362,3.10564023388194,\"6\"\n3.16346609199094,-2.64029447006184,3.04242562210816,3.10294585704955,\"6\"\n3.15935417646097,-2.61482101821473,3.04114878397336,3.10025148021717,\"6\"\n3.15526919249152,-2.58934756636762,3.03984501427806,3.09755710338479,\"6\"\n3.15121313395721,-2.5638741145205,3.0385123191476,3.0948627265524,\"6\"\n3.14718814852155,-2.53840066267339,3.03714855091849,3.09216834972002,\"6\"\n3.14319654510344,-2.51292721082628,3.03575140067183,3.08947397288764,\"6\"\n3.13924080010789,-2.48745375897916,3.03431839200261,3.08677959605525,\"6\"\n3.13532356185485,-2.46198030713205,3.03284687659089,3.08408521922287,\"6\"\n3.13144765252198,-2.43650685528493,3.031334032259,3.08139084239049,\"6\"\n3.12761606679582,-2.41103340343782,3.02977686432039,3.0786964655581,\"6\"\n3.123831966311,-2.38555995159071,3.02817221114044,3.07600208872572,\"6\"\n3.12009866886407,-2.36008649974359,3.02651675492261,3.07330771189334,\"6\"\n3.11641963133669,-2.33461304789648,3.02480703878522,3.07061333506096,\"6\"\n3.11279842527768,-2.30913959604936,3.02303949117946,3.06791895822857,\"6\"\n3.10923870420436,-2.28366614420225,3.02121045858802,3.06522458139619,\"6\"\n3.10574416191999,-2.25819269235514,3.01931624720762,3.06253020456381,\"6\"\n3.10231848153318,-2.23271924050802,3.01735317392966,3.05983582773142,\"6\"\n3.09896527541985,-2.20724578866091,3.01531762637823,3.05714145089904,\"6\"\n3.09568801708331,-2.18177233681379,3.01320613105,3.05444707406666,\"6\"\n3.09248996670717,-2.15629888496668,3.01101542776138,3.05175269723427,\"6\"\n3.08937409308654,-2.13082543311957,3.00874254771724,3.04905832040189,\"6\"\n3.08634299546,-2.10535198127245,3.00638489167902,3.04636394356951,\"6\"\n3.08339882941356,-2.07987852942534,3.00394030406069,3.04366956673712,\"6\"\n3.08054324135301,-2.05440507757822,3.00140713845647,3.04097518990474,\"6\"\n3.07777731592845,-2.02893162573111,2.99878431021627,3.03828081307236,\"6\"\n3.07510154018971,-2.003458173884,2.99607133229024,3.03558643623997,\"6\"\n3.07251578717649,-1.97798472203688,2.99326833163869,3.03289205940759,\"6\"\n3.0700193202148,-1.95251127018977,2.99037604493562,3.03019768257521,\"6\"\n3.06761081758465,-1.92703781834265,2.987395793901,3.02750330574283,\"6\"\n3.06528841566403,-1.90156436649554,2.98432944215685,3.02480892891044,\"6\"\n3.06304976735018,-1.87609091464843,2.98117933680594,3.02211455207806,\"6\"\n3.06089211166978,-1.85061746280131,2.97794823882158,3.01942017524568,\"6\"\n3.05881235008961,-1.8251440109542,2.97463924673698,3.01672579841329,\"6\"\n3.05680712511797,-1.79967055910708,2.97125571804385,3.01403142158091,\"6\"\n3.05487289726144,-1.77419710725997,2.96780119223562,3.01133704474853,\"6\"\n3.0530060171451,-1.74872365541286,2.96427931868718,3.00864266791614,\"6\"\n3.05120279047823,-1.72325020356574,2.96069379168929,3.00594829108376,\"6\"\n3.04945953442822,-1.69777675171863,2.95704829407454,3.00325391425138,\"6\"\n3.04777262475987,-1.67230329987151,2.95334645007812,3.00055953741899,\"6\"\n3.0461385337486,-1.6468298480244,2.94959178742462,2.99786516058661,\"6\"\n3.04455385936182,-1.62135639617729,2.94578770814663,2.99517078375423,\"6\"\n3.0430153465263,-1.59588294433017,2.94193746731739,2.99247640692185,\"6\"\n3.04151990148043,-1.57040949248306,2.93804415869849,2.98978203008946,\"6\"\n3.04006460027787,-1.54493604063594,2.93411070623629,2.98708765325708,\"6\"\n3.03864669249288,-1.51946258878883,2.93013986035652,2.9843932764247,\"6\"\n3.03726360110631,-1.49398913694172,2.92613419807832,2.98169889959231,\"6\"\n3.03591291944671,-1.4685156850946,2.92209612607315,2.97900452275993,\"6\"\n3.03459240594233,-1.44304223324749,2.91802788591277,2.97631014592755,\"6\"\n3.03329997731852,-1.41756878140037,2.91393156087181,2.97361576909516,\"6\"\n3.03203370076047,-1.39209532955326,2.9098090837651,2.97092139226278,\"6\"\n3.03079178545661,-1.36662187770615,2.90566224540419,2.9682270154304,\"6\"\n3.0295725738478,-1.34114842585903,2.90149270334822,2.96553263859801,\"6\"\n3.02837453282991,-1.31567497401192,2.89730199070135,2.96283826176563,\"6\"\n3.0271962450936,-1.2902015221648,2.8930915247729,2.96014388493325,\"6\"\n3.02603640073338,-1.26472807031769,2.88886261546835,2.95744950810086,\"6\"\n3.02489378921647,-1.23925461847058,2.8846164733205,2.95475513126848,\"6\"\n3.60305542558526,-3.34189127576475,3.09164661414485,3.34735101986506,\"7\"\n3.59656664978974,-3.31221064252455,3.09460023066101,3.34558344022537,\"7\"\n3.59010556405566,-3.28253000928434,3.09752615711572,3.34381586058569,\"7\"\n3.58367378200293,-3.25284937604414,3.10042277988909,3.34204828094601,\"7\"\n3.57727303379891,-3.22316874280394,3.10328836881374,3.34028070130632,\"7\"\n3.57090517560381,-3.19348810956373,3.10612106772947,3.33851312166664,\"7\"\n3.56457219977267,-3.16380747632353,3.10891888428124,3.33674554202696,\"7\"\n3.55827624585497,-3.13412684308333,3.11167967891958,3.33497796238728,\"7\"\n3.55201961242779,-3.10444620984312,3.11440115306739,3.33321038274759,\"7\"\n3.5458047697915,-3.07476557660292,3.11708083642432,3.33144280310791,\"7\"\n3.5396343735457,-3.04508494336272,3.11971607339075,3.32967522346823,\"7\"\n3.53351127904846,-3.01540431012251,3.12230400860863,3.32790764382854,\"7\"\n3.52743855674064,-2.98572367688231,3.12484157163708,3.32614006418886,\"7\"\n3.52141950829017,-2.95604304364211,3.12732546080818,3.32437248454918,\"7\"\n3.51545768347475,-2.9263624104019,3.12975212634424,3.32260490490949,\"7\"\n3.50955689767575,-2.8966817771617,3.13211775286387,3.32083732526981,\"7\"\n3.50372124979787,-2.8670011439215,3.13441824146239,3.31906974563013,\"7\"\n3.49795514035657,-2.8373205106813,3.13664919162432,3.31730216599045,\"7\"\n3.49226328938689,-2.80763987744109,3.13880588331463,3.31553458635076,\"7\"\n3.48665075372024,-2.77795924420089,3.14088325970192,3.31376700671108,\"7\"\n3.48112294304895,-2.74827861096069,3.14287591109384,3.3119994270714,\"7\"\n3.47568563405167,-2.71859797772048,3.14477806081176,3.31023184743171,\"7\"\n3.47034498168575,-2.68891734448028,3.14658355389831,3.30846426779203,\"7\"\n3.46510752657001,-2.65923671124008,3.14828584973468,3.30669668815235,\"7\"\n3.45998019718741,-2.62955607799987,3.14987801983792,3.30492910851266,\"7\"\n3.45497030544353,-2.59987544475967,3.15135275230243,3.30316152887298,\"7\"\n3.45008553393822,-2.57019481151947,3.15270236452838,3.3013939492333,\"7\"\n3.44533391316587,-2.54051417827926,3.15391882602136,3.29962636959361,\"7\"\n3.44072378678398,-2.51083354503906,3.15499379312389,3.29785878995393,\"7\"\n3.43626376311338,-2.48115291179886,3.15591865751511,3.29609121031425,\"7\"\n3.43196265119851,-2.45147227855866,3.15668461015062,3.29432363067457,\"7\"\n3.42782938010216,-2.42179164531845,3.1572827219676,3.29255605103488,\"7\"\n3.42387290067484,-2.39211101207825,3.15770404211556,3.2907884713952,\"7\"\n3.42010206984763,-2.36243037883805,3.1579397136634,3.28902089175552,\"7\"\n3.41652551855223,-2.33274974559784,3.15798110567943,3.28725331211583,\"7\"\n3.41315150563968,-2.30306911235764,3.15781995931262,3.28548573247615,\"7\"\n3.40998776157719,-2.27338847911744,3.15744854409574,3.28371815283647,\"7\"\n3.40704132712856,-2.24370784587723,3.15685981926501,3.28195057319678,\"7\"\n3.40431839350607,-2.21402721263703,3.15604759360813,3.2801829935571,\"7\"\n3.40182415143365,-2.18434657939683,3.15500667640119,3.27841541391742,\"7\"\n3.39956265699574,-2.15466594615662,3.15373301155973,3.27664783427773,\"7\"\n3.39753672191896,-2.12498531291642,3.15222378735714,3.27488025463805,\"7\"\n3.39574783497078,-2.09530467967622,3.15047751502596,3.27311267499837,\"7\"\n3.39419611949227,-2.06562404643602,3.1484940712251,3.27134509535869,\"7\"\n3.39288032985344,-2.03594341319581,3.14627470158457,3.269577515719,\"7\"\n3.39179788706594,-2.00626277995561,3.1438219850927,3.26780993607932,\"7\"\n3.39094495120787,-1.97658214671541,3.1411397616714,3.26604235643964,\"7\"\n3.39031652600792,-1.9469015134752,3.13823302759199,3.26427477679995,\"7\"\n3.38990658915472,-1.917220880235,3.13510780516582,3.26250719716027,\"7\"\n3.38970824080276,-1.8875402469948,3.13177099423842,3.26073961752059,\"7\"\n3.38971386238871,-1.85785961375459,3.1282302133731,3.2589720378809,\"7\"\n3.3899152781978,-1.82817898051439,3.12449363828464,3.25720445824122,\"7\"\n3.39030391299117,-1.79849834727419,3.12056984421191,3.25543687860154,\"7\"\n3.39087094024267,-1.76881771403398,3.11646765768104,3.25366929896186,\"7\"\n3.39160741694697,-1.73913708079378,3.11219602169737,3.25190171932217,\"7\"\n3.39250440238192,-1.70945644755358,3.10776387698306,3.25013413968249,\"7\"\n3.39355305950802,-1.67977581431337,3.10318006057759,3.24836656004281,\"7\"\n3.39474473878456,-1.65009518107317,3.09845322202168,3.24659898040312,\"7\"\n3.3960710450352,-1.62041454783297,3.09359175649168,3.24483140076344,\"7\"\n3.39752388860423,-1.59073391459277,3.08860375364328,3.24306382112376,\"7\"\n3.39909552242757,-1.56105328135256,3.08349696054057,3.24129624148407,\"7\"\n3.40077856683688,-1.53137264811236,3.0782787568519,3.23952866184439,\"7\"\n3.40256602396155,-1.50169201487216,3.07295614044787,3.23776108220471,\"7\"\n3.40445128353241,-1.47201138163195,3.06753572159765,3.23599350256503,\"7\"\n3.40642812175863,-1.44233074839175,3.06202372409205,3.23422592292534,\"7\"\n3.40849069477587,-1.41265011515155,3.05642599179545,3.23245834328566,\"7\"\n3.41063352797096,-1.38296948191134,3.05074799932099,3.23069076364598,\"7\"\n3.41285150229428,-1.35328884867114,3.04499486571831,3.22892318400629,\"7\"\n3.41513983848523,-1.32360821543094,3.03917137024799,3.22715560436661,\"7\"\n3.4174940799664,-1.29392758219073,3.03328196948745,3.22538802472693,\"7\"\n3.41991007501141,-1.26424694895053,3.02733081516307,3.22362044508724,\"7\"\n3.42238395866119,-1.23456631571033,3.02132177223394,3.22185286544756,\"7\"\n3.42491213475284,-1.20488568247013,3.01525843686291,3.22008528580788,\"7\"\n3.42749125833381,-1.17520504922992,3.00914415400257,3.21831770616819,\"7\"\n3.43011821865847,-1.14552441598972,3.00298203439855,3.21655012652851,\"7\"\n3.43279012290413,-1.11584378274952,2.99677497087352,3.21478254688883,\"7\"\n3.43550428069519,-1.08616314950931,2.9905256538031,3.21301496724915,\"7\"\n3.43825818948661,-1.05648251626911,2.98423658573231,3.21124738760946,\"7\"\n3.44104952082901,-1.02680188302891,2.97791009511055,3.20947980796978,\"7\"\n3.44387610751592,-0.997121249788704,2.97154834914427,3.2077122283301,\"7\"\n3.30768539479099,-2.27031153244375,2.77945461394602,3.04357000436851,\"8\"\n3.30984936055472,-2.24755202301456,2.79006585104164,3.04995760579818,\"8\"\n3.31203013589353,-2.22479251358537,2.80066027856219,3.05634520722786,\"8\"\n3.31422856434147,-2.20203300415618,2.8112370529736,3.06273280865754,\"8\"\n3.31644554292094,-2.17927349472699,2.82179527725349,3.06912041008721,\"8\"\n3.31868202608121,-2.15651398529779,2.83233399695257,3.07550801151689,\"8\"\n3.32093902994725,-2.1337544758686,2.84285219594589,3.08189561294657,\"8\"\n3.32321763690159,-2.11099496643941,2.8533487918509,3.08828321437625,\"8\"\n3.32551900052306,-2.08823545701022,2.86382263108878,3.09467081580592,\"8\"\n3.32784435090655,-2.06547594758103,2.87427248356465,3.1010584172356,\"8\"\n3.3301950003885,-2.04271643815184,2.88469703694206,3.10744601866528,\"8\"\n3.33257234970264,-2.01995692872265,2.89509489048727,3.11383362009495,\"8\"\n3.33497789458966,-1.99719741929346,2.9054645484596,3.12022122152463,\"8\"\n3.33741323288319,-1.97443790986427,2.91580441302543,3.12660882295431,\"8\"\n3.33988007209179,-1.95167840043508,2.92611277667618,3.13299642438399,\"8\"\n3.34238023749318,-1.92891889100588,2.93638781413415,3.13938402581366,\"8\"\n3.34491568075112,-1.90615938157669,2.94662757373557,3.14577162724334,\"8\"\n3.34748848905841,-1.8833998721475,2.95682996828763,3.15215922867302,\"8\"\n3.35010089479939,-1.86064036271831,2.96699276540601,3.1585468301027,\"8\"\n3.35275528571227,-1.83788085328912,2.97711357735247,3.16493443153237,\"8\"\n3.35545421551533,-1.81512134385993,2.98718985040877,3.17132203296205,\"8\"\n3.35820041493945,-1.79236183443074,2.997218853844,3.17770963439173,\"8\"\n3.36099680308358,-1.76960232500155,3.00719766855923,3.1840972358214,\"8\"\n3.36384649897674,-1.74684281557236,3.01712317552542,3.19048483725108,\"8\"\n3.36675283319098,-1.72408330614316,3.02699204417054,3.19687243868076,\"8\"\n3.36971935930145,-1.70132379671397,3.03680072091943,3.20326004011044,\"8\"\n3.37274986493405,-1.67856428728478,3.04654541814618,3.20964764154011,\"8\"\n3.37584838207444,-1.65580477785559,3.05622210386514,3.21603524296979,\"8\"\n3.37901919623705,-1.6330452684264,3.06582649256188,3.22242284439947,\"8\"\n3.3822668540079,-1.61028575899721,3.07535403765039,3.22881044582914,\"8\"\n3.38559616838183,-1.58752624956802,3.08479992613582,3.23519804725882,\"8\"\n3.38901222121687,-1.56476674013883,3.09415907616013,3.2415856486885,\"8\"\n3.39252036202816,-1.54200723070964,3.1034261382082,3.24797325011818,\"8\"\n3.39612620224927,-1.51924772128045,3.11259550084644,3.25436085154785,\"8\"\n3.39983560400665,-1.49648821185125,3.12166130194841,3.26074845297753,\"8\"\n3.4036546623961,-1.47372870242206,3.13061744641832,3.26713605440721,\"8\"\n3.40758968023149,-1.45096919299287,3.13945763144228,3.27352365583689,\"8\"\n3.41164713427298,-1.42820968356368,3.14817538026015,3.27991125726656,\"8\"\n3.41583363205169,-1.40545017413449,3.15676408534079,3.28629885869624,\"8\"\n3.42015585860952,-1.3826906647053,3.16521706164232,3.29268646012592,\"8\"\n3.4246205127807,-1.35993115527611,3.17352761033049,3.29907406155559,\"8\"\n3.4292342330668,-1.33717164584692,3.18168909290374,3.30546166298527,\"8\"\n3.4340035136979,-1.31441213641773,3.189695015132,3.31184926441495,\"8\"\n3.43893461211646,-1.29165262698853,3.1975391195728,3.31823686584463,\"8\"\n3.44403344983525,-1.26889311755934,3.20521548471336,3.3246244672743,\"8\"\n3.44930550935606,-1.24613360813015,3.2127186280519,3.33101206870398,\"8\"\n3.45475573052381,-1.22337409870096,3.22004360974351,3.33739967013366,\"8\"\n3.46038841025078,-1.20061458927177,3.22718613287589,3.34378727156333,\"8\"\n3.46620710989314,-1.17785507984258,3.23414263609288,3.35017487299301,\"8\"\n3.47221457462329,-1.15509557041339,3.24091037422209,3.35656247442269,\"8\"\n3.47841266886627,-1.1323360609842,3.24748748283846,3.36295007585237,\"8\"\n3.48480233124205,-1.10957655155501,3.25387302332204,3.36933767728204,\"8\"\n3.49138355150981,-1.08681704212582,3.26006700591363,3.37572527871172,\"8\"\n3.49815537082086,-1.06405753269662,3.26607038946193,3.3821128801414,\"8\"\n3.50511590526569,-1.04129802326743,3.27188505787646,3.38850048157108,\"8\"\n3.51226239138116,-1.01853851383824,3.27751377462034,3.39488808300075,\"8\"\n3.51959125109859,-0.995779004409051,3.28296011776227,3.40127568443043,\"8\"\n3.52709817267364,-0.97301949497986,3.28822839904658,3.40766328586011,\"8\"\n3.53477820352036,-0.950259985550669,3.29332357105921,3.41405088728978,\"8\"\n3.54262585060373,-0.927500476121478,3.29825112683519,3.42043848871946,\"8\"\n3.55063518411304,-0.904740966692287,3.30301699618524,3.42682609014914,\"8\"\n3.55879994049171,-0.881981457263096,3.30762744266592,3.43321369157882,\"8\"\n3.56711362146258,-0.859221947833905,3.3120889645544,3.43960129300849,\"8\"\n3.57556958637752,-0.836462438404714,3.31640820249882,3.44598889443817,\"8\"\n3.58416113595576,-0.813702928975523,3.32059185577994,3.45237649586785,\"8\"\n3.59288158618933,-0.790943419546332,3.32464660840572,3.45876409729753,\"8\"\n3.60172433183542,-0.768183910117141,3.32857906561898,3.4651516987272,\"8\"\n3.61068289945452,-0.74542440068795,3.33239570085923,3.47153930015688,\"8\"\n3.61975099037552,-0.722664891258759,3.33610281279759,3.47792690158656,\"8\"\n3.62892251427446,-0.699905381829567,3.33970649175801,3.48431450301623,\"8\"\n3.6381916142533,-0.677145872400376,3.34321259463853,3.49070210444591,\"8\"\n3.64755268441287,-0.654386362971185,3.34662672733831,3.49708970587559,\"8\"\n3.65700038094998,-0.631626853541994,3.34995423366055,3.50347730730527,\"8\"\n3.66652962778891,-0.608867344112803,3.35320018968098,3.50986490873494,\"8\"\n3.67613561769983,-0.586107834683612,3.35636940262941,3.51625251016462,\"8\"\n3.68581380977479,-0.563348325254421,3.3594664134138,3.5226401115943,\"8\"\n3.69555992403616,-0.54058881582523,3.36249550201179,3.52902771302397,\"8\"\n3.70536993385318,-0.517829306396039,3.36546069505413,3.53541531445365,\"8\"\n3.71524005674362,-0.495069796966848,3.36836577502304,3.54180291588333,\"8\"\n3.72516674404494,-0.472310287537657,3.37121429058108,3.54819051731301,\"8\"\n3.35311952905792,0.866499466770358,3.0756832551877,3.21440139212281,\"24\"\n3.33228577204311,0.912345915523137,3.062124914417,3.19720534323006,\"24\"\n3.31147839012362,0.958192364275915,3.04854019855098,3.1800092943373,\"24\"\n3.29069961770684,1.00403881302869,3.03492687318225,3.16281324544455,\"24\"\n3.26995192797842,1.04988526178147,3.02128246512516,3.14561719655179,\"24\"\n3.24923806217202,1.09573171053425,3.00760423314604,3.12842114765903,\"24\"\n3.2285610625239,1.14157815928703,2.99388913500866,3.11122509876628,\"24\"\n3.2079243092762,1.18742460803981,2.98013379047085,3.09402904987352,\"24\"\n3.18733156207031,1.23327105679258,2.96633443989123,3.07683300098077,\"24\"\n3.16678700601381,1.27911750554536,2.95248689816222,3.05963695208801,\"24\"\n3.14629530259521,1.32496395429814,2.93858650379531,3.04244090319526,\"24\"\n3.12586164543666,1.37081040305092,2.92462806316835,3.0252448543025,\"24\"\n3.10549182058542,1.4166568518037,2.91060579023408,3.00804880540975,\"24\"\n3.08519227061316,1.46250330055648,2.89651324242082,2.99085275651699,\"24\"\n3.06497016116895,1.50834974930925,2.88234325407953,2.97365670762424,\"24\"\n3.04483344776524,1.55419619806203,2.86808786969773,2.95646065873148,\"24\"\n3.02479093940999,1.60004264681481,2.85373828026747,2.93926460983873,\"24\"\n3.00485235418622,1.64588909556759,2.83928476770573,2.92206856094597,\"24\"\n2.98502836000917,1.69173554432037,2.82471666409726,2.90487251205322,\"24\"\n2.96533059160716,1.73758199307315,2.81002233471376,2.88767646316046,\"24\"\n2.94577163243476,1.78342844182592,2.79518919610065,2.87048041426771,\"24\"\n2.92636494806506,1.8292748905787,2.78020378268485,2.85328436537495,\"24\"\n2.9071247561773,1.87512133933148,2.7650518767871,2.8360883164822,\"24\"\n2.88806581837129,1.92096778808426,2.74971871680759,2.81889226758944,\"24\"\n2.86920314171919,1.96681423683704,2.73418929567418,2.80169621869669,\"24\"\n2.85055158424108,2.01266068558982,2.71844875536679,2.78450016980393,\"24\"\n2.83212536902715,2.05850713434259,2.7024828727952,2.76730412091118,\"24\"\n2.81393752627319,2.10435358309537,2.68627861776366,2.75010807201842,\"24\"\n2.79599929932772,2.15020003184815,2.66982474692362,2.73291202312567,\"24\"\n2.77831956655564,2.19604648060093,2.65311238191019,2.71571597423291,\"24\"\n2.76090434078031,2.24189292935371,2.6361355099,2.69851992534016,\"24\"\n2.74375640781442,2.28773937810648,2.61889134508038,2.6813238764474,\"24\"\n2.72687515276427,2.33358582685926,2.60138050234503,2.66412782755465,\"24\"\n2.71025659870911,2.37943227561204,2.58360695861467,2.64693177866189,\"24\"\n2.69389365221094,2.42527872436482,2.56557780732734,2.62973572976914,\"24\"\n2.67777652135066,2.4711251731176,2.5473028404021,2.61253968087638,\"24\"\n2.66189325153979,2.51697162187038,2.52879401242747,2.59534363198363,\"24\"\n2.64623031611516,2.56281807062315,2.51006485006658,2.57814758309087,\"24\"\n2.63077320256852,2.60866451937593,2.49112986582771,2.56095153419812,\"24\"\n2.61550694784781,2.65451096812871,2.47200402276291,2.54375548530536,\"24\"\n2.60041659276809,2.70035741688149,2.45270228005712,2.52655943641261,\"24\"\n2.5854875418852,2.74620386563427,2.4332392331545,2.50936338751985,\"24\"\n2.57070582839959,2.79205031438705,2.4136288488546,2.4921673386271,\"24\"\n2.5560582926302,2.83789676313982,2.39388428683848,2.47497128973434,\"24\"\n2.54153268747475,2.8837432118926,2.37401779420842,2.45777524084158,\"24\"\n2.52711772590546,2.92958966064538,2.3540406579922,2.44057919194883,\"24\"\n2.51280308499971,2.97543610939816,2.33396320111244,2.42338314305607,\"24\"\n2.498579379231,3.02128255815094,2.31379480909564,2.40618709416332,\"24\"\n2.48443811346983,3.06712900690371,2.2935439770713,2.38899104527056,\"24\"\n2.47037162383395,3.11297545565649,2.27321836892167,2.37179499637781,\"24\"\n2.45637301244466,3.15882190440927,2.25282488252544,2.35459894748505,\"24\"\n2.44243608040244,3.20466835316205,2.23236971678216,2.3374028985923,\"24\"\n2.42855526191175,3.25051480191483,2.21185843748734,2.32020684969954,\"24\"\n2.41472556143237,3.29636125066761,2.1912960401812,2.30301080080679,\"24\"\n2.4009424949621,3.34220769942038,2.17068700886596,2.28581475191403,\"24\"\n2.38720203600714,3.38805414817316,2.15003537003541,2.26861870302128,\"24\"\n2.3735005664211,3.43390059692594,2.12934474183594,2.25142265412852,\"24\"\n2.35983483204586,3.47974704567872,2.10861837842567,2.23422660523577,\"24\"\n2.34620190293359,3.5255934944315,2.08785920975244,2.21703055634301,\"24\"\n2.33259913784048,3.57143994318428,2.06706987706004,2.19983450745026,\"24\"\n2.31902415263963,3.61728639193705,2.04625276447537,2.1826384585575,\"24\"\n2.30547479228699,3.66313284068983,2.02541002704251,2.16544240966475,\"24\"\n2.29194910598043,3.70897928944261,2.00454361556355,2.14824636077199,\"24\"\n2.27844532517008,3.75482573819539,1.9836552985884,2.13105031187924,\"24\"\n2.26496184410191,3.80067218694817,1.96274668187106,2.11385426298648,\"24\"\n2.25149720260465,3.84651863570095,1.9418192255828,2.09665821409373,\"24\"\n2.23805007085775,3.89236508445372,1.9208742595442,2.07946216520097,\"24\"\n2.22461923590591,3.9382115332065,1.89991299671053,2.06226611630822,\"24\"\n2.21120358971179,3.98405798195928,1.87893654511913,2.04507006741546,\"24\"\n2.19780211856226,4.02990443071206,1.85794591848315,2.02787401852271,\"24\"\n2.18441389366555,4.07575087946484,1.83694204559435,2.01067796962995,\"24\"\n2.17103806279637,4.12159732821762,1.81592577867802,1.9934819207372,\"24\"\n2.15767384286335,4.16744377697039,1.79489790082553,1.97628587184444,\"24\"\n2.14432051328878,4.21329022572317,1.77385913261459,1.95908982295169,\"24\"\n2.13097741010432,4.25913667447595,1.75281013801355,1.94189377405893,\"24\"\n2.11764392067807,4.30498312322873,1.73175152965428,1.92469772516618,\"24\"\n2.10431947899936,4.35082957198151,1.71068387354748,1.90750167627342,\"24\"\n2.09100356145624,4.39667602073428,1.68960769330509,1.89030562738067,\"24\"\n2.07769568304911,4.44252246948706,1.66852347392671,1.87310957848791,\"24\"\n2.06439539399067,4.48836891823984,1.64743166519964,1.85591352959516,\"24\""
        },
        {
            "name": ".0/group_by1/model_prediction2",
            "source": ".0/group_by1/model_prediction2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.rad"
                    ]
                }
            ]
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"1\"\n\"2\"\n\"3\"\n\"4\"\n\"5\"\n\"6\"\n\"7\"\n\"8\"\n\"24\""
        },
        {
            "name": "scale/stroke",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"1\"\n\"2\"\n\"3\"\n\"4\"\n\"5\"\n\"6\"\n\"7\"\n\"8\"\n\"24\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-5.54165632027653\n4.965989167693"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n1.4943086577844\n4.02715226007785"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "stroke",
            "type": "ordinal",
            "domain": {
                "data": "scale/stroke",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "symbol",
            "properties": {
                "update": {
                    "size": {
                        "value": 50
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.log_crim"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.log_medv"
                    },
                    "fill": {
                        "scale": "fill",
                        "field": "data.rad"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0"
                    }
                }
            },
            "from": {
                "data": ".0"
            }
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/model_prediction2"
            },
            "marks": [
                {
                    "type": "area",
                    "properties": {
                        "update": {
                            "fill": {
                                "value": "#333333"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.resp_upr_"
                            },
                            "fillOpacity": {
                                "value": 0.2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_lwr_"
                            },
                            "stroke": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/model_prediction2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/model_prediction2"
            },
            "marks": [
                {
                    "type": "line",
                    "properties": {
                        "update": {
                            "strokeWidth": {
                                "value": 2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_"
                            },
                            "stroke": {
                                "scale": "stroke",
                                "field": "data.rad"
                            },
                            "fill": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/model_prediction2"
                            }
                        }
                    }
                }
            ]
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "rad"
        },
        {
            "orient": "right",
            "stroke": "stroke",
            "title": "rad"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "log_crim"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "log_medv"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id694102749").parseSpec(plot_id694102749_spec);
</script><!--/html_preserve-->

Coloring points by `rad` illuminates the issues mentioned in
problem 2.  We can see that most of the slopes are relatively similar,
with two major exceptions, telling us we have an interaction.

When `rad` $=8$ we actually see a positive linear relationship, which
was buried in the noise of the full data set above.  This could again
be caused by yet a fourth variable, or simply noise because of our
small sample sizes. Subsetting the data and comparing points by
`chas` offers no explanation.  Either this radial highway score
naturally changes our relationship, or we have a confounding variable
other than `chas`.


```r
summary(rad)
```

```
##   1   2   3   4   5   6   7   8  24 
##  20  24  38 110 115  26  17  24 132
```

```r
Boston %>% filter(rad == 8) %>%
    ggvis(~log_crim, ~log_medv) %>%
    layer_points(fill = ~chas)
```

<!--html_preserve--><div id="plot_id773436083-container" class="ggvis-output-container">
<div id="plot_id773436083" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id773436083_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id773436083" data-renderer="svg">SVG</a>
 | 
<a id="plot_id773436083_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id773436083" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id773436083_download" class="ggvis-download" data-plot-id="plot_id773436083">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id773436083_spec = {
    "data": [
        {
            "name": ".0",
            "format": {
                "type": "csv",
                "parse": {
                    "log_crim": "number",
                    "log_medv": "number"
                }
            },
            "values": "\"log_crim\",\"log_medv\",\"chas\"\n-1.86788485961755,3.14845336057165,\"Not on River\"\n-2.27031153244375,2.97552956623647,\"Not on River\"\n-1.90166362493575,2.92852352386054,\"Not on River\"\n-1.76194827165643,2.77258872223978,\"Not on River\"\n-2.20482337521155,3.10009228887823,\"Not on River\"\n-2.06751297081456,3.2188758248682,\"Not on River\"\n-1.02697092752823,3.2846635654062,\"On River\"\n-0.897199141618634,3.07731226054641,\"On River\"\n-0.472310287537657,3.31418600467253,\"On River\"\n-0.486620935069173,3.40452517175483,\"Not on River\"\n-1.15413556947876,3.80220813942094,\"Not on River\"\n-0.640687566587583,3.91202300542815,\"Not on River\"\n-0.961968245370808,3.62700405039585,\"Not on River\"\n-0.885810024620568,3.45315712059287,\"Not on River\"\n-1.21002441175437,3.84374416467485,\"Not on River\"\n-0.816943258373457,3.44998754583159,\"Not on River\"\n-0.621757184473272,3.1904763503465,\"Not on River\"\n-0.770114621716554,3.45631668088323,\"Not on River\"\n-0.552881017499318,3.73050112880476,\"Not on River\"\n-1.10421797118894,3.87743156065853,\"Not on River\"\n-0.803162959605968,3.36729582998647,\"On River\"\n-1.10729991706568,3.17805383034795,\"Not on River\"\n-0.652811704370542,3.22286784613774,\"On River\"\n-0.669762740327209,3.44998754583159,\"Not on River\""
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-2.36021159468905\n-0.382410225292352"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n2.71561700808036\n3.96899471958756"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "symbol",
            "properties": {
                "update": {
                    "size": {
                        "value": 50
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.log_crim"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.log_medv"
                    },
                    "fill": {
                        "scale": "fill",
                        "field": "data.chas"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0"
                    }
                }
            },
            "from": {
                "data": ".0"
            }
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "chas"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "log_crim"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "log_medv"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id773436083").parseSpec(plot_id773436083_spec);
</script><!--/html_preserve-->

Most telling, however, is what we see when `rad` $=24$. This subset
makes up almost the entirety of the problems discussed in question 2.
We can see that the change in slope once `log_crim` $> 1$ is in fact
interaction with `rad` rather than a quadratic relationship.

b) a) Fill (color) the points according to the Charles River (`chas`). Group by `chas` and fit linear models. Does there appear to be an interaction between `chas` and `log_crim`? Discuss.

```r
Boston %>% ggvis(~log_crim, ~log_medv) %>%
    layer_points(fill = ~chas) %>%
    group_by(chas) %>%
    layer_model_predictions(model="lm", se=TRUE)
```

```
## Guessing formula = log_medv ~ log_crim
```

<!--html_preserve--><div id="plot_id617908707-container" class="ggvis-output-container">
<div id="plot_id617908707" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id617908707_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id617908707" data-renderer="svg">SVG</a>
 | 
<a id="plot_id617908707_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id617908707" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id617908707_download" class="ggvis-download" data-plot-id="plot_id617908707">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id617908707_spec = {
    "data": [
        {
            "name": ".0",
            "format": {
                "type": "csv",
                "parse": {
                    "log_crim": "number",
                    "log_medv": "number"
                }
            },
            "values": "\"log_crim\",\"log_medv\",\"chas\"\n-5.06403607082337,3.17805383034795,\"Not on River\"\n-3.60050234349652,3.07269331469012,\"Not on River\"\n-3.60123494426189,3.54673968695281,\"Not on River\"\n-3.43052321104398,3.50855589998265,\"Not on River\"\n-2.67292439912684,3.58905911883173,\"Not on River\"\n-3.51157043914353,3.35689712276558,\"Not on River\"\n-2.42712842806865,3.13113691056019,\"Not on River\"\n-1.93412981051978,3.29953372788566,\"Not on River\"\n-1.5547603511434,2.80336038090653,\"Not on River\"\n-1.77172157549155,2.9391619220656,\"Not on River\"\n-1.49214388521174,2.70805020110221,\"Not on River\"\n-2.14157229714634,2.9391619220656,\"Not on River\"\n-2.3668036653207,3.07731226054641,\"Not on River\"\n-0.462416484558303,3.01553490085017,\"Not on River\"\n-0.449479693527584,2.90142159408275,\"Not on River\"\n-0.46618692214761,2.99071973173045,\"Not on River\"\n0.0525260342514466,3.13983261752775,\"Not on River\"\n-0.243091189123906,2.86220088092947,\"Not on River\"\n-0.219761775967802,3.00568260440716,\"Not on River\"\n-0.320480784203167,2.90142159408275,\"Not on River\"\n0.224574526979991,2.61006979274201,\"Not on River\"\n-0.160121804898053,2.97552956623647,\"Not on River\"\n0.209020285867676,2.72129542785223,\"Not on River\"\n-0.0116374532441208,2.67414864942653,\"Not on River\"\n-0.287335465860119,2.74727091425549,\"Not on River\"\n-0.17371073654606,2.63188884013665,\"Not on River\"\n-0.397630875999648,2.8094026953625,\"Not on River\"\n-0.0452379806501943,2.69462718077007,\"Not on River\"\n-0.257489167089002,2.91235066461494,\"Not on River\"\n0.00244700364305185,3.04452243772342,\"Not on River\"\n0.122934190094978,2.54160199346455,\"Not on River\"\n0.30359479091183,2.67414864942653,\"Not on River\"\n0.327856657447708,2.58021682959233,\"Not on River\"\n0.141256477175355,2.57261223020711,\"Not on River\"\n0.477984199611674,2.60268968544438,\"Not on River\"\n-2.74621946721971,2.9391619220656,\"Not on River\"\n-2.32851847502055,2.99573227355399,\"Not on River\"\n-2.52398017377414,3.04452243772342,\"Not on River\"\n-1.74268363158146,3.20680324363393,\"Not on River\"\n-3.58885314004681,3.42751468997953,\"Not on River\"\n-3.39352687535794,3.55248682920838,\"Not on River\"\n-2.06010961338034,3.28091121578765,\"Not on River\"\n-1.95545556189884,3.23080439573347,\"Not on River\"\n-1.83658948514585,3.20680324363393,\"Not on River\"\n-2.09809443017905,3.05400118167797,\"Not on River\"\n-1.7636385935114,2.96010509591084,\"Not on River\"\n-1.66940025360678,2.99573227355399,\"Not on River\"\n-1.47285493064757,2.8094026953625,\"Not on River\"\n-1.37093295400719,2.66722820658195,\"Not on River\"\n-1.51517373404402,2.96527306606928,\"Not on River\"\n-2.42215722813489,2.98061863574394,\"Not on River\"\n-3.13798732113486,3.02042488614436,\"Not on River\"\n-2.92620621090538,3.2188758248682,\"Not on River\"\n-2.99953951189695,3.15273602236366,\"Not on River\"\n-4.29768548624013,2.9391619220656,\"Not on River\"\n-4.33437998120653,3.56671182013973,\"Not on River\"\n-3.88489433803989,3.20680324363393,\"Not on River\"\n-4.24609811744964,3.45315712059287,\"Not on River\"\n-1.86788485961755,3.14845336057165,\"Not on River\"\n-2.27031153244375,2.97552956623647,\"Not on River\"\n-1.90166362493575,2.92852352386054,\"Not on River\"\n-1.76194827165643,2.77258872223978,\"Not on River\"\n-2.20482337521155,3.10009228887823,\"Not on River\"\n-2.06751297081456,3.2188758248682,\"Not on River\"\n-3.93682812434712,3.49650756146648,\"Not on River\"\n-3.32869069087541,3.15700042115011,\"Not on River\"\n-3.12834979816883,2.96527306606928,\"Not on River\"\n-2.84921062089122,3.09104245335832,\"Not on River\"\n-1.99848847927417,2.85647020622048,\"Not on River\"\n-2.05447579566209,3.03974915897077,\"Not on River\"\n-2.42746827514074,3.18635263316264,\"Not on River\"\n-1.84036165106727,3.07731226054641,\"Not on River\"\n-2.38988742139684,3.1267605359604,\"Not on River\"\n-1.63275771775572,3.15273602236366,\"Not on River\"\n-2.53881388385691,3.18221184049661,\"Not on River\"\n-2.35261602659961,3.06339092202781,\"Not on River\"\n-2.28740095766901,2.99573227355399,\"Not on River\"\n-2.44104288614161,3.03495298670727,\"Not on River\"\n-2.87422285615679,3.05400118167797,\"Not on River\"\n-2.47848729798582,3.01062088604774,\"Not on River\"\n-3.1910174967398,3.3322045101752,\"Not on River\"\n-3.10957308997775,3.17387845893747,\"Not on River\"\n-3.30798029995102,3.21084365317094,\"Not on River\"\n-3.33794093202714,3.13113691056019,\"Not on River\"\n-2.98400135067829,3.17387845893747,\"Not on River\"\n-2.85858243540676,3.28091121578765,\"Not on River\"\n-2.95882191953389,3.11351530921037,\"Not on River\"\n-2.63791797892183,3.10009228887823,\"Not on River\"\n-2.871746293773,3.16124671203156,\"Not on River\"\n-2.93708607812126,3.35689712276558,\"Not on River\"\n-3.06101774025262,3.11794990627824,\"Not on River\"\n-3.23602198370317,3.09104245335832,\"Not on River\"\n-3.16937162996511,3.13113691056019,\"Not on River\"\n-3.54911751173878,3.2188758248682,\"Not on River\"\n-3.1479514865315,3.02529107579554,\"Not on River\"\n-2.10340641913367,3.34638914516716,\"Not on River\"\n-2.1624753850094,3.06339092202781,\"Not on River\"\n-2.11337067994292,3.65583960003574,\"Not on River\"\n-2.50262265559378,3.7796338173824,\"Not on River\"\n-2.6794627442503,3.50254987592244,\"Not on River\"\n-1.90609345968499,3.31418600467253,\"Not on River\"\n-2.16875374536053,3.27714473299218,\"Not on River\"\n-1.47508185993502,2.92316158071916,\"Not on River\"\n-1.55301032113546,2.96010509591084,\"Not on River\"\n-1.96897408865386,3.00071981506503,\"Not on River\"\n-2.02026738304142,2.9704144655697,\"Not on River\"\n-1.7649228152745,2.9704144655697,\"Not on River\"\n-2.03126108715508,3.01553490085017,\"Not on River\"\n-2.05556877726828,2.98568193770049,\"Not on River\"\n-1.3332086740283,2.96527306606928,\"Not on River\"\n-2.22627241014488,3.07731226054641,\"Not on River\"\n-2.29422017666242,3.1267605359604,\"Not on River\"\n-2.09321597510168,2.9338568698359,\"Not on River\"\n-1.50453750260873,2.92852352386054,\"Not on River\"\n-1.94974750228657,2.91777073208428,\"Not on River\"\n-1.76410539244624,2.90690105984738,\"Not on River\"\n-2.02814024732429,3.05400118167797,\"Not on River\"\n-1.89060790127066,2.95491027903374,\"Not on River\"\n-2.03576921322365,3.01553490085017,\"Not on River\"\n-1.9326780802866,2.96010509591084,\"Not on River\"\n-2.67379371242412,3.09104245335832,\"Not on River\"\n-2.63596212470796,3.01062088604774,\"Not on River\"\n-2.37526331849203,3.02042488614436,\"Not on River\"\n-1.89458985503226,2.85070650150373,\"Not on River\"\n-2.31780025880053,2.9338568698359,\"Not on River\"\n-1.7777382278658,3.06339092202781,\"Not on River\"\n-0.948426601904225,2.75366071235426,\"Not on River\"\n-1.35034823434642,2.78501124223834,\"Not on River\"\n-1.12260789422433,2.89037175789616,\"Not on River\"\n-0.126413924855659,2.66025953726586,\"Not on River\"\n-1.0786332063528,2.95491027903374,\"Not on River\"\n0.176420848472986,2.97552956623647,\"Not on River\"\n-0.527547999910379,3.13549421592915,\"Not on River\"\n-1.10920822788151,2.91235066461494,\"Not on River\"\n-0.0241185274088078,2.74727091425549,\"Not on River\"\n-0.583790659576773,2.89591193827178,\"Not on River\"\n-1.13121812841702,2.85647020622048,\"Not on River\"\n-1.0431870425627,2.83907846350861,\"Not on River\"\n-1.38709468129066,2.58776403522771,\"Not on River\"\n-0.607850606337865,2.87919845729804,\"Not on River\"\n-1.23477571348098,2.63905732961526,\"Not on River\"\n0.487745310721893,2.66722820658195,\"Not on River\"\n1.20028099798739,2.59525470695687,\"On River\"\n1.41035262621296,2.74727091425549,\"Not on River\"\n1.02235739814894,2.46809953147162,\"Not on River\"\n0.866823138301229,2.62466859216316,\"Not on River\"\n0.767813925142701,2.74727091425549,\"Not on River\"\n0.862307507076077,2.68102152871429,\"Not on River\"\n0.846293070040128,2.87919845729804,\"Not on River\"\n1.00575476530812,2.73436750941958,\"Not on River\"\n0.504767309182027,3.06805293513362,\"Not on River\"\n0.403008760421457,2.97552956623647,\"Not on River\"\n0.119186494791163,2.72785282839839,\"On River\"\n0.76508637404103,2.96527306606928,\"Not on River\"\n0.346316479810509,2.83321334405622,\"On River\"\n1.26271612819885,2.74727091425549,\"On River\"\n0.894732003534746,2.57261223020711,\"Not on River\"\n0.201780987950174,3.72086249996699,\"Not on River\"\n0.294786774181713,3.1904763503465,\"Not on River\"\n0.354185848709842,3.14845336057165,\"Not on River\"\n0.241737605442712,3.29583686600433,\"On River\"\n0.380735161487553,3.91202300542815,\"Not on River\"\n0.606373957027711,3.91202300542815,\"On River\"\n0.418065390083903,3.91202300542815,\"On River\"\n0.807528882678661,3.12236492448736,\"Not on River\"\n1.07295254188753,3.2188758248682,\"Not on River\"\n0.698229244966739,3.91202300542815,\"Not on River\"\n0.587942208360164,3.16968558067743,\"Not on River\"\n0.833083020857462,3.16968558067743,\"Not on River\"\n0.895896169418922,3.10458667846607,\"Not on River\"\n0.188485851761799,2.85647020622048,\"Not on River\"\n0.838934412625926,2.94968833505258,\"Not on River\"\n-1.97227465848664,3.13983261752775,\"Not on River\"\n-2.38836087001545,3.16124671203156,\"Not on River\"\n-2.47135883724273,3.11794990627824,\"Not on River\"\n-2.70845028112355,3.38099467434464,\"Not on River\"\n-2.65612210824185,3.14415227867226,\"Not on River\"\n-2.91415228656157,3.20274644293832,\"Not on River\"\n-2.71175706303354,3.39785848039664,\"Not on River\"\n-2.85076650330381,3.6163087612791,\"Not on River\"\n-2.7199203736727,3.68386691229039,\"Not on River\"\n-2.67538941886266,3.58905911883173,\"Not on River\"\n-2.39656615646494,3.63495111208838,\"Not on River\"\n-2.30178541282348,3.48124008933569,\"Not on River\"\n-2.48795127997423,3.27336401015227,\"Not on River\"\n-2.80560790469702,3.38777436133001,\"Not on River\"\n-2.88204650915017,3.91202300542815,\"Not on River\"\n-2.54147700127639,3.46573590279973,\"Not on River\"\n-2.07314142913136,3.39450839351136,\"Not on River\"\n-2.48051630148671,3.55248682920838,\"Not on River\"\n-2.40041845334281,3.61091791264422,\"Not on River\"\n-2.67205584087883,3.41772668361337,\"Not on River\"\n-2.4459936762894,3.5945687746427,\"Not on River\"\n-3.82263944429346,3.43720781918519,\"Not on River\"\n-4.24122175808286,3.37073817417745,\"Not on River\"\n-4.28236231156094,3.91202300542815,\"Not on River\"\n-3.21612959920018,3.5055573969864,\"Not on River\"\n-3.06486801238885,3.41114771251532,\"Not on River\"\n-3.27862582927397,3.54385368206368,\"Not on River\"\n-3.45776773315055,3.55248682920838,\"Not on River\"\n-4.02968104889638,3.49347265777133,\"Not on River\"\n-3.36824628152247,3.18221184049661,\"Not on River\"\n-3.82722240373568,3.74478708605223,\"Not on River\"\n-3.34955414851032,3.88156379794344,\"Not on River\"\n-3.90753310015529,3.91202300542815,\"Not on River\"\n-1.99201691675556,3.11794990627824,\"Not on River\"\n-1.47102470528047,3.19458313229916,\"Not on River\"\n-1.37836587479777,3.11351530921037,\"Not on River\"\n-1.9960567327459,3.19458313229916,\"On River\"\n-0.830778394549941,2.99573227355399,\"On River\"\n-1.74605978997706,3.07731226054641,\"On River\"\n-0.978751413216761,2.96010509591084,\"On River\"\n-1.52698273249791,3.10906095886099,\"On River\"\n-1.96240545158451,3.3357695763397,\"Not on River\"\n-1.23942728531034,3.16547504814109,\"Not on River\"\n-1.61938724328777,3.2188758248682,\"Not on River\"\n-3.0878475624618,3.14845336057165,\"On River\"\n-2.65740461643332,3.35689712276558,\"Not on River\"\n-2.2010217775846,3.06805293513362,\"On River\"\n-2.16936624920782,3.13549421592915,\"On River\"\n-1.02697092752823,3.2846635654062,\"On River\"\n-0.897199141618634,3.07731226054641,\"On River\"\n-0.472310287537657,3.31418600467253,\"On River\"\n-0.486620935069173,3.40452517175483,\"Not on River\"\n-1.15413556947876,3.80220813942094,\"Not on River\"\n-0.640687566587583,3.91202300542815,\"Not on River\"\n-0.961968245370808,3.62700405039585,\"Not on River\"\n-0.885810024620568,3.45315712059287,\"Not on River\"\n-1.21002441175437,3.84374416467485,\"Not on River\"\n-0.816943258373457,3.44998754583159,\"Not on River\"\n-0.621757184473272,3.1904763503465,\"Not on River\"\n-0.770114621716554,3.45631668088323,\"Not on River\"\n-0.552881017499318,3.73050112880476,\"Not on River\"\n-1.10421797118894,3.87743156065853,\"Not on River\"\n-0.803162959605968,3.36729582998647,\"On River\"\n-1.10729991706568,3.17805383034795,\"Not on River\"\n-0.652811704370542,3.22286784613774,\"On River\"\n-0.669762740327209,3.44998754583159,\"Not on River\"\n-2.49568452295988,3.16547504814109,\"Not on River\"\n-2.3803304416189,3.14845336057165,\"Not on River\"\n-2.17780437609674,3.09104245335832,\"Not on River\"\n-2.2431847497126,3.00071981506503,\"Not on River\"\n-2.27399763614213,3.10009228887823,\"Not on River\"\n-2.05909004543194,3.16547504814109,\"Not on River\"\n-1.57949083606615,2.86789890204411,\"Not on River\"\n-1.65375559308523,2.91777073208428,\"Not on River\"\n-1.07930978641361,3.1904763503465,\"Not on River\"\n-1.62673667701243,3.02042488614436,\"Not on River\"\n-1.80551362546072,3.19867311755068,\"Not on River\"\n-1.6568964635938,3.26575941076705,\"Not on River\"\n-1.96397229187372,3.19458313229916,\"Not on River\"\n-1.54135879162351,3.21084365317094,\"Not on River\"\n-2.4984783298181,3.38777436133001,\"Not on River\"\n-0.997121249788704,3.75653810258775,\"Not on River\"\n-3.0326037483299,3.08648663682246,\"Not on River\"\n-3.33878612154076,3.03974915897077,\"Not on River\"\n-4.17468731490464,3.78418963391826,\"Not on River\"\n-0.49177491307519,3.91202300542815,\"Not on River\"\n-0.410211353733397,3.58351893845611,\"Not on River\"\n-0.420604126950968,3.40452517175483,\"Not on River\"\n-0.615982456464896,3.52046080248897,\"Not on River\"\n-0.627134746166374,3.7635229971097,\"Not on River\"\n-0.653657272873533,3.8877303128591,\"Not on River\"\n-0.192056790782112,3.43398720448515,\"Not on River\"\n-0.597709736126834,3.59731226058845,\"Not on River\"\n-0.272307535345581,3.1267605359604,\"Not on River\"\n-0.241180238800361,3.42426265459315,\"Not on River\"\n-0.547593347958205,3.91202300542815,\"Not on River\"\n-0.615260641902874,3.77276093809464,\"Not on River\"\n-2.40074934178128,3.03013370027132,\"On River\"\n-1.20677673165867,3.04927304048202,\"Not on River\"\n-1.81948016182866,3.22684399451738,\"Not on River\"\n-2.1663074747015,3.19458313229916,\"Not on River\"\n-1.5056185837951,3.56104608260405,\"On River\"\n-2.87457715199752,3.47815842279828,\"On River\"\n-2.34299050762908,3.46573590279973,\"Not on River\"\n-2.25675167665087,3.50254987592244,\"On River\"\n-2.79246495224457,3.49953328238302,\"On River\"\n-2.52848243250488,3.37073817417745,\"Not on River\"\n-1.55883985967101,3.55820113047182,\"Not on River\"\n-3.33036620090156,3.8155121050473,\"Not on River\"\n-3.29548692724004,3.56671182013973,\"Not on River\"\n-2.7921385814845,3.8286413964891,\"On River\"\n-4.19903863333677,3.91202300542815,\"On River\"\n-4.70388615892725,3.47196645255036,\"Not on River\"\n-4.51350299746227,3.09104245335832,\"Not on River\"\n-3.92967794066687,3.00071981506503,\"Not on River\"\n-3.25165731439258,3.14415227867226,\"Not on River\"\n-3.08129016191564,3.10458667846607,\"Not on River\"\n-3.14725308119523,3.21084365317094,\"Not on River\"\n-3.35183595212443,3.3499040872746,\"Not on River\"\n-2.54008115053266,3.61899332664977,\"Not on River\"\n-3.32007833037736,3.32862668882732,\"Not on River\"\n-2.4931404547151,3.17387845893747,\"Not on River\"\n-2.50115799037405,3.07731226054641,\"Not on River\"\n-2.0454653261249,3.35340671782581,\"Not on River\"\n-2.9239699073271,3.29953372788566,\"Not on River\"\n-1.95878264527799,3.01062088604774,\"Not on River\"\n-2.73861250668485,3.11351530921037,\"Not on River\"\n-2.88939223778266,3.36729582998647,\"Not on River\"\n-3.1197094533737,3.21084365317094,\"Not on River\"\n-3.34189127576475,3.09104245335832,\"Not on River\"\n-2.37881839899363,3.27336401015227,\"Not on River\"\n-2.30258509299405,3.49953328238302,\"Not on River\"\n-2.89769853328263,3.58629286533884,\"Not on River\"\n-2.90424758343181,3.34638914516716,\"Not on River\"\n-2.5898672454245,3.50855589998265,\"Not on River\"\n-3.00942560068599,3.33932197794407,\"Not on River\"\n-0.707286673713667,3.1267605359604,\"Not on River\"\n-1.05153788128218,3.01062088604774,\"Not on River\"\n0.969065328591483,2.77881927199042,\"Not on River\"\n-0.23520348080665,3.09557760852371,\"Not on River\"\n-1.3405946818689,2.96527306606928,\"Not on River\"\n-1.31163225681147,3.07269331469012,\"Not on River\"\n-0.99641677635344,3.16968558067743,\"Not on River\"\n-1.37215479756617,2.78501124223834,\"Not on River\"\n-1.14485519984285,2.87919845729804,\"Not on River\"\n-1.4055995121779,2.98568193770049,\"Not on River\"\n-0.911253440356887,3.13983261752775,\"Not on River\"\n-0.743451490469693,3.04452243772342,\"Not on River\"\n-1.78617509093415,3.16968558067743,\"Not on River\"\n-1.70600388041043,3.13983261752775,\"Not on River\"\n-1.04657027464107,3.01553490085017,\"Not on River\"\n-1.2590627706439,2.91777073208428,\"Not on River\"\n-1.07560890690315,3.2188758248682,\"Not on River\"\n-1.65098933959234,3.20274644293832,\"Not on River\"\n-1.19247252015573,3.13549421592915,\"Not on River\"\n-1.42283387191084,3.10009228887823,\"Not on River\"\n-2.71552809095817,2.96010509591084,\"Not on River\"\n-2.69948697044172,3.11794990627824,\"Not on River\"\n-3.09136250456924,2.98568193770049,\"Not on River\"\n-2.99114282122018,2.83907846350861,\"Not on River\"\n-3.36216899469468,2.96527306606928,\"Not on River\"\n-2.97926854752333,3.10009228887823,\"Not on River\"\n-3.28661947695472,3.03013370027132,\"Not on River\"\n-3.22867366734831,3.04927304048202,\"Not on River\"\n-3.37348494309562,2.9704144655697,\"Not on River\"\n-3.49298377729286,2.91777073208428,\"Not on River\"\n-3.40943118658926,3.02529107579554,\"Not on River\"\n-2.90096769710957,2.94443897916644,\"Not on River\"\n-2.78855551576186,2.92852352386054,\"Not on River\"\n-4.34203698645772,3.48737507790321,\"Not on River\"\n-3.68967977428471,2.80336038090653,\"Not on River\"\n-3.67182569954811,3.17387845893747,\"Not on River\"\n-3.49035651798197,3.44041809481544,\"Not on River\"\n-3.46958329452862,2.86220088092947,\"Not on River\"\n-2.78676878581361,2.84490938381941,\"Not on River\"\n-3.9792317551216,3.13983261752775,\"Not on River\"\n-4.19903863333677,3.19867311755068,\"Not on River\"\n-3.54080433604857,3.28091121578765,\"Not on River\"\n-2.77884827241093,3.13113691056019,\"Not on River\"\n-2.53199825732185,3.18221184049661,\"Not on River\"\n-2.62499664596692,2.92316158071916,\"Not on River\"\n-4.06926178185464,3.40452517175483,\"Not on River\"\n-3.14632263218649,2.90142159408275,\"Not on River\"\n-2.23876558028109,3.02529107579554,\"Not on River\"\n2.19532944938176,2.87919845729804,\"On River\"\n1.34799522318569,3.07731226054641,\"On River\"\n1.64899895228507,3.12236492448736,\"On River\"\n1.44957662474632,3.11794990627824,\"Not on River\"\n1.51334983014208,3.2188758248682,\"Not on River\"\n1.34464911115118,2.99071973173045,\"Not on River\"\n1.30242893951019,3.03495298670727,\"Not on River\"\n1.44040131844278,2.82137888640921,\"On River\"\n1.24538726330446,3.08648663682246,\"On River\"\n1.51641651137626,3.31418600467253,\"Not on River\"\n1.30750815538373,3.08648663682246,\"Not on River\"\n2.60433277927054,3.13983261752775,\"Not on River\"\n1.58887187381363,3.91202300542815,\"Not on River\"\n1.73518559039658,3.91202300542815,\"On River\"\n1.87774754504581,3.91202300542815,\"On River\"\n2.22270820490502,3.91202300542815,\"Not on River\"\n2.1123019265294,3.91202300542815,\"On River\"\n2.40767457192474,2.62466859216316,\"Not on River\"\n2.91767343005329,2.62466859216316,\"Not on River\"\n2.97599374420349,2.70805020110221,\"Not on River\"\n2.72706820693797,2.63188884013665,\"Not on River\"\n2.28477645637644,2.58776403522771,\"Not on River\"\n3.16328700210486,2.57261223020711,\"Not on River\"\n2.88293864507855,2.32238772029023,\"Not on River\"\n4.48836891823984,2.34180580614733,\"Not on River\"\n2.76470774878891,2.3887627892351,\"Not on River\"\n2.21779161827618,2.42480272571829,\"Not on River\"\n2.07850109960278,2.50959926237837,\"Not on River\"\n2.99996828895892,2.17475172148416,\"Not on River\"\n2.82208102080754,1.97408102602201,\"Not on River\"\n3.19432900165004,2.35137525716348,\"Not on River\"\n3.11782157946064,2.00148000021012,\"Not on River\"\n2.6626134080936,2.32238772029023,\"Not on River\"\n2.09823140139806,2.4423470353692,\"Not on River\"\n1.94048833469004,2.71469474382088,\"Not on River\"\n1.66639463926993,3.14415227867226,\"Not on River\"\n2.44909810854921,2.27212588550934,\"Not on River\"\n2.15695335703792,2.62466859216316,\"Not on River\"\n2.59225019793657,2.54160199346455,\"Not on River\"\n2.16524646202657,2.57261223020711,\"Not on River\"\n1.77020380626234,2.52572864430826,\"Not on River\"\n2.03757994445991,2.14006616349627,\"Not on River\"\n3.64680146282653,1.6094379124341,\"Not on River\"\n2.29420507854844,1.84054963339749,\"Not on River\"\n3.22071812678739,1.7227665977411,\"Not on River\"\n2.6557880164394,1.97408102602201,\"Not on River\"\n2.2613161235954,2.4932054526027,\"Not on River\"\n3.2109121992087,2.11625551480255,\"Not on River\"\n3.72639679427388,2.14006616349627,\"Not on River\"\n4.21834232049674,1.6094379124341,\"Not on River\"\n3.03091600288847,2.47653840011748,\"Not on River\"\n2.48082332435036,3.32862668882732,\"Not on River\"\n2.00200553776674,2.84490938381941,\"Not on River\"\n2.66988439800228,3.31418600467253,\"Not on River\"\n3.93448483899724,2.70805020110221,\"Not on River\"\n2.64267221660273,2.84490938381941,\"Not on River\"\n2.93444180511088,2.88480071284671,\"Not on River\"\n3.35535586595285,2.79116510781272,\"Not on River\"\n3.82310654218625,1.94591014905531,\"Not on River\"\n2.8950607473823,1.97408102602201,\"Not on River\"\n2.38270779746775,2.01490302054226,\"Not on River\"\n3.25580930892149,2.34180580614733,\"Not on River\"\n4.29774924420755,2.17475172148416,\"Not on River\"\n2.4691413614596,2.12823170584927,\"Not on River\"\n2.4058093284293,2.81540871942271,\"Not on River\"\n1.94913209586287,2.65324196460721,\"Not on River\"\n2.48891527118536,3.03495298670727,\"Not on River\"\n1.95308718951777,2.59525470695687,\"Not on River\"\n2.17385586577979,2.45958884180371,\"Not on River\"\n2.76381913153852,2.11625551480255,\"Not on River\"\n2.50529733943573,2.32238772029023,\"Not on River\"\n3.62864897336371,2.3887627892351,\"Not on River\"\n1.99702549904027,2.39789527279837,\"Not on River\"\n2.2341874014952,2.2512917986065,\"Not on River\"\n2.13913985224951,2.67414864942653,\"Not on River\"\n2.30879576677076,2.64617479738412,\"Not on River\"\n1.86315722444043,2.77881927199042,\"Not on River\"\n1.71938051428274,2.66025953726586,\"Not on River\"\n2.63285240453631,2.45958884180371,\"Not on River\"\n2.41237179860475,2.59525470695687,\"Not on River\"\n2.66867160882001,2.26176309847379,\"Not on River\"\n2.71979430172596,2.16332302566054,\"Not on River\"\n2.61579601365956,2.12823170584927,\"Not on River\"\n2.23971238362132,2.54944517092557,\"Not on River\"\n3.09336248726987,2.35137525716348,\"Not on River\"\n2.27461556718342,2.83907846350861,\"Not on River\"\n1.73454870107647,2.91235066461494,\"Not on River\"\n2.29923348261767,2.73436750941958,\"Not on River\"\n2.54962484228371,2.37954613413017,\"Not on River\"\n2.36760474836797,2.46809953147162,\"Not on River\"\n1.83865418738046,2.70136121295141,\"Not on River\"\n2.29504171310892,2.53369681395743,\"Not on River\"\n2.23313747526855,2.64617479738412,\"Not on River\"\n2.01836502089744,2.56494935746154,\"Not on River\"\n1.90474881125035,2.59525470695687,\"Not on River\"\n1.693988597737,2.72129542785223,\"Not on River\"\n1.62731122882592,2.77881927199042,\"Not on River\"\n2.1099816583913,2.87919845729804,\"Not on River\"\n2.25272550719709,2.70136121295141,\"Not on River\"\n1.55864344098212,2.64617479738412,\"Not on River\"\n1.54090850495968,2.54160199346455,\"Not on River\"\n2.10420488347616,2.60268968544438,\"Not on River\"\n2.04798054391097,2.70136121295141,\"Not on River\"\n1.91709465620517,2.99573227355399,\"Not on River\"\n1.57113981354136,2.79728133483015,\"Not on River\"\n1.30646892150861,2.87356463957978,\"Not on River\"\n1.89535643073801,2.9704144655697,\"Not on River\"\n1.76149783672584,3.00568260440716,\"Not on River\"\n2.05915209590677,3.06339092202781,\"Not on River\"\n1.15171061966313,2.99071973173045,\"Not on River\"\n1.32839508467371,2.94443897916644,\"Not on River\"\n1.48665540019546,2.94968833505258,\"Not on River\"\n2.7457120074838,2.94968833505258,\"Not on River\"\n2.5707096581053,3.00071981506503,\"Not on River\"\n1.46989764548713,2.99071973173045,\"Not on River\"\n1.39585105014985,2.97552956623647,\"Not on River\"\n1.27219577951878,3.14415227867226,\"Not on River\"\n1.53619817863696,3.39450839351136,\"Not on River\"\n2.08639108754919,2.62466859216316,\"Not on River\"\n1.85522241213869,2.58776403522771,\"Not on River\"\n1.58338342291018,2.81540871942271,\"Not on River\"\n2.7096089855662,2.484906649788,\"Not on River\"\n2.32561779210462,2.68102152871429,\"Not on River\"\n2.6626134080936,3.06339092202781,\"Not on River\"\n1.76198902792588,3.13549421592915,\"Not on River\"\n1.74190023380554,3.16547504814109,\"Not on River\"\n1.74591795351875,3.2188758248682,\"Not on River\"\n1.0361622517949,3.08190996979504,\"Not on River\"\n0.866499466770358,3.02529107579554,\"Not on River\"\n1.30119116239956,3.05400118167797,\"Not on River\"\n1.73901775796999,2.94968833505258,\"Not on River\"\n1.57601969221081,3.02529107579554,\"Not on River\"\n-1.89140302455665,2.72129542785223,\"Not on River\"\n-1.69624930942106,1.94591014905531,\"Not on River\"\n-1.57281672897846,2.09186406167839,\"Not on River\"\n-2.24677202817483,2.61006979274201,\"Not on River\"\n-2.19534634232445,3.00071981506503,\"Not on River\"\n-1.75267338052085,3.08190996979504,\"Not on River\"\n-1.27450257051646,3.19867311755068,\"Not on River\"\n-1.72042534062373,3.13983261752775,\"Not on River\"\n-1.23925461847058,2.98061863574394,\"Not on River\"\n-1.31535139230933,2.90690105984738,\"Not on River\"\n-1.43078976100645,3.05400118167797,\"Not on River\"\n-1.72692724122657,2.86220088092947,\"Not on River\"\n-1.49441423586532,2.82137888640921,\"Not on River\"\n-2.77051088244482,3.10906095886099,\"Not on River\"\n-3.09511071753427,3.02529107579554,\"Not on River\"\n-2.80082360125457,3.17387845893747,\"Not on River\"\n-2.21100914950684,3.09104245335832,\"Not on River\"\n-3.04892210206811,2.47653840011748,\"Not on River\""
        },
        {
            "name": ".0/group_by1/model_prediction2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "resp_upr_": "number",
                    "pred_": "number",
                    "resp_lwr_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"chas\",\"resp_upr_\",\"pred_\",\"resp_lwr_\",\"resp_\"\n\"Not on River\",3.55845058657393,-5.06403607082337,3.43023618528433,3.49434338592913\n\"Not on River\",3.543485570806,-4.94311955197447,3.41813968510703,3.48081262795652\n\"Not on River\",3.52852964067364,-4.82220303312557,3.40603409929416,3.4672818699839\n\"Not on River\",3.5135834454244,-4.70128651427667,3.39391877859818,3.45375111201129\n\"Not on River\",3.49864769256268,-4.58036999542777,3.38179301551466,3.44022035403867\n\"Not on River\",3.48372315382364,-4.45945347657886,3.36965603830848,3.42668959606606\n\"Not on River\",3.46881067177503,-4.33853695772996,3.35750700441186,3.41315883809345\n\"Not on River\",3.45391116709923,-4.21762043888106,3.34534499314243,3.39962808012083\n\"Not on River\",3.43902564660529,-4.09670392003216,3.33316899769115,3.38609732214822\n\"Not on River\",3.42415521201511,-3.97578740118326,3.3209779163361,3.3725665641756\n\"Not on River\",3.40930106955782,-3.85487088233436,3.30877054284816,3.35903580620299\n\"Not on River\",3.3944645403897,-3.73395436348546,3.29654555607105,3.34550504823037\n\"Not on River\",3.37964707183189,-3.61303784463655,3.28430150868363,3.33197429025776\n\"Not on River\",3.36485024938167,-3.49212132578765,3.27203681518862,3.31844353228515\n\"Not on River\",3.35007580940192,-3.37120480693875,3.25974973922314,3.30491277431253\n\"Not on River\",3.33532565232331,-3.25028828808985,3.24743838035652,3.29138201633992\n\"Not on River\",3.32060185610048,-3.12937176924095,3.23510066063413,3.2778512583673\n\"Not on River\",3.30590668954196,-3.00845525039205,3.22273431124742,3.26432050039469\n\"Not on River\",3.2912426249793,-2.88753873154315,3.21033685986485,3.25078974242208\n\"Not on River\",3.27661234954993,-2.76662221269424,3.19790561934899,3.23725898444946\n\"Not on River\",3.26201877414099,-2.64570569384534,3.1854376788127,3.22372822647685\n\"Not on River\",3.24746503877996,-2.52478917499644,3.17292989822851,3.21019746850423\n\"Not on River\",3.23295451297595,-2.40387265614754,3.16037890808729,3.19666671053162\n\"Not on River\",3.21849078923474,-2.28295613729864,3.14778111588327,3.18313595255901\n\"Not on River\",3.20407766773011,-2.16203961844974,3.13513272144267,3.16960519458639\n\"Not on River\",3.18971912997049,-2.04112309960084,3.12242974325706,3.15607443661378\n\"Not on River\",3.1754192993294,-1.92020658075193,3.10966805795293,3.14254367864116\n\"Not on River\",3.1611823866006,-1.79929006190303,3.0968434547365,3.12901292066855\n\"Not on River\",3.14701261938809,-1.67837354305413,3.08395170600378,3.11548216269594\n\"Not on River\",3.13291415521849,-1.55745702420523,3.07098865422816,3.10195140472332\n\"Not on River\",3.11889097978765,-1.43654050535633,3.05795031371376,3.08842064675071\n\"Not on River\",3.10494679365344,-1.31562398650743,3.04483298390275,3.07488988877809\n\"Not on River\",3.09108489276603,-1.19470746765853,3.03163336884492,3.06135913080548\n\"Not on River\",3.07730805016713,-1.07379094880962,3.0183486954986,3.04782837283286\n\"Not on River\",3.06361840757592,-0.952874429960723,3.00497682214458,3.03429761486025\n\"Not on River\",3.05001738600021,-0.831957911111821,2.99151632777506,3.02076685688764\n\"Not on River\",3.03650562367035,-0.71104139226292,2.9779665741597,3.00723609891502\n\"Not on River\",3.023082947441,-0.590124873414019,2.96432773444382,2.99370534094241\n\"Not on River\",3.00974838059398,-0.469208354565117,2.95060078534561,2.98017458296979\n\"Not on River\",2.9965001862355,-0.348291835716215,2.93678746375886,2.96664382499718\n\"Not on River\",2.9833359418881,-0.227375316867314,2.92289019216103,2.95311306702457\n\"Not on River\",2.97025263806531,-0.106458798018413,2.9089119800386,2.93958230905195\n\"Not on River\",2.95724679200972,0.0144577208304888,2.89485631014896,2.92605155107934\n\"Not on River\",2.94431456749368,0.135374239679391,2.88072701871977,2.91252079310672\n\"Not on River\",2.93145189245896,0.256290758528292,2.86652817780926,2.89899003513411\n\"Not on River\",2.91865456794703,0.377207277377193,2.85226398637596,2.8854592771615\n\"Not on River\",2.90591836381695,0.498123796226094,2.83793867456081,2.87192851918888\n\"Not on River\",2.89323909878375,0.619040315074996,2.82355642364878,2.85839776121627\n\"Not on River\",2.88061270406611,0.739956833923897,2.80912130242119,2.84486700324365\n\"Not on River\",2.86803527126946,0.860873352772798,2.79463721927261,2.83133624527104\n\"Not on River\",2.85550308601935,0.9817898716217,2.7801078885775,2.81780548729842\n\"Not on River\",2.84301264934692,1.1027063904706,2.7655368093047,2.80427472932581\n\"Not on River\",2.83056068899725,1.2236229093195,2.75092725370915,2.7907439713532\n\"Not on River\",2.81814416277682,1.3445394281684,2.73628226398435,2.77721321338058\n\"Not on River\",2.80576025586326,1.46545594701731,2.72160465495268,2.76368245540797\n\"Not on River\",2.79340637373723,1.58637246586621,2.70689702113348,2.75015169743535\n\"Not on River\",2.78108013211118,1.70728898471511,2.6921617468143,2.73662093946274\n\"Not on River\",2.76877934495444,1.82820550356401,2.67740101802581,2.72309018149013\n\"Not on River\",2.75650201146634,1.94912202241291,2.66261683556868,2.70955942351751\n\"Not on River\",2.74424630263699,2.07003854126181,2.6478110284528,2.6960286655449\n\"Not on River\",2.73201054786043,2.19095506011071,2.63298526728414,2.68249790757228\n\"Not on River\",2.71979322192489,2.31187157895962,2.61814107727445,2.66896714959967\n\"Not on River\",2.70759293259614,2.43278809780852,2.60327985065797,2.65543639162706\n\"Not on River\",2.69540840892693,2.55370461665742,2.58840285838195,2.64190563365444\n\"Not on River\",2.68323849036409,2.67462113550632,2.57351126099957,2.62837487568183\n\"Not on River\",2.67108211668051,2.79553765435522,2.55860611873792,2.61484411770921\n\"Not on River\",2.65893831872777,2.91645417320412,2.54368840074543,2.6013133597366\n\"Not on River\",2.64680620998408,3.03737069205302,2.52875899354389,2.58778260176399\n\"Not on River\",2.63468497885858,3.15828721090193,2.51381870872417,2.57425184379137\n\"Not on River\",2.62257388170487,3.27920372975083,2.49886828993264,2.56072108581876\n\"Not on River\",2.61047223649273,3.40012024859973,2.48390841919955,2.54719032784614\n\"Not on River\",2.5983794170854,3.52103676744863,2.46893972266166,2.53365956987353\n\"Not on River\",2.58629484807089,3.64195328629753,2.45396277573094,2.52012881190091\n\"Not on River\",2.5742180000974,3.76286980514643,2.4389781077592,2.5065980539283\n\"Not on River\",2.56214838566571,3.88378632399533,2.42398620624566,2.49306729595569\n\"Not on River\",2.55008555533472,4.00470284284424,2.40898752063142,2.47953653798307\n\"Not on River\",2.53802909429958,4.12561936169314,2.39398246572134,2.46600578001046\n\"Not on River\",2.5259786193052,4.24653588054204,2.37897142477049,2.45247502203784\n\"Not on River\",2.51393377586149,4.36745239939094,2.36395475226897,2.43894426406523\n\"Not on River\",2.50189423572948,4.48836891823984,2.34893277645575,2.42541350609262\n\"On River\",3.68745318327281,-4.19903863333677,3.06221749384718,3.37483533856\n\"On River\",3.67951151456634,-4.11809726520109,3.06558298776217,3.37254725116425\n\"On River\",3.67159431395582,-4.03715589706541,3.0689240135812,3.37025916376851\n\"On River\",3.66370297865956,-3.95621452892973,3.07223917408598,3.36797107637277\n\"On River\",3.65583900714872,-3.87527316079406,3.07552697080533,3.36568298897703\n\"On River\",3.64800400771049,-3.79433179265838,3.07878579545207,3.36339490158128\n\"On River\",3.64019970779162,-3.7133904245227,3.08201392057946,3.36110681418554\n\"On River\",3.63242796418958,-3.63244905638702,3.08520948939002,3.3588187267898\n\"On River\",3.62469077416225,-3.55150768825135,3.08837050462586,3.35653063939406\n\"On River\",3.61699028752925,-3.47056632011567,3.09149481646738,3.35424255199831\n\"On River\",3.60932881983964,-3.38962495197999,3.0945801093655,3.35195446460257\n\"On River\",3.60170886668052,-3.30868358384431,3.09762388773314,3.34966637720683\n\"On River\",3.59413311919811,-3.22774221570864,3.10062346042406,3.34737828981109\n\"On River\",3.58660448089775,-3.14680084757296,3.10357592393294,3.34509020241534\n\"On River\",3.57912608577852,-3.06585947943728,3.10647814426068,3.3428021150196\n\"On River\",3.57170131784324,-2.9849181113016,3.10932673740448,3.34051402762386\n\"On River\",3.5643338320011,-2.90397674316593,3.11211804845513,3.33822594022812\n\"On River\",3.55702757634842,-2.82303537503025,3.11484812931633,3.33593785283237\n\"On River\",3.54978681576838,-2.74209400689457,3.11751271510489,3.33364976543663\n\"On River\",3.54261615673221,-2.66115263875889,3.12010719934957,3.33136167804089\n\"On River\",3.53552057310677,-2.58021127062322,3.12262660818353,3.32907359064515\n\"On River\",3.52850543267435,-2.49926990248754,3.12506557382446,3.3267855032494\n\"On River\",3.52157652394539,-2.41832853435186,3.12741830776193,3.32449741585366\n\"On River\",3.51474008268848,-2.33738716621618,3.12967857422736,3.32220932845792\n\"On River\",3.5080028174121,-2.25644579808051,3.13183966471225,3.31992124106218\n\"On River\",3.50137193280489,-2.17550442994483,3.13389437452798,3.31763315366643\n\"On River\",3.49485514987503,-2.09456306180915,3.13583498266636,3.31534506627069\n\"On River\",3.48846072122759,-2.01362169367347,3.13765323652231,3.31305697887495\n\"On River\",3.48219743958692,-1.93268032553779,3.1393403433715,3.31076889147921\n\"On River\",3.47607463732519,-1.85173895740212,3.14088697084174,3.30848080408346\n\"On River\",3.4701021744207,-1.77079758926644,3.14228325895474,3.30619271668772\n\"On River\",3.46429041197647,-1.68985622113076,3.14351884660749,3.30390462929198\n\"On River\",3.45865016823035,-1.60891485299508,3.14458291556212,3.30161654189624\n\"On River\",3.45319265394695,-1.52797348485941,3.14546425505404,3.29932845450049\n\"On River\",3.44792938427394,-1.44703211672373,3.14615134993556,3.29704036710475\n\"On River\",3.44287206465552,-1.36609074858805,3.1466324947625,3.29475227970901\n\"On River\",3.43803244930031,-1.28514938045237,3.14689593532622,3.29246419231327\n\"On River\",3.43342217205803,-1.2042080123167,3.14693003777702,3.29017610491753\n\"On River\",3.42905255138043,-1.12326664418102,3.14672348366314,3.28788801752178\n\"On River\",3.42493437327333,-1.04232527604534,3.14626548697875,3.28559993012604\n\"On River\",3.42107765864314,-0.961383907909664,3.14554602681746,3.2833118427303\n\"On River\",3.41749142396133,-0.880442539773986,3.14455608670778,3.28102375533456\n\"On River\",3.41418344638517,-0.799501171638309,3.14328788949246,3.27873566793881\n\"On River\",3.41116004600566,-0.718559803502631,3.14173511508048,3.27644758054307\n\"On River\",3.40842589838443,-0.637618435366953,3.13989308791022,3.27415949314733\n\"On River\",3.40598388973019,-0.556677067231276,3.13775892177298,3.27187140575159\n\"On River\",3.4038350248696,-0.475735699095598,3.13533161184208,3.26958331835584\n\"On River\",3.40197839473395,-0.39479433095992,3.13261206718625,3.2672952309601\n\"On River\",3.40041120578459,-0.313852962824243,3.12960308134413,3.26500714356436\n\"On River\",3.39912886918321,-0.232911594688566,3.12630924315402,3.26271905616862\n\"On River\",3.39812514318465,-0.151970226552888,3.12273679436109,3.26043096877287\n\"On River\",3.39739231874238,-0.0710288584172103,3.11889344401188,3.25814288137713\n\"On River\",3.39692143605337,0.00991250971846735,3.11478815190941,3.25585479398139\n\"On River\",3.39670251888893,0.090853877854145,3.11043089428236,3.25356670658565\n\"On River\",3.39672481398821,0.171795245989823,3.1058324243916,3.2512786191899\n\"On River\",3.39697702427987,0.2527366141255,3.10100403930845,3.24899053179416\n\"On River\",3.39744752688693,0.333677982261178,3.09595736190991,3.24670244439842\n\"On River\",3.39812456938313,0.414619350396856,3.09070414462222,3.24441435700268\n\"On River\",3.39899644027348,0.495560718532532,3.08525609894039,3.24212626960693\n\"On River\",3.4000516119204,0.57650208666821,3.07962475250199,3.23983818221119\n\"On River\",3.40127885598084,0.657443454803888,3.07382133365006,3.23755009481545\n\"On River\",3.4026673327999,0.738384822939565,3.06785668203951,3.23526200741971\n\"On River\",3.40420665713307,0.819326191075243,3.06174118291486,3.23297392002396\n\"On River\",3.40588694309682,0.900267559210921,3.05548472215962,3.23068583262822\n\"On River\",3.40769883145428,0.981208927346598,3.04909665901068,3.22839774523248\n\"On River\",3.40963350231078,1.06215029548228,3.04258581336269,3.22610965783674\n\"On River\",3.41168267610174,1.14309166361795,3.03596046478024,3.22382157044099\n\"On River\",3.41383860546521,1.22403303175363,3.02922836062529,3.22153348304525\n\"On River\",3.4160940602555,1.30497439988931,3.02239673104352,3.21924539564951\n\"On River\",3.41844230760781,1.38591576802499,3.01547230889973,3.21695730825377\n\"On River\",3.42087708863121,1.46685713616066,3.00846135308484,3.21466922085802\n\"On River\",3.4233925930036,1.54779850429634,3.00136967392096,3.21238113346228\n\"On River\",3.42598343247428,1.62873987243202,2.99420265965879,3.21009304606654\n\"On River\",3.42864461405031,1.7096812405677,2.98696530329128,3.2078049586708\n\"On River\",3.43137151345079,1.79062260870337,2.97966222909932,3.20551687127505\n\"On River\",3.43415984925564,1.87156397683905,2.97229771850299,3.20322878387931\n\"On River\",3.43700565804858,1.95250534497473,2.96487573491856,3.20094069648357\n\"On River\",3.4399052707537,2.03344671311041,2.95739994742196,3.19865260908783\n\"On River\",3.44285529028672,2.11438808124608,2.94987375309745,3.19636452169208\n\"On River\",3.44585257058254,2.19532944938176,2.94230029801014,3.19407643429634"
        },
        {
            "name": ".0/group_by1/model_prediction2",
            "source": ".0/group_by1/model_prediction2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.chas"
                    ]
                }
            ]
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-5.54165632027653\n4.965989167693"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n1.4943086577844\n4.02715226007785"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "symbol",
            "properties": {
                "update": {
                    "size": {
                        "value": 50
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.log_crim"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.log_medv"
                    },
                    "fill": {
                        "scale": "fill",
                        "field": "data.chas"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0"
                    }
                }
            },
            "from": {
                "data": ".0"
            }
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/model_prediction2"
            },
            "marks": [
                {
                    "type": "area",
                    "properties": {
                        "update": {
                            "fill": {
                                "value": "#333333"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.resp_upr_"
                            },
                            "fillOpacity": {
                                "value": 0.2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_lwr_"
                            },
                            "stroke": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/model_prediction2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/model_prediction2"
            },
            "marks": [
                {
                    "type": "line",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "strokeWidth": {
                                "value": 2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_"
                            },
                            "fill": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/model_prediction2"
                            }
                        }
                    }
                }
            ]
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "chas"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "log_crim"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "log_medv"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id617908707").parseSpec(plot_id617908707_spec);
</script><!--/html_preserve-->

We do see some interaction in that the slopes are different, but this
is hard to say with any certainty since `chas= "On River"` is such a
small sample size in comparison to its complement.  Overall the slope
seems to be similar (the orange points fitting well inside of the
blue), but a few influential outliers in the top-right of the
plot are likely throwing the slope off.


```r
summary(chas)
```

```
## Not on River     On River 
##          471           35
```

4. To see how the the log crime variable (`crim`) depends on `rad` and `chas`, **make the x-axis scales equal** and:  
a) Plot the density curve of the log of crime (`log_crim`).

```r
domain <- range(Boston$log_crim)*1.3
Boston %>% ggvis(~log_crim) %>%
    layer_densities() %>%
    scale_numeric("x", domain = domain, nice = TRUE)
```

<!--html_preserve--><div id="plot_id271790523-container" class="ggvis-output-container">
<div id="plot_id271790523" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id271790523_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id271790523" data-renderer="svg">SVG</a>
 | 
<a id="plot_id271790523_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id271790523" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id271790523_download" class="ggvis-download" data-plot-id="plot_id271790523">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id271790523_spec = {
    "data": [
        {
            "name": ".0/density1",
            "format": {
                "type": "csv",
                "parse": {
                    "pred_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"pred_\",\"resp_\"\n-6.74438490568631,1.9058364799651e-05\n-6.69374528741655,2.52598402879211e-05\n-6.64310566914679,3.31762756698862e-05\n-6.59246605087703,4.33453870661111e-05\n-6.54182643260726,5.6182270754308e-05\n-6.4911868143375,7.23945056116932e-05\n-6.44054719606774,9.26368706403166e-05\n-6.38990757779798,0.000117793825061989\n-6.33926795952822,0.000148872205633306\n-6.28862834125846,0.000186915669215466\n-6.2379887229887,0.000233435911961094\n-6.18734910471894,0.000289582106373292\n-6.13670948644917,0.000357574027915249\n-6.08606986817941,0.000438564331606842\n-6.03543024990965,0.000535745208096256\n-5.98479063163989,0.000650108694323742\n-5.93415101337013,0.000786154850836649\n-5.88351139510037,0.000944473048314738\n-5.83287177683061,0.00113128438583456\n-5.78223215856085,0.00134643706585757\n-5.73159254029109,0.00159836312503365\n-5.68095292202132,0.00188571496632395\n-5.63031330375156,0.0022196885950333\n-5.5796736854818,0.00259715414867519\n-5.52903406721204,0.00303265735341914\n-5.47839444894228,0.00352056560349096\n-5.42775483067252,0.00407934503805124\n-5.37711521240276,0.0047004529767521\n-5.326475594133,0.00540550200386755\n-5.27583597586324,0.00618405183598116\n-5.22519635759348,0.00705893425476574\n-5.17455673932371,0.0080191072023048\n-5.12391712105395,0.00908742061428166\n-5.07327750278419,0.0102525700318956\n-5.02263788451443,0.011536540264123\n-4.97199826624467,0.0129282242456318\n-4.92135864797491,0.0144479725847901\n-4.87071902970515,0.0160854589152081\n-4.82007941143539,0.0178588816684187\n-4.76943979316562,0.0197593886061119\n-4.71880017489586,0.0218028218173057\n-4.6681605566261,0.0239825706883182\n-4.61752093835634,0.0263121695587262\n-4.56688132008658,0.0287880266179779\n-4.51624170181682,0.0314214732453827\n-4.46560208354706,0.0342126266953308\n-4.4149624652773,0.0371704895038212\n-4.36432284700754,0.0402993628627757\n-4.31368322873777,0.0436052877099654\n-4.26304361046801,0.0470968563596105\n-4.21240399219825,0.0507758726077093\n-4.16176437392849,0.0546548022399704\n-4.11112475565873,0.058729408644741\n-4.06048513738897,0.0630149572051555\n-4.00984551911921,0.0674992464963307\n-3.95920590084945,0.0721985439924027\n-3.90856628257969,0.0770912735941867\n-3.85792666430992,0.0821922166606427\n-3.80728704604016,0.0874702301479517\n-3.7566474277704,0.0929355851708663\n-3.70600780950064,0.0985491508736191\n-3.65536819123088,0.104313399282685\n-3.60472857296112,0.110184767643045\n-3.55408895469136,0.11615472510567\n-3.5034493364216,0.122180558246561\n-3.45280971815184,0.128239977733774\n-3.40217009988207,0.134297445213312\n-3.35153048161231,0.140314905441855\n-3.30089086334255,0.146269599368018\n-3.25025124507279,0.152109068249077\n-3.19961162680303,0.157824836164785\n-3.14897200853327,0.163355463700294\n-3.09833239026351,0.168704663467091\n-3.04769277199375,0.173807957556638\n-2.99705315372399,0.178678542635089\n-2.94641353545422,0.183254024014386\n-2.89577391718446,0.187553852520533\n-2.8451342989147,0.191521684337911\n-2.79449468064494,0.195179931459512\n-2.74385506237518,0.19848101398523\n-2.69321544410542,0.201447343857017\n-2.64257582583566,0.20404174197848\n-2.5919362075659,0.206284266173775\n-2.54129658929614,0.20814901449414\n-2.49065697102637,0.209652086376535\n-2.44001735275661,0.210779273533808\n-2.38937773448685,0.211541889452256\n-2.33873811621709,0.211937551715821\n-2.28809849794733,0.211972680663853\n-2.23745887967757,0.211656578629744\n-2.18681926140781,0.210991280076465\n-2.13617964313805,0.209997236046581\n-2.08554002486829,0.208673086938074\n-2.03490040659852,0.207049327302789\n-1.98426078832876,0.205122540291294\n-1.933621170059,0.20293145536231\n-1.88298155178924,0.200472136557306\n-1.83234193351948,0.197789014419159\n-1.78170231524972,0.19487922672888\n-1.73106269697996,0.191789768697138\n-1.6804230787102,0.188520351706501\n-1.62978346044043,0.185117051956721\n-1.57914384217067,0.181583418423317\n-1.52850422390091,0.177961120531498\n-1.47786460563115,0.174258446367745\n-1.42722498736139,0.17050953703742\n-1.37658536909163,0.166727864998606\n-1.32594575082187,0.162937624444763\n-1.27530613255211,0.15915741929058\n-1.22466651428235,0.155400447481837\n-1.17402689601259,0.151688677691185\n-1.12338727774282,0.148025735142591\n-1.07274765947306,0.144433968270808\n-1.0221080412033,0.140910081597249\n-0.97146842293354,0.137474310372855\n-0.920828804663779,0.134118923748513\n-0.870189186394018,0.130860563373832\n-0.819549568124257,0.127689269215366\n-0.768909949854496,0.124617595512591\n-0.718270331584735,0.121635188021134\n-0.667630713314974,0.118750834645593\n-0.616991095045213,0.115955213014107\n-0.566351476775452,0.113254190538598\n-0.51571185850569,0.110640508216639\n-0.465072240235929,0.108118141563836\n-0.414432621966168,0.105682590024831\n-0.363793003696407,0.103336832520212\n-0.313153385426646,0.101079574211764\n-0.262513767156885,0.0989133341309811\n-0.211874148887124,0.0968403240855814\n-0.161234530617363,0.0948626777688089\n-0.110594912347602,0.0929863628021366\n-0.0599552940778407,0.0912127483632783\n-0.00931567580807968,0.0895518237462779\n0.0413239424616823,0.0880034411379172\n0.0919635607314433,0.086581920482245\n0.142603179001204,0.0852845762523963\n0.193242797270965,0.0841303856779744\n0.243882415540726,0.0831129350780719\n0.294522033810487,0.0822561243281719\n0.345161652080249,0.0815485412522298\n0.39580127035001,0.0810190882111485\n0.446440888619771,0.0806501081562931\n0.497080506889532,0.0804752487658557\n0.547720125159293,0.0804695335413515\n0.598359743429054,0.080670608831731\n0.648999361698816,0.0810454828592973\n0.699638979968577,0.0816344415813186\n0.750278598238338,0.0823966253094308\n0.800918216508099,0.0833722844495797\n0.85155783477786,0.0845159428921624\n0.902197453047621,0.0858595118068199\n0.952837071317382,0.087359719889212\n1.00347668958714,0.0890364411810332\n1.0541163078569,0.0908494854265292\n1.10475592612667,0.0928058321421178\n1.15539554439643,0.0948690119374035\n1.20603516266619,0.0970333320295491\n1.25667478093595,0.0992663334710114\n1.30731439920571,0.101550995446656\n1.35795401747547,0.103858737838229\n1.40859363574523,0.106163577570116\n1.45923325401499,0.108440251668637\n1.50987287228475,0.110656988961072\n1.56051249055452,0.112790912975721\n1.61115210882428,0.114808167388431\n1.66179172709404,0.116687089088201\n1.7124313453638,0.118395665945242\n1.76307096363356,0.119912208684428\n1.81371058190332,0.121210407574131\n1.86435020017308,0.122267443519685\n1.91498981844284,0.123066211446259\n1.9656294367126,0.123581995475729\n2.01626905498236,0.123809765183479\n2.06690867325213,0.123722649022899\n2.11754829152189,0.123329658998929\n2.16818790979165,0.12260213381072\n2.21882752806141,0.121563943174975\n2.26946714633117,0.120185669181015\n2.32010676460093,0.118505505099393\n2.37074638287069,0.116494933419321\n2.42138600114045,0.114204491403603\n2.47202561941022,0.11160871226918\n2.52266523767998,0.108767103606388\n2.57330485594974,0.105659685132325\n2.6239444742195,0.102350392847247\n2.67458409248926,0.0988271899092355\n2.72522371075902,0.0951531265420756\n2.77586332902878,0.0913262970406674\n2.82650294729854,0.0874030495154418\n2.8771425655683,0.0833940178000616\n2.92778218383807,0.0793431432196026\n2.97842180210783,0.0752738717827504\n3.02906142037759,0.0712155296104978\n3.07970103864735,0.0672002725293287\n3.13034065691711,0.0632446304043856\n3.18098027518687,0.0593842257775338\n3.23161989345663,0.055625124555797\n3.28225951172639,0.0520016755346323\n3.33289912999616,0.0485120404440375\n3.38353874826592,0.0451855094332692\n3.43417836653568,0.0420148631691944\n3.48481798480544,0.0390217947785376\n3.5354576030752,0.0361959780950405\n3.58609722134496,0.0335503197126245\n3.63673683961472,0.0310732702832359\n3.68737645788448,0.0287690257278698\n3.73801607615424,0.026626240946604\n3.788655694424,0.0246415037277945\n3.83929531269377,0.0228046481879148\n3.88993493096353,0.021106441366155\n3.94057454923329,0.0195384761447752\n3.99121416750305,0.0180877892202396\n4.04185378577281,0.016748005045651\n4.09249340404257,0.0155044640513742\n4.14313302231233,0.0143528859089667\n4.19377264058209,0.0132786067573224\n4.24441225885185,0.012279382913479\n4.29505187712162,0.0113417164792519\n4.34569149539138,0.0104652827501569\n4.39633111366114,0.00963833605350667\n4.4469707319309,0.00886232814001695\n4.49761035020066,0.00812731388234524\n4.54824996847042,0.00743636228357907\n4.59888958674018,0.00678096833532732\n4.64952920500994,0.00616563333659405\n4.70016882327971,0.00558270008599158\n4.75080844154947,0.00503787678456034\n4.80144805981923,0.00452371531471804\n4.85208767808899,0.00404685558755046\n4.90272729635875,0.00359971419348659\n4.95336691462851,0.00318899390661082\n5.00400653289827,0.00280737879522019\n5.05464615116803,0.00246060587453467\n5.10528576943779,0.00214202657543967\n5.15592538770755,0.00185602642548727\n5.20656500597732,0.00159660699909905\n5.25720462424708,0.00136673690085742\n5.30784424251684,0.00116111797122008\n5.3584838607866,0.000981386052433441\n5.40912347905636,0.000822999789460201\n5.45976309732612,0.00068647104906379\n5.51040271559588,0.000568040905176132\n5.56104233386564,0.000467380806362385\n5.6116819521354,0.000381493990005194\n5.66232157040517,0.000309513757450964\n5.71296118867493,0.000249141526338047\n5.76360080694469,0.000199246083454596\n5.81424042521445,0.000158131751547988\n5.86488004348421,0.000124617557797427\n5.91551966175397,9.75002767588506e-05\n5.96615928002373,7.56936202985758e-05\n6.01679889829349,5.83760724740532e-05\n6.06743851656326,4.46341950692847e-05\n6.11807813483302,3.39280500516269e-05\n6.16871775310278,2.5542574698958e-05"
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-7.20415321635949\n6.4557859180009"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-0.0105986340331927\n0.222571314697046"
        }
    ],
    "scales": [
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "nice": true,
            "zero": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "area",
            "properties": {
                "update": {
                    "fill": {
                        "value": "#333333"
                    },
                    "y2": {
                        "scale": "y",
                        "value": 0
                    },
                    "fillOpacity": {
                        "value": 0.2
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.pred_"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.resp_"
                    },
                    "stroke": {
                        "value": "transparent"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0/density1"
                    }
                }
            },
            "from": {
                "data": ".0/density1"
            }
        },
        {
            "type": "line",
            "properties": {
                "update": {
                    "stroke": {
                        "value": "#000000"
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.pred_"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.resp_"
                    },
                    "fill": {
                        "value": "transparent"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0/density1"
                    }
                }
            },
            "from": {
                "data": ".0/density1"
            }
        }
    ],
    "legends": [

    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "log_crim"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "density"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id271790523").parseSpec(plot_id271790523_spec);
</script><!--/html_preserve-->

It seems like we have a mixture of two distinct normal distributions
within the distribution of `log_crim`.

b) Plot the density curve of  log crime (`lob_crim`) grouped by and filled (coloured) by the radial highway locations (`rad`). Discuss differences due highway locations and compare to the plot in a).

```r
Boston %>% ggvis(~log_crim) %>%
    group_by(rad) %>%
    layer_densities(fill=~rad) %>%
    scale_numeric("x", domain = domain, nice = TRUE)
```

<!--html_preserve--><div id="plot_id716726155-container" class="ggvis-output-container">
<div id="plot_id716726155" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id716726155_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id716726155" data-renderer="svg">SVG</a>
 | 
<a id="plot_id716726155_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id716726155" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id716726155_download" class="ggvis-download" data-plot-id="plot_id716726155">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id716726155_spec = {
    "data": [
        {
            "name": ".0/group_by1/density2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "pred_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"pred_\",\"resp_\",\"rad\"\n-6.19142339113506,0.000621199501952393,\"1\"\n-6.17139279677451,0.000728381025700052,\"1\"\n-6.15136220241396,0.000853227925482797,\"1\"\n-6.13133160805341,0.000996753836329054,\"1\"\n-6.11130101369286,0.001160227778796,\"1\"\n-6.09127041933231,0.00134584734662917,\"1\"\n-6.07123982497177,0.00156013020272125,\"1\"\n-6.05120923061122,0.00180270259653403,\"1\"\n-6.03117863625067,0.00207587914015647,\"1\"\n-6.01114804189012,0.00238303831813292,\"1\"\n-5.99111744752957,0.00273330176325073,\"1\"\n-5.97108685316902,0.00312450875007876,\"1\"\n-5.95105625880847,0.00356015635033618,\"1\"\n-5.93102566444793,0.00404607954738564,\"1\"\n-5.91099507008738,0.00459137913185526,\"1\"\n-5.89096447572683,0.00519364375343787,\"1\"\n-5.87093388136628,0.00585693284329619,\"1\"\n-5.85090328700573,0.00659040907759927,\"1\"\n-5.83087269264518,0.00740111083225793,\"1\"\n-5.81084209828463,0.00828670789389909,\"1\"\n-5.79081150392408,0.00925147719182721,\"1\"\n-5.77078090956354,0.0103086471140534,\"1\"\n-5.75075031520299,0.0114605612058025,\"1\"\n-5.73071972084244,0.0127054565622651,\"1\"\n-5.71068912648189,0.0140473316594146,\"1\"\n-5.69065853212134,0.0155038669786581,\"1\"\n-5.67062793776079,0.017069973181529,\"1\"\n-5.65059734340024,0.0187450356293031,\"1\"\n-5.6305667490397,0.0205322232266348,\"1\"\n-5.61053615467915,0.0224535296920415,\"1\"\n-5.5905055603186,0.0244942786408743,\"1\"\n-5.57047496595805,0.0266555760373938,\"1\"\n-5.5504443715975,0.0289418559795427,\"1\"\n-5.53041377723695,0.0313710615239197,\"1\"\n-5.5103831828764,0.033925687327298,\"1\"\n-5.49035258851585,0.0366064534360498,\"1\"\n-5.47032199415531,0.0394201432787243,\"1\"\n-5.45029139979476,0.0423756345871959,\"1\"\n-5.43026080543421,0.0454571882619961,\"1\"\n-5.41023021107366,0.0486640750554571,\"1\"\n-5.39019961671311,0.0520051974491108,\"1\"\n-5.37016902235256,0.0554801864924662,\"1\"\n-5.35013842799201,0.0590761364751111,\"1\"\n-5.33010783363147,0.0627912173511502,\"1\"\n-5.31007723927092,0.0666361447708207,\"1\"\n-5.29004664491037,0.0706024687343509,\"1\"\n-5.27001605054982,0.0746807606821289,\"1\"\n-5.24998545618927,0.0788686107597598,\"1\"\n-5.22995486182872,0.0831782719526158,\"1\"\n-5.20992426746817,0.087595051036818,\"1\"\n-5.18989367310762,0.0921131281275867,\"1\"\n-5.16986307874708,0.0967299646456828,\"1\"\n-5.14983248438653,0.101459046035794,\"1\"\n-5.12980189002598,0.106281155450369,\"1\"\n-5.10977129566543,0.111193701137874,\"1\"\n-5.08974070130488,0.1161966469099,\"1\"\n-5.06971010694433,0.121296588633297,\"1\"\n-5.04967951258378,0.126478479386401,\"1\"\n-5.02964891822324,0.13173959703877,\"1\"\n-5.00961832386269,0.137081443682723,\"1\"\n-4.98958772950214,0.1425042428534,\"1\"\n-4.96955713514159,0.147996213684537,\"1\"\n-4.94952654078104,0.153553812246447,\"1\"\n-4.92949594642049,0.159178325013897,\"1\"\n-4.90946535205994,0.164864121090675,\"1\"\n-4.88943475769939,0.170601468812304,\"1\"\n-4.86940416333885,0.176385207490314,\"1\"\n-4.8493735689783,0.182213821538999,\"1\"\n-4.82934297461775,0.188077218482692,\"1\"\n-4.8093123802572,0.193966503126507,\"1\"\n-4.78928178589665,0.19987432034502,\"1\"\n-4.7692511915361,0.205793104087395,\"1\"\n-4.74922059717555,0.211711749674978,\"1\"\n-4.72919000281501,0.217620980029338,\"1\"\n-4.70915940845446,0.223511027860781,\"1\"\n-4.68912881409391,0.229365672587999,\"1\"\n-4.66909821973336,0.235178066234318,\"1\"\n-4.64906762537281,0.240937549731612,\"1\"\n-4.62903703101226,0.246630381282338,\"1\"\n-4.60900643665171,0.252236412882844,\"1\"\n-4.58897584229117,0.257753606152379,\"1\"\n-4.56894524793062,0.263171160052295,\"1\"\n-4.54891465357007,0.26847022131155,\"1\"\n-4.52888405920952,0.273634279801723,\"1\"\n-4.50885346484897,0.278665388510255,\"1\"\n-4.48882287048842,0.28355453093135,\"1\"\n-4.46879227612787,0.288277807604743,\"1\"\n-4.44876168176732,0.292830086336012,\"1\"\n-4.42873108740678,0.297216188780631,\"1\"\n-4.40870049304623,0.301430767291931,\"1\"\n-4.38866989868568,0.305446521301401,\"1\"\n-4.36863930432513,0.309274992278696,\"1\"\n-4.34860870996458,0.312922233382784,\"1\"\n-4.32857811560403,0.316387854825928,\"1\"\n-4.30854752124348,0.319644355564828,\"1\"\n-4.28851692688294,0.322720665091702,\"1\"\n-4.26848633252239,0.325622875574064,\"1\"\n-4.24845573816184,0.328353129276714,\"1\"\n-4.22842514380129,0.330896218111294,\"1\"\n-4.20839454944074,0.333285612917175,\"1\"\n-4.18836395508019,0.335529817963043,\"1\"\n-4.16833336071964,0.337632422624227,\"1\"\n-4.14830276635909,0.339596105240403,\"1\"\n-4.12827217199855,0.34144878743381,\"1\"\n-4.108241577638,0.343202392498306,\"1\"\n-4.08821098327745,0.344863772445225,\"1\"\n-4.0681803889169,0.34644734113173,\"1\"\n-4.04814979455635,0.347974331731091,\"1\"\n-4.0281192001958,0.349458237253098,\"1\"\n-4.00808860583525,0.350911013108181,\"1\"\n-3.98805801147471,0.352350085114707,\"1\"\n-3.96802741711416,0.353790585360178,\"1\"\n-3.94799682275361,0.355245689606242,\"1\"\n-3.92796622839306,0.35673379029204,\"1\"\n-3.90793563403251,0.358266377482867,\"1\"\n-3.88790503967196,0.359854219602891,\"1\"\n-3.86787444531141,0.361508535111905,\"1\"\n-3.84784385095086,0.363254445709577,\"1\"\n-3.82781325659032,0.365090287220598,\"1\"\n-3.80778266222977,0.367024492304845,\"1\"\n-3.78775206786922,0.36906791154751,\"1\"\n-3.76772147350867,0.371242275527618,\"1\"\n-3.74769087914812,0.373537399041294,\"1\"\n-3.72766028478757,0.375957855007567,\"1\"\n-3.70762969042702,0.378514418662782,\"1\"\n-3.68759909606648,0.381217450778005,\"1\"\n-3.66756850170593,0.384052492633954,\"1\"\n-3.64753790734538,0.38701878751877,\"1\"\n-3.62750731298483,0.390124964373967,\"1\"\n-3.60747671862428,0.393366347837741,\"1\"\n-3.58744612426373,0.39672629489573,\"1\"\n-3.56741552990318,0.400197655465453,\"1\"\n-3.54738493554263,0.403782340238734,\"1\"\n-3.52735434118209,0.407461513286854,\"1\"\n-3.50732374682154,0.411216917701946,\"1\"\n-3.48729315246099,0.415034258503022,\"1\"\n-3.46726255810044,0.418901661117573,\"1\"\n-3.44723196373989,0.422792925882725,\"1\"\n-3.42720136937934,0.426687102051672,\"1\"\n-3.40717077501879,0.430562597651025,\"1\"\n-3.38714018065825,0.434385578552458,\"1\"\n-3.3671095862977,0.438135416799615,\"1\"\n-3.34707899193715,0.441786091144792,\"1\"\n-3.3270483975766,0.445305040993311,\"1\"\n-3.30701780321605,0.448642906802812,\"1\"\n-3.2869872088555,0.451788925090584,\"1\"\n-3.26695661449495,0.454713519056331,\"1\"\n-3.2469260201344,0.457368507698815,\"1\"\n-3.22689542577386,0.459705925197253,\"1\"\n-3.20686483141331,0.461723915152095,\"1\"\n-3.18683423705276,0.463393513068032,\"1\"\n-3.16680364269221,0.464648234470126,\"1\"\n-3.14677304833166,0.465462378004239,\"1\"\n-3.12674245397111,0.465840381487472,\"1\"\n-3.10671185961056,0.465758979724192,\"1\"\n-3.08668126525002,0.465134516465421,\"1\"\n-3.06665067088947,0.463982758562091,\"1\"\n-3.04662007652892,0.462310991973998,\"1\"\n-3.02658948216837,0.460106535831615,\"1\"\n-3.00655888780782,0.45727467517487,\"1\"\n-2.98652829344727,0.453883610283296,\"1\"\n-2.96649769908672,0.449940435252521,\"1\"\n-2.94646710472617,0.445439221118468,\"1\"\n-2.92643651036563,0.440306727176472,\"1\"\n-2.90640591600508,0.434639508165636,\"1\"\n-2.88637532164453,0.428450281766006,\"1\"\n-2.86634472728398,0.421734056614112,\"1\"\n-2.84631413292343,0.4144728181144,\"1\"\n-2.82628353856288,0.406754203856545,\"1\"\n-2.80625294420233,0.398604160763148,\"1\"\n-2.78622234984179,0.390024011768349,\"1\"\n-2.76619175548124,0.381042067142674,\"1\"\n-2.74616116112069,0.37173059093895,\"1\"\n-2.72613056676014,0.362124417085837,\"1\"\n-2.70609997239959,0.352236884983933,\"1\"\n-2.68606937803904,0.342121468708377,\"1\"\n-2.66603878367849,0.331832904035486,\"1\"\n-2.64600818931794,0.321408862263549,\"1\"\n-2.6259775949574,0.310878760285631,\"1\"\n-2.60594700059685,0.300294556799614,\"1\"\n-2.5859164062363,0.289695312448084,\"1\"\n-2.56588581187575,0.279115339473531,\"1\"\n-2.5458552175152,0.268601074181618,\"1\"\n-2.52582462315465,0.258178951792436,\"1\"\n-2.5057940287941,0.24787683599993,\"1\"\n-2.48576343443356,0.237724691925808,\"1\"\n-2.46573284007301,0.227771994378858,\"1\"\n-2.44570224571246,0.218016387957246,\"1\"\n-2.42567165135191,0.20847598623518,\"1\"\n-2.40564105699136,0.199179216191448,\"1\"\n-2.38561046263081,0.190157874921175,\"1\"\n-2.36557986827026,0.181393817798869,\"1\"\n-2.34554927390971,0.172894664774187,\"1\"\n-2.32551867954917,0.164688360048889,\"1\"\n-2.30548808518862,0.156780596487045,\"1\"\n-2.28545749082807,0.14914786408597,\"1\"\n-2.26542689646752,0.141789267487776,\"1\"\n-2.24539630210697,0.13473245240175,\"1\"\n-2.22536570774642,0.127958339682698,\"1\"\n-2.20533511338587,0.121445527893329,\"1\"\n-2.18530451902533,0.115187837545724,\"1\"\n-2.16527392466478,0.109212599865998,\"1\"\n-2.14524333030423,0.103483073507801,\"1\"\n-2.12521273594368,0.0979840448573541,\"1\"\n-2.10518214158313,0.0927075155926558,\"1\"\n-2.08515154722258,0.0876803300222651,\"1\"\n-2.06512095286203,0.0828567785550839,\"1\"\n-2.04509035850148,0.07822894993472,\"1\"\n-2.02505976414094,0.0737947441608806,\"1\"\n-2.00502916978039,0.0695654746995867,\"1\"\n-1.98499857541984,0.0655081613508074,\"1\"\n-1.96496798105929,0.0616164665041418,\"1\"\n-1.94493738669874,0.0578938533798625,\"1\"\n-1.92490679233819,0.0543415595967741,\"1\"\n-1.90487619797764,0.0509371848081448,\"1\"\n-1.8848456036171,0.0476762815631836,\"1\"\n-1.86481500925655,0.0445670988656241,\"1\"\n-1.844784414896,0.0416037676560283,\"1\"\n-1.82475382053545,0.0387717889535127,\"1\"\n-1.8047232261749,0.0360680943274174,\"1\"\n-1.78469263181435,0.0335046543156621,\"1\"\n-1.7646620374538,0.0310694853649021,\"1\"\n-1.74463144309325,0.0287536984700498,\"1\"\n-1.72460084873271,0.0265547878644868,\"1\"\n-1.70457025437216,0.024487171787449,\"1\"\n-1.68453966001161,0.0225330564423875,\"1\"\n-1.66450906565106,0.0206875400625294,\"1\"\n-1.64447847129051,0.0189489725283316,\"1\"\n-1.62444787692996,0.0173299048201004,\"1\"\n-1.60441728256941,0.0158104162935319,\"1\"\n-1.58438668820887,0.0143875387365958,\"1\"\n-1.56435609384832,0.0130617545858898,\"1\"\n-1.54432549948777,0.0118376029987784,\"1\"\n-1.52429490512722,0.0106995073406496,\"1\"\n-1.50426431076667,0.00964404966043326,\"1\"\n-1.48423371640612,0.00867297989558514,\"1\"\n-1.46420312204557,0.00778443384479206,\"1\"\n-1.44417252768502,0.00696680798005168,\"1\"\n-1.42414193332448,0.00621646789829411,\"1\"\n-1.40411133896393,0.00553574175697064,\"1\"\n-1.38408074460338,0.00491851289529485,\"1\"\n-1.36405015024283,0.00435668630067264,\"1\"\n-1.34401955588228,0.00384674775384254,\"1\"\n-1.32398896152173,0.0033910937103875,\"1\"\n-1.30395836716118,0.00298157512977605,\"1\"\n-1.28392777280064,0.00261297636016605,\"1\"\n-1.26389717844009,0.00228219831850854,\"1\"\n-1.24386658407954,0.00199137872994841,\"1\"\n-1.22383598971899,0.00173217193573698,\"1\"\n-1.20380539535844,0.00150152074040808,\"1\"\n-1.18377480099789,0.00129734327660609,\"1\"\n-1.16374420663734,0.00112007432334001,\"1\"\n-1.1437136122768,0.000963659144878039,\"1\"\n-1.12368301791625,0.000826077050288404,\"1\"\n-1.1036524235557,0.000706176280787464,\"1\"\n-1.08362182919515,0.00060304263628771,\"1\"\n-4.90738691857581,0.000940526447373951,\"2\"\n-4.887851809175,0.00112227003656111,\"2\"\n-4.86831669977419,0.00133323328220165,\"2\"\n-4.84878159037337,0.00158345735650975,\"2\"\n-4.82924648097256,0.00187297626506606,\"2\"\n-4.80971137157174,0.00220590241438111,\"2\"\n-4.79017626217093,0.00259255585041323,\"2\"\n-4.77064115277012,0.00303975336007979,\"2\"\n-4.7511060433693,0.00354916212146561,\"2\"\n-4.73157093396849,0.00412914456321699,\"2\"\n-4.71203582456768,0.00479895120060982,\"2\"\n-4.69250071516686,0.00555472075264108,\"2\"\n-4.67296560576605,0.00640458021504425,\"2\"\n-4.65343049636523,0.00737225091362602,\"2\"\n-4.63389538696442,0.00845955443109489,\"2\"\n-4.61436027756361,0.00967068471051748,\"2\"\n-4.59482516816279,0.0110245037112476,\"2\"\n-4.57529005876198,0.0125415352873462,\"2\"\n-4.55575494936117,0.0142153487898455,\"2\"\n-4.53621983996035,0.0160562145860762,\"2\"\n-4.51668473055954,0.0181076095032825,\"2\"\n-4.49714962115872,0.0203513027670205,\"2\"\n-4.47761451175791,0.0227957327859779,\"2\"\n-4.4580794023571,0.0254736877969865,\"2\"\n-4.43854429295628,0.028391461215863,\"2\"\n-4.41900918355547,0.0315405020930814,\"2\"\n-4.39947407415466,0.0349368772343142,\"2\"\n-4.37993896475384,0.0386189802058319,\"2\"\n-4.36040385535303,0.0425559804483816,\"2\"\n-4.34086874595221,0.0467530910651994,\"2\"\n-4.3213336365514,0.0512534855641055,\"2\"\n-4.30179852715059,0.0560324217086183,\"2\"\n-4.28226341774977,0.0610807625827756,\"2\"\n-4.26272830834896,0.0664183995596459,\"2\"\n-4.24319319894815,0.072053953916256,\"2\"\n-4.22365808954733,0.0779542522804084,\"2\"\n-4.20412298014652,0.084115318393519,\"2\"\n-4.1845878707457,0.0905768232765354,\"2\"\n-4.16505276134489,0.0972836389112661,\"2\"\n-4.14551765194408,0.104227111211306,\"2\"\n-4.12598254254326,0.111422122433086,\"2\"\n-4.10644743314245,0.1188453300721,\"2\"\n-4.08691232374164,0.126468580596042,\"2\"\n-4.06737721434082,0.134286099677402,\"2\"\n-4.04784210494001,0.142298601461302,\"2\"\n-4.02830699553919,0.150465319707396,\"2\"\n-4.00877188613838,0.158771324132921,\"2\"\n-3.98923677673757,0.167218326380944,\"2\"\n-3.96970166733675,0.175772952295103,\"2\"\n-3.95016655793594,0.18441636148201,\"2\"\n-3.93063144853513,0.193138345906277,\"2\"\n-3.91109633913431,0.201921991869196,\"2\"\n-3.8915612297335,0.210746033275852,\"2\"\n-3.87202612033268,0.219597025265521,\"2\"\n-3.85249101093187,0.228461902446829,\"2\"\n-3.83295590153106,0.237324654463962,\"2\"\n-3.81342079213024,0.24617393163254,\"2\"\n-3.79388568272943,0.254994774169219,\"2\"\n-3.77435057332862,0.263777017527381,\"2\"\n-3.7548154639278,0.27251322872726,\"2\"\n-3.73528035452699,0.281191651805448,\"2\"\n-3.71574524512617,0.289797813855208,\"2\"\n-3.69621013572536,0.298331973113383,\"2\"\n-3.67667502632455,0.306787183323214,\"2\"\n-3.65713991692373,0.315141425380876,\"2\"\n-3.63760480752292,0.323399711372387,\"2\"\n-3.61806969812211,0.331557226480452,\"2\"\n-3.59853458872129,0.339596623973407,\"2\"\n-3.57899947932048,0.347509685267841,\"2\"\n-3.55946436991966,0.355299291740809,\"2\"\n-3.53992926051885,0.362954992022909,\"2\"\n-3.52039415111804,0.370447591390993,\"2\"\n-3.50085904171722,0.377790578192017,\"2\"\n-3.48132393231641,0.384975203827773,\"2\"\n-3.4617888229156,0.391968444534833,\"2\"\n-3.44225371351478,0.398773129639716,\"2\"\n-3.42271860411397,0.405388044538354,\"2\"\n-3.40318349471315,0.411790023003663,\"2\"\n-3.38364838531234,0.417954211217199,\"2\"\n-3.36411327591153,0.423895546892555,\"2\"\n-3.34457816651071,0.429604300398313,\"2\"\n-3.3250430571099,0.435026924142242,\"2\"\n-3.30550794770908,0.440193702968708,\"2\"\n-3.28597283830827,0.445098946507346,\"2\"\n-3.26643772890746,0.449706822571293,\"2\"\n-3.24690261950664,0.454015735233897,\"2\"\n-3.22736751010583,0.458042914105747,\"2\"\n-3.20783240070502,0.461775524289345,\"2\"\n-3.1882972913042,0.465175515827161,\"2\"\n-3.16876218190339,0.468286287254389,\"2\"\n-3.14922707250258,0.47110844336096,\"2\"\n-3.12969196310176,0.473602883467793,\"2\"\n-3.11015685370095,0.475803668924206,\"2\"\n-3.09062174430013,0.477727514811745,\"2\"\n-3.07108663489932,0.47936163535049,\"2\"\n-3.05155152549851,0.48070476395461,\"2\"\n-3.03201641609769,0.481796138485632,\"2\"\n-3.01248130669688,0.482645421420854,\"2\"\n-2.99294619729607,0.483222485295014,\"2\"\n-2.97341108789525,0.48358293090425,\"2\"\n-2.95387597849444,0.48373858504757,\"2\"\n-2.93434086909362,0.483681122540489,\"2\"\n-2.91480575969281,0.483433461392612,\"2\"\n-2.895270650292,0.48302146120444,\"2\"\n-2.87573554089118,0.482451357612141,\"2\"\n-2.85620043149037,0.481722735878501,\"2\"\n-2.83666532208956,0.480868064793418,\"2\"\n-2.81713021268874,0.479897741401671,\"2\"\n-2.79759510328793,0.478806432710292,\"2\"\n-2.77805999388711,0.47761733450638,\"2\"\n-2.7585248844863,0.476341231389314,\"2\"\n-2.73898977508549,0.474978581754665,\"2\"\n-2.71945466568467,0.47353446658212,\"2\"\n-2.69991955628386,0.472021055077309,\"2\"\n-2.68038444688304,0.47044061964921,\"2\"\n-2.66084933748223,0.468784496962768,\"2\"\n-2.64131422808142,0.467064409640315,\"2\"\n-2.6217791186806,0.46527948322546,\"2\"\n-2.60224400927979,0.463419071940497,\"2\"\n-2.58270889987898,0.461483404091206,\"2\"\n-2.56317379047816,0.459472139667067,\"2\"\n-2.54363868107735,0.457375253107264,\"2\"\n-2.52410357167653,0.455177618843793,\"2\"\n-2.50456846227572,0.452882703956165,\"2\"\n-2.48503335287491,0.45048213523461,\"2\"\n-2.46549824347409,0.447946281655927,\"2\"\n-2.44596313407328,0.445280413734101,\"2\"\n-2.42642802467247,0.442476511453667,\"2\"\n-2.40689291527165,0.439508843457357,\"2\"\n-2.38735780587084,0.436363648403892,\"2\"\n-2.36782269647002,0.433043642252306,\"2\"\n-2.34828758706921,0.429533378502345,\"2\"\n-2.3287524776684,0.425790948937762,\"2\"\n-2.30921736826758,0.421836717848466,\"2\"\n-2.28968225886677,0.417660187915888,\"2\"\n-2.27014714946596,0.413218996309805,\"2\"\n-2.25061204006514,0.408521469939674,\"2\"\n-2.23107693066433,0.40357157360975,\"2\"\n-2.21154182126351,0.398345731962162,\"2\"\n-2.1920067118627,0.392820366047383,\"2\"\n-2.17247160246189,0.387023536336422,\"2\"\n-2.15293649306107,0.380952314704316,\"2\"\n-2.13340138366026,0.374556971653435,\"2\"\n-2.11386627425945,0.367883503739708,\"2\"\n-2.09433116485863,0.360937221478229,\"2\"\n-2.07479605545782,0.353695265342701,\"2\"\n-2.055260946057,0.346170679998897,\"2\"\n-2.03572583665619,0.338393986591383,\"2\"\n-2.01619072725538,0.330368362000873,\"2\"\n-1.99665561785456,0.322079897258334,\"2\"\n-1.97712050845375,0.313579976662211,\"2\"\n-1.95758539905294,0.304884655465142,\"2\"\n-1.93805028965212,0.295988858981664,\"2\"\n-1.91851518025131,0.286933663563332,\"2\"\n-1.89898007085049,0.27774744078259,\"2\"\n-1.87944496144968,0.26844661273122,\"2\"\n-1.85990985204887,0.259056505454716,\"2\"\n-1.84037474264805,0.249613623862499,\"2\"\n-1.82083963324724,0.240143504494683,\"2\"\n-1.80130452384643,0.230676960828246,\"2\"\n-1.78176941444561,0.221244177500753,\"2\"\n-1.7622343050448,0.211871910072661,\"2\"\n-1.74269919564398,0.202599891061403,\"2\"\n-1.72316408624317,0.193456518306048,\"2\"\n-1.70362897684236,0.184460861495285,\"2\"\n-1.68409386744154,0.175646206695183,\"2\"\n-1.66455875804073,0.167062095943597,\"2\"\n-1.64502364863992,0.158705249531295,\"2\"\n-1.6254885392391,0.150597154412705,\"2\"\n-1.60595342983829,0.142802189362322,\"2\"\n-1.58641832043747,0.135308366412264,\"2\"\n-1.56688321103666,0.128123120770236,\"2\"\n-1.54734810163585,0.121288075854867,\"2\"\n-1.52781299223503,0.114824988514524,\"2\"\n-1.50827788283422,0.108709265352029,\"2\"\n-1.48874277343341,0.102951428795251,\"2\"\n-1.46920766403259,0.0976165897579318,\"2\"\n-1.44967255463178,0.0926437756035973,\"2\"\n-1.43013744523096,0.0880324719752866,\"2\"\n-1.41060233583015,0.0838234286974888,\"2\"\n-1.39106722642934,0.0799885473673716,\"2\"\n-1.37153211702852,0.0764975597631583,\"2\"\n-1.35199700762771,0.0733568968728606,\"2\"\n-1.3324618982269,0.0705790494528682,\"2\"\n-1.31292678882608,0.0681051587395737,\"2\"\n-1.29339167942527,0.0659194327983331,\"2\"\n-1.27385657002445,0.0640460263811854,\"2\"\n-1.25432146062364,0.0624247254489869,\"2\"\n-1.23478635122283,0.0610298517941361,\"2\"\n-1.21525124182201,0.0598574526703045,\"2\"\n-1.1957161324212,0.058881411525117,\"2\"\n-1.17618102302039,0.0580610723775788,\"2\"\n-1.15664591361957,0.0573766105629286,\"2\"\n-1.13711080421876,0.0568183237977411,\"2\"\n-1.11757569481794,0.056343691119026,\"2\"\n-1.09804058541713,0.0559318576637414,\"2\"\n-1.07850547601632,0.0555657189245577,\"2\"\n-1.0589703666155,0.0552193811197058,\"2\"\n-1.03943525721469,0.0548731517075202,\"2\"\n-1.01990014781388,0.0545086365863609,\"2\"\n-1.00036503841306,0.0541059322692401,\"2\"\n-0.980829929012248,0.0536546792055958,\"2\"\n-0.961294819611434,0.0531435529959871,\"2\"\n-0.94175971021062,0.0525493425035552,\"2\"\n-0.922224600809807,0.0518747465550201,\"2\"\n-0.902689491408993,0.0511149126588291,\"2\"\n-0.883154382008179,0.0502564783453955,\"2\"\n-0.863619272607365,0.0492985625786835,\"2\"\n-0.844084163206552,0.0482484808083593,\"2\"\n-0.824549053805738,0.0471048601099275,\"2\"\n-0.805013944404924,0.0458599410241825,\"2\"\n-0.785478835004111,0.0445321408477401,\"2\"\n-0.765943725603297,0.0431268702025344,\"2\"\n-0.746408616202483,0.0416413853738059,\"2\"\n-0.726873506801669,0.0400914620911065,\"2\"\n-0.707338397400855,0.0384881588989783,\"2\"\n-0.687803288000041,0.0368375653552145,\"2\"\n-0.668268178599228,0.0351490375771225,\"2\"\n-0.648733069198414,0.0334366234354026,\"2\"\n-0.6291979597976,0.0317095073308791,\"2\"\n-0.609662850396786,0.0299773824377041,\"2\"\n-0.590127740995973,0.0282512082436156,\"2\"\n-0.570592631595159,0.0265394091282878,\"2\"\n-0.551057522194345,0.0248533658108078,\"2\"\n-0.531522412793532,0.0232011240042603,\"2\"\n-0.511987303392718,0.0215875817437033,\"2\"\n-0.492452193991904,0.0200207944079423,\"2\"\n-0.47291708459109,0.018512388693992,\"2\"\n-0.453381975190276,0.0170600128528606,\"2\"\n-0.433846865789462,0.0156673767949872,\"2\"\n-0.414311756388649,0.0143474791450126,\"2\"\n-0.394776646987835,0.0130952744147818,\"2\"\n-0.375241537587021,0.0119101443278166,\"2\"\n-0.355706428186208,0.0107984275816336,\"2\"\n-0.336171318785394,0.00976191694752552,\"2\"\n-0.31663620938458,0.00879301032080754,\"2\"\n-0.297101099983767,0.0078913278642026,\"2\"\n-0.277565990582953,0.00706615519647453,\"2\"\n-0.258030881182139,0.00630390095073184,\"2\"\n-0.238495771781325,0.00560240554866213,\"2\"\n-0.218960662380511,0.00496604938965043,\"2\"\n-0.199425552979697,0.00438825023960272,\"2\"\n-0.179890443578884,0.00386252603365427,\"2\"\n-0.16035533417807,0.00338825855790168,\"2\"\n-0.140820224777256,0.00296583119197915,\"2\"\n-0.121285115376442,0.0025856966232284,\"2\"\n-0.101750005975629,0.00224489438035901,\"2\"\n-0.0822148965748148,0.00194598284108612,\"2\"\n-0.0626797871740008,0.00168058883974943,\"2\"\n-0.0431446777731876,0.00144519893868427,\"2\"\n-0.0236095683723736,0.00123946433573194,\"2\"\n-0.00407445897155956,0.00106045202938108,\"2\"\n0.0154606504292536,0.000903337084353199,\"2\"\n0.0349957598300676,0.000766295392796678,\"2\"\n0.0545308692308817,0.000649580583525902,\"2\"\n0.0740659786316947,0.000548190988151861,\"2\"\n-5.38136580228526,0.000475561999953937,\"3\"\n-5.36074513802381,0.00056716279425218,\"3\"\n-5.34012447376235,0.00067571135598707,\"3\"\n-5.31950380950089,0.000802550588957001,\"3\"\n-5.29888314523943,0.000949052295189251,\"3\"\n-5.27826248097797,0.00111896542747491,\"3\"\n-5.25764181671652,0.00131758271872487,\"3\"\n-5.23702115245506,0.00154486510739582,\"3\"\n-5.2164004881936,0.00180404335326051,\"3\"\n-5.19577982393214,0.00210488315947341,\"3\"\n-5.17515915967069,0.00244690605066817,\"3\"\n-5.15453849540923,0.00283329922179777,\"3\"\n-5.13391783114777,0.00327289813835408,\"3\"\n-5.11329716688631,0.00377211555362158,\"3\"\n-5.09267650262485,0.00433079880179813,\"3\"\n-5.0720558383634,0.00495458875543881,\"3\"\n-5.05143517410194,0.00566129715187609,\"3\"\n-5.03081450984048,0.00644468744073773,\"3\"\n-5.01019384557902,0.00731023400306159,\"3\"\n-4.98957318131757,0.00827450501616034,\"3\"\n-4.96895251705611,0.00933970486531782,\"3\"\n-4.94833185279465,0.0105053937591306,\"3\"\n-4.92771118853319,0.0117816803102067,\"3\"\n-4.90709052427173,0.0131860568467377,\"3\"\n-4.88646986001028,0.0147081365500995,\"3\"\n-4.86584919574882,0.0163525723365947,\"3\"\n-4.84522853148736,0.0181433801675117,\"3\"\n-4.8246078672259,0.02007009841481,\"3\"\n-4.80398720296445,0.0221315878050789,\"3\"\n-4.78336653870299,0.0243409302617156,\"3\"\n-4.76274587444153,0.0267052175609015,\"3\"\n-4.74212521018007,0.0292103170219402,\"3\"\n-4.72150454591861,0.0318564917190443,\"3\"\n-4.70088388165716,0.0346688383411843,\"3\"\n-4.6802632173957,0.0376198368846638,\"3\"\n-4.65964255313424,0.040706856312853,\"3\"\n-4.63902188887278,0.0439403859644212,\"3\"\n-4.61840122461133,0.0473109191490399,\"3\"\n-4.59778056034987,0.0508029686568303,\"3\"\n-4.57715989608841,0.0544138377162345,\"3\"\n-4.55653923182695,0.058148147050558,\"3\"\n-4.5359185675655,0.061980724040982,\"3\"\n-4.51529790330404,0.0659031294208444,\"3\"\n-4.49467723904258,0.0699164751569272,\"3\"\n-4.47405657478112,0.0740018069292398,\"3\"\n-4.45343591051966,0.0781459358236788,\"3\"\n-4.43281524625821,0.082341464765017,\"3\"\n-4.41219458199675,0.0865784767372799,\"3\"\n-4.39157391773529,0.0908416961469501,\"3\"\n-4.37095325347383,0.0951216579080339,\"3\"\n-4.35033258921238,0.0994091099876552,\"3\"\n-4.32971192495092,0.10369321872011,\"3\"\n-4.30909126068946,0.107966226470498,\"3\"\n-4.288470596428,0.112218762021308,\"3\"\n-4.26784993216654,0.116443752513557,\"3\"\n-4.24722926790509,0.12063802911192,\"3\"\n-4.22660860364363,0.12479683221803,\"3\"\n-4.20598793938217,0.1289116148571,\"3\"\n-4.18536727512071,0.132986620658122,\"3\"\n-4.16474661085926,0.137021267918413,\"3\"\n-4.1441259465978,0.141010804782265,\"3\"\n-4.12350528233634,0.144960005597998,\"3\"\n-4.10288461807488,0.148873287207069,\"3\"\n-4.08226395381342,0.152751976597281,\"3\"\n-4.06164328955197,0.156598224547669,\"3\"\n-4.04102262529051,0.160420236832334,\"3\"\n-4.02040196102905,0.164222309476383,\"3\"\n-3.99978129676759,0.16800703404659,\"3\"\n-3.97916063250614,0.171781655265856,\"3\"\n-3.95853996824468,0.175550626246062,\"3\"\n-3.93791930398322,0.179317959767797,\"3\"\n-3.91729863972176,0.183087775432512,\"3\"\n-3.89667797546031,0.18686287624794,\"3\"\n-3.87605731119885,0.190645505483236,\"3\"\n-3.85543664693739,0.19443874786316,\"3\"\n-3.83481598267593,0.198241836348163,\"3\"\n-3.81419531841447,0.202054758474049,\"3\"\n-3.79357465415302,0.20587790972123,\"3\"\n-3.77295398989156,0.209709319674642,\"3\"\n-3.7523333256301,0.213546761027922,\"3\"\n-3.73171266136864,0.217388403752538,\"3\"\n-3.71109199710719,0.221231978769718,\"3\"\n-3.69047133284573,0.225075011489252,\"3\"\n-3.66985066858427,0.228915755431003,\"3\"\n-3.64923000432281,0.232752150340125,\"3\"\n-3.62860934006136,0.236583883521613,\"3\"\n-3.6079886757999,0.240411231708249,\"3\"\n-3.58736801153844,0.244235108209857,\"3\"\n-3.56674734727698,0.248058117739317,\"3\"\n-3.54612668301552,0.25188399221572,\"3\"\n-3.52550601875407,0.255717257041966,\"3\"\n-3.50488535449261,0.259567006201543,\"3\"\n-3.48426469023115,0.263438619505108,\"3\"\n-3.46364402596969,0.267340009048522,\"3\"\n-3.44302336170824,0.271285300051632,\"3\"\n-3.42240269744678,0.275284404874268,\"3\"\n-3.40178203318532,0.279344266511885,\"3\"\n-3.38116136892386,0.283477839345886,\"3\"\n-3.3605407046624,0.28770596624122,\"3\"\n-3.33992004040095,0.292026884783042,\"3\"\n-3.31929937613949,0.296448944370962,\"3\"\n-3.29867871187803,0.300995950569402,\"3\"\n-3.27805804761657,0.30566282379309,\"3\"\n-3.25743738335512,0.310448535810175,\"3\"\n-3.23681671909366,0.315362590394564,\"3\"\n-3.2161960548322,0.320408749142481,\"3\"\n-3.19557539057074,0.32556858381721,\"3\"\n-3.17495472630928,0.33083491571203,\"3\"\n-3.15433406204783,0.336212737855849,\"3\"\n-3.13371339778637,0.34167024988084,\"3\"\n-3.11309273352491,0.347190246499566,\"3\"\n-3.09247206926345,0.352755571987311,\"3\"\n-3.071851405002,0.358338292940349,\"3\"\n-3.05123074074054,0.363910370412357,\"3\"\n-3.03061007647908,0.369443217003541,\"3\"\n-3.00998941221762,0.37489394666854,\"3\"\n-2.98936874795616,0.38023971717215,\"3\"\n-2.96874808369471,0.385449782625879,\"3\"\n-2.94812741943325,0.390466178613689,\"3\"\n-2.92750675517179,0.395266794261747,\"3\"\n-2.90688609091033,0.399830173350926,\"3\"\n-2.88626542664888,0.404109851142352,\"3\"\n-2.86564476238742,0.408053718102899,\"3\"\n-2.84502409812596,0.411671625437768,\"3\"\n-2.8244034338645,0.414941551669984,\"3\"\n-2.80378276960305,0.417779653386729,\"3\"\n-2.78316210534159,0.420224049151393,\"3\"\n-2.76254144108013,0.422269227289933,\"3\"\n-2.74192077681867,0.423868915263354,\"3\"\n-2.72130011255721,0.425018318048828,\"3\"\n-2.70067944829576,0.425755670338052,\"3\"\n-2.6800587840343,0.426078364820244,\"3\"\n-2.65943811977284,0.425935159644671,\"3\"\n-2.63881745551138,0.425406553406122,\"3\"\n-2.61819679124993,0.424507699293816,\"3\"\n-2.59757612698847,0.423215075930148,\"3\"\n-2.57695546272701,0.421577061766904,\"3\"\n-2.55633479846555,0.419637895002148,\"3\"\n-2.53571413420409,0.417410705584978,\"3\"\n-2.51509346994264,0.414906703465223,\"3\"\n-2.49447280568118,0.412190369365042,\"3\"\n-2.47385214141972,0.409290374440931,\"3\"\n-2.45323147715826,0.406215693933229,\"3\"\n-2.43261081289681,0.403019811263807,\"3\"\n-2.41199014863535,0.39973458090611,\"3\"\n-2.39136948437389,0.396384158710194,\"3\"\n-2.37074882011243,0.392999139507947,\"3\"\n-2.35012815585098,0.389608909127594,\"3\"\n-2.32950749158952,0.386235642922798,\"3\"\n-2.30888682732806,0.382908926479566,\"3\"\n-2.2882661630666,0.379641614104866,\"3\"\n-2.26764549880514,0.376448432759285,\"3\"\n-2.24702483454369,0.373353021789658,\"3\"\n-2.22640417028223,0.370362794498787,\"3\"\n-2.20578350602077,0.367478282062399,\"3\"\n-2.18516284175931,0.364708278990245,\"3\"\n-2.16454217749786,0.362066506611607,\"3\"\n-2.1439215132364,0.359535754436132,\"3\"\n-2.12330084897494,0.357112791617932,\"3\"\n-2.10268018471348,0.354806549225217,\"3\"\n-2.08205952045202,0.352596670257752,\"3\"\n-2.06143885619057,0.35046998404257,\"3\"\n-2.04081819192911,0.348419711581106,\"3\"\n-2.02019752766765,0.346432723657571,\"3\"\n-1.99957686340619,0.344487187198135,\"3\"\n-1.97895619914474,0.342568036168695,\"3\"\n-1.95833553488328,0.340658274437793,\"3\"\n-1.93771487062182,0.338737166896017,\"3\"\n-1.91709420636036,0.336787502573551,\"3\"\n-1.8964735420989,0.334784965221644,\"3\"\n-1.87585287783745,0.332709437214568,\"3\"\n-1.85523221357599,0.330547389835458,\"3\"\n-1.83461154931453,0.32827791479026,\"3\"\n-1.81399088505307,0.32586593387503,\"3\"\n-1.79337022079162,0.323312206240196,\"3\"\n-1.77274955653016,0.320601389052297,\"3\"\n-1.7521288922687,0.317693153647492,\"3\"\n-1.73150822800724,0.314587827310377,\"3\"\n-1.71088756374578,0.311281343070765,\"3\"\n-1.69026689948433,0.307748794039462,\"3\"\n-1.66964623522287,0.303965894332175,\"3\"\n-1.64902557096141,0.299951155503733,\"3\"\n-1.62840490669995,0.29569843979165,\"3\"\n-1.6077842424385,0.291160881355112,\"3\"\n-1.58716357817704,0.286374195606068,\"3\"\n-1.56654291391558,0.281340774661135,\"3\"\n-1.54592224965412,0.276038213091416,\"3\"\n-1.52530158539267,0.270472049109589,\"3\"\n-1.50468092113121,0.264669038175033,\"3\"\n-1.48406025686975,0.258631362275412,\"3\"\n-1.46343959260829,0.252337242765473,\"3\"\n-1.44281892834683,0.24583469452429,\"3\"\n-1.42219826408538,0.239135717676437,\"3\"\n-1.40157759982392,0.232232074240226,\"3\"\n-1.38095693556246,0.22515511631813,\"3\"\n-1.360336271301,0.217931297456905,\"3\"\n-1.33971560703955,0.210572314021329,\"3\"\n-1.31909494277809,0.203092001589733,\"3\"\n-1.29847427851663,0.195526264150593,\"3\"\n-1.27785361425517,0.187895226378608,\"3\"\n-1.25723294999371,0.180215651596842,\"3\"\n-1.23661228573226,0.172517844038357,\"3\"\n-1.2159916214708,0.164823733232677,\"3\"\n-1.19537095720934,0.157158301390187,\"3\"\n-1.17475029294788,0.149547430465807,\"3\"\n-1.15412962868643,0.142008294924953,\"3\"\n-1.13350896442497,0.134561579112651,\"3\"\n-1.11288830016351,0.127247442079889,\"3\"\n-1.09226763590205,0.120066509395938,\"3\"\n-1.07164697164059,0.113035309799318,\"3\"\n-1.05102630737914,0.106190796040322,\"3\"\n-1.03040564311768,0.099540754881224,\"3\"\n-1.00978497885622,0.0930866307654583,\"3\"\n-0.989164314594764,0.0868490281925709,\"3\"\n-0.968543650333306,0.0808605065540834,\"3\"\n-0.947922986071848,0.0750991783248104,\"3\"\n-0.927302321810391,0.0695713900799963,\"3\"\n-0.906681657548933,0.064319034246599,\"3\"\n-0.886060993287475,0.0593171747963841,\"3\"\n-0.865440329026018,0.0545597940510891,\"3\"\n-0.84481966476456,0.0500660333950664,\"3\"\n-0.824199000503102,0.0458423328303212,\"3\"\n-0.803578336241644,0.0418598328277457,\"3\"\n-0.782957671980187,0.0381153053259444,\"3\"\n-0.762337007718729,0.0346461696201067,\"3\"\n-0.741716343457271,0.0314033173008123,\"3\"\n-0.721095679195813,0.0283804357182451,\"3\"\n-0.700475014934356,0.0255930578027883,\"3\"\n-0.679854350672898,0.0230245538606154,\"3\"\n-0.65923368641144,0.0206508919410785,\"3\"\n-0.638613022149983,0.0184695445228139,\"3\"\n-0.617992357888524,0.0164907520071382,\"3\"\n-0.597371693627067,0.0146777585637034,\"3\"\n-0.576751029365609,0.0130214950196454,\"3\"\n-0.556130365104152,0.0115326390318328,\"3\"\n-0.535509700842693,0.0101857683104614,\"3\"\n-0.514889036581236,0.00896591193489175,\"3\"\n-0.494268372319778,0.00787171730125705,\"3\"\n-0.47364770805832,0.00689858908282524,\"3\"\n-0.453027043796863,0.00602478803350458,\"3\"\n-0.432406379535405,0.00524257830893895,\"3\"\n-0.411785715273947,0.00455811014596018,\"3\"\n-0.391165051012489,0.00394942350176171,\"3\"\n-0.370544386751032,0.00340922945108366,\"3\"\n-0.349923722489574,0.00293696439344067,\"3\"\n-0.329303058228116,0.00252467963344341,\"3\"\n-0.308682393966659,0.0021619289611479,\"3\"\n-0.288061729705201,0.00184475374149113,\"3\"\n-0.267441065443743,0.00157325890684163,\"3\"\n-0.246820401182285,0.00133642845884117,\"3\"\n-0.226199736920828,0.00113052366561719,\"3\"\n-0.205579072659369,0.000955346184446084,\"3\"\n-0.184958408397912,0.000805041753504045,\"3\"\n-0.164337744136454,0.000675488388812433,\"3\"\n-0.143717079874997,0.000565116412851831,\"3\"\n-0.123096415613538,0.00047240326893228,\"3\"\n-5.60499616229728,0.000158591571901416,\"4\"\n-5.57402872958503,0.000197267150206112,\"4\"\n-5.54306129687277,0.000244009309169921,\"4\"\n-5.51209386416052,0.00029981497250775,\"4\"\n-5.48112643144826,0.000368075078360448,\"4\"\n-5.45015899873601,0.000448508262914336,\"4\"\n-5.41919156602375,0.000544534091117848,\"4\"\n-5.38822413331149,0.000658451120583369,\"4\"\n-5.35725670059924,0.000790774241774615,\"4\"\n-5.32628926788698,0.000948265435724174,\"4\"\n-5.29532183517473,0.00112999830821423,\"4\"\n-5.26435440246247,0.0013401497021959,\"4\"\n-5.23338696975022,0.001584408757305,\"4\"\n-5.20241953703796,0.00186157430982416,\"4\"\n-5.17145210432571,0.00218068472781033,\"4\"\n-5.14048467161345,0.00254192273938169,\"4\"\n-5.1095172389012,0.00294736521839244,\"4\"\n-5.07854980618894,0.00340822950786582,\"4\"\n-5.04758237347669,0.00391910419983927,\"4\"\n-5.01661494076443,0.00448928519848745,\"4\"\n-4.98564750805217,0.00512151719258299,\"4\"\n-4.95468007533992,0.00581278992816553,\"4\"\n-4.92371264262766,0.00657823156281815,\"4\"\n-4.89274520991541,0.00740852638153025,\"4\"\n-4.86177777720315,0.00830927859798955,\"4\"\n-4.8308103444909,0.00928673869380086,\"4\"\n-4.79984291177864,0.0103321762401518,\"4\"\n-4.76887547906639,0.0114570860838928,\"4\"\n-4.73790804635413,0.0126543146863466,\"4\"\n-4.70694061364188,0.0139217684557772,\"4\"\n-4.67597318092962,0.01526901763245,\"4\"\n-4.64500574821737,0.0166824448175644,\"4\"\n-4.61403831550511,0.018168520215628,\"4\"\n-4.58307088279286,0.0197242999243387,\"4\"\n-4.5521034500806,0.021342209932494,\"4\"\n-4.52113601736835,0.0230336111773387,\"4\"\n-4.49016858465609,0.0247855462143787,\"4\"\n-4.45920115194383,0.0266016292705876,\"4\"\n-4.42823371923158,0.028485790118383,\"4\"\n-4.39726628651932,0.030430633019504,\"4\"\n-4.36629885380707,0.0324485074473793,\"4\"\n-4.33533142109481,0.0345354605193791,\"4\"\n-4.30436398838256,0.0366945589245754,\"4\"\n-4.2733965556703,0.0389423766796224,\"4\"\n-4.24242912295805,0.0412719359208989,\"4\"\n-4.21146169024579,0.0437019965255325,\"4\"\n-4.18049425753354,0.0462396268747291,\"4\"\n-4.14952682482128,0.0488850809860389,\"4\"\n-4.11855939210903,0.051674584502178,\"4\"\n-4.08759195939677,0.0545953143478792,\"4\"\n-4.05662452668451,0.0576692039284654,\"4\"\n-4.02565709397226,0.0609168840842694,\"4\"\n-3.99468966126,0.064328577440536,\"4\"\n-3.96372222854775,0.0679442218739666,\"4\"\n-3.93275479583549,0.0717519686833274,\"4\"\n-3.90178736312324,0.0757578421821854,\"4\"\n-3.87081993041098,0.0799941162065868,\"4\"\n-3.83985249769873,0.0844290011842185,\"4\"\n-3.80888506498647,0.0890878604049659,\"4\"\n-3.77791763227422,0.0939595490836406,\"4\"\n-3.74695019956196,0.0990206542845521,\"4\"\n-3.71598276684971,0.104299744237889,\"4\"\n-3.68501533413745,0.109744547995456,\"4\"\n-3.6540479014252,0.115351623125605,\"4\"\n-3.62308046871294,0.121104782084739,\"4\"\n-3.59211303600069,0.126964381091676,\"4\"\n-3.56114560328843,0.132918135591366,\"4\"\n-3.53017817057617,0.138925652571511,\"4\"\n-3.49921073786392,0.144956809464054,\"4\"\n-3.46824330515166,0.150977215956826,\"4\"\n-3.43727587243941,0.1569560563453,\"4\"\n-3.40630843972715,0.162851901871557,\"4\"\n-3.3753410070149,0.168633886427636,\"4\"\n-3.34437357430264,0.174281391950436,\"4\"\n-3.31340614159039,0.179730287535468,\"4\"\n-3.28243870887813,0.184988508114945,\"4\"\n-3.25147127616588,0.190014305743631,\"4\"\n-3.22050384345362,0.19477722225013,\"4\"\n-3.18953641074137,0.199290192980077,\"4\"\n-3.15856897802911,0.203493005121816,\"4\"\n-3.12760154531686,0.207412844383751,\"4\"\n-3.0966341126046,0.211041384510624,\"4\"\n-3.06566667989234,0.21434365639934,\"4\"\n-3.03469924718009,0.217368464075921,\"4\"\n-3.00373181446783,0.220083654987822,\"4\"\n-2.97276438175558,0.222513483811593,\"4\"\n-2.94179694904332,0.224687353969038,\"4\"\n-2.91082951633107,0.226569719361161,\"4\"\n-2.87986208361881,0.228225984043255,\"4\"\n-2.84889465090656,0.229651026389899,\"4\"\n-2.8179272181943,0.230857643719785,\"4\"\n-2.78695978548205,0.23188596690211,\"4\"\n-2.75599235276979,0.232723349696995,\"4\"\n-2.72502492005754,0.233411034794001,\"4\"\n-2.69405748734528,0.233962069672188,\"4\"\n-2.66309005463303,0.234379639979872,\"4\"\n-2.63212262192077,0.234696514919235,\"4\"\n-2.60115518920851,0.234912904759489,\"4\"\n-2.57018775649626,0.235047288080449,\"4\"\n-2.539220323784,0.235114918081174,\"4\"\n-2.50825289107175,0.235115030857083,\"4\"\n-2.47728545835949,0.235068240183763,\"4\"\n-2.44631802564724,0.234978443401368,\"4\"\n-2.41535059293498,0.234853416923191,\"4\"\n-2.38438316022273,0.234703863008396,\"4\"\n-2.35341572751047,0.234533515032265,\"4\"\n-2.32244829479822,0.234352255711766,\"4\"\n-2.29148086208596,0.234166674877649,\"4\"\n-2.26051342937371,0.233984389390523,\"4\"\n-2.22954599666145,0.233811953612518,\"4\"\n-2.1985785639492,0.23365893585217,\"4\"\n-2.16761113123694,0.2335318587246,\"4\"\n-2.13664369852469,0.233436450387893,\"4\"\n-2.10567626581243,0.233387262218904,\"4\"\n-2.07470883310017,0.233383635651204,\"4\"\n-2.04374140038792,0.233437215191697,\"4\"\n-2.01277396767566,0.233554970863351,\"4\"\n-1.98180653496341,0.233735261013712,\"4\"\n-1.95083910225115,0.233994831351225,\"4\"\n-1.9198716695389,0.234325313298171,\"4\"\n-1.88890423682664,0.23473038432743,\"4\"\n-1.85793680411439,0.235215847218455,\"4\"\n-1.82696937140213,0.235768571318745,\"4\"\n-1.79600193868988,0.236393652970621,\"4\"\n-1.76503450597762,0.237079092506314,\"4\"\n-1.73406707326537,0.237814247067252,\"4\"\n-1.70309964055311,0.238595281714347,\"4\"\n-1.67213220784086,0.23940255306885,\"4\"\n-1.6411647751286,0.240224451174883,\"4\"\n-1.61019734241634,0.241044693902225,\"4\"\n-1.57922990970409,0.241848346634129,\"4\"\n-1.54826247699183,0.24261236903218,\"4\"\n-1.51729504427958,0.243325582318169,\"4\"\n-1.48632761156732,0.243969334268224,\"4\"\n-1.45536017885507,0.244520562323523,\"4\"\n-1.42439274614281,0.244977363753412,\"4\"\n-1.39342531343056,0.24531093127319,\"4\"\n-1.3624578807183,0.24552014647333,\"4\"\n-1.33149044800605,0.245601667787475,\"4\"\n-1.30052301529379,0.245528614477455,\"4\"\n-1.26955558258154,0.245323786603346,\"4\"\n-1.23858814986928,0.244973610248274,\"4\"\n-1.20762071715703,0.244484028753152,\"4\"\n-1.17665328444477,0.243875335784886,\"4\"\n-1.14568585173252,0.243136568595725,\"4\"\n-1.11471841902026,0.242301545148652,\"4\"\n-1.083750986308,0.241381274860666,\"4\"\n-1.05278355359575,0.240390302866128,\"4\"\n-1.02181612088349,0.239356797549544,\"4\"\n-0.990848688171238,0.238297570931415,\"4\"\n-0.959881255458983,0.237236909625962,\"4\"\n-0.928913822746727,0.236195028432088,\"4\"\n-0.897946390034472,0.235198396839135,\"4\"\n-0.866978957322217,0.234258219026046,\"4\"\n-0.836011524609962,0.233398368006593,\"4\"\n-0.805044091897706,0.232632143933991,\"4\"\n-0.774076659185451,0.231960886888687,\"4\"\n-0.743109226473195,0.231412144492791,\"4\"\n-0.71214179376094,0.230969056654063,\"4\"\n-0.681174361048685,0.230636733669625,\"4\"\n-0.65020692833643,0.230415542492669,\"4\"\n-0.619239495624174,0.230282169455188,\"4\"\n-0.588272062911919,0.230234735965451,\"4\"\n-0.557304630199663,0.230246363218331,\"4\"\n-0.526337197487408,0.230294973123647,\"4\"\n-0.495369764775153,0.23035668891972,\"4\"\n-0.464402332062898,0.230402203588414,\"4\"\n-0.433434899350642,0.230397618659727,\"4\"\n-0.402467466638387,0.230310660267331,\"4\"\n-0.371500033926131,0.230118437544837,\"4\"\n-0.340532601213877,0.229759562156955,\"4\"\n-0.309565168501621,0.229229675439308,\"4\"\n-0.278597735789366,0.228485436080947,\"4\"\n-0.24763030307711,0.227483197074008,\"4\"\n-0.216662870364855,0.226228192243304,\"4\"\n-0.185695437652599,0.22465050684466,\"4\"\n-0.154728004940345,0.2227616314481,\"4\"\n-0.123760572228089,0.220548225345728,\"4\"\n-0.0927931395158339,0.217951346776023,\"4\"\n-0.0618257068035781,0.215020487738646,\"4\"\n-0.0308582740913232,0.211708875553441,\"4\"\n0.000109158620932526,0.208028604257401,\"4\"\n0.0310765913331874,0.204014492309471,\"4\"\n0.0620440240454432,0.199605811210567,\"4\"\n0.0930114567576981,0.194884293148468,\"4\"\n0.123978889469954,0.18984170873047,\"4\"\n0.154946322182209,0.184486597337637,\"4\"\n0.185913754894464,0.178878339971541,\"4\"\n0.216881187606719,0.173004013913197,\"4\"\n0.247848620318975,0.166922527040814,\"4\"\n0.27881605303123,0.160664449702294,\"4\"\n0.309783485743486,0.15424507507748,\"4\"\n0.340750918455741,0.147721180727226,\"4\"\n0.371718351167996,0.141115732614349,\"4\"\n0.402685783880251,0.134467745504092,\"4\"\n0.433653216592507,0.127811526481235,\"4\"\n0.464620649304762,0.121184739726166,\"4\"\n0.495588082017018,0.114613234356727,\"4\"\n0.526555514729273,0.108131441774706,\"4\"\n0.557522947441528,0.101772482033662,\"4\"\n0.588490380153783,0.0955462254819922,\"4\"\n0.619457812866039,0.0895019695372447,\"4\"\n0.650425245578294,0.0836387219801148,\"4\"\n0.68139267829055,0.077974159323398,\"4\"\n0.712360111002805,0.0725476141316072,\"4\"\n0.74332754371506,0.0673354807440404,\"4\"\n0.774294976427315,0.0623757991051552,\"4\"\n0.805262409139571,0.0576627320962127,\"4\"\n0.836229841851826,0.0531818706000573,\"4\"\n0.867197274564082,0.0489817053579163,\"4\"\n0.898164707276337,0.0450118001375051,\"4\"\n0.929132139988592,0.0412902541769603,\"4\"\n0.960099572700847,0.0378184392446715,\"4\"\n0.991067005413103,0.0345627038685429,\"4\"\n1.02203443812536,0.0315533203475837,\"4\"\n1.05300187083761,0.028751120827866,\"4\"\n1.08396930354987,0.0261501068899467,\"4\"\n1.11493673626212,0.0237607526181952,\"4\"\n1.14590416897438,0.0215430495955887,\"4\"\n1.17687160168663,0.0195097112451973,\"4\"\n1.20783903439889,0.017639898561944,\"4\"\n1.23880646711115,0.0159138630785062,\"4\"\n1.2697738998234,0.0143478244639536,\"4\"\n1.30074133253566,0.0129048194152801,\"4\"\n1.33170876524791,0.0115876225830446,\"4\"\n1.36267619796017,0.0103892915213973,\"4\"\n1.39364363067242,0.00928939908619926,\"4\"\n1.42461106338468,0.00829731135937509,\"4\"\n1.45557849609693,0.00739169096325986,\"4\"\n1.48654592880919,0.00656804254575743,\"4\"\n1.51751336152144,0.00582742299644953,\"4\"\n1.5484807942337,0.00515264824145373,\"4\"\n1.57944822694595,0.00454728371320411,\"4\"\n1.61041565965821,0.00400176780405923,\"4\"\n1.64138309237046,0.00350841110924638,\"4\"\n1.67235052508272,0.00307168510163681,\"4\"\n1.70331795779498,0.00267798237655791,\"4\"\n1.73428539050723,0.00232780136107979,\"4\"\n1.76525282321949,0.00201778479297464,\"4\"\n1.79622025593174,0.00174064061335289,\"4\"\n1.827187688644,0.00149897643822716,\"4\"\n1.85815512135625,0.00128497415069551,\"4\"\n1.88912255406851,0.00109686702569121,\"4\"\n1.92008998678076,0.000934142872289872,\"4\"\n1.95105741949302,0.000791077405869325,\"4\"\n1.98202485220527,0.000668185779829528,\"4\"\n2.01299228491753,0.000561969314756845,\"4\"\n2.04395971762978,0.000469895887043438,\"4\"\n2.07492715034204,0.000392444195388104,\"4\"\n2.10589458305429,0.000325656753289631,\"4\"\n2.13686201576655,0.000269223237330578,\"4\"\n2.16782944847881,0.000221812693371117,\"4\"\n2.19879688119106,0.00018145260664535,\"4\"\n2.22976431390332,0.000148319450723035,\"4\"\n2.26073174661557,0.000120459593148635,\"4\"\n2.29169917932783,9.7307670967592e-05,\"4\"\n-5.91561709142291,0.000133947662228185,\"5\"\n-5.88068686856858,0.000164602118508265,\"5\"\n-5.84575664571426,0.000200899735517259,\"5\"\n-5.81082642285994,0.000244412343895862,\"5\"\n-5.77589620000562,0.000296392171717156,\"5\"\n-5.74096597715129,0.00035715566533205,\"5\"\n-5.70603575429697,0.000429497482804288,\"5\"\n-5.67110553144265,0.000514133479807174,\"5\"\n-5.63617530858833,0.000611818913672987,\"5\"\n-5.601245085734,0.000727175615583548,\"5\"\n-5.56631486287968,0.000859522331658443,\"5\"\n-5.53138464002536,0.00101097996810611,\"5\"\n-5.49645441717104,0.00118684014383518,\"5\"\n-5.46152419431671,0.00138564322235445,\"5\"\n-5.42659397146239,0.00161151198176177,\"5\"\n-5.39166374860807,0.00186856505282284,\"5\"\n-5.35673352575375,0.00215556695227458,\"5\"\n-5.32180330289942,0.00247894857319704,\"5\"\n-5.2868730800451,0.00284020718099876,\"5\"\n-5.25194285719078,0.00323870623006353,\"5\"\n-5.21701263433646,0.003683643156997,\"5\"\n-5.18208241148213,0.0041723078120603,\"5\"\n-5.14715218862781,0.00470545364462723,\"5\"\n-5.11222196577349,0.00529430640106215,\"5\"\n-5.07729174291917,0.00593154486157336,\"5\"\n-5.04236152006484,0.00662156223601793,\"5\"\n-5.00743129721052,0.00737136399777242,\"5\"\n-4.9725010743562,0.00817439322877747,\"5\"\n-4.93757085150188,0.00903738384508658,\"5\"\n-4.90264062864755,0.00996204908648497,\"5\"\n-4.86771040579323,0.0109433140447637,\"5\"\n-4.83278018293891,0.0119905210767103,\"5\"\n-4.79784996008459,0.0130996449999341,\"5\"\n-4.76291973723026,0.0142677927507687,\"5\"\n-4.72798951437594,0.0155073831584803,\"5\"\n-4.69305929152162,0.0168086570106708,\"5\"\n-4.65812906866729,0.0181735401766065,\"5\"\n-4.62319884581297,0.0196125762704353,\"5\"\n-4.58826862295865,0.0211160695840356,\"5\"\n-4.55333840010433,0.0226909199491436,\"5\"\n-4.51840817725,0.0243432170945497,\"5\"\n-4.48347795439568,0.0260665781518883,\"5\"\n-4.44854773154136,0.0278733343306762,\"5\"\n-4.41361750868704,0.029764533154345,\"5\"\n-4.37868728583272,0.0317375344906705,\"5\"\n-4.34375706297839,0.0338118946572578,\"5\"\n-4.30882684012407,0.0359815852070471,\"5\"\n-4.27389661726975,0.0382491167360108,\"5\"\n-4.23896639441542,0.0406406549059963,\"5\"\n-4.2040361715611,0.0431420137151153,\"5\"\n-4.16910594870678,0.0457657636149078,\"5\"\n-4.13417572585246,0.0485313535859752,\"5\"\n-4.09924550299813,0.051426672681768,\"5\"\n-4.06431528014381,0.0544721065313416,\"5\"\n-4.02938505728949,0.0576757769299715,\"5\"\n-3.99445483443517,0.0610283637612866,\"5\"\n-3.95952461158084,0.0645585489923948,\"5\"\n-3.92459438872652,0.0682584092307463,\"5\"\n-3.8896641658722,0.0721224038237849,\"5\"\n-3.85473394301788,0.0761870149115045,\"5\"\n-3.81980372016355,0.0804248394682082,\"5\"\n-3.78487349730923,0.084840582798086,\"5\"\n-3.74994327445491,0.0894600659758533,\"5\"\n-3.71501305160059,0.0942521865736211,\"5\"\n-3.68008282874626,0.0992288194627495,\"5\"\n-3.64515260589194,0.104395529106304,\"5\"\n-3.61022238303762,0.109726285885798,\"5\"\n-3.5752921601833,0.115235571082148,\"5\"\n-3.54036193732897,0.120909414446074,\"5\"\n-3.50543171447465,0.126727368412605,\"5\"\n-3.47050149162033,0.132702984278144,\"5\"\n-3.43557126876601,0.13880677584424,\"5\"\n-3.40064104591168,0.145023988508397,\"5\"\n-3.36571082305736,0.151359447780104,\"5\"\n-3.33078060020304,0.157778272999974,\"5\"\n-3.29585037734872,0.164269257516133,\"5\"\n-3.26092015449439,0.170818830883928,\"5\"\n-3.22598993164007,0.177400570346781,\"5\"\n-3.19105970878575,0.183996563266054,\"5\"\n-3.15612948593143,0.190583583537144,\"5\"\n-3.1211992630771,0.197141110054783,\"5\"\n-3.08626904022278,0.203638285624532,\"5\"\n-3.05133881736846,0.210054289058024,\"5\"\n-3.01640859451414,0.216370739624941,\"5\"\n-2.98147837165981,0.222537320963681,\"5\"\n-2.94654814880549,0.228550419364653,\"5\"\n-2.91161792595117,0.234382514908888,\"5\"\n-2.87668770309685,0.239977990439559,\"5\"\n-2.84175748024252,0.245346639016669,\"5\"\n-2.8068272573882,0.250445011010352,\"5\"\n-2.77189703453388,0.255233614192987,\"5\"\n-2.73696681167956,0.259725493247382,\"5\"\n-2.70203658882523,0.263859468477631,\"5\"\n-2.66710636597091,0.267627928396714,\"5\"\n-2.63217614311659,0.27104175700816,\"5\"\n-2.59724592026227,0.274020883082397,\"5\"\n-2.56231569740794,0.276601829959933,\"5\"\n-2.52738547455362,0.278784196684882,\"5\"\n-2.4924552516993,0.280483224847153,\"5\"\n-2.45752502884498,0.281773171314113,\"5\"\n-2.42259480599065,0.28262779045763,\"5\"\n-2.38766458313633,0.283007480861644,\"5\"\n-2.35273436028201,0.282977372242972,\"5\"\n-2.31780413742769,0.282500479201835,\"5\"\n-2.28287391457336,0.281584405242869,\"5\"\n-2.24794369171904,0.280280797482857,\"5\"\n-2.21301346886472,0.278545615425354,\"5\"\n-2.1780832460104,0.276428441380882,\"5\"\n-2.14315302315607,0.27396575349879,\"5\"\n-2.10822280030175,0.271111621905256,\"5\"\n-2.07329257744743,0.267946599868394,\"5\"\n-2.03836235459311,0.264483900702528,\"5\"\n-2.00343213173878,0.260709073669559,\"5\"\n-1.96850190888446,0.256691711862137,\"5\"\n-1.93357168603014,0.252436400581433,\"5\"\n-1.89864146317582,0.247959885140318,\"5\"\n-1.86371124032149,0.243311433243772,\"5\"\n-1.82878101746717,0.238496553740463,\"5\"\n-1.79385079461285,0.233548915630409,\"5\"\n-1.75892057175852,0.228502199438459,\"5\"\n-1.7239903489042,0.223368538871428,\"5\"\n-1.68906012604988,0.218183953314866,\"5\"\n-1.65412990319556,0.212972240061654,\"5\"\n-1.61919968034123,0.207757590384718,\"5\"\n-1.58426945748691,0.202564353960597,\"5\"\n-1.54933923463259,0.197416119057367,\"5\"\n-1.51440901177827,0.192342290594228,\"5\"\n-1.47947878892395,0.187354189584235,\"5\"\n-1.44454856606962,0.182482296726438,\"5\"\n-1.4096183432153,0.177747666590248,\"5\"\n-1.37468812036098,0.173154577717587,\"5\"\n-1.33975789750666,0.168743327994387,\"5\"\n-1.30482767465233,0.164515126429,\"5\"\n-1.26989745179801,0.160472423738965,\"5\"\n-1.23496722894369,0.156666187149955,\"5\"\n-1.20003700608936,0.153069772230633,\"5\"\n-1.16510678323504,0.149695682376572,\"5\"\n-1.13017656038072,0.14658088149169,\"5\"\n-1.0952463375264,0.143691061381054,\"5\"\n-1.06031611467207,0.141046778454362,\"5\"\n-1.02538589181775,0.138658683030173,\"5\"\n-0.990455668963429,0.136496363348466,\"5\"\n-0.955525446109107,0.134585261424031,\"5\"\n-0.920595223254785,0.132909716157505,\"5\"\n-0.885665000400462,0.131446466456753,\"5\"\n-0.85073477754614,0.130224272182314,\"5\"\n-0.815804554691817,0.129205930695085,\"5\"\n-0.780874331837494,0.128379182174718,\"5\"\n-0.745944108983172,0.127768066952402,\"5\"\n-0.71101388612885,0.127327917815489,\"5\"\n-0.676083663274527,0.127060091910063,\"5\"\n-0.641153440420204,0.12696959026364,\"5\"\n-0.606223217565882,0.127025279299274,\"5\"\n-0.571292994711559,0.127234825470062,\"5\"\n-0.536362771857236,0.127590771811489,\"5\"\n-0.501432549002915,0.128073891627571,\"5\"\n-0.466502326148592,0.128698083494919,\"5\"\n-0.431572103294269,0.129447485867021,\"5\"\n-0.396641880439947,0.130312354082155,\"5\"\n-0.361711657585624,0.131312487825702,\"5\"\n-0.326781434731302,0.132424382469306,\"5\"\n-0.291851211876979,0.133649053253107,\"5\"\n-0.256920989022657,0.134999059493359,\"5\"\n-0.221990766168334,0.136453641880357,\"5\"\n-0.187060543314011,0.138018548794583,\"5\"\n-0.152130320459689,0.139694090260426,\"5\"\n-0.117200097605367,0.141463196271245,\"5\"\n-0.0822698747510442,0.143331130847467,\"5\"\n-0.0473396518967215,0.145285205484484,\"5\"\n-0.0124094290423988,0.147310113081507,\"5\"\n0.0225207938119238,0.149404740268899,\"5\"\n0.0574510166662465,0.151546597985921,\"5\"\n0.0923812395205683,0.153720375568546,\"5\"\n0.127311462374891,0.155910486412366,\"5\"\n0.162241685229214,0.158094450781129,\"5\"\n0.197171908083536,0.160250887216763,\"5\"\n0.232102130937859,0.16235112987536,\"5\"\n0.267032353792182,0.164378409579996,\"5\"\n0.301962576646503,0.166297838997016,\"5\"\n0.336892799500826,0.16808115848904,\"5\"\n0.371823022355149,0.16971557378492,\"5\"\n0.406753245209472,0.171147903882913,\"5\"\n0.441683468063794,0.172367674950678,\"5\"\n0.476613690918117,0.173362832388145,\"5\"\n0.511543913772439,0.174059237785092,\"5\"\n0.546474136626761,0.174481491739891,\"5\"\n0.581404359481084,0.17460286850273,\"5\"\n0.616334582335407,0.174363401116036,\"5\"\n0.651264805189729,0.173801022771522,\"5\"\n0.686195028044052,0.172876558162896,\"5\"\n0.721125250898374,0.171566539839258,\"5\"\n0.756055473752697,0.169908882131969,\"5\"\n0.790985696607019,0.167856673818322,\"5\"\n0.825915919461342,0.165430331911184,\"5\"\n0.860846142315665,0.162662621461473,\"5\"\n0.895776365169986,0.159504384612581,\"5\"\n0.930706588024309,0.156017242568882,\"5\"\n0.965636810878632,0.152222005651658,\"5\"\n1.00056703373295,0.148087470012003,\"5\"\n1.03549725658728,0.143691534680804,\"5\"\n1.0704274794416,0.139042662164712,\"5\"\n1.10535770229592,0.134150906143699,\"5\"\n1.14028792515024,0.129075918667055,\"5\"\n1.17521814800457,0.123829971616209,\"5\"\n1.21014837085889,0.118447811347506,\"5\"\n1.24507859371321,0.112972078502368,\"5\"\n1.28000881656753,0.10742437456364,\"5\"\n1.31493903942186,0.101843707666156,\"5\"\n1.34986926227618,0.0962598476439359,\"5\"\n1.3847994851305,0.0907079522178153,\"5\"\n1.41972970798482,0.0852114992473654,\"5\"\n1.45465993083915,0.0797969981482345,\"5\"\n1.48959015369347,0.0745033958347029,\"5\"\n1.52452037654779,0.0693351592198652,\"5\"\n1.55945059940211,0.0643230964508577,\"5\"\n1.59438082225644,0.0594921915133567,\"5\"\n1.62931104511076,0.054836841196903,\"5\"\n1.66424126796508,0.0503936072001229,\"5\"\n1.69917149081941,0.0461628185480324,\"5\"\n1.73410171367373,0.0421360265116492,\"5\"\n1.76903193652805,0.038355318014712,\"5\"\n1.80396215938237,0.0347927132598511,\"5\"\n1.8388923822367,0.031445401961929,\"5\"\n1.87382260509102,0.0283494763522973,\"5\"\n1.90875282794534,0.0254609257850265,\"5\"\n1.94368305079966,0.0227879296493657,\"5\"\n1.97861327365398,0.0203407914266722,\"5\"\n2.01354349650831,0.0180833959406527,\"5\"\n2.04847371936263,0.0160275952573509,\"5\"\n2.08340394221695,0.0141621137411988,\"5\"\n2.11833416507127,0.0124604423567929,\"5\"\n2.1532643879256,0.0109366277914256,\"5\"\n2.18819461077992,0.00956433998020762,\"5\"\n2.22312483363424,0.00832636916365686,\"5\"\n2.25805505648857,0.00723734255342811,\"5\"\n2.29298527934289,0.00626281748969401,\"5\"\n2.32791550219721,0.00539710059860742,\"5\"\n2.36284572505153,0.00464275262675417,\"5\"\n2.39777594790586,0.00397450837190299,\"5\"\n2.43270617076018,0.00339093759544906,\"5\"\n2.4676363936145,0.00288591081007254,\"5\"\n2.50256661646882,0.00244341046885571,\"5\"\n2.53749683932315,0.00206393219830854,\"5\"\n2.57242706217747,0.00173740814852374,\"5\"\n2.60735728503179,0.00145445088108533,\"5\"\n2.64228750788611,0.0012164334825721,\"5\"\n2.67721773074044,0.0010125689881288,\"5\"\n2.71214795359476,0.000838276817038867,\"5\"\n2.74707817644908,0.000693893971517983,\"5\"\n2.7820083993034,0.000571000891870684,\"5\"\n2.81693862215773,0.000467876287667003,\"5\"\n2.85186884501205,0.000382912743741038,\"5\"\n2.88679906786637,0.000311400506475236,\"5\"\n2.92172929072069,0.000252571418622842,\"5\"\n2.95665951357502,0.000204313238927472,\"5\"\n2.99158973642934,0.00016415367510403,\"5\"\n-3.82321178475254,0.00109885623775574,\"6\"\n-3.81083722931473,0.00134228292910102,\"6\"\n-3.79846267387692,0.00162944715992803,\"6\"\n-3.78608811843911,0.00197569807857972,\"6\"\n-3.7737135630013,0.00238402727354838,\"6\"\n-3.76133900756348,0.00285992489967634,\"6\"\n-3.74896445212567,0.00342838022784902,\"6\"\n-3.73658989668786,0.00408736490542501,\"6\"\n-3.72421534125005,0.00484782112871532,\"6\"\n-3.71184078581224,0.00574324962782409,\"6\"\n-3.69946623037443,0.00676625310281328,\"6\"\n-3.68709167493662,0.0079371861060772,\"6\"\n-3.6747171194988,0.00929053421547763,\"6\"\n-3.66234256406099,0.0108177209002876,\"6\"\n-3.64996800862318,0.0125498222052998,\"6\"\n-3.63759345318537,0.0145162127417523,\"6\"\n-3.62521889774756,0.0167077289506284,\"6\"\n-3.61284434230975,0.0191681545992134,\"6\"\n-3.60046978687194,0.0219134649282464,\"6\"\n-3.58809523143412,0.0249347200661956,\"6\"\n-3.57572067599631,0.028288769304288,\"6\"\n-3.5633461205585,0.0319689085290267,\"6\"\n-3.55097156512069,0.0359671877445769,\"6\"\n-3.53859700968288,0.0403510648919935,\"6\"\n-3.52622245424507,0.0450831384549304,\"6\"\n-3.51384789880726,0.0501567099916651,\"6\"\n-3.50147334336944,0.0556434711535007,\"6\"\n-3.48909878793163,0.0614716857258657,\"6\"\n-3.47672423249382,0.0676446617740396,\"6\"\n-3.46434967705601,0.0741985665171368,\"6\"\n-3.4519751216182,0.0810602845203344,\"6\"\n-3.43960056618039,0.0882279255689368,\"6\"\n-3.42722601074257,0.0956958868885426,\"6\"\n-3.41485145530476,0.103395250582435,\"6\"\n-3.40247689986695,0.111309308899458,\"6\"\n-3.39010234442914,0.119396146713422,\"6\"\n-3.37772778899133,0.127593333650631,\"6\"\n-3.36535323355352,0.13585869631177,\"6\"\n-3.35297867811571,0.144130583490066,\"6\"\n-3.34060412267789,0.152353395385857,\"6\"\n-3.32822956724008,0.160449063156544,\"6\"\n-3.31585501180227,0.168362678179846,\"6\"\n-3.30348045636446,0.176043644948295,\"6\"\n-3.29110590092665,0.183370353221239,\"6\"\n-3.27873134548884,0.190326776155155,\"6\"\n-3.26635679005103,0.196857653665313,\"6\"\n-3.25398223461321,0.20281400867902,\"6\"\n-3.2416076791754,0.208231381295613,\"6\"\n-3.22923312373759,0.213036060283949,\"6\"\n-3.21685856829978,0.217104337302421,\"6\"\n-3.20448401286197,0.220498129842904,\"6\"\n-3.19210945742416,0.223132276579602,\"6\"\n-3.17973490198635,0.224938827821239,\"6\"\n-3.16736034654853,0.225995443760686,\"6\"\n-3.15498579111072,0.226211626831233,\"6\"\n-3.14261123567291,0.225593130803295,\"6\"\n-3.1302366802351,0.224222684392792,\"6\"\n-3.11786212479729,0.222015081625535,\"6\"\n-3.10548756935948,0.219050448315929,\"6\"\n-3.09311301392166,0.215408174003678,\"6\"\n-3.08073845848385,0.211022118722696,\"6\"\n-3.06836390304604,0.206029377704134,\"6\"\n-3.05598934760823,0.200501536100462,\"6\"\n-3.04361479217042,0.194406272714162,\"6\"\n-3.03124023673261,0.187905525338332,\"6\"\n-3.0188656812948,0.181055872625032,\"6\"\n-3.00649112585698,0.173894155804735,\"6\"\n-2.99411657041917,0.16654478798607,\"6\"\n-2.98174201498136,0.15907312138652,\"6\"\n-2.96936745954355,0.151558618018336,\"6\"\n-2.95699290410574,0.14408382215321,\"6\"\n-2.94461834866793,0.136731792051113,\"6\"\n-2.93224379323012,0.129588116815988,\"6\"\n-2.9198692377923,0.122701081982302,\"6\"\n-2.90749468235449,0.116177197701323,\"6\"\n-2.89512012691668,0.110074458432109,\"6\"\n-2.88274557147887,0.104418208056077,\"6\"\n-2.87037101604106,0.0993421925962389,\"6\"\n-2.85799646060325,0.0948506462533182,\"6\"\n-2.84562190516543,0.0909584369894347,\"6\"\n-2.83324734972762,0.0878281207147874,\"6\"\n-2.82087279428981,0.0853936440578022,\"6\"\n-2.808498238852,0.0836786960367838,\"6\"\n-2.79612368341419,0.0828505105803705,\"6\"\n-2.78374912797638,0.0827891166666992,\"6\"\n-2.77137457253857,0.0835462417133542,\"6\"\n-2.75900001710075,0.0852368713653443,\"6\"\n-2.74662546166294,0.0877419080427458,\"6\"\n-2.73425090622513,0.0911300876021765,\"6\"\n-2.72187635078732,0.0954555609830379,\"6\"\n-2.70950179534951,0.100610253118172,\"6\"\n-2.6971272399117,0.106679522297103,\"6\"\n-2.68475268447389,0.113654300915773,\"6\"\n-2.67237812903607,0.121443694268756,\"6\"\n-2.66000357359826,0.130146432930416,\"6\"\n-2.64762901816045,0.13969293531532,\"6\"\n-2.63525446272264,0.15001213183166,\"6\"\n-2.62287990728483,0.161210136343781,\"6\"\n-2.61050535184702,0.173164241565037,\"6\"\n-2.59813079640921,0.185822913635135,\"6\"\n-2.58575624097139,0.199291227045014,\"6\"\n-2.57338168553358,0.213405509083429,\"6\"\n-2.56100713009577,0.228145571670052,\"6\"\n-2.54863257465796,0.24356162708186,\"6\"\n-2.53625801922015,0.259509587853001,\"6\"\n-2.52388346378234,0.275978056234815,\"6\"\n-2.51150890834453,0.292961230273099,\"6\"\n-2.49913435290671,0.310345889577506,\"6\"\n-2.4867597974689,0.32811732545652,\"6\"\n-2.47438524203109,0.346230411542423,\"6\"\n-2.46201068659328,0.364599400259425,\"6\"\n-2.44963613115547,0.383198194311807,\"6\"\n-2.43726157571766,0.401962917608343,\"6\"\n-2.42488702027984,0.420830116801238,\"6\"\n-2.41251246484203,0.43975482595598,\"6\"\n-2.40013790940422,0.458676777697982,\"6\"\n-2.38776335396641,0.47754818418611,\"6\"\n-2.3753887985286,0.496298947157187,\"6\"\n-2.36301424309079,0.514894005299908,\"6\"\n-2.35063968765298,0.533291619078262,\"6\"\n-2.33826513221516,0.551397922930718,\"6\"\n-2.32589057677735,0.569215448135059,\"6\"\n-2.31351602133954,0.586692075663765,\"6\"\n-2.30114146590173,0.603738534084454,\"6\"\n-2.28876691046392,0.620377254507319,\"6\"\n-2.27639235502611,0.636542148174036,\"6\"\n-2.2640177995883,0.652165982116496,\"6\"\n-2.25164324415048,0.667280757412212,\"6\"\n-2.23926868871267,0.681801295476263,\"6\"\n-2.22689413327486,0.695695806269853,\"6\"\n-2.21451957783705,0.708996648401956,\"6\"\n-2.20214502239924,0.721595499153812,\"6\"\n-2.18977046696143,0.73350605420661,\"6\"\n-2.17739591152361,0.744753550222757,\"6\"\n-2.1650213560858,0.755203913078584,\"6\"\n-2.15264680064799,0.764923925672328,\"6\"\n-2.14027224521018,0.773926209968269,\"6\"\n-2.12789768977237,0.782050333967783,\"6\"\n-2.11552313433456,0.789421671715358,\"6\"\n-2.10314857889675,0.796018993255498,\"6\"\n-2.09077402345893,0.801710607070742,\"6\"\n-2.07839946802112,0.806630087852278,\"6\"\n-2.06602491258331,0.8107312863136,\"6\"\n-2.0536503571455,0.813937388230362,\"6\"\n-2.04127580170769,0.816367465234246,\"6\"\n-2.02890124626988,0.817958891904905,\"6\"\n-2.01652669083207,0.81869348883784,\"6\"\n-2.00415213539425,0.818672742631484,\"6\"\n-1.99177757995644,0.81782179013246,\"6\"\n-1.97940302451863,0.816179733709464,\"6\"\n-1.96702846908082,0.813828700677439,\"6\"\n-1.95465391364301,0.810687512503024,\"6\"\n-1.9422793582052,0.806845624989117,\"6\"\n-1.92990480276738,0.802365751005515,\"6\"\n-1.91753024732957,0.797167726977527,\"6\"\n-1.90515569189176,0.791379386729135,\"6\"\n-1.89278113645395,0.785042833652835,\"6\"\n-1.88040658101614,0.778097044527718,\"6\"\n-1.86803202557833,0.770682724151156,\"6\"\n-1.85565747014052,0.762820207375551,\"6\"\n-1.8432829147027,0.75450249666156,\"6\"\n-1.83090835926489,0.745838967984908,\"6\"\n-1.81853380382708,0.736850791217394,\"6\"\n-1.80615924838927,0.727570057647888,\"6\"\n-1.79378469295146,0.718078643583336,\"6\"\n-1.78141013751365,0.708406277234728,\"6\"\n-1.76903558207584,0.698605844762619,\"6\"\n-1.75666102663802,0.688736693909459,\"6\"\n-1.74428647120021,0.678843881362111,\"6\"\n-1.7319119157624,0.668980082751045,\"6\"\n-1.71953736032459,0.659187419704171,\"6\"\n-1.70716280488678,0.64953015522069,\"6\"\n-1.69478824944897,0.640038940986595,\"6\"\n-1.68241369401116,0.630743559536146,\"6\"\n-1.67003913857334,0.621726416337757,\"6\"\n-1.65766458313553,0.612977305939184,\"6\"\n-1.64529002769772,0.604523958276577,\"6\"\n-1.63291547225991,0.596439790488583,\"6\"\n-1.6205409168221,0.588682780872429,\"6\"\n-1.60816636138429,0.581278616500132,\"6\"\n-1.59579180594647,0.574263476073666,\"6\"\n-1.58341725050866,0.567580441729302,\"6\"\n-1.57104269507085,0.561244352301494,\"6\"\n-1.55866813963304,0.555245141568025,\"6\"\n-1.54629358419523,0.549519880418624,\"6\"\n-1.53391902875742,0.544063303133526,\"6\"\n-1.52154447331961,0.538825501733407,\"6\"\n-1.50916991788179,0.533744349871669,\"6\"\n-1.49679536244398,0.528784404723181,\"6\"\n-1.48442080700617,0.523877425120436,\"6\"\n-1.47204625156836,0.518965798633609,\"6\"\n-1.45967169613055,0.513975861280616,\"6\"\n-1.44729714069274,0.508852971212775,\"6\"\n-1.43492258525493,0.503543482215164,\"6\"\n-1.42254802981711,0.497935535370389,\"6\"\n-1.4101734743793,0.492019253647261,\"6\"\n-1.39779891894149,0.485731641684545,\"6\"\n-1.38542436350368,0.478964489746411,\"6\"\n-1.37304980806587,0.471742174970085,\"6\"\n-1.36067525262806,0.463995560858861,\"6\"\n-1.34830069719025,0.455653582712216,\"6\"\n-1.33592614175243,0.446765470602031,\"6\"\n-1.32355158631462,0.437261438482166,\"6\"\n-1.31117703087681,0.4271295628404,\"6\"\n-1.298802475439,0.416432951197835,\"6\"\n-1.28642792000119,0.405110833098906,\"6\"\n-1.27405336456338,0.393215092147135,\"6\"\n-1.26167880912556,0.380813041274268,\"6\"\n-1.24930425368775,0.367865474948671,\"6\"\n-1.23692969824994,0.35447392501713,\"6\"\n-1.22455514281213,0.340703632543011,\"6\"\n-1.21218058737432,0.326550031689058,\"6\"\n-1.19980603193651,0.312134470548073,\"6\"\n-1.1874314764987,0.297515620479735,\"6\"\n-1.17505692106088,0.28274177727497,\"6\"\n-1.16268236562307,0.267908087988082,\"6\"\n-1.15030781018526,0.253079554906934,\"6\"\n-1.13793325474745,0.238337519592408,\"6\"\n-1.12555869930964,0.223738301540287,\"6\"\n-1.11318414387183,0.209360783409465,\"6\"\n-1.10080958843402,0.19528330097782,\"6\"\n-1.0884350329962,0.18152938759854,\"6\"\n-1.07606047755839,0.168192894537406,\"6\"\n-1.06368592212058,0.155317676751832,\"6\"\n-1.05131136668277,0.142904402048855,\"6\"\n-1.03893681124496,0.131059980566298,\"6\"\n-1.02656225580715,0.119773289391051,\"6\"\n-1.01418770036934,0.109033100051998,\"6\"\n-1.00181314493152,0.0989547635592856,\"6\"\n-0.989438589493712,0.0894655593221106,\"6\"\n-0.977064034055901,0.0805530971488358,\"6\"\n-0.964689478618089,0.0723314340359464,\"6\"\n-0.952314923180277,0.0646761096664305,\"6\"\n-0.939940367742466,0.0575949849962406,\"6\"\n-0.927565812304655,0.0511465532260803,\"6\"\n-0.915191256866843,0.0452180295599524,\"6\"\n-0.902816701429032,0.0398208275142241,\"6\"\n-0.89044214599122,0.0349644699718506,\"6\"\n-0.878067590553409,0.0305556259920161,\"6\"\n-0.865693035115597,0.0266080281232836,\"6\"\n-0.853318479677786,0.0230951762273051,\"6\"\n-0.840943924239974,0.0199458390633528,\"6\"\n-0.828569368802162,0.0171744046606393,\"6\"\n-0.816194813364351,0.0147331592858004,\"6\"\n-0.80382025792654,0.0125717763236962,\"6\"\n-0.791445702488728,0.0107037943048808,\"6\"\n-0.779071147050917,0.00907352537253826,\"6\"\n-0.766696591613105,0.00764808353752346,\"6\"\n-0.754322036175294,0.00643913098904862,\"6\"\n-0.741947480737482,0.0053927624606854,\"6\"\n-0.729572925299671,0.00449130152730552,\"6\"\n-0.717198369861859,0.00373781964895239,\"6\"\n-0.704823814424048,0.00309222623834214,\"6\"\n-0.692449258986236,0.00254563329459155,\"6\"\n-0.680074703548425,0.00209313523324068,\"6\"\n-0.667700148110614,0.0017101820503717,\"6\"\n-4.40711684483062,0.000788661390969075,\"7\"\n-4.38956695791255,0.000916571983917834,\"7\"\n-4.37201707099448,0.00106206992864565,\"7\"\n-4.35446718407641,0.00122714003709511,\"7\"\n-4.33691729715834,0.0014157589446348,\"7\"\n-4.31936741024027,0.00163087574043698,\"7\"\n-4.3018175233222,0.001873286569701,\"7\"\n-4.28426763640413,0.00214575663637194,\"7\"\n-4.26671774948607,0.00245156637344766,\"7\"\n-4.249167862568,0.00279940753066608,\"7\"\n-4.23161797564993,0.00318774989155322,\"7\"\n-4.21406808873186,0.0036202317410609,\"7\"\n-4.19651820181379,0.00410069560965839,\"7\"\n-4.17896831489572,0.00463870749210444,\"7\"\n-4.16141842797765,0.00523723326275766,\"7\"\n-4.14386854105958,0.00589772709731828,\"7\"\n-4.12631865414151,0.00662488066736133,\"7\"\n-4.10876876722344,0.0074259631402627,\"7\"\n-4.09121888030537,0.00831403724898372,\"7\"\n-4.0736689933873,0.00928528787656174,\"7\"\n-4.05611910646923,0.0103450769234266,\"7\"\n-4.03856921955116,0.0114988778171149,\"7\"\n-4.02101933263309,0.0127649163846473,\"7\"\n-4.00346944571502,0.0141412579045136,\"7\"\n-3.98591955879695,0.0156300321655189,\"7\"\n-3.96836967187888,0.0172369410285612,\"7\"\n-3.95081978496081,0.0189746295796528,\"7\"\n-3.93326989804274,0.0208560442951078,\"7\"\n-3.91572001112467,0.0228739554145163,\"7\"\n-3.8981701242066,0.025033776461964,\"7\"\n-3.88062023728853,0.0273407715412185,\"7\"\n-3.86307035037046,0.0298224267835883,\"7\"\n-3.84552046345239,0.0324644478583474,\"7\"\n-3.82797057653432,0.0352693637544521,\"7\"\n-3.81042068961625,0.0382414052196124,\"7\"\n-3.79287080269818,0.0413977564023435,\"7\"\n-3.77532091578011,0.0447432391673469,\"7\"\n-3.75777102886204,0.0482671921760035,\"7\"\n-3.74022114194397,0.0519722055501377,\"7\"\n-3.7226712550259,0.0558621465720477,\"7\"\n-3.70512136810783,0.0599641441330737,\"7\"\n-3.68757148118976,0.064252296646597,\"7\"\n-3.67002159427169,0.0687270217308586,\"7\"\n-3.65247170735362,0.0733881760272389,\"7\"\n-3.63492182043555,0.0782531632348053,\"7\"\n-3.61737193351748,0.0833126519229701,\"7\"\n-3.59982204659941,0.0885538705111068,\"7\"\n-3.58227215968134,0.0939740348739596,\"7\"\n-3.56472227276327,0.0995751536688601,\"7\"\n-3.5471723858452,0.105368122798998,\"7\"\n-3.52962249892713,0.111326369384058,\"7\"\n-3.51207261200906,0.117444290865854,\"7\"\n-3.49452272509099,0.123715635522687,\"7\"\n-3.47697283817292,0.130150263695216,\"7\"\n-3.45942295125485,0.136726767437106,\"7\"\n-3.44187306433678,0.143431714231757,\"7\"\n-3.42432317741872,0.150256133124759,\"7\"\n-3.40677329050065,0.157195830914153,\"7\"\n-3.38922340358258,0.164243436824525,\"7\"\n-3.37167351666451,0.171377210613519,\"7\"\n-3.35412362974644,0.178585857689778,\"7\"\n-3.33657374282837,0.185857638848123,\"7\"\n-3.3190238559103,0.19318590124829,\"7\"\n-3.30147396899223,0.200549815526077,\"7\"\n-3.28392408207416,0.207935863282407,\"7\"\n-3.26637419515609,0.215330899906387,\"7\"\n-3.24882430823802,0.222720166088,\"7\"\n-3.23127442131995,0.230087449071993,\"7\"\n-3.21372453440188,0.237419787025967,\"7\"\n-3.19617464748381,0.244703684235777,\"7\"\n-3.17862476056574,0.251924965208289,\"7\"\n-3.16107487364767,0.259058091544699,\"7\"\n-3.1435249867296,0.266099620320222,\"7\"\n-3.12597509981153,0.273036895310335,\"7\"\n-3.10842521289346,0.279857618348587,\"7\"\n-3.09087532597539,0.286536011265547,\"7\"\n-3.07332543905732,0.29306420968782,\"7\"\n-3.05577555213925,0.299438908795071,\"7\"\n-3.03822566522118,0.3056499926752,\"7\"\n-3.02067577830311,0.311682045944021,\"7\"\n-3.00312589138504,0.317508305292088,\"7\"\n-2.98557600446697,0.323142470618828,\"7\"\n-2.9680261175489,0.328577358542768,\"7\"\n-2.95047623063083,0.333806522070127,\"7\"\n-2.93292634371276,0.338797988942165,\"7\"\n-2.91537645679469,0.34356469209762,\"7\"\n-2.89782656987662,0.348109991846183,\"7\"\n-2.88027668295855,0.352430749872896,\"7\"\n-2.86272679604048,0.356512066643394,\"7\"\n-2.84517690912241,0.360341658192734,\"7\"\n-2.82762702220434,0.363941852929202,\"7\"\n-2.81007713528627,0.367312809026899,\"7\"\n-2.7925272483682,0.370455430820245,\"7\"\n-2.77497736145013,0.373339126379737,\"7\"\n-2.75742747453206,0.375996694099546,\"7\"\n-2.73987758761399,0.378434213781333,\"7\"\n-2.72232770069592,0.380655844813328,\"7\"\n-2.70477781377785,0.382650698470747,\"7\"\n-2.68722792685978,0.384425535560933,\"7\"\n-2.66967803994171,0.386003582515285,\"7\"\n-2.65212815302364,0.387392418841374,\"7\"\n-2.63457826610558,0.388598647517899,\"7\"\n-2.6170283791875,0.389610090518912,\"7\"\n-2.59947849226944,0.390463053443839,\"7\"\n-2.58192860535137,0.3911688335583,\"7\"\n-2.5643787184333,0.39173961193723,\"7\"\n-2.54682883151523,0.392177769383949,\"7\"\n-2.52927894459716,0.392505203071389,\"7\"\n-2.51172905767909,0.39274370960087,\"7\"\n-2.49417917076102,0.39290921474428,\"7\"\n-2.47662928384295,0.393017199965442,\"7\"\n-2.45907939692488,0.393084727990575,\"7\"\n-2.44152951000681,0.393136471035332,\"7\"\n-2.42397962308874,0.393191215031279,\"7\"\n-2.40642973617067,0.393268148736481,\"7\"\n-2.3888798492526,0.393394275282814,\"7\"\n-2.37132996233453,0.393588423723364,\"7\"\n-2.35378007541646,0.393868580835827,\"7\"\n-2.33623018849839,0.394253956358175,\"7\"\n-2.31868030158032,0.394771233293568,\"7\"\n-2.30113041466225,0.395448011601081,\"7\"\n-2.28358052774418,0.396288230381721,\"7\"\n-2.26603064082611,0.397307449424645,\"7\"\n-2.24848075390804,0.398519796815303,\"7\"\n-2.23093086698997,0.399968975034747,\"7\"\n-2.2133809800719,0.401638988979798,\"7\"\n-2.19583109315383,0.403535228059262,\"7\"\n-2.17828120623576,0.405663045982231,\"7\"\n-2.16073131931769,0.40804350447852,\"7\"\n-2.14318143239962,0.410676158752999,\"7\"\n-2.12563154548155,0.413539016968883,\"7\"\n-2.10808165856348,0.416625391632537,\"7\"\n-2.09053177164541,0.419927805141844,\"7\"\n-2.07298188472734,0.423457713134292,\"7\"\n-2.05543199780927,0.427167766803972,\"7\"\n-2.0378821108912,0.431038103356611,\"7\"\n-2.02033222397313,0.435045976735041,\"7\"\n-2.00278233705506,0.439174163156947,\"7\"\n-1.98523245013699,0.443384116285196,\"7\"\n-1.96768256321892,0.447638663912023,\"7\"\n-1.95013267630085,0.451904543254576,\"7\"\n-1.93258278938278,0.456144593532448,\"7\"\n-1.91503290246471,0.460310548222232,\"7\"\n-1.89748301554664,0.46436850105645,\"7\"\n-1.87993312862857,0.468280024943505,\"7\"\n-1.8623832417105,0.472006477898915,\"7\"\n-1.84483335479243,0.475477206522088,\"7\"\n-1.82728346787436,0.478668930276046,\"7\"\n-1.8097335809563,0.481552735331195,\"7\"\n-1.79218369403823,0.484093616087903,\"7\"\n-1.77463380712016,0.486235479295331,\"7\"\n-1.75708392020209,0.487924347478176,\"7\"\n-1.73953403328402,0.48917093352226,\"7\"\n-1.72198414636595,0.489950712901693,\"7\"\n-1.70443425944788,0.490242330457478,\"7\"\n-1.68688437252981,0.489952316386453,\"7\"\n-1.66933448561174,0.489133425796994,\"7\"\n-1.65178459869367,0.48778161829163,\"7\"\n-1.6342347117756,0.485891238715919,\"7\"\n-1.61668482485753,0.483418211708773,\"7\"\n-1.59913493793946,0.480366259337551,\"7\"\n-1.58158505102139,0.476783953708263,\"7\"\n-1.56403516410332,0.472681656362076,\"7\"\n-1.54648527718525,0.468067309113863,\"7\"\n-1.52893539026718,0.462896340044347,\"7\"\n-1.51138550334911,0.457261639720987,\"7\"\n-1.49383561643104,0.45118635935939,\"7\"\n-1.47628572951297,0.44469583858961,\"7\"\n-1.4587358425949,0.437781207656114,\"7\"\n-1.44118595567683,0.430494330435785,\"7\"\n-1.42363606875876,0.422886429069062,\"7\"\n-1.40608618184069,0.41498845299113,\"7\"\n-1.38853629492262,0.40682405330468,\"7\"\n-1.37098640800455,0.398410159121888,\"7\"\n-1.35343652108648,0.389808161691873,\"7\"\n-1.33588663416841,0.381048280550366,\"7\"\n-1.31833674725034,0.372159792516939,\"7\"\n-1.30078686033227,0.363161799228197,\"7\"\n-1.2832369734142,0.354093714589637,\"7\"\n-1.26568708649613,0.344983207986562,\"7\"\n-1.24813719957806,0.335853097475956,\"7\"\n-1.23058731265999,0.326725613192851,\"7\"\n-1.21303742574192,0.317623896862288,\"7\"\n-1.19548753882385,0.308563840102552,\"7\"\n-1.17793765190578,0.299560003717381,\"7\"\n-1.16038776498771,0.290625080896091,\"7\"\n-1.14283787806964,0.281782935746592,\"7\"\n-1.12528799115157,0.273032654598132,\"7\"\n-1.1077381042335,0.264380501639216,\"7\"\n-1.09018821731543,0.255832148995383,\"7\"\n-1.07263833039736,0.247400841585266,\"7\"\n-1.05508844347929,0.239090094532089,\"7\"\n-1.03753855656122,0.23089365598872,\"7\"\n-1.01998866964315,0.222812913555212,\"7\"\n-1.00243878272508,0.214850159273615,\"7\"\n-0.984888895807015,0.207021156781742,\"7\"\n-0.967339008888945,0.199309340245603,\"7\"\n-0.949789121970875,0.191714784904337,\"7\"\n-0.932239235052806,0.184237615688446,\"7\"\n-0.914689348134736,0.176890093252195,\"7\"\n-0.897139461216666,0.169667325814084,\"7\"\n-0.879589574298596,0.162563873541297,\"7\"\n-0.862039687380526,0.15558104139286,\"7\"\n-0.844489800462457,0.148724870987546,\"7\"\n-0.826939913544387,0.142008601131973,\"7\"\n-0.809390026626317,0.135420057919781,\"7\"\n-0.791840139708247,0.128962262947211,\"7\"\n-0.774290252790177,0.122638574343454,\"7\"\n-0.756740365872107,0.116470189058756,\"7\"\n-0.739190478954038,0.110448924501095,\"7\"\n-0.721640592035968,0.104574478923271,\"7\"\n-0.704090705117898,0.0988510192923371,\"7\"\n-0.686540818199829,0.0932918466176625,\"7\"\n-0.668990931281759,0.0879083473138788,\"7\"\n-0.651441044363689,0.0826890509671881,\"7\"\n-0.633891157445619,0.0776375921131831,\"7\"\n-0.61634127052755,0.0727572958655331,\"7\"\n-0.59879138360948,0.0680769478267198,\"7\"\n-0.58124149669141,0.0635754416613702,\"7\"\n-0.56369160977334,0.0592530226375643,\"7\"\n-0.54614172285527,0.0551110462436995,\"7\"\n-0.5285918359372,0.0511647657839675,\"7\"\n-0.51104194901913,0.0474136681083478,\"7\"\n-0.493492062101061,0.0438434170209266,\"7\"\n-0.475942175182991,0.0404528658361124,\"7\"\n-0.458392288264921,0.0372425758470585,\"7\"\n-0.440842401346852,0.0342325498689028,\"7\"\n-0.423292514428782,0.0313944748709132,\"7\"\n-0.405742627510712,0.0287248324601803,\"7\"\n-0.388192740592642,0.0262196362430672,\"7\"\n-0.370642853674572,0.0238902829904644,\"7\"\n-0.353092966756503,0.0217233049176576,\"7\"\n-0.335543079838433,0.0197049202345995,\"7\"\n-0.317993192920363,0.0178295941863093,\"7\"\n-0.300443306002293,0.0160961920295204,\"7\"\n-0.282893419084224,0.014508825775912,\"7\"\n-0.265343532166153,0.0130450433125678,\"7\"\n-0.247793645248084,0.0116986438464,\"7\"\n-0.230243758330014,0.010463393960099,\"7\"\n-0.212693871411944,0.0093453323500519,\"7\"\n-0.195143984493875,0.00832760635040373,\"7\"\n-0.177594097575804,0.00740091639703221,\"7\"\n-0.160044210657735,0.0065593231748543,\"7\"\n-0.142494323739665,0.00580127147598459,\"7\"\n-0.124944436821595,0.00512306755198394,\"7\"\n-0.107394549903526,0.00451173362950727,\"7\"\n-0.0898446629854561,0.0039621358412585,\"7\"\n-0.0722947760673858,0.0034693681007416,\"7\"\n-0.0547448891493163,0.00303576463734481,\"7\"\n-0.0371950022312468,0.00264922054774069,\"7\"\n-0.0196451153131765,0.00230519448090548,\"7\"\n-0.00209522839510701,0.00199984383993877,\"7\"\n0.0154546585229633,0.00173210021849269,\"7\"\n0.0330045454410328,0.00149782231033284,\"7\"\n0.0505544323591023,0.00129138610413529,\"7\"\n0.0681043192771723,0.0011099846761427,\"7\"\n-2.99860581072291,0.00114923663002185,\"8\"\n-2.98584271346226,0.00135082461451082,\"8\"\n-2.97307961620162,0.00158225561847665,\"8\"\n-2.96031651894097,0.00185335558799127,\"8\"\n-2.94755342168033,0.00216318583571333,\"8\"\n-2.93479032441968,0.00251606126787221,\"8\"\n-2.92202722715904,0.00291730962929004,\"8\"\n-2.90926412989839,0.00338085665086687,\"8\"\n-2.89650103263775,0.00390455779251049,\"8\"\n-2.8837379353771,0.00449443918989317,\"8\"\n-2.87097483811646,0.0051588066045165,\"8\"\n-2.85821174085581,0.00591535893374733,\"8\"\n-2.84544864359517,0.00676057759156962,\"8\"\n-2.83268554633452,0.00770207744149082,\"8\"\n-2.81992244907388,0.00875199039620036,\"8\"\n-2.80715935181324,0.00993080547221522,\"8\"\n-2.79439625455259,0.0112330875668211,\"8\"\n-2.78163315729195,0.0126676242412361,\"8\"\n-2.7688700600313,0.0142509640457528,\"8\"\n-2.75610696277066,0.0160041927368212,\"8\"\n-2.74334386551001,0.0179193989271575,\"8\"\n-2.73058076824937,0.0200056305908793,\"8\"\n-2.71781767098872,0.0222838584152837,\"8\"\n-2.70505457372808,0.0247723940201074,\"8\"\n-2.69229147646743,0.0274603792015842,\"8\"\n-2.67952837920679,0.030355719966203,\"8\"\n-2.66676528194614,0.033482896639902,\"8\"\n-2.6540021846855,0.0368533849820834,\"8\"\n-2.64123908742485,0.0404531273960606,\"8\"\n-2.62847599016421,0.0442871685687658,\"8\"\n-2.61571289290356,0.0483814483683091,\"8\"\n-2.60294979564292,0.0527368228913662,\"8\"\n-2.59018669838228,0.0573361158403183,\"8\"\n-2.57742360112163,0.0621798201469811,\"8\"\n-2.56466050386099,0.0672921693061348,\"8\"\n-2.55189740660034,0.0726611631603189,\"8\"\n-2.5391343093397,0.07826697639933,\"8\"\n-2.52637121207905,0.0841043378850613,\"8\"\n-2.51360811481841,0.0901918338385151,\"8\"\n-2.50084501755776,0.0965050961345878,\"8\"\n-2.48808192029712,0.103022464612916,\"8\"\n-2.47531882303647,0.109732581308652,\"8\"\n-2.46255572577583,0.116644297500127,\"8\"\n-2.44979262851518,0.123724692196598,\"8\"\n-2.43702953125454,0.130951268661051,\"8\"\n-2.42426643399389,0.138307354621672,\"8\"\n-2.41150333673325,0.145788583602168,\"8\"\n-2.3987402394726,0.153360289078449,\"8\"\n-2.38597714221196,0.161000067685642,\"8\"\n-2.37321404495131,0.168687681498383,\"8\"\n-2.36045094769067,0.176403289404951,\"8\"\n-2.34768785043003,0.184118986565244,\"8\"\n-2.33492475316938,0.191813044066417,\"8\"\n-2.32216165590874,0.19946387719751,\"8\"\n-2.30939855864809,0.207035585848393,\"8\"\n-2.29663546138745,0.21451519471652,\"8\"\n-2.2838723641268,0.221881726064602,\"8\"\n-2.27110926686616,0.229112354437274,\"8\"\n-2.25834616960551,0.236161912812766,\"8\"\n-2.24558307234487,0.243032569857712,\"8\"\n-2.23281997508422,0.249705568725829,\"8\"\n-2.22005687782358,0.256156351126979,\"8\"\n-2.20729378056293,0.262337978393389,\"8\"\n-2.19453068330229,0.268265264340555,\"8\"\n-2.18176758604164,0.273922619240339,\"8\"\n-2.169004488781,0.279282976398192,\"8\"\n-2.15624139152035,0.284303724196393,\"8\"\n-2.14347829425971,0.289008846111709,\"8\"\n-2.13071519699906,0.29338615842169,\"8\"\n-2.11795209973842,0.297405276124734,\"8\"\n-2.10518900247778,0.301032391198829,\"8\"\n-2.09242590521713,0.304297194457881,\"8\"\n-2.07966280795649,0.307190913679467,\"8\"\n-2.06689971069584,0.309679528273679,\"8\"\n-2.0541366134352,0.311741441503929,\"8\"\n-2.04137351617455,0.31340938941724,\"8\"\n-2.02861041891391,0.31467838025395,\"8\"\n-2.01584732165326,0.315511662601633,\"8\"\n-2.00308422439262,0.3159028442233,\"8\"\n-1.99032112713197,0.315886206172755,\"8\"\n-1.97755802987133,0.315461667890367,\"8\"\n-1.96479493261068,0.314592593205783,\"8\"\n-1.95203183535004,0.313290378501662,\"8\"\n-1.93926873808939,0.311590422808611,\"8\"\n-1.92650564082875,0.309499403592032,\"8\"\n-1.9137425435681,0.306986002699385,\"8\"\n-1.90097944630746,0.304080567830792,\"8\"\n-1.88821634904681,0.300819877752939,\"8\"\n-1.87545325178617,0.29721939589105,\"8\"\n-1.86269015452553,0.29326052719444,\"8\"\n-1.84992705726488,0.288990548865609,\"8\"\n-1.83716396000424,0.284448122800039,\"8\"\n-1.82440086274359,0.279658863587526,\"8\"\n-1.81163776548295,0.274625656762812,\"8\"\n-1.7988746682223,0.269405799688825,\"8\"\n-1.78611157096166,0.264040259299414,\"8\"\n-1.77334847370101,0.258564673235123,\"8\"\n-1.76058537644037,0.253012049531773,\"8\"\n-1.74782227917972,0.247437236748732,\"8\"\n-1.73505918191908,0.241883707887721,\"8\"\n-1.72229608465843,0.236395003832558,\"8\"\n-1.70953298739779,0.231040961899066,\"8\"\n-1.69676989013714,0.225856981225685,\"8\"\n-1.6840067928765,0.220888997430637,\"8\"\n-1.67124369561585,0.216185871035728,\"8\"\n-1.65848059835521,0.2118527283194,\"8\"\n-1.64571750109457,0.20789129913827,\"8\"\n-1.63295440383392,0.204348153546344,\"8\"\n-1.62019130657328,0.201278211233512,\"8\"\n-1.60742820931263,0.198801002017764,\"8\"\n-1.59466511205199,0.196886870402208,\"8\"\n-1.58190201479134,0.195576030967684,\"8\"\n-1.5691389175307,0.194927730966345,\"8\"\n-1.55637582027005,0.195055195459301,\"8\"\n-1.54361272300941,0.195900257385444,\"8\"\n-1.53084962574876,0.197491109456434,\"8\"\n-1.51808652848812,0.199887930298575,\"8\"\n-1.50532343122747,0.20317825691651,\"8\"\n-1.49256033396683,0.207281461901156,\"8\"\n-1.47979723670618,0.21220942312218,\"8\"\n-1.46703413944554,0.21801831140033,\"8\"\n-1.45427104218489,0.224755941584567,\"8\"\n-1.44150794492425,0.232327452814182,\"8\"\n-1.4287448476636,0.240726313819134,\"8\"\n-1.41598175040296,0.249998718211925,\"8\"\n-1.40321865314232,0.260147040696085,\"8\"\n-1.39045555588167,0.27107124792226,\"8\"\n-1.37769245862103,0.282746945658355,\"8\"\n-1.36492936136038,0.29520422333515,\"8\"\n-1.35216626409974,0.308403795344352,\"8\"\n-1.33940316683909,0.322248956360661,\"8\"\n-1.32664006957845,0.336700575434114,\"8\"\n-1.3138769723178,0.351767402266265,\"8\"\n-1.30111387505716,0.367380821917311,\"8\"\n-1.28835077779651,0.383454195756368,\"8\"\n-1.27558768053587,0.399938757611069,\"8\"\n-1.26282458327522,0.416818488387146,\"8\"\n-1.25006148601458,0.434013433028268,\"8\"\n-1.23729838875393,0.451451167353239,\"8\"\n-1.22453529149329,0.469079300258086,\"8\"\n-1.21177219423264,0.486855866936802,\"8\"\n-1.199009096972,0.504709135619343,\"8\"\n-1.18624599971135,0.522582100218876,\"8\"\n-1.17348290245071,0.54042437044552,\"8\"\n-1.16071980519007,0.558168873118712,\"8\"\n-1.14795670792942,0.575769147085931,\"8\"\n-1.13519361066878,0.593182006927337,\"8\"\n-1.12243051340813,0.610363135034556,\"8\"\n-1.10966741614749,0.627222312356354,\"8\"\n-1.09690431888684,0.643750034719199,\"8\"\n-1.0841412216262,0.659913111466317,\"8\"\n-1.07137812436555,0.67567513006851,\"8\"\n-1.05861502710491,0.69092458728996,\"8\"\n-1.04585192984426,0.705694502894587,\"8\"\n-1.03308883258362,0.719956489015755,\"8\"\n-1.02032573532297,0.733676809060638,\"8\"\n-1.00756263806233,0.746737929843511,\"8\"\n-0.994799540801684,0.759201676028754,\"8\"\n-0.982036443541039,0.77104373198549,\"8\"\n-0.969273346280394,0.782226257856644,\"8\"\n-0.956510249019749,0.792639638287269,\"8\"\n-0.943747151759105,0.802358543124456,\"8\"\n-0.93098405449846,0.811362619690409,\"8\"\n-0.918220957237815,0.819605625223209,\"8\"\n-0.905457859977171,0.826990605965531,\"8\"\n-0.892694762716526,0.833598083314887,\"8\"\n-0.879931665455881,0.839410131135445,\"8\"\n-0.867168568195237,0.844369083859796,\"8\"\n-0.854405470934592,0.848395041843083,\"8\"\n-0.841642373673947,0.85157035613454,\"8\"\n-0.828879276413302,0.85387959552377,\"8\"\n-0.816116179152658,0.855252871832476,\"8\"\n-0.803353081892013,0.855633158845953,\"8\"\n-0.790589984631368,0.855102995773585,\"8\"\n-0.777826887370723,0.853651418275165,\"8\"\n-0.765063790110079,0.851198519309633,\"8\"\n-0.752300692849434,0.84771740418079,\"8\"\n-0.739537595588789,0.84329047824889,\"8\"\n-0.726774498328144,0.837914660145483,\"8\"\n-0.7140114010675,0.831505593866336,\"8\"\n-0.701248303806855,0.824073300859689,\"8\"\n-0.68848520654621,0.815699978268065,\"8\"\n-0.675722109285565,0.806394207383197,\"8\"\n-0.662959012024921,0.796075820915062,\"8\"\n-0.650195914764276,0.784794934964271,\"8\"\n-0.637432817503631,0.772632983943476,\"8\"\n-0.624669720242987,0.759612996125024,\"8\"\n-0.611906622982342,0.745669625305717,\"8\"\n-0.599143525721697,0.730889593163604,\"8\"\n-0.586380428461053,0.715352309283209,\"8\"\n-0.573617331200408,0.699095840177978,\"8\"\n-0.560854233939763,0.68208053740789,\"8\"\n-0.548091136679118,0.664417963994571,\"8\"\n-0.535328039418474,0.646183756765936,\"8\"\n-0.522564942157829,0.627428780242422,\"8\"\n-0.509801844897184,0.608148108840603,\"8\"\n-0.497038747636539,0.588458887304625,\"8\"\n-0.484275650375895,0.568431220282039,\"8\"\n-0.47151255311525,0.548123847059692,\"8\"\n-0.458749455854605,0.527571797560458,\"8\"\n-0.44598635859396,0.506874308576432,\"8\"\n-0.433223261333316,0.486094664262486,\"8\"\n-0.420460164072671,0.465292787241739,\"8\"\n-0.407697066812026,0.444543840489126,\"8\"\n-0.394933969551381,0.423906368350149,\"8\"\n-0.382170872290737,0.403436311265974,\"8\"\n-0.369407775030092,0.383191186569518,\"8\"\n-0.356644677769447,0.363270831151144,\"8\"\n-0.343881580508802,0.34368653252377,\"8\"\n-0.331118483248158,0.32448369141202,\"8\"\n-0.318355385987513,0.305716582715535,\"8\"\n-0.305592288726868,0.287485072259701,\"8\"\n-0.292829191466224,0.269761099900904,\"8\"\n-0.280066094205579,0.252575279031662,\"8\"\n-0.267302996944934,0.235977517320874,\"8\"\n-0.25453989968429,0.220048220814295,\"8\"\n-0.241776802423645,0.20473114277341,\"8\"\n-0.229013705163,0.190040645294093,\"8\"\n-0.216250607902355,0.176021127592888,\"8\"\n-0.203487510641711,0.162722074271276,\"8\"\n-0.190724413381066,0.15007137571351,\"8\"\n-0.177961316120421,0.138068364793853,\"8\"\n-0.165198218859776,0.126751029891964,\"8\"\n-0.152435121599132,0.116135113447489,\"8\"\n-0.139672024338487,0.106144010087067,\"8\"\n-0.126908927077842,0.0967652680958445,\"8\"\n-0.114145829817197,0.0880300034639254,\"8\"\n-0.101382732556553,0.0799245219011695,\"8\"\n-0.0886196352959079,0.07237660024556,\"8\"\n-0.0758565380352634,0.0653662707477243,\"8\"\n-0.0630934407746184,0.0589178464164986,\"8\"\n-0.0503303435139739,0.0529968330987682,\"8\"\n-0.037567246253329,0.0475410680466758,\"8\"\n-0.0248041489926845,0.0425273486074104,\"8\"\n-0.0120410517320395,0.0379737479606325,\"8\"\n0.000722045528604998,0.0338349052706117,\"8\"\n0.0134851427892499,0.0300612728874242,\"8\"\n0.0262482400498945,0.026629940474928,\"8\"\n0.0390113373105394,0.0235536613060398,\"8\"\n0.0517744345711839,0.0207850094968718,\"8\"\n0.0645375318318289,0.0182871158928079,\"8\"\n0.0773006290924734,0.0160397399059906,\"8\"\n0.0900637263531183,0.0140514080477241,\"8\"\n0.102826823613763,0.0122789135174033,\"8\"\n0.115589920874408,0.0106965188027097,\"8\"\n0.128353018135052,0.00928784404423159,\"8\"\n0.141116115395697,0.00805828030061836,\"8\"\n0.153879212656342,0.00697226734416712,\"8\"\n0.166642309916987,0.00601289909099831,\"8\"\n0.179405407177631,0.00516787773097609,\"8\"\n0.192168504438276,0.00444042015144521,\"8\"\n0.204931601698921,0.00380360232514677,\"8\"\n0.217694698959566,0.00324695569142434,\"8\"\n0.23045779622021,0.0027620257756977,\"8\"\n0.243220893480855,0.00235008585211938,\"8\"\n0.2559839907415,0.00199271804149916,\"8\"\n0.16492089370734,0.000161283736017212,\"24\"\n0.184626880364578,0.000208115012177115,\"24\"\n0.204332867021815,0.00026705398411794,\"24\"\n0.224038853679052,0.000340482964118214,\"24\"\n0.24374484033629,0.000430868530915413,\"24\"\n0.263450826993527,0.000542795523676944,\"24\"\n0.283156813650764,0.000677702496042719,\"24\"\n0.302862800308002,0.000843648861554031,\"24\"\n0.322568786965239,0.00103975281052843,\"24\"\n0.342274773622476,0.00127922743826743,\"24\"\n0.361980760279714,0.0015589675507708,\"24\"\n0.381686746936951,0.00189373016549959,\"24\"\n0.401392733594188,0.00228264273820123,\"24\"\n0.421098720251426,0.00273937919803542,\"24\"\n0.440804706908663,0.00326701796316727,\"24\"\n0.4605106935659,0.00387608188070018,\"24\"\n0.480216680223138,0.00457577622101057,\"24\"\n0.499922666880375,0.00537098173487688,\"24\"\n0.519628653537612,0.00627968622891125,\"24\"\n0.53933464019485,0.00729829672988011,\"24\"\n0.559040626852087,0.00845678996899084,\"24\"\n0.578746613509324,0.00974114012433558,\"24\"\n0.598452600166562,0.0111936261654382,\"24\"\n0.618158586823799,0.0127954348696169,\"24\"\n0.637864573481036,0.0145878323815086,\"24\"\n0.657570560138273,0.0165621818928541,\"24\"\n0.677276546795511,0.0187521070646918,\"24\"\n0.696982533452748,0.0211636380751384,\"24\"\n0.716688520109985,0.0238189594014691,\"24\"\n0.736394506767223,0.0267442797398481,\"24\"\n0.75610049342446,0.0299450270523889,\"24\"\n0.775806480081697,0.0334742058153592,\"24\"\n0.795512466738935,0.0373131704951683,\"24\"\n0.815218453396172,0.0415492222492839,\"24\"\n0.834924440053409,0.0461368799077728,\"24\"\n0.854630426710647,0.0511855748597722,\"24\"\n0.874336413367884,0.0566441489772008,\"24\"\n0.894042400025121,0.062607817045105,\"24\"\n0.913748386682359,0.0690491659681032,\"24\"\n0.933454373339596,0.0760294048605438,\"24\"\n0.953160359996833,0.0835501982625987,\"24\"\n0.972866346654071,0.0916271620344836,\"24\"\n0.992572333311308,0.100294053122865,\"24\"\n1.01227831996855,0.109512438447452,\"24\"\n1.03198430662578,0.119347118377114,\"24\"\n1.05169029328302,0.129703174095561,\"24\"\n1.07139627994026,0.140669884376933,\"24\"\n1.09110226659749,0.152113465659526,\"24\"\n1.11080825325473,0.164099781491482,\"24\"\n1.13051423991197,0.176507816657828,\"24\"\n1.15022022656921,0.189346250669538,\"24\"\n1.16992621322644,0.202524740648751,\"24\"\n1.18963219988368,0.216000101411511,\"24\"\n1.20933818654092,0.229702093100735,\"24\"\n1.22904417319816,0.243556793803992,\"24\"\n1.24875015985539,0.257499137126792,\"24\"\n1.26845614651263,0.271450878652831,\"24\"\n1.28816213316987,0.285335095961703,\"24\"\n1.30786811982711,0.299097024738422,\"24\"\n1.32757410648434,0.312631463894038,\"24\"\n1.34728009314158,0.325921770998854,\"24\"\n1.36698607979882,0.338852771282437,\"24\"\n1.38669206645605,0.351424356266807,\"24\"\n1.40639805311329,0.36354138822216,\"24\"\n1.42610403977053,0.375198964162529,\"24\"\n1.44581002642777,0.386343531609328,\"24\"\n1.465516013085,0.396952133865524,\"24\"\n1.48522199974224,0.407025384229519,\"24\"\n1.50492798639948,0.416514927386096,\"24\"\n1.52463397305672,0.425479634176687,\"24\"\n1.54433995971395,0.433844831555899,\"24\"\n1.56404594637119,0.441722436319302,\"24\"\n1.58375193302843,0.449019730793472,\"24\"\n1.60345791968567,0.455871079797014,\"24\"\n1.6231639063429,0.46222483328691,\"24\"\n1.64286989300014,0.468178669920756,\"24\"\n1.66257587965738,0.473732926510524,\"24\"\n1.68228186631461,0.47895347494308,\"24\"\n1.70198785297185,0.483878013953383,\"24\"\n1.72169383962909,0.488548740739393,\"24\"\n1.74139982628633,0.493022593936407,\"24\"\n1.76110581294356,0.497326996467095,\"24\"\n1.7808117996008,0.501519722317123,\"24\"\n1.80051778625804,0.505620310154217,\"24\"\n1.82022377291528,0.509672129370593,\"24\"\n1.83992975957251,0.513689922918564,\"24\"\n1.85963574622975,0.517694268472736,\"24\"\n1.87934173288699,0.521690151015149,\"24\"\n1.89904771954423,0.525676971208677,\"24\"\n1.91875370620146,0.52964226150065,\"24\"\n1.9384596928587,0.533567692872544,\"24\"\n1.95816567951594,0.537423755481512,\"24\"\n1.97787166617317,0.54117191663891,\"24\"\n1.99757765283041,0.544776913173239,\"24\"\n2.01728363948765,0.548172474558831,\"24\"\n2.03698962614489,0.55133766086558,\"24\"\n2.05669561280212,0.554168313754787,\"24\"\n2.07640159945936,0.556674915645088,\"24\"\n2.0961075861166,0.5587281658946,\"24\"\n2.11581357277384,0.560357529041252,\"24\"\n2.13551955943107,0.561451211429335,\"24\"\n2.15522554608831,0.562028736487227,\"24\"\n2.17493153274555,0.562024662648308,\"24\"\n2.19463751940278,0.561439275330813,\"24\"\n2.21434350606002,0.560267893733202,\"24\"\n2.23404949271726,0.558486829364106,\"24\"\n2.2537554793745,0.556154639443319,\"24\"\n2.27346146603173,0.553224546938444,\"24\"\n2.29316745268897,0.549808694857388,\"24\"\n2.31287343934621,0.545842734465862,\"24\"\n2.33257942600345,0.54146314660141,\"24\"\n2.35228541266068,0.536627382548853,\"24\"\n2.37199139931792,0.531442838470796,\"24\"\n2.39169738597516,0.525904196752488,\"24\"\n2.4114033726324,0.520077231630791,\"24\"\n2.43110935928963,0.51398008172592,\"24\"\n2.45081534594687,0.507643771495583,\"24\"\n2.47052133260411,0.501094653289086,\"24\"\n2.49022731926134,0.494334865019984,\"24\"\n2.50993330591858,0.487392032255061,\"24\"\n2.52963929257582,0.480242903933009,\"24\"\n2.54934527923306,0.472918362993236,\"24\"\n2.56905126589029,0.465371399395547,\"24\"\n2.58875725254753,0.457637096013377,\"24\"\n2.60846323920477,0.449668173868919,\"24\"\n2.62816922586201,0.441489773225551,\"24\"\n2.64787521251924,0.433071366535098,\"24\"\n2.66758119917648,0.424428589219484,\"24\"\n2.68728718583372,0.415556206990383,\"24\"\n2.70699317249096,0.406462792307998,\"24\"\n2.72669915914819,0.39717078482324,\"24\"\n2.74640514580543,0.38768574075997,\"24\"\n2.76611113246267,0.378052248042856,\"24\"\n2.7858171191199,0.368279090751621,\"24\"\n2.80552310577714,0.358419852322757,\"24\"\n2.82522909243438,0.348494711895504,\"24\"\n2.84493507909162,0.338549623250536,\"24\"\n2.86464106574885,0.328619641543728,\"24\"\n2.88434705240609,0.318735049980939,\"24\"\n2.90405303906333,0.308933278788202,\"24\"\n2.92375902572057,0.299235743975368,\"24\"\n2.9434650123778,0.289667441169051,\"24\"\n2.96317099903504,0.280247227436102,\"24\"\n2.98287698569228,0.270978685506377,\"24\"\n3.00258297234951,0.261882384189282,\"24\"\n3.02228895900675,0.252938912902796,\"24\"\n3.04199494566399,0.244172560099669,\"24\"\n3.06170093232123,0.235547444138125,\"24\"\n3.08140691897846,0.227087177056837,\"24\"\n3.1011129056357,0.218755584077811,\"24\"\n3.12081889229294,0.210566501909852,\"24\"\n3.14052487895018,0.202498725131533,\"24\"\n3.16023086560741,0.194559426605839,\"24\"\n3.17993685226465,0.186743752497942,\"24\"\n3.19964283892189,0.179057274647103,\"24\"\n3.21934882557913,0.171513376266664,\"24\"\n3.23905481223636,0.164115811106953,\"24\"\n3.2587607988936,0.156899704060357,\"24\"\n3.27846678555084,0.149860443566473,\"24\"\n3.29817277220808,0.143059857429865,\"24\"\n3.31787875886531,0.136478712256518,\"24\"\n3.33758474552255,0.130194564509579,\"24\"\n3.35729073217979,0.124183951814525,\"24\"\n3.37699671883702,0.118514311615174,\"24\"\n3.39670270549426,0.113173057662323,\"24\"\n3.4164086921515,0.108200207353182,\"24\"\n3.43611467880874,0.103597574496683,\"24\"\n3.45582066546597,0.0993677005541033,\"24\"\n3.47552665212321,0.0955281230558316,\"24\"\n3.49523263878045,0.0920405455154245,\"24\"\n3.51493862543769,0.0889360044719417,\"24\"\n3.53464461209492,0.0861401500883254,\"24\"\n3.55435059875216,0.0836916301392831,\"24\"\n3.5740565854094,0.0814990771720459,\"24\"\n3.59376257206663,0.0795798059516288,\"24\"\n3.61346855872387,0.0778604712672079,\"24\"\n3.63317454538111,0.0763286341171883,\"24\"\n3.65288053203835,0.0749338746487192,\"24\"\n3.67258651869558,0.0736463758835365,\"24\"\n3.69229250535282,0.0724321757883004,\"24\"\n3.71199849201006,0.0712594732558445,\"24\"\n3.7317044786673,0.0701044800665395,\"24\"\n3.75141046532453,0.0689451745178326,\"24\"\n3.77111645198177,0.0677637164939,\"24\"\n3.79082243863901,0.0665536614637242,\"24\"\n3.81052842529625,0.0653019279247971,\"24\"\n3.83023441195348,0.0640156000555727,\"24\"\n3.84994039861072,0.0626917589765087,\"24\"\n3.86964638526796,0.0613422398388009,\"24\"\n3.8893523719252,0.0599751339653258,\"24\"\n3.90905835858243,0.0586037069233519,\"24\"\n3.92876434523967,0.0572421655735129,\"24\"\n3.94847033189691,0.0559048459188387,\"24\"\n3.96817631855414,0.054604658839835,\"24\"\n3.98788230521138,0.0533578801864127,\"24\"\n4.00758829186862,0.0521690438620683,\"24\"\n4.02729427852586,0.0510568984519206,\"24\"\n4.04700026518309,0.050013669565748,\"24\"\n4.06670625184033,0.049058216439775,\"24\"\n4.08641223849757,0.0481738361757567,\"24\"\n4.10611822515481,0.0473692531425955,\"24\"\n4.12582421181204,0.0466283128791732,\"24\"\n4.14553019846928,0.0459469235675466,\"24\"\n4.16523618512652,0.0453102625881891,\"24\"\n4.18494217178375,0.0447048991863422,\"24\"\n4.20464815844099,0.0441162236788556,\"24\"\n4.22435414509823,0.0435277003794216,\"24\"\n4.24406013175547,0.0429226595283849,\"24\"\n4.2637661184127,0.0422886782632972,\"24\"\n4.28347210506994,0.0416046656301736,\"24\"\n4.30317809172718,0.0408676724196303,\"24\"\n4.32288407838442,0.0400536662400685,\"24\"\n4.34259006504165,0.0391677464082333,\"24\"\n4.36229605169889,0.0381914892564041,\"24\"\n4.38200203835613,0.0371311667589499,\"24\"\n4.40170802501337,0.0359791282450087,\"24\"\n4.4214140116706,0.0347405606557108,\"24\"\n4.44111999832784,0.0334195608126694,\"24\"\n4.46082598498508,0.0320197628635465,\"24\"\n4.48053197164232,0.0305549770787546,\"24\"\n4.50023795829955,0.0290285084108072,\"24\"\n4.51994394495679,0.0274595217404292,\"24\"\n4.53964993161403,0.0258534256526383,\"24\"\n4.55935591827126,0.0242289518662385,\"24\"\n4.5790619049285,0.0225969750831054,\"24\"\n4.59876789158574,0.0209710734324247,\"24\"\n4.61847387824298,0.0193659622154866,\"24\"\n4.63817986490021,0.0177913706508776,\"24\"\n4.65788585155745,0.0162610283163576,\"24\"\n4.67759183821469,0.0147838894649297,\"24\"\n4.69729782487193,0.0133681724108477,\"24\"\n4.71700381152916,0.0120246477352712,\"24\"\n4.7367097981864,0.0107529516898814,\"24\"\n4.75641578484364,0.00956736511166496,\"24\"\n4.77612177150087,0.00845759795968024,\"24\"\n4.79582775815811,0.00744207859999705,\"24\"\n4.81553374481535,0.00650339028871507,\"24\"\n4.83523973147259,0.00565633372790838,\"24\"\n4.85494571812982,0.00488577517507673,\"24\"\n4.87465170478706,0.0041983844770841,\"24\"\n4.8943576914443,0.00358405024268089,\"24\"\n4.91406367810154,0.00304168350556284,\"24\"\n4.93376966475877,0.00256597867657244,\"24\"\n4.95347565141601,0.00214990790495905,\"24\"\n4.97318163807325,0.00179214362019213,\"24\"\n4.99288762473049,0.00148183832609406,\"24\"\n5.01259361138772,0.00122052674384955,\"24\"\n5.03229959804496,0.000995566387270444,\"24\"\n5.0520055847022,0.000810218350462016,\"24\"\n5.07171157135943,0.000652696211534413,\"24\"\n5.09141755801667,0.000524051998653433,\"24\"\n5.11112354467391,0.000416943972323534,\"24\"\n5.13082953133115,0.000330151276130147,\"24\"\n5.15053551798838,0.000259435708621555,\"24\"\n5.17024150464562,0.000202524614374211,\"24\"\n5.18994749130286,0.000157194793555412,\"24\""
        },
        {
            "name": ".0/group_by1/density2",
            "source": ".0/group_by1/density2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.rad"
                    ]
                }
            ]
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"1\"\n\"2\"\n\"3\"\n\"4\"\n\"5\"\n\"6\"\n\"7\"\n\"8\"\n\"24\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-7.20415321635949\n6.4557859180009"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-0.0427816579422976\n0.89841481678825"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "nice": true,
            "zero": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/density2"
            },
            "marks": [
                {
                    "type": "area",
                    "properties": {
                        "update": {
                            "y2": {
                                "scale": "y",
                                "value": 0
                            },
                            "fillOpacity": {
                                "value": 0.2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.rad"
                            },
                            "stroke": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/density2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/density2"
            },
            "marks": [
                {
                    "type": "line",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_"
                            },
                            "fill": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/density2"
                            }
                        }
                    }
                }
            ]
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "rad"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "log_crim"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "density"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id716726155").parseSpec(plot_id716726155_spec);
</script><!--/html_preserve-->

While there is certainly some variation in the distributions when
`rad` $< 24$, they are mostly centered around $-4 \le$ `log_crim` 
$< 1$. Again, given the small sizes of most of these subsets
coming to a concrete conclusion is difficult.

What is distinct, however, is the crime is substantially higher when
`rad` $= 24$, and that this distribution seems to line up with the
higher mean of the mixture in the overall distribution.

c) Plot the density curve of  log crime (`lob_crim`) grouped by and filled (coloured) by the Charles River (`chas`). Discuss differences due to Charles River and compare to the plot in a).

```r
Boston %>% ggvis(~log_crim) %>%
    group_by(chas) %>%
    layer_densities(fill=~chas) %>%
    scale_numeric("x", domain = domain, nice = TRUE)
```

<!--html_preserve--><div id="plot_id402356561-container" class="ggvis-output-container">
<div id="plot_id402356561" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id402356561_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id402356561" data-renderer="svg">SVG</a>
 | 
<a id="plot_id402356561_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id402356561" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id402356561_download" class="ggvis-download" data-plot-id="plot_id402356561">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id402356561_spec = {
    "data": [
        {
            "name": ".0/group_by1/density2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "pred_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"pred_\",\"resp_\",\"chas\"\n-6.79055773429344,2.01965859420685e-05,\"Not on River\"\n-6.73955597619147,2.66305916875987e-05,\"Not on River\"\n-6.68855421808949,3.48358409131578e-05,\"Not on River\"\n-6.63755245998752,4.52962864211141e-05,\"Not on River\"\n-6.58655070188555,5.85147340727237e-05,\"Not on River\"\n-6.53554894378357,7.50682422271906e-05,\"Not on River\"\n-6.4845471856816,9.57959708714265e-05,\"Not on River\"\n-6.43354542757962,0.000121319068406816,\"Not on River\"\n-6.38254366947765,0.000152990872932154,\"Not on River\"\n-6.33154191137568,0.000191376387660603,\"Not on River\"\n-6.2805401532737,0.000238589955297596,\"Not on River\"\n-6.22953839517173,0.000294970326458942,\"Not on River\"\n-6.17853663706975,0.000363725102403286,\"Not on River\"\n-6.12753487896778,0.000444706875785755,\"Not on River\"\n-6.07653312086581,0.000542651153643437,\"Not on River\"\n-6.02553136276383,0.000656553990225627,\"Not on River\"\n-5.97452960466186,0.000793231827190735,\"Not on River\"\n-5.92352784655989,0.000950794167162144,\"Not on River\"\n-5.87252608845791,0.00113740661110645,\"Not on River\"\n-5.82152433035594,0.0013513311283346,\"Not on River\"\n-5.77052257225396,0.001601598302057,\"Not on River\"\n-5.71952081415199,0.00188696760040592,\"Not on River\"\n-5.66851905605002,0.0022169956564882,\"Not on River\"\n-5.61751729794804,0.0025913525639896,\"Not on River\"\n-5.56651553984607,0.00301961918787759,\"Not on River\"\n-5.51551378174409,0.00350288600920735,\"Not on River\"\n-5.46451202364212,0.00405006522606008,\"Not on River\"\n-5.41351026554015,0.0046642627112519,\"Not on River\"\n-5.36250850743817,0.0053528432017609,\"Not on River\"\n-5.3115067493362,0.00612158814428979,\"Not on River\"\n-5.26050499123422,0.00697528932012266,\"Not on River\"\n-5.20950323313225,0.00792310482048449,\"Not on River\"\n-5.15850147503028,0.00896615678362434,\"Not on River\"\n-5.1074997169283,0.0101177018826742,\"Not on River\"\n-5.05649795882633,0.0113741237862278,\"Not on River\"\n-5.00549620072435,0.0127535153156307,\"Not on River\"\n-4.95449444262238,0.0142465737179719,\"Not on River\"\n-4.90349268452041,0.0158769981020862,\"Not on River\"\n-4.85249092641843,0.0176290197454109,\"Not on River\"\n-4.80148916831646,0.0195327826976869,\"Not on River\"\n-4.75048741021448,0.0215654102365603,\"Not on River\"\n-4.69948565211251,0.0237644328113723,\"Not on River\"\n-4.64848389401054,0.0261021204251561,\"Not on River\"\n-4.59748213590856,0.0286158060641334,\"Not on River\"\n-4.54648037780659,0.031281261336107,\"Not on River\"\n-4.49547861970462,0.0341323161458959,\"Not on River\"\n-4.44447686160264,0.0371491483397126,\"Not on River\"\n-4.39347510350067,0.0403610466447481,\"Not on River\"\n-4.34247334539869,0.043753104059086,\"Not on River\"\n-4.29147158729672,0.0473485991689445,\"Not on River\"\n-4.24046982919475,0.051137747436732,\"Not on River\"\n-4.18946807109277,0.0551358752867486,\"Not on River\"\n-4.1384663129908,0.0593381099055894,\"Not on River\"\n-4.08746455488882,0.0637496993827402,\"Not on River\"\n-4.03646279678685,0.068369904744559,\"Not on River\"\n-3.98546103868488,0.0731921445355894,\"Not on River\"\n-3.9344592805829,0.078218318460778,\"Not on River\"\n-3.88345752248093,0.0834293574928552,\"Not on River\"\n-3.83245576437895,0.088827491128732,\"Not on River\"\n-3.78145400627698,0.094382276225026,\"Not on River\"\n-3.73045224817501,0.100093167108597,\"Not on River\"\n-3.67945049007303,0.10592164271046,\"Not on River\"\n-3.62844873197106,0.111860653685147,\"Not on River\"\n-3.57744697386908,0.117869048920891,\"Not on River\"\n-3.52644521576711,0.123929249404587,\"Not on River\"\n-3.47544345766514,0.130004546116111,\"Not on River\"\n-3.42444169956316,0.136062934328853,\"Not on River\"\n-3.37343994146119,0.142078453545798,\"Not on River\"\n-3.32243818335921,0.148005732061643,\"Not on River\"\n-3.27143642525724,0.153830169199809,\"Not on River\"\n-3.22043466715527,0.159499156792441,\"Not on River\"\n-3.16943290905329,0.165006265642421,\"Not on River\"\n-3.11843115095132,0.170298828814527,\"Not on River\"\n-3.06742939284934,0.175374769653274,\"Not on River\"\n-3.01642763474737,0.180187749507121,\"Not on River\"\n-2.9654258766454,0.184736074307999,\"Not on River\"\n-2.91442411854342,0.188984715251723,\"Not on River\"\n-2.86342236044145,0.192928677560077,\"Not on River\"\n-2.81242060233948,0.196547599556093,\"Not on River\"\n-2.7614188442375,0.199830109342815,\"Not on River\"\n-2.71041708613553,0.202772376284969,\"Not on River\"\n-2.65941532803355,0.205354369967007,\"Not on River\"\n-2.60841356993158,0.207589499897444,\"Not on River\"\n-2.55741181182961,0.209447671152123,\"Not on River\"\n-2.50641005372763,0.210959462235324,\"Not on River\"\n-2.45540829562566,0.212084198633433,\"Not on River\"\n-2.40440653752368,0.212869046753902,\"Not on River\"\n-2.35340477942171,0.213263118339784,\"Not on River\"\n-2.30240302131974,0.213329172631179,\"Not on River\"\n-2.25140126321776,0.213007340280391,\"Not on River\"\n-2.20039950511579,0.212374493835254,\"Not on River\"\n-2.14939774701381,0.21136386327011,\"Not on River\"\n-2.09839598891184,0.210058549570224,\"Not on River\"\n-2.04739423080987,0.208405020483728,\"Not on River\"\n-1.99639247270789,0.206473376277059,\"Not on River\"\n-1.94539071460592,0.204229707557804,\"Not on River\"\n-1.89438895650394,0.201731604252312,\"Not on River\"\n-1.84338719840197,0.198963686412751,\"Not on River\"\n-1.7923854403,0.195972252009769,\"Not on River\"\n-1.74138368219802,0.19275824420221,\"Not on River\"\n-1.69038192409605,0.189358410004432,\"Not on River\"\n-1.63938016599407,0.185786758303062,\"Not on River\"\n-1.5883784078921,0.182072654194083,\"Not on River\"\n-1.53737664979013,0.178239009748088,\"Not on River\"\n-1.48637489168815,0.17431023750399,\"Not on River\"\n-1.43537313358618,0.170313375472783,\"Not on River\"\n-1.38437137548421,0.166270339616908,\"Not on River\"\n-1.33336961738223,0.162207323486088,\"Not on River\"\n-1.28236785928026,0.158145958460455,\"Not on River\"\n-1.23136610117828,0.15410694728656,\"Not on River\"\n-1.18036434307631,0.150113344576355,\"Not on River\"\n-1.12936258497434,0.146176588899983,\"Not on River\"\n-1.07836082687236,0.142322169600613,\"Not on River\"\n-1.02735906877039,0.13854984738453,\"Not on River\"\n-0.976357310668414,0.134887792698574,\"Not on River\"\n-0.925355552566439,0.13132334732385,\"Not on River\"\n-0.874353794464466,0.12788693518312,\"Not on River\"\n-0.823352036362492,0.124556635941845,\"Not on River\"\n-0.772350278260518,0.121357716773548,\"Not on River\"\n-0.721348520158544,0.118267849988862,\"Not on River\"\n-0.67034676205657,0.115304353217687,\"Not on River\"\n-0.619345003954596,0.112447039949188,\"Not on River\"\n-0.568343245852622,0.109705977933581,\"Not on River\"\n-0.517341487750648,0.107064831121181,\"Not on River\"\n-0.466339729648674,0.104528230977546,\"Not on River\"\n-0.4153379715467,0.102084717279196,\"Not on River\"\n-0.364336213444727,0.099735683736312,\"Not on River\"\n-0.313334455342752,0.0974751098984121,\"Not on River\"\n-0.262332697240779,0.095303009544523,\"Not on River\"\n-0.211330939138805,0.0932191481752192,\"Not on River\"\n-0.160329181036831,0.0912231219348642,\"Not on River\"\n-0.109327422934857,0.0893208117306689,\"Not on River\"\n-0.0583256648328829,0.0875111955433536,\"Not on River\"\n-0.00732390673090944,0.0858066967510132,\"Not on River\"\n0.0436778513710649,0.0842043615549077,\"Not on River\"\n0.0946796094730384,0.082723693451709,\"Not on River\"\n0.145681367575013,0.0813576903145493,\"Not on River\"\n0.196683125676986,0.0801335088990509,\"Not on River\"\n0.247684883778961,0.0790376360610224,\"Not on River\"\n0.298686641880934,0.0781053576594317,\"Not on River\"\n0.349688399982909,0.0773143258842422,\"Not on River\"\n0.400690158084882,0.0767081491716067,\"Not on River\"\n0.451691916186856,0.0762571466918072,\"Not on River\"\n0.50269367428883,0.0760032022733928,\"Not on River\"\n0.553695432390804,0.0759184896036896,\"Not on River\"\n0.604697190492778,0.0760380572008988,\"Not on River\"\n0.655698948594752,0.076336988727801,\"Not on River\"\n0.706700706696726,0.0768414999764242,\"Not on River\"\n0.7577024647987,0.0775303883778879,\"Not on River\"\n0.808704222900674,0.0784196007669584,\"Not on River\"\n0.859705981002648,0.0794919855948558,\"Not on River\"\n0.910707739104621,0.0807524682478495,\"Not on River\"\n0.961709497206596,0.082187664736222,\"Not on River\"\n1.01271125530857,0.0837915292863837,\"Not on River\"\n1.06371301341054,0.0855534470276333,\"Not on River\"\n1.11471477151252,0.0874573835035442,\"Not on River\"\n1.16571652961449,0.0894937455101885,\"Not on River\"\n1.21671828771647,0.0916385460102649,\"Not on River\"\n1.26772004581844,0.0938807487400367,\"Not on River\"\n1.31872180392041,0.0961915737277406,\"Not on River\"\n1.36972356202239,0.0985554667501168,\"Not on River\"\n1.42072532012436,0.100943105086329,\"Not on River\"\n1.47172707822634,0.103330922839461,\"Not on River\"\n1.52272883632831,0.105694219485093,\"Not on River\"\n1.57373059443028,0.107997783034969,\"Not on River\"\n1.62473235253226,0.110227263109147,\"Not on River\"\n1.67573411063423,0.112332438561071,\"Not on River\"\n1.7267358687362,0.114312026657224,\"Not on River\"\n1.77773762683818,0.116107261863071,\"Not on River\"\n1.82873938494015,0.117724108371094,\"Not on River\"\n1.87974114304213,0.119102493240089,\"Not on River\"\n1.9307429011441,0.120252441451377,\"Not on River\"\n1.98174465924607,0.121119004183507,\"Not on River\"\n2.03274641734805,0.1217131477367,\"Not on River\"\n2.08374817545002,0.121991024901269,\"Not on River\"\n2.134749933552,0.121961866390684,\"Not on River\"\n2.18575169165397,0.121597813886956,\"Not on River\"\n2.23675344975594,0.120904474344977,\"Not on River\"\n2.28775520785792,0.119873197012684,\"Not on River\"\n2.33875696595989,0.118505131176091,\"Not on River\"\n2.38975872406187,0.116811940699902,\"Not on River\"\n2.44076048216384,0.114790677694502,\"Not on River\"\n2.49176224026581,0.112472083462534,\"Not on River\"\n2.54276399836779,0.1098506513913,\"Not on River\"\n2.59376575646976,0.106972654500547,\"Not on River\"\n2.64476751457174,0.103832560131985,\"Not on River\"\n2.69576927267371,0.100486647312654,\"Not on River\"\n2.74677103077568,0.0969325478968804,\"Not on River\"\n2.79777278887766,0.0932296419173313,\"Not on River\"\n2.84877454697963,0.0893821262014766,\"Not on River\"\n2.89977630508161,0.0854449827184913,\"Not on River\"\n2.95077806318358,0.081432143298434,\"Not on River\"\n3.00177982128555,0.0773869267648194,\"Not on River\"\n3.05278157938753,0.073335531457139,\"Not on River\"\n3.1037833374895,0.0693055768322702,\"Not on River\"\n3.15478509559147,0.0653305427451632,\"Not on River\"\n3.20578685369345,0.0614261148660666,\"Not on River\"\n3.25678861179542,0.057626123479183,\"Not on River\"\n3.3077903698974,0.0539381999708349,\"Not on River\"\n3.35879212799937,0.0503907950610987,\"Not on River\"\n3.40979388610134,0.0469870241469875,\"Not on River\"\n3.46079564420332,0.0437459495085031,\"Not on River\"\n3.51179740230529,0.0406690388635127,\"Not on River\"\n3.56279916040727,0.0377639043117635,\"Not on River\"\n3.61380091850924,0.0350323927824786,\"Not on River\"\n3.66480267661121,0.0324704800960272,\"Not on River\"\n3.71580443471319,0.0300815648043222,\"Not on River\"\n3.76680619281516,0.0278513518423054,\"Not on River\"\n3.81780795091714,0.0257852286187785,\"Not on River\"\n3.86880970901911,0.023861050619435,\"Not on River\"\n3.91981146712108,0.0220861026207386,\"Not on River\"\n3.97081322522306,0.0204333014462228,\"Not on River\"\n4.02181498332503,0.0189114489349689,\"Not on River\"\n4.072816741427,0.0174913846683463,\"Not on River\"\n4.12381849952898,0.0161829806972496,\"Not on River\"\n4.17482025763095,0.0149574142992935,\"Not on River\"\n4.22582201573293,0.0138251824835428,\"Not on River\"\n4.2768237738349,0.0127607422688864,\"Not on River\"\n4.32782553193688,0.0117713912660817,\"Not on River\"\n4.37882729003885,0.010838857022944,\"Not on River\"\n4.42982904814082,0.00996736449930094,\"Not on River\"\n4.4808308062428,0.00914438999377296,\"Not on River\"\n4.53183256434477,0.00837242195535952,\"Not on River\"\n4.58283432244674,0.00764331678595636,\"Not on River\"\n4.63383608054872,0.0069585437531023,\"Not on River\"\n4.68483783865069,0.00631321972350106,\"Not on River\"\n4.73583959675267,0.00570800794576339,\"Not on River\"\n4.78684135485464,0.00514043237193618,\"Not on River\"\n4.83784311295661,0.00461024172335215,\"Not on River\"\n4.88884487105859,0.00411674402865961,\"Not on River\"\n4.93984662916056,0.00365854105149688,\"Not on River\"\n4.99084838726254,0.00323627086526011,\"Not on River\"\n5.04185014536451,0.00284720000785179,\"Not on River\"\n5.09285190346648,0.00249294843810904,\"Not on River\"\n5.14385366156846,0.00216941224787201,\"Not on River\"\n5.19485541967043,0.00187890222356611,\"Not on River\"\n5.24585717777241,0.00161610101696466,\"Not on River\"\n5.29685893587438,0.0013837487769909,\"Not on River\"\n5.34786069397635,0.00117564088040978,\"Not on River\"\n5.39886245207833,0.000994707586482351,\"Not on River\"\n5.4498642101803,0.000834297737281423,\"Not on River\"\n5.50086596828227,0.00069728959532664,\"Not on River\"\n5.55186772638425,0.000577370718781485,\"Not on River\"\n5.60286948448622,0.000476283647751909,\"Not on River\"\n5.6538712425882,0.000389224846322749,\"Not on River\"\n5.70487300069017,0.000316780992820246,\"Not on River\"\n5.75587475879215,0.000255442375257613,\"Not on River\"\n5.80687651689412,0.000205041852302523,\"Not on River\"\n5.85787827499609,0.000163119260914711,\"Not on River\"\n5.90888003309807,0.000129092381802419,\"Not on River\"\n5.95988179120004,0.000101307859255012,\"Not on River\"\n6.01088354930202,7.90219959147921e-05,\"Not on River\"\n6.06188530740399,6.11701052076352e-05,\"Not on River\"\n6.11288706550596,4.70136729726823e-05,\"Not on River\"\n6.16388882360794,3.58962040143979e-05,\"Not on River\"\n6.21489058170991,2.71762514187986e-05,\"Not on River\"\n-6.52761393168195,0.000166187756986173,\"On River\"\n-6.48427464274309,0.000196379157580335,\"On River\"\n-6.44093535380423,0.000231344368916192,\"On River\"\n-6.39759606486537,0.000271402170243181,\"On River\"\n-6.35425677592651,0.000317323282108843,\"On River\"\n-6.31091748698765,0.000370630862704934,\"On River\"\n-6.2675781980488,0.00043113151973345,\"On River\"\n-6.22423890910994,0.000499560224280416,\"On River\"\n-6.18089962017108,0.00057778775684678,\"On River\"\n-6.13756033123222,0.000666402384971888,\"On River\"\n-6.09422104229336,0.000765692432452805,\"On River\"\n-6.0508817533545,0.000876800197843451,\"On River\"\n-6.00754246441564,0.00100269409391696,\"On River\"\n-5.96420317547679,0.00114243593341628,\"On River\"\n-5.92086388653793,0.00129704924126282,\"On River\"\n-5.87752459759907,0.00146931477484969,\"On River\"\n-5.83418530866021,0.00166015296419908,\"On River\"\n-5.79084601972135,0.00186935949629412,\"On River\"\n-5.74750673078249,0.00209811626241282,\"On River\"\n-5.70416744184364,0.00235115192213943,\"On River\"\n-5.66082815290478,0.00262604351506283,\"On River\"\n-5.61748886396592,0.00292380563826509,\"On River\"\n-5.57414957502706,0.00324765929526572,\"On River\"\n-5.5308102860882,0.00359878493956674,\"On River\"\n-5.48747099714934,0.00397588670980414,\"On River\"\n-5.44413170821049,0.00437981011926821,\"On River\"\n-5.40079241927163,0.00481629608773012,\"On River\"\n-5.35745313033277,0.00528154336451036,\"On River\"\n-5.31411384139391,0.00577605335159829,\"On River\"\n-5.27077455245505,0.00630284166400931,\"On River\"\n-5.22743526351619,0.00686339169838111,\"On River\"\n-5.18409597457734,0.0074551678276444,\"On River\"\n-5.14075668563848,0.00807870638539433,\"On River\"\n-5.09741739669962,0.00873986410860845,\"On River\"\n-5.05407810776076,0.00943462038569581,\"On River\"\n-5.0107388188219,0.0101629510223067,\"On River\"\n-4.96739952988304,0.0109277810406857,\"On River\"\n-4.92406024094419,0.0117316774703207,\"On River\"\n-4.88072095200533,0.0125715122293945,\"On River\"\n-4.83738166306647,0.0134482205118867,\"On River\"\n-4.79404237412761,0.0143687634598448,\"On River\"\n-4.75070308518875,0.01533007054761,\"On River\"\n-4.70736379624989,0.0163324562801908,\"On River\"\n-4.66402450731104,0.0173800115289335,\"On River\"\n-4.62068521837218,0.0184783216373593,\"On River\"\n-4.57734592943332,0.0196241005543706,\"On River\"\n-4.53400664049446,0.0208197723358217,\"On River\"\n-4.4906673515556,0.0220756606379858,\"On River\"\n-4.44732806261674,0.0233901612788274,\"On River\"\n-4.40398877367788,0.0247641793860841,\"On River\"\n-4.36064948473903,0.0262038552970372,\"On River\"\n-4.31731019580017,0.0277198686369147,\"On River\"\n-4.27397090686131,0.0293070369956725,\"On River\"\n-4.23063161792245,0.0309691136361349,\"On River\"\n-4.18729232898359,0.0327203095762225,\"On River\"\n-4.14395304004474,0.0345594321232856,\"On River\"\n-4.10061375110588,0.0364857927722245,\"On River\"\n-4.05727446216702,0.0385056933660367,\"On River\"\n-4.01393517322816,0.0406343628194015,\"On River\"\n-3.9705958842893,0.0428608150146365,\"On River\"\n-3.92725659535044,0.0451875228262624,\"On River\"\n-3.88391730641158,0.0476283770002348,\"On River\"\n-3.84057801747273,0.0501801905394813,\"On River\"\n-3.79723872853387,0.0528368424962917,\"On River\"\n-3.75389943959501,0.0556002543706786,\"On River\"\n-3.71056015065615,0.0584846765457175,\"On River\"\n-3.66722086171729,0.0614713585667633,\"On River\"\n-3.62388157277843,0.0645578505297227,\"On River\"\n-3.58054228383958,0.067750314497239,\"On River\"\n-3.53720299490072,0.0710413197407213,\"On River\"\n-3.49386370596186,0.0744181189407457,\"On River\"\n-3.450524417023,0.0778750992868185,\"On River\"\n-3.40718512808414,0.081416404216916,\"On River\"\n-3.36384583914528,0.0850205619159996,\"On River\"\n-3.32050655020643,0.0886790360654656,\"On River\"\n-3.27716726126757,0.0923857868797997,\"On River\"\n-3.23382797232871,0.0961287536645939,\"On River\"\n-3.19048868338985,0.0998936614811974,\"On River\"\n-3.14714939445099,0.103669765782411,\"On River\"\n-3.10381010551213,0.107444113708908,\"On River\"\n-3.06047081657328,0.111203976673894,\"On River\"\n-3.01713152763442,0.114938091171629,\"On River\"\n-2.97379223869556,0.118631426391302,\"On River\"\n-2.9304529497567,0.122269385221857,\"On River\"\n-2.88711366081784,0.1258454012412,\"On River\"\n-2.84377437187898,0.129349246740932,\"On River\"\n-2.80043508294013,0.132756373640283,\"On River\"\n-2.75709579400127,0.13606831285744,\"On River\"\n-2.71375650506241,0.139278380727756,\"On River\"\n-2.67041721612355,0.142371413809775,\"On River\"\n-2.62707792718469,0.145333904789322,\"On River\"\n-2.58373863824583,0.148173143090865,\"On River\"\n-2.54039934930697,0.150884573961809,\"On River\"\n-2.49706006036812,0.153444355770104,\"On River\"\n-2.45372077142926,0.155865386177262,\"On River\"\n-2.4103814824904,0.158150196365491,\"On River\"\n-2.36704219355154,0.160290812752529,\"On River\"\n-2.32370290461268,0.162277800268788,\"On River\"\n-2.28036361567382,0.164129839447751,\"On River\"\n-2.23702432673497,0.165848987354156,\"On River\"\n-2.19368503779611,0.167420395734095,\"On River\"\n-2.15034574885725,0.168860553610512,\"On River\"\n-2.10700645991839,0.170178880294875,\"On River\"\n-2.06366717097953,0.171374956094462,\"On River\"\n-2.02032788204067,0.172443422496479,\"On River\"\n-1.97698859310182,0.17340524789585,\"On River\"\n-1.93364930416296,0.174265341799035,\"On River\"\n-1.8903100152241,0.175016998379772,\"On River\"\n-1.84697072628524,0.175672855985473,\"On River\"\n-1.80363143734638,0.176242684539251,\"On River\"\n-1.76029214840752,0.176728477432047,\"On River\"\n-1.71695285946867,0.177125578336251,\"On River\"\n-1.67361357052981,0.177449639929206,\"On River\"\n-1.63027428159095,0.177703792582526,\"On River\"\n-1.58693499265209,0.177883358831985,\"On River\"\n-1.54359570371323,0.177994246514361,\"On River\"\n-1.50025641477437,0.178042643359916,\"On River\"\n-1.45691712583552,0.178028867338301,\"On River\"\n-1.41357783689666,0.177945435987285,\"On River\"\n-1.3702385479578,0.177802893328696,\"On River\"\n-1.32689925901894,0.177601684102334,\"On River\"\n-1.28355997008008,0.177336138025656,\"On River\"\n-1.24022068114122,0.177007885231246,\"On River\"\n-1.19688139220237,0.17662167830993,\"On River\"\n-1.15354210326351,0.176177435140721,\"On River\"\n-1.11020281432465,0.175666018283617,\"On River\"\n-1.06686352538579,0.175098228392447,\"On River\"\n-1.02352423644693,0.174475113426285,\"On River\"\n-0.980184947508073,0.173793396765438,\"On River\"\n-0.936845658569215,0.173055445446737,\"On River\"\n-0.893506369630357,0.172268586623519,\"On River\"\n-0.850167080691498,0.17143581140496,\"On River\"\n-0.80682779175264,0.170553932747242,\"On River\"\n-0.763488502813781,0.169634834946204,\"On River\"\n-0.720149213874923,0.168683538358966,\"On River\"\n-0.676809924936065,0.16770369125814,\"On River\"\n-0.633470635997206,0.166701896636992,\"On River\"\n-0.590131347058348,0.165687103412039,\"On River\"\n-0.54679205811949,0.164665994423861,\"On River\"\n-0.503452769180631,0.163646850279751,\"On River\"\n-0.460113480241772,0.162637873475735,\"On River\"\n-0.416774191302914,0.161646438998328,\"On River\"\n-0.373434902364056,0.160682202673387,\"On River\"\n-0.330095613425198,0.159755713769539,\"On River\"\n-0.28675632448634,0.158871143979347,\"On River\"\n-0.243417035547481,0.158035511282091,\"On River\"\n-0.200077746608622,0.157264893212697,\"On River\"\n-0.156738457669764,0.156559775197765,\"On River\"\n-0.113399168730906,0.155924127805161,\"On River\"\n-0.0700598797920478,0.155367404906024,\"On River\"\n-0.0267205908531887,0.154900626497381,\"On River\"\n0.0166186980856695,0.154517783649528,\"On River\"\n0.0599579870245277,0.154221730918162,\"On River\"\n0.103297275963386,0.154027062292452,\"On River\"\n0.146636564902244,0.153926463987306,\"On River\"\n0.189975853841103,0.153916299561634,\"On River\"\n0.233315142779961,0.153999543086289,\"On River\"\n0.27665443171882,0.154182540010845,\"On River\"\n0.319993720657678,0.154450070188629,\"On River\"\n0.363333009596537,0.154798409777861,\"On River\"\n0.406672298535395,0.155231888366735,\"On River\"\n0.450011587474253,0.155738277053615,\"On River\"\n0.493350876413111,0.156306867898188,\"On River\"\n0.53669016535197,0.156931583267533,\"On River\"\n0.580029454290829,0.157608023392054,\"On River\"\n0.623368743229687,0.158319003559617,\"On River\"\n0.666708032168545,0.159054558692693,\"On River\"\n0.710047321107403,0.159804327961146,\"On River\"\n0.753386610046261,0.160554075921259,\"On River\"\n0.79672589898512,0.161291123600901,\"On River\"\n0.840065187923979,0.162002418865956,\"On River\"\n0.883404476862837,0.162667399885166,\"On River\"\n0.926743765801695,0.163277692602193,\"On River\"\n0.970083054740553,0.163820126807211,\"On River\"\n1.01342234367941,0.164271727478271,\"On River\"\n1.05676163261827,0.164618544220272,\"On River\"\n1.10010092155713,0.16485423069595,\"On River\"\n1.14344021049599,0.164965333583834,\"On River\"\n1.18677949943485,0.164914683331142,\"On River\"\n1.2301187883737,0.164713121120095,\"On River\"\n1.27345807731256,0.164349932292447,\"On River\"\n1.31679736625142,0.163798769072654,\"On River\"\n1.36013665519028,0.163048845249542,\"On River\"\n1.40347594412914,0.162108192907873,\"On River\"\n1.446815233068,0.160970292791932,\"On River\"\n1.49015452200686,0.159593651125552,\"On River\"\n1.53349381094571,0.158008317738684,\"On River\"\n1.57683309988457,0.156213229117315,\"On River\"\n1.62017238882343,0.15419100873688,\"On River\"\n1.66351167776229,0.15193767282386,\"On River\"\n1.70685096670115,0.149477227056558,\"On River\"\n1.75019025564,0.146813548565159,\"On River\"\n1.79352954457886,0.143920720088057,\"On River\"\n1.83686883351772,0.140835394300538,\"On River\"\n1.88020812245658,0.137569445944793,\"On River\"\n1.92354741139544,0.134121834929214,\"On River\"\n1.9668867003343,0.130498130543215,\"On River\"\n2.01022598927316,0.126729661571924,\"On River\"\n2.05356527821201,0.122829393301703,\"On River\"\n2.09690456715087,0.11879546988495,\"On River\"\n2.14024385608973,0.11465898702433,\"On River\"\n2.18358314502859,0.110438475580975,\"On River\"\n2.22692243396745,0.10614657418804,\"On River\"\n2.2702617229063,0.101798408059671,\"On River\"\n2.31360101184516,0.0974178694805283,\"On River\"\n2.35694030078402,0.0930206190016776,\"On River\"\n2.40027958972288,0.0886241129894779,\"On River\"\n2.44361887866174,0.0842460637069772,\"On River\"\n2.4869581676006,0.0799008981666183,\"On River\"\n2.53029745653946,0.0756051575760834,\"On River\"\n2.57363674547831,0.0713803449194663,\"On River\"\n2.61697603441717,0.0672316764586872,\"On River\"\n2.66031532335603,0.0631705066200166,\"On River\"\n2.70365461229489,0.0592209851780445,\"On River\"\n2.74699390123375,0.0553869651402069,\"On River\"\n2.79033319017261,0.0516712119271614,\"On River\"\n2.83367247911146,0.0480849075142015,\"On River\"\n2.87701176805032,0.0446509183453817,\"On River\"\n2.92035105698918,0.0413548474893314,\"On River\"\n2.96369034592804,0.0382004315387458,\"On River\"\n3.0070296348669,0.035207252524752,\"On River\"\n3.05036892380576,0.0323701976537833,\"On River\"\n3.09370821274461,0.02968031982761,\"On River\"\n3.13704750168347,0.027140084141033,\"On River\"\n3.18038679062233,0.0247691270037197,\"On River\"\n3.22372607956119,0.0225416353410259,\"On River\"\n3.26706536850005,0.0204549740549411,\"On River\"\n3.31040465743891,0.0185196805113355,\"On River\"\n3.35374394637776,0.0167276899904826,\"On River\"\n3.39708323531662,0.0150640202322565,\"On River\"\n3.44042252425548,0.0135247563042571,\"On River\"\n3.48376181319434,0.0121238763117486,\"On River\"\n3.5271011021332,0.0108348014376528,\"On River\"\n3.57044039107206,0.00965209027519181,\"On River\"\n3.61377968001092,0.00857880925350158,\"On River\"\n3.65711896894977,0.00760798113665251,\"On River\"\n3.70045825788863,0.00672506217206396,\"On River\"\n3.74379754682749,0.00592450667764981,\"On River\"\n3.78713683576635,0.00521338646146209,\"On River\"\n3.83047612470521,0.00457265504394043,\"On River\"\n3.87381541364406,0.00399675898375808,\"On River\"\n3.91715470258292,0.00348505538693829,\"On River\"\n3.96049399152178,0.00303305067443211,\"On River\"\n4.00383328046064,0.00263029861858187,\"On River\"\n4.0471725693995,0.00227256026922357,\"On River\"\n4.09051185833836,0.0019618934891112,\"On River\"\n4.13385114727722,0.00168808128310711,\"On River\"\n4.17719043621607,0.00144697055339288,\"On River\"\n4.22052972515493,0.00123710115249953,\"On River\"\n4.26386901409379,0.00105615139846913,\"On River\"\n4.30720830303265,0.000898177754104394,\"On River\"\n4.35054759197151,0.000760719990947412,\"On River\"\n4.39388688091037,0.000643962819436623,\"On River\"\n4.43722616984922,0.000543355347830024,\"On River\"\n4.48056545878808,0.000456562353440997,\"On River\"\n4.52390474772694,0.000382524171609742,\"On River\""
        },
        {
            "name": ".0/group_by1/density2",
            "source": ".0/group_by1/density2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.chas"
                    ]
                }
            ]
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-7.20415321635949\n6.4557859180009"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-0.0106664586315589\n0.223995631262738"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "nice": true,
            "zero": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/density2"
            },
            "marks": [
                {
                    "type": "area",
                    "properties": {
                        "update": {
                            "y2": {
                                "scale": "y",
                                "value": 0
                            },
                            "fillOpacity": {
                                "value": 0.2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_"
                            },
                            "fill": {
                                "scale": "fill",
                                "field": "data.chas"
                            },
                            "stroke": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/density2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/density2"
            },
            "marks": [
                {
                    "type": "line",
                    "properties": {
                        "update": {
                            "stroke": {
                                "value": "#000000"
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_"
                            },
                            "fill": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/density2"
                            }
                        }
                    }
                }
            ]
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "chas"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "log_crim"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "density"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id402356561").parseSpec(plot_id402356561_spec);
</script><!--/html_preserve-->
When separating by `chas`, we see that `"Not on River"` is an almost
perfect mirror of the aggregate data. `"On River"` is still bimodal,
but the distinction is much less profound.

5. Explore the effect of `lstat` on the median home (`log_medv`) by:
a) Plotting percent lower status (`lstat`) relative to the log of the median home (`log_medv`) with points coloured (filled) by the radial highway locations (`rad`). Add the linear fits and standard errors grouped by `rad`. Discuss the efficacy of the fits and whether or not there appears to be an interaction between `lstat` and `rad`.

```r
Boston %>% ggvis(~lstat, ~log_medv) %>%
    layer_points(fill = ~rad) %>%
    group_by(rad) %>%
    layer_model_predictions(model="lm", se=TRUE, stroke = ~rad)
```

```
## Guessing formula = log_medv ~ lstat
```

<!--html_preserve--><div id="plot_id488841763-container" class="ggvis-output-container">
<div id="plot_id488841763" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id488841763_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id488841763" data-renderer="svg">SVG</a>
 | 
<a id="plot_id488841763_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id488841763" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id488841763_download" class="ggvis-download" data-plot-id="plot_id488841763">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id488841763_spec = {
    "data": [
        {
            "name": ".0",
            "format": {
                "type": "csv",
                "parse": {
                    "lstat": "number",
                    "log_medv": "number"
                }
            },
            "values": "\"lstat\",\"log_medv\",\"rad\"\n4.98,3.17805383034795,\"1\"\n9.14,3.07269331469012,\"2\"\n4.03,3.54673968695281,\"2\"\n2.94,3.50855589998265,\"3\"\n5.33,3.58905911883173,\"3\"\n5.21,3.35689712276558,\"3\"\n12.43,3.13113691056019,\"5\"\n19.15,3.29953372788566,\"5\"\n29.93,2.80336038090653,\"5\"\n17.1,2.9391619220656,\"5\"\n20.45,2.70805020110221,\"5\"\n13.27,2.9391619220656,\"5\"\n15.71,3.07731226054641,\"5\"\n8.26,3.01553490085017,\"4\"\n10.26,2.90142159408275,\"4\"\n8.47,2.99071973173045,\"4\"\n6.58,3.13983261752775,\"4\"\n14.67,2.86220088092947,\"4\"\n11.69,3.00568260440716,\"4\"\n11.28,2.90142159408275,\"4\"\n21.02,2.61006979274201,\"4\"\n13.83,2.97552956623647,\"4\"\n18.72,2.72129542785223,\"4\"\n19.88,2.67414864942653,\"4\"\n16.3,2.74727091425549,\"4\"\n16.51,2.63188884013665,\"4\"\n14.81,2.8094026953625,\"4\"\n17.28,2.69462718077007,\"4\"\n12.8,2.91235066461494,\"4\"\n11.98,3.04452243772342,\"4\"\n22.6,2.54160199346455,\"4\"\n13.04,2.67414864942653,\"4\"\n27.71,2.58021682959233,\"4\"\n18.35,2.57261223020711,\"4\"\n20.34,2.60268968544438,\"4\"\n9.68,2.9391619220656,\"5\"\n11.41,2.99573227355399,\"5\"\n8.77,3.04452243772342,\"5\"\n10.13,3.20680324363393,\"5\"\n4.32,3.42751468997953,\"3\"\n1.98,3.55248682920838,\"3\"\n4.84,3.28091121578765,\"3\"\n5.81,3.23080439573347,\"3\"\n7.44,3.20680324363393,\"3\"\n9.55,3.05400118167797,\"3\"\n10.21,2.96010509591084,\"3\"\n14.15,2.99573227355399,\"3\"\n18.8,2.8094026953625,\"3\"\n30.81,2.66722820658195,\"3\"\n16.2,2.96527306606928,\"3\"\n13.45,2.98061863574394,\"4\"\n9.43,3.02042488614436,\"4\"\n5.28,3.2188758248682,\"4\"\n8.43,3.15273602236366,\"4\"\n14.8,2.9391619220656,\"3\"\n4.81,3.56671182013973,\"5\"\n5.77,3.20680324363393,\"2\"\n3.95,3.45315712059287,\"5\"\n6.86,3.14845336057165,\"8\"\n9.22,2.97552956623647,\"8\"\n13.15,2.92852352386054,\"8\"\n14.44,2.77258872223978,\"8\"\n6.73,3.10009228887823,\"8\"\n9.5,3.2188758248682,\"8\"\n8.05,3.49650756146648,\"3\"\n4.67,3.15700042115011,\"4\"\n10.24,2.96527306606928,\"4\"\n8.1,3.09104245335832,\"4\"\n13.09,2.85647020622048,\"4\"\n8.79,3.03974915897077,\"4\"\n6.72,3.18635263316264,\"4\"\n9.88,3.07731226054641,\"4\"\n5.52,3.1267605359604,\"4\"\n7.54,3.15273602236366,\"4\"\n6.78,3.18221184049661,\"5\"\n8.94,3.06339092202781,\"5\"\n11.97,2.99573227355399,\"5\"\n10.27,3.03495298670727,\"5\"\n12.34,3.05400118167797,\"5\"\n9.1,3.01062088604774,\"5\"\n5.29,3.3322045101752,\"4\"\n7.22,3.17387845893747,\"4\"\n6.72,3.21084365317094,\"4\"\n7.51,3.13113691056019,\"4\"\n9.62,3.17387845893747,\"3\"\n6.53,3.28091121578765,\"3\"\n12.86,3.11351530921037,\"3\"\n8.44,3.10009228887823,\"3\"\n5.5,3.16124671203156,\"2\"\n5.7,3.35689712276558,\"2\"\n8.81,3.11794990627824,\"2\"\n8.2,3.09104245335832,\"2\"\n8.16,3.13113691056019,\"4\"\n6.21,3.2188758248682,\"4\"\n10.59,3.02529107579554,\"4\"\n6.65,3.34638914516716,\"2\"\n11.34,3.06339092202781,\"2\"\n4.21,3.65583960003574,\"2\"\n3.57,3.7796338173824,\"2\"\n6.19,3.50254987592244,\"2\"\n9.42,3.31418600467253,\"5\"\n7.67,3.27714473299218,\"5\"\n10.63,2.92316158071916,\"5\"\n13.44,2.96010509591084,\"5\"\n12.33,3.00071981506503,\"5\"\n16.47,2.9704144655697,\"5\"\n18.66,2.9704144655697,\"5\"\n14.09,3.01553490085017,\"5\"\n12.27,2.98568193770049,\"5\"\n15.55,2.96527306606928,\"5\"\n13,3.07731226054641,\"5\"\n10.16,3.1267605359604,\"6\"\n16.21,2.9338568698359,\"6\"\n17.09,2.92852352386054,\"6\"\n10.45,2.91777073208428,\"6\"\n15.76,2.90690105984738,\"6\"\n12.04,3.05400118167797,\"6\"\n10.3,2.95491027903374,\"6\"\n15.37,3.01553490085017,\"6\"\n13.61,2.96010509591084,\"6\"\n14.37,3.09104245335832,\"2\"\n14.27,3.01062088604774,\"2\"\n17.93,3.02042488614436,\"2\"\n25.41,2.85070650150373,\"2\"\n17.58,2.9338568698359,\"2\"\n14.81,3.06339092202781,\"2\"\n27.26,2.75366071235426,\"2\"\n17.19,2.78501124223834,\"4\"\n15.39,2.89037175789616,\"4\"\n18.34,2.66025953726586,\"4\"\n12.6,2.95491027903374,\"4\"\n12.26,2.97552956623647,\"4\"\n11.12,3.13549421592915,\"4\"\n15.03,2.91235066461494,\"4\"\n17.31,2.74727091425549,\"4\"\n16.96,2.89591193827178,\"4\"\n16.9,2.85647020622048,\"4\"\n14.59,2.83907846350861,\"4\"\n21.32,2.58776403522771,\"4\"\n18.46,2.87919845729804,\"4\"\n24.16,2.63905732961526,\"4\"\n34.41,2.66722820658195,\"4\"\n26.82,2.59525470695687,\"5\"\n26.42,2.74727091425549,\"5\"\n29.29,2.46809953147162,\"5\"\n27.8,2.62466859216316,\"5\"\n16.65,2.74727091425549,\"5\"\n29.53,2.68102152871429,\"5\"\n28.32,2.87919845729804,\"5\"\n21.45,2.73436750941958,\"5\"\n14.1,3.06805293513362,\"5\"\n13.28,2.97552956623647,\"5\"\n12.12,2.72785282839839,\"5\"\n15.79,2.96527306606928,\"5\"\n15.12,2.83321334405622,\"5\"\n15.02,2.74727091425549,\"5\"\n16.14,2.57261223020711,\"5\"\n4.59,3.72086249996699,\"5\"\n6.43,3.1904763503465,\"5\"\n7.39,3.14845336057165,\"5\"\n5.5,3.29583686600433,\"5\"\n1.73,3.91202300542815,\"5\"\n1.92,3.91202300542815,\"5\"\n3.32,3.91202300542815,\"5\"\n11.64,3.12236492448736,\"5\"\n9.81,3.2188758248682,\"5\"\n3.7,3.91202300542815,\"5\"\n12.14,3.16968558067743,\"5\"\n11.1,3.16968558067743,\"5\"\n11.32,3.10458667846607,\"5\"\n14.43,2.85647020622048,\"5\"\n12.03,2.94968833505258,\"5\"\n14.69,3.13983261752775,\"5\"\n9.04,3.16124671203156,\"5\"\n9.64,3.11794990627824,\"5\"\n5.33,3.38099467434464,\"5\"\n10.11,3.14415227867226,\"5\"\n6.29,3.20274644293832,\"5\"\n6.92,3.39785848039664,\"5\"\n5.04,3.6163087612791,\"3\"\n7.56,3.68386691229039,\"3\"\n9.45,3.58905911883173,\"3\"\n4.82,3.63495111208838,\"3\"\n5.68,3.48124008933569,\"3\"\n13.98,3.27336401015227,\"3\"\n13.15,3.38777436133001,\"3\"\n4.45,3.91202300542815,\"3\"\n6.68,3.46573590279973,\"5\"\n4.56,3.39450839351136,\"5\"\n5.39,3.55248682920838,\"5\"\n5.1,3.61091791264422,\"5\"\n4.69,3.41772668361337,\"5\"\n2.87,3.5945687746427,\"5\"\n5.03,3.43720781918519,\"1\"\n4.38,3.37073817417745,\"1\"\n2.97,3.91202300542815,\"4\"\n4.08,3.5055573969864,\"2\"\n8.61,3.41114771251532,\"2\"\n6.62,3.54385368206368,\"2\"\n4.56,3.55248682920838,\"3\"\n4.45,3.49347265777133,\"3\"\n7.43,3.18221184049661,\"2\"\n3.11,3.74478708605223,\"2\"\n3.81,3.88156379794344,\"4\"\n2.88,3.91202300542815,\"4\"\n10.87,3.11794990627824,\"4\"\n10.97,3.19458313229916,\"4\"\n18.06,3.11351530921037,\"4\"\n14.66,3.19458313229916,\"4\"\n23.09,2.99573227355399,\"4\"\n17.27,3.07731226054641,\"4\"\n23.98,2.96010509591084,\"4\"\n16.03,3.10906095886099,\"4\"\n9.38,3.3357695763397,\"4\"\n29.55,3.16547504814109,\"4\"\n9.47,3.2188758248682,\"4\"\n13.51,3.14845336057165,\"5\"\n9.69,3.35689712276558,\"5\"\n17.92,3.06805293513362,\"5\"\n10.5,3.13549421592915,\"5\"\n9.71,3.2846635654062,\"8\"\n21.46,3.07731226054641,\"8\"\n9.93,3.31418600467253,\"8\"\n7.6,3.40452517175483,\"8\"\n4.14,3.80220813942094,\"8\"\n4.63,3.91202300542815,\"8\"\n3.13,3.62700405039585,\"8\"\n6.36,3.45315712059287,\"8\"\n3.92,3.84374416467485,\"8\"\n3.76,3.44998754583159,\"8\"\n11.65,3.1904763503465,\"8\"\n5.25,3.45631668088323,\"8\"\n2.47,3.73050112880476,\"8\"\n3.95,3.87743156065853,\"8\"\n8.05,3.36729582998647,\"8\"\n10.88,3.17805383034795,\"8\"\n9.54,3.22286784613774,\"8\"\n4.73,3.44998754583159,\"8\"\n6.36,3.16547504814109,\"6\"\n7.37,3.14845336057165,\"6\"\n11.38,3.09104245335832,\"6\"\n12.4,3.00071981506503,\"6\"\n11.22,3.10009228887823,\"6\"\n5.19,3.16547504814109,\"6\"\n12.5,2.86789890204411,\"7\"\n18.46,2.91777073208428,\"7\"\n9.16,3.1904763503465,\"7\"\n10.15,3.02042488614436,\"7\"\n9.52,3.19867311755068,\"7\"\n6.56,3.26575941076705,\"7\"\n5.9,3.19458313229916,\"7\"\n3.59,3.21084365317094,\"7\"\n3.53,3.38777436133001,\"7\"\n3.54,3.75653810258775,\"7\"\n6.57,3.08648663682246,\"1\"\n9.25,3.03974915897077,\"1\"\n3.11,3.78418963391826,\"3\"\n5.12,3.91202300542815,\"5\"\n7.79,3.58351893845611,\"5\"\n6.9,3.40452517175483,\"5\"\n9.59,3.52046080248897,\"5\"\n7.26,3.7635229971097,\"5\"\n5.91,3.8877303128591,\"5\"\n11.25,3.43398720448515,\"5\"\n8.1,3.59731226058845,\"5\"\n10.45,3.1267605359604,\"5\"\n14.79,3.42426265459315,\"5\"\n7.44,3.91202300542815,\"5\"\n3.16,3.77276093809464,\"5\"\n13.65,3.03013370027132,\"3\"\n13,3.04927304048202,\"3\"\n6.59,3.22684399451738,\"3\"\n7.73,3.19458313229916,\"3\"\n6.58,3.56104608260405,\"3\"\n3.53,3.47815842279828,\"4\"\n2.98,3.46573590279973,\"4\"\n6.05,3.50254987592244,\"4\"\n4.16,3.49953328238302,\"4\"\n7.19,3.37073817417745,\"4\"\n4.85,3.55820113047182,\"5\"\n3.76,3.8155121050473,\"5\"\n4.59,3.56671182013973,\"5\"\n3.01,3.8286413964891,\"5\"\n3.16,3.91202300542815,\"1\"\n7.85,3.47196645255036,\"1\"\n8.23,3.09104245335832,\"1\"\n12.93,3.00071981506503,\"1\"\n7.14,3.14415227867226,\"6\"\n7.6,3.10458667846607,\"6\"\n9.51,3.21084365317094,\"6\"\n3.33,3.3499040872746,\"4\"\n3.56,3.61899332664977,\"4\"\n4.7,3.32862668882732,\"4\"\n8.58,3.17387845893747,\"4\"\n10.4,3.07731226054641,\"4\"\n6.27,3.35340671782581,\"4\"\n7.39,3.29953372788566,\"4\"\n15.84,3.01062088604774,\"4\"\n4.97,3.11351530921037,\"5\"\n4.74,3.36729582998647,\"5\"\n6.07,3.21084365317094,\"5\"\n9.5,3.09104245335832,\"7\"\n8.67,3.27336401015227,\"7\"\n4.86,3.49953328238302,\"7\"\n6.93,3.58629286533884,\"7\"\n8.93,3.34638914516716,\"7\"\n6.47,3.50855589998265,\"7\"\n7.53,3.33932197794407,\"7\"\n4.54,3.1267605359604,\"4\"\n9.97,3.01062088604774,\"4\"\n12.64,2.77881927199042,\"4\"\n5.98,3.09557760852371,\"4\"\n11.72,2.96527306606928,\"4\"\n7.9,3.07269331469012,\"4\"\n9.28,3.16968558067743,\"4\"\n11.5,2.78501124223834,\"4\"\n18.33,2.87919845729804,\"4\"\n15.94,2.98568193770049,\"4\"\n10.36,3.13983261752775,\"4\"\n12.73,3.04452243772342,\"4\"\n7.2,3.16968558067743,\"5\"\n6.87,3.13983261752775,\"5\"\n7.7,3.01553490085017,\"5\"\n11.74,2.91777073208428,\"5\"\n6.12,3.2188758248682,\"5\"\n5.08,3.20274644293832,\"5\"\n6.15,3.13549421592915,\"5\"\n12.79,3.10009228887823,\"5\"\n9.97,2.96010509591084,\"4\"\n7.34,3.11794990627824,\"4\"\n9.09,2.98568193770049,\"4\"\n12.43,2.83907846350861,\"1\"\n7.83,2.96527306606928,\"1\"\n5.68,3.10009228887823,\"5\"\n6.75,3.03013370027132,\"5\"\n8.01,3.04927304048202,\"5\"\n9.8,2.9704144655697,\"5\"\n10.56,2.91777073208428,\"5\"\n8.51,3.02529107579554,\"5\"\n9.74,2.94443897916644,\"5\"\n9.29,2.92852352386054,\"5\"\n5.49,3.48737507790321,\"1\"\n8.65,2.80336038090653,\"1\"\n7.18,3.17387845893747,\"5\"\n4.61,3.44041809481544,\"5\"\n10.53,2.86220088092947,\"3\"\n12.67,2.84490938381941,\"3\"\n6.36,3.13983261752775,\"4\"\n5.99,3.19867311755068,\"4\"\n5.89,3.28091121578765,\"1\"\n5.98,3.13113691056019,\"1\"\n5.49,3.18221184049661,\"4\"\n7.79,2.92316158071916,\"4\"\n4.5,3.40452517175483,\"5\"\n8.05,2.90142159408275,\"4\"\n5.57,3.02529107579554,\"4\"\n17.6,2.87919845729804,\"24\"\n13.27,3.07731226054641,\"24\"\n11.48,3.12236492448736,\"24\"\n12.67,3.11794990627824,\"24\"\n7.79,3.2188758248682,\"24\"\n14.19,2.99071973173045,\"24\"\n10.19,3.03495298670727,\"24\"\n14.64,2.82137888640921,\"24\"\n5.29,3.08648663682246,\"24\"\n7.12,3.31418600467253,\"24\"\n14,3.08648663682246,\"24\"\n13.33,3.13983261752775,\"24\"\n3.26,3.91202300542815,\"24\"\n3.73,3.91202300542815,\"24\"\n2.96,3.91202300542815,\"24\"\n9.53,3.91202300542815,\"24\"\n8.88,3.91202300542815,\"24\"\n34.77,2.62466859216316,\"24\"\n37.97,2.62466859216316,\"24\"\n13.44,2.70805020110221,\"24\"\n23.24,2.63188884013665,\"24\"\n21.24,2.58776403522771,\"24\"\n23.69,2.57261223020711,\"24\"\n21.78,2.32238772029023,\"24\"\n17.21,2.34180580614733,\"24\"\n21.08,2.3887627892351,\"24\"\n23.6,2.42480272571829,\"24\"\n24.56,2.50959926237837,\"24\"\n30.63,2.17475172148416,\"24\"\n30.81,1.97408102602201,\"24\"\n28.28,2.35137525716348,\"24\"\n31.99,2.00148000021012,\"24\"\n30.62,2.32238772029023,\"24\"\n20.85,2.4423470353692,\"24\"\n17.11,2.71469474382088,\"24\"\n18.76,3.14415227867226,\"24\"\n25.68,2.27212588550934,\"24\"\n15.17,2.62466859216316,\"24\"\n16.35,2.54160199346455,\"24\"\n17.12,2.57261223020711,\"24\"\n19.37,2.52572864430826,\"24\"\n19.92,2.14006616349627,\"24\"\n30.59,1.6094379124341,\"24\"\n29.97,1.84054963339749,\"24\"\n26.77,1.7227665977411,\"24\"\n20.32,1.97408102602201,\"24\"\n20.31,2.4932054526027,\"24\"\n19.77,2.11625551480255,\"24\"\n27.38,2.14006616349627,\"24\"\n22.98,1.6094379124341,\"24\"\n23.34,2.47653840011748,\"24\"\n12.13,3.32862668882732,\"24\"\n26.4,2.84490938381941,\"24\"\n19.78,3.31418600467253,\"24\"\n10.11,2.70805020110221,\"24\"\n21.22,2.84490938381941,\"24\"\n34.37,2.88480071284671,\"24\"\n20.08,2.79116510781272,\"24\"\n36.98,1.94591014905531,\"24\"\n29.05,1.97408102602201,\"24\"\n25.79,2.01490302054226,\"24\"\n26.64,2.34180580614733,\"24\"\n20.62,2.17475172148416,\"24\"\n22.74,2.12823170584927,\"24\"\n15.02,2.81540871942271,\"24\"\n15.7,2.65324196460721,\"24\"\n14.1,3.03495298670727,\"24\"\n23.29,2.59525470695687,\"24\"\n17.16,2.45958884180371,\"24\"\n24.39,2.11625551480255,\"24\"\n15.69,2.32238772029023,\"24\"\n14.52,2.3887627892351,\"24\"\n21.52,2.39789527279837,\"24\"\n24.08,2.2512917986065,\"24\"\n17.64,2.67414864942653,\"24\"\n19.69,2.64617479738412,\"24\"\n12.03,2.77881927199042,\"24\"\n16.22,2.66025953726586,\"24\"\n15.17,2.45958884180371,\"24\"\n23.27,2.59525470695687,\"24\"\n18.05,2.26176309847379,\"24\"\n26.45,2.16332302566054,\"24\"\n34.02,2.12823170584927,\"24\"\n22.88,2.54944517092557,\"24\"\n22.11,2.35137525716348,\"24\"\n19.52,2.83907846350861,\"24\"\n16.59,2.91235066461494,\"24\"\n18.85,2.73436750941958,\"24\"\n23.79,2.37954613413017,\"24\"\n23.98,2.46809953147162,\"24\"\n17.79,2.70136121295141,\"24\"\n16.44,2.53369681395743,\"24\"\n18.13,2.64617479738412,\"24\"\n19.31,2.56494935746154,\"24\"\n17.44,2.59525470695687,\"24\"\n17.73,2.72129542785223,\"24\"\n17.27,2.77881927199042,\"24\"\n16.74,2.87919845729804,\"24\"\n18.71,2.70136121295141,\"24\"\n18.13,2.64617479738412,\"24\"\n19.01,2.54160199346455,\"24\"\n16.94,2.60268968544438,\"24\"\n16.23,2.70136121295141,\"24\"\n14.7,2.99573227355399,\"24\"\n16.42,2.79728133483015,\"24\"\n14.65,2.87356463957978,\"24\"\n13.99,2.9704144655697,\"24\"\n10.29,3.00568260440716,\"24\"\n13.22,3.06339092202781,\"24\"\n14.13,2.99071973173045,\"24\"\n17.15,2.94443897916644,\"24\"\n21.32,2.94968833505258,\"24\"\n18.13,2.94968833505258,\"24\"\n14.76,3.00071981506503,\"24\"\n16.29,2.99071973173045,\"24\"\n12.87,2.97552956623647,\"24\"\n14.36,3.14415227867226,\"24\"\n11.66,3.39450839351136,\"24\"\n18.14,2.62466859216316,\"24\"\n24.1,2.58776403522771,\"24\"\n18.68,2.81540871942271,\"24\"\n24.91,2.484906649788,\"24\"\n18.03,2.68102152871429,\"24\"\n13.11,3.06339092202781,\"24\"\n10.74,3.13549421592915,\"24\"\n7.74,3.16547504814109,\"24\"\n7.01,3.2188758248682,\"24\"\n10.42,3.08190996979504,\"24\"\n13.34,3.02529107579554,\"24\"\n10.58,3.05400118167797,\"24\"\n14.98,2.94968833505258,\"24\"\n11.45,3.02529107579554,\"24\"\n18.06,2.72129542785223,\"4\"\n23.97,1.94591014905531,\"4\"\n29.68,2.09186406167839,\"4\"\n18.07,2.61006979274201,\"4\"\n13.35,3.00071981506503,\"4\"\n12.01,3.08190996979504,\"6\"\n13.59,3.19867311755068,\"6\"\n17.6,3.13983261752775,\"6\"\n21.14,2.98061863574394,\"6\"\n14.1,2.90690105984738,\"6\"\n12.92,3.05400118167797,\"6\"\n15.1,2.86220088092947,\"6\"\n14.33,2.82137888640921,\"6\"\n9.67,3.10906095886099,\"1\"\n9.08,3.02529107579554,\"1\"\n5.64,3.17387845893747,\"1\"\n6.48,3.09104245335832,\"1\"\n7.88,2.47653840011748,\"1\""
        },
        {
            "name": ".0/group_by1/model_prediction2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "resp_upr_": "number",
                    "pred_": "number",
                    "resp_lwr_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"resp_upr_\",\"pred_\",\"resp_lwr_\",\"resp_\",\"rad\"\n3.68604535641329,3.16,3.23819723975669,3.46212129808499,\"1\"\n3.67195163306877,3.28367088607595,3.23386813669498,3.45290988488188,\"1\"\n3.65789713724889,3.4073417721519,3.22949980610864,3.44369847167876,\"1\"\n3.64388459081061,3.53101265822785,3.22508952614069,3.43448705847565,\"1\"\n3.62991694632541,3.6546835443038,3.22063434421966,3.42527564527254,\"1\"\n3.61599740835078,3.77835443037975,3.21613105578807,3.41606423206942,\"1\"\n3.60212945646535,3.9020253164557,3.21157618126727,3.40685281886631,\"1\"\n3.58831687010138,4.02569620253165,3.20696594122501,3.3976414056632,\"1\"\n3.57456375516353,4.14936708860759,3.20229622975664,3.38842999246008,\"1\"\n3.56087457236005,4.27303797468354,3.19756258615389,3.37921857925697,\"1\"\n3.54725416708655,4.39670886075949,3.19276016502116,3.37000716605385,\"1\"\n3.53370780058772,4.52037974683544,3.18788370511376,3.36079575285074,\"1\"\n3.52024118197193,4.64405063291139,3.18292749732332,3.35158433964763,\"1\"\n3.50686050046064,4.76772151898734,3.17788535242839,3.34237292644451,\"1\"\n3.49357245701181,4.89139240506329,3.17275056947099,3.3331615132414,\"1\"\n3.48038429415712,5.01506329113924,3.16751590591945,3.32395010003829,\"1\"\n3.46730382253334,5.13873417721519,3.162173551137,3.31473868683517,\"1\"\n3.45433944216773,5.26240506329114,3.15671510509639,3.30552727363206,\"1\"\n3.44150015610404,5.38607594936709,3.15113156475385,3.29631586042895,\"1\"\n3.42879557344675,5.50974683544304,3.14541332100491,3.28710444722583,\"1\"\n3.41623589839067,5.63341772151899,3.13955016965477,3.27789303402272,\"1\"\n3.40383190134466,5.75708860759494,3.13353134029455,3.2686816208196,\"1\"\n3.39159486793078,5.88075949367089,3.1273455473022,3.25947020761649,\"1\"\n3.37953652154775,6.00443037974684,3.120981067279,3.25025879441338,\"1\"\n3.36766891545934,6.12810126582279,3.11442584696119,3.24104738121026,\"1\"\n3.35600429114491,6.25177215189873,3.10766764486939,3.23183596800715,\"1\"\n3.34455490106285,6.37544303797468,3.10069420854522,3.22262455480404,\"1\"\n3.33333279611777,6.49911392405063,3.09349348708408,3.21341314160092,\"1\"\n3.32234958099269,6.62278481012658,3.08605387580293,3.20420172839781,\"1\"\n3.31161614397554,6.74645569620253,3.07836448641385,3.19499031519469,\"1\"\n3.30114237167567,6.87012658227848,3.07041543230749,3.18577890199158,\"1\"\n3.29093686261285,6.99379746835443,3.06219811496408,3.17656748878847,\"1\"\n3.28100665645983,7.11746835443038,3.05370549471087,3.16735607558535,\"1\"\n3.27135699708976,7.24113924050633,3.04493232767472,3.15814466238224,\"1\"\n3.26199114699898,7.36481012658228,3.03587535135928,3.14893324917913,\"1\"\n3.25291026790108,7.48848101265823,3.02653340405094,3.13972183597601,\"1\"\n3.24411337747287,7.61215189873418,3.01690746807293,3.1305104227729,\"1\"\n3.23559738595444,7.73582278481013,3.00700063318513,3.12129900956979,\"1\"\n3.22735720946773,7.85949367088608,2.99681798326561,3.11208759636667,\"1\"\n3.21938595055438,7.98316455696203,2.98636641577274,3.10287618316356,\"1\"\n3.21167513146804,8.10683544303797,2.97565440845285,3.09366476996044,\"1\"\n3.20421496279842,8.23050632911392,2.96469175071624,3.08445335675733,\"1\"\n3.19699462924379,8.35417721518987,2.95348925786465,3.07524194355422,\"1\"\n3.1900025755726,8.47784810126582,2.9420584851296,3.0660305303511,\"1\"\n3.18322677851737,8.60151898734177,2.93041145577861,3.05681911714799,\"1\"\n3.17665499388948,8.72518987341772,2.91856041400027,3.04760770394488,\"1\"\n3.17027497197536,8.84886075949367,2.90651760950817,3.03839629074176,\"1\"\n3.16407463778187,8.97253164556962,2.89429511729543,3.02918487753865,\"1\"\n3.15804223562626,9.09620253164557,2.88190469304481,3.01997346433553,\"1\"\n3.152166439769,9.21987341772152,2.86935766249584,3.01076205113242,\"1\"\n3.14643643425984,9.34354430379747,2.85666484159877,3.00155063792931,\"1\"\n3.14084196599274,9.46721518987342,2.84383648345965,2.99233922472619,\"1\"\n3.13537337527361,9.59088607594937,2.83088224777255,2.98312781152308,\"1\"\n3.13002160813839,9.71455696202532,2.81781118850154,2.97391639831997,\"1\"\n3.12477821434612,9.83822784810127,2.80463175588758,2.96470498511685,\"1\"\n3.1196353345216,9.96189873417721,2.79135180930588,2.95549357191374,\"1\"\n3.11458567941307,10.0855696202532,2.77797863800818,2.94628215871063,\"1\"\n3.10962250372039,10.2092405063291,2.76451898729463,2.93707074550751,\"1\"\n3.10473957647138,10.3329113924051,2.75097908813741,2.9278593323044,\"1\"\n3.09993114949914,10.456582278481,2.73736468870343,2.91864791910128,\"1\"\n3.09519192520832,10.580253164557,2.72368108658802,2.90943650589817,\"1\"\n3.09051702451402,10.7039240506329,2.7099331608761,2.90022509269506,\"1\"\n3.08590195558979,10.8275949367089,2.6961254033941,2.89101367949194,\"1\"\n3.08134258386421,10.9512658227848,2.68226194871345,2.88180226628883,\"1\"\n3.07683510355185,11.0749367088608,2.66834660261958,2.87259085308572,\"1\"\n3.07237601088691,11.1986075949367,2.65438286887829,2.8633794398826,\"1\"\n3.06796207913966,11.3222784810127,2.64037397421932,2.85416802667949,\"1\"\n3.0635903354312,11.4459493670886,2.62632289152155,2.84495661347637,\"1\"\n3.0592580393161,11.5696202531646,2.61223236123042,2.83574520027326,\"1\"\n3.05496266307066,11.6932911392405,2.59810491106963,2.82653378707015,\"1\"\n3.05070187360389,11.8169620253165,2.58394287413018,2.81732237386703,\"1\"\n3.04647351589583,11.9406329113924,2.56974840543201,2.80811096066392,\"1\"\n3.0422755978615,12.0643037974684,2.55552349706011,2.79889954746081,\"1\"\n3.03810627653658,12.1879746835443,2.54126999197881,2.78968813425769,\"1\"\n3.03396384548231,12.3116455696203,2.52698959662685,2.78047672105458,\"1\"\n3.02984672331028,12.4353164556962,2.51268389239265,2.77126530785146,\"1\"\n3.02575344323238,12.5589873417722,2.49835434606432,2.76205389464835,\"1\"\n3.02168264354692,12.6826582278481,2.48400231934356,2.75284248144524,\"1\"\n3.01763305897757,12.8063291139241,2.46962907750668,2.74363106824212,\"1\"\n3.01360351278837,12.93,2.45523579728965,2.73441965503901,\"1\"\n3.59389284280613,3.11,3.39394906513957,3.49392095397285,\"2\"\n3.58081559111338,3.41569620253165,3.38550447100884,3.48316003106111,\"2\"\n3.56779058536127,3.72139240506329,3.37700763093748,3.47239910814937,\"2\"\n3.5548216338942,4.02708860759494,3.36845473658106,3.46163818523763,\"2\"\n3.54191281552196,4.33278481012658,3.35984170912984,3.4508772623259,\"2\"\n3.52906848638775,4.63848101265823,3.35116419244057,3.44011633941416,\"2\"\n3.51629328293603,4.94417721518987,3.34241755006881,3.42935541650242,\"2\"\n3.50359211974672,5.24987341772152,3.33359686743464,3.41859449359068,\"2\"\n3.49097018083123,5.55556962025316,3.32469696052665,3.40783357067894,\"2\"\n3.47843290284536,5.86126582278481,3.31571239268904,3.3970726477672,\"2\"\n3.46598594859483,6.16696202531646,3.3066375011161,3.38631172485546,\"2\"\n3.45363516922602,6.4726582278481,3.29746643466143,3.37555080194373,\"2\"\n3.44138655365039,6.77835443037975,3.28819320441358,3.36478987903199,\"2\"\n3.42924616408916,7.08405063291139,3.27881174815134,3.35402895612025,\"2\"\n3.41722005718349,7.38974683544304,3.26931600923352,3.34326803320851,\"2\"\n3.40531419091859,7.69544303797468,3.25970002967495,3.33250711029677,\"2\"\n3.39353431865104,8.00113924050633,3.24995805611902,3.32174618738503,\"2\"\n3.38188587276389,8.30683544303798,3.2400846561827,3.31098526447329,\"2\"\n3.37037384180643,8.61253164556962,3.23007484131668,3.30022434156155,\"2\"\n3.3590026462595,8.91822784810127,3.21992419104013,3.28946341864982,\"2\"\n3.34777601911583,9.22392405063291,3.20962897236033,3.27870249573808,\"2\"\n3.33669689807754,9.52962025316456,3.19918624757514,3.26794157282634,\"2\"\n3.32576733617584,9.8353164556962,3.18859396365336,3.2571806499146,\"2\"\n3.31498843690406,10.1410126582278,3.17785101710166,3.24641972700286,\"2\"\n3.30436031852894,10.4467088607595,3.1669572896533,3.23565880409112,\"2\"\n3.29388211023141,10.7524050632911,3.15591365212736,3.22489788117938,\"2\"\n3.28355198036714,11.0581012658228,3.14472193616815,3.21413695826764,\"2\"\n3.27336719473737,11.3637974683544,3.13338487597445,3.20337603535591,\"2\"\n3.26332420063915,11.6694936708861,3.12190602424918,3.19261511244417,\"2\"\n3.25341873088141,11.9751898734177,3.11028964818345,3.18185418953243,\"2\"\n3.24364592106511,12.2808860759494,3.09854061217627,3.17109326662069,\"2\"\n3.23400043326399,12.586582278481,3.08666425415391,3.16033234370895,\"2\"\n3.22447657972346,12.8922784810127,3.07466626187097,3.14957142079721,\"2\"\n3.21506844115803,13.1979746835443,3.06255255461292,3.13881049788547,\"2\"\n3.20576997547368,13.503670886076,3.05032917447379,3.12804957497374,\"2\"\n3.19657511407729,13.8093670886076,3.03800219004671,3.117288652062,\"2\"\n3.18747784420422,14.1150632911392,3.0255776140963,3.10652772915026,\"2\"\n3.17847227678862,14.4207594936709,3.01306133568842,3.09576680623852,\"2\"\n3.16955270026211,14.7264556962025,3.00045906639145,3.08500588332678,\"2\"\n3.16071362128242,15.0321518987342,2.98777629954767,3.07424496041504,\"2\"\n3.15194979378117,15.3378481012658,2.97501828122544,3.0634840375033,\"2\"\n3.1432562379153,15.6435443037975,2.96218999126783,3.05272311459156,\"2\"\n3.13462825055255,15.9492405063291,2.9492961328071,3.04196219167983,\"2\"\n3.12606140886168,16.2549367088608,2.9363411286745,3.03120126876809,\"2\"\n3.11755156844913,16.5606329113924,2.92332912326357,3.02044034585635,\"2\"\n3.1090948573181,16.8663291139241,2.91026398857112,3.00967942294461,\"2\"\n3.10068766674457,17.1720253164557,2.89714933332117,2.99891850003287,\"2\"\n3.09232663998548,17.4777215189873,2.88398851425678,2.98815757712113,\"2\"\n3.08400865956618,17.783417721519,2.8707846488526,2.97739665420939,\"2\"\n3.07573083374352,18.0891139240506,2.85754062885179,2.96663573129766,\"2\"\n3.06749048260987,18.3948101265823,2.84425913416196,2.95587480838592,\"2\"\n3.05928512419296,18.7005063291139,2.8309426467554,2.94511388547418,\"2\"\n3.05111246081423,19.0062025316456,2.81759346431064,2.93435296256244,\"2\"\n3.04297036589445,19.3118987341772,2.80421371340695,2.9235920396507,\"2\"\n3.03485687133538,19.6175949367089,2.79080536214255,2.91283111673896,\"2\"\n3.02677015556021,19.9232911392405,2.77737023209424,2.90207019382722,\"2\"\n3.01870853225914,20.2289873417722,2.76391000957183,2.89130927091548,\"2\"\n3.01067043985954,20.5346835443038,2.75042625614795,2.88054834800375,\"2\"\n3.00265443171993,20.8403797468354,2.73692041846408,2.86978742509201,\"2\"\n2.9946591670325,21.1460759493671,2.72339383732803,2.85902650218027,\"2\"\n2.9866834024088,21.4517721518987,2.70984775612826,2.84826557926853,\"2\"\n2.97872598411638,21.7574683544304,2.69628332859721,2.83750465635679,\"2\"\n2.97078584093028,22.063164556962,2.68270162595982,2.82674373344505,\"2\"\n2.96286197756088,22.3688607594937,2.66910364350574,2.81598281053331,\"2\"\n2.95495346861889,22.6745569620253,2.65549030662426,2.80522188762157,\"2\"\n2.94705945307887,22.980253164557,2.6418624763408,2.79446096470984,\"2\"\n2.93917912920342,23.2859493670886,2.62822095439277,2.7837000417981,\"2\"\n2.93131174989181,23.5916455696203,2.61456648788091,2.77293911888636,\"2\"\n2.92345661841886,23.8973417721519,2.60089977353038,2.76217819597462,\"2\"\n2.91561308453179,24.2030379746835,2.58722146159397,2.75141727306288,\"2\"\n2.90778054087493,24.5087341772152,2.57353215942735,2.74065635015114,\"2\"\n2.89995841971439,24.8144303797468,2.55983243476441,2.7298954272394,\"2\"\n2.89214618993685,25.1201265822785,2.54612281871848,2.71913450432766,\"2\"\n2.88434335429876,25.4258227848101,2.5324038085331,2.70837358141593,\"2\"\n2.87654944690404,25.7315189873418,2.51867587010433,2.69761265850419,\"2\"\n2.86876403089046,26.0372151898734,2.50493944029444,2.68685173559245,\"2\"\n2.86098669630619,26.3429113924051,2.49119492905523,2.67609081268071,\"2\"\n2.85321705816005,26.6486075949367,2.47744272137789,2.66532988976897,\"2\"\n2.84545475463021,26.9543037974684,2.46368317908426,2.65456896685723,\"2\"\n2.83769944541734,27.26,2.44991664247365,2.64380804394549,\"2\"\n3.68609406890399,1.98,3.47346950030588,3.57978178460494,\"3\"\n3.66760077597698,2.34493670886076,3.46179575846825,3.56469826722261,\"3\"\n3.64917983746805,2.70987341772152,3.45004966221253,3.54961474984029,\"3\"\n3.63083877573486,3.07481012658228,3.43822368918108,3.53453123245797,\"3\"\n3.61258597231219,3.43974683544304,3.42630945783911,3.51944771507565,\"3\"\n3.59443074465398,3.8046835443038,3.41429765073268,3.50436419769333,\"3\"\n3.57638341650852,4.16962025316456,3.4021779441135,3.48928068031101,\"3\"\n3.55845537484952,4.53455696202532,3.38993895100786,3.47419716292869,\"3\"\n3.54065910373027,4.89949367088608,3.37756818736247,3.45911364554637,\"3\"\n3.52300818255939,5.26443037974684,3.36505207376871,3.44403012816405,\"3\"\n3.50551723343198,5.62936708860759,3.35237598813148,3.42894661078173,\"3\"\n3.48820179988093,5.99430379746835,3.33952438691788,3.41386309339941,\"3\"\n3.4710781386651,6.35924050632911,3.32648101336907,3.39877957601709,\"3\"\n3.45416290823027,6.72417721518987,3.31322920903926,3.38369605863477,\"3\"\n3.43747274364967,7.08911392405063,3.29975233885522,3.36861254125245,\"3\"\n3.42102371927489,7.45405063291139,3.28603432846536,3.35352902387013,\"3\"\n3.40483071713521,7.81898734177215,3.2720602958404,3.33844550648781,\"3\"\n3.38890673968815,8.18392405063291,3.25781723852282,3.32336198910549,\"3\"\n3.37326222597383,8.54886075949367,3.2432947174725,3.30827847172317,\"3\"\n3.35790444479937,8.91379746835443,3.22848546388232,3.29319495434084,\"3\"\n3.34283704120921,9.27873417721519,3.21338583270783,3.27811143695852,\"3\"\n3.32805979935187,9.64367088607595,3.19799603980054,3.2630279195762,\"3\"\n3.31356865670408,10.0086075949367,3.18232014768368,3.24794440219388,\"3\"\n3.29935596751853,10.3735443037975,3.16636580210459,3.23286088481156,\"3\"\n3.28541097682367,10.7384810126582,3.15014375803481,3.21777736742924,\"3\"\n3.27172043960887,11.103417721519,3.13366726048497,3.20269385004692,\"3\"\n3.25826930849013,11.4683544303797,3.11695135683907,3.1876103326646,\"3\"\n3.24504141735798,11.8332911392405,3.10001221320658,3.17252681528228,\"3\"\n3.2320201040464,12.1982278481013,3.08286649175352,3.15744329789996,\"3\"\n3.2191887357828,12.563164556962,3.06553082525248,3.14235978051764,\"3\"\n3.20653112147594,12.9281012658228,3.0480214047947,3.12727626313532,\"3\"\n3.19403181115193,13.2930379746835,3.03035368035406,3.112192745753,\"3\"\n3.18167629365898,13.6579746835443,3.01254216308238,3.09710922837068,\"3\"\n3.1694511094084,14.0229113924051,2.99460031256831,3.08202571098836,\"3\"\n3.15734389656375,14.3878481012658,2.97654049064832,3.06694219360604,\"3\"\n3.14534338811679,14.7527848101266,2.95837396433064,3.05185867622372,\"3\"\n3.13343937491503,15.1177215189873,2.94011094276776,3.03677515884139,\"3\"\n3.12162264681705,15.4826582278481,2.9217606361011,3.02169164145907,\"3\"\n3.10988492130587,15.8475949367089,2.90333132684764,3.00660812407675,\"3\"\n3.09821876638125,16.2125316455696,2.88483044700762,2.99152460669443,\"3\"\n3.08661752249077,16.5774683544304,2.86626465613345,2.97644108931211,\"3\"\n3.07507522665408,16.9424050632911,2.84763991720551,2.96135757192979,\"3\"\n3.06358654073431,17.3073417721519,2.82896156836064,2.94627405454747,\"3\"\n3.05214668494609,17.6722784810127,2.81023438938421,2.93119053716515,\"3\"\n3.04075137708791,18.0372151898734,2.79146266247775,2.91610701978283,\"3\"\n3.02939677758391,18.4021518987342,2.77265022721711,2.90102350240051,\"3\"\n3.01807944016295,18.7670886075949,2.75380052987343,2.88593998501819,\"3\"\n3.00679626784875,19.1320253164557,2.73491666742299,2.87085646763587,\"3\"\n2.99554447385262,19.4969620253165,2.71600142665447,2.85577295025355,\"3\"\n2.98432154692592,19.8618987341772,2.69705731881654,2.84068943287123,\"3\"\n2.97312522072641,20.226835443038,2.6780866102514,2.82560591548891,\"3\"\n2.96195344676906,20.5917721518987,2.65909134944411,2.81052239810659,\"3\"\n2.95080437055905,20.9567088607595,2.64007339088948,2.79543888072427,\"3\"\n2.9396763105382,21.3216455696203,2.62103441614569,2.78035536334195,\"3\"\n2.92856773951101,21.686582278481,2.60197595240824,2.76527184595962,\"3\"\n2.91747726825145,22.0515189873418,2.58289938890316,2.7501883285773,\"3\"\n2.90640363102507,22.4164556962025,2.56380599136489,2.73510481119498,\"3\"\n2.89534567279171,22.7813924050633,2.54469691483362,2.72002129381266,\"3\"\n2.88430233788248,23.1463291139241,2.52557321497821,2.70493777643034,\"3\"\n2.87327265996998,23.5112658227848,2.50643585812606,2.68985425904802,\"3\"\n2.86225575317315,23.8762025316456,2.48728573015826,2.6747707416657,\"3\"\n2.85125080415818,24.2411392405063,2.46812364440858,2.65968722428338,\"3\"\n2.8402570651147,24.6060759493671,2.44895034868742,2.64460370690106,\"3\"\n2.82927384750132,24.9710126582278,2.42976653153616,2.62952018951874,\"3\"\n2.8183005164686,25.3359493670886,2.41057282780424,2.61443667213642,\"3\"\n2.80733648587872,25.7008860759494,2.39136982362948,2.5993531547541,\"3\"\n2.79638121385174,26.0658227848101,2.37215806089182,2.58426963737178,\"3\"\n2.7854341987769,26.4307594936709,2.35293804120201,2.56918611998946,\"3\"\n2.77449497573522,26.7956962025316,2.33371022947905,2.55410260260714,\"3\"\n2.76356311328641,27.1606329113924,2.31447505716323,2.53901908522482,\"3\"\n2.75263821057874,27.5255696202532,2.29523292510625,2.5239355678425,\"3\"\n2.74171989474591,27.8905063291139,2.27598420617444,2.50885205046017,\"3\"\n2.73080781855887,28.2554430379747,2.25672924759684,2.49376853307785,\"3\"\n2.71990165830491,28.6203797468354,2.23746837308616,2.47868501569553,\"3\"\n2.70900111186925,28.9853164556962,2.21820188475717,2.46360149831321,\"3\"\n2.69810589699754,29.350253164557,2.19893006486425,2.44851798093089,\"3\"\n2.68721574972006,29.7151898734177,2.17965317737709,2.43343446354857,\"3\"\n2.67633042292078,30.0801265822785,2.16037146941173,2.41835094616625,\"3\"\n2.66544968503624,30.4450632911392,2.14108517253163,2.40326742878393,\"3\"\n2.65457331887098,30.81,2.12179450393224,2.38818391140161,\"3\"\n3.41143310305381,2.88,3.27890347816079,3.3451682906073,\"4\"\n3.39546657898107,3.27911392405063,3.2667523331594,3.33110945607023,\"4\"\n3.37952908767983,3.67822784810127,3.2545721553865,3.31705062153317,\"4\"\n3.36362332793488,4.0773417721519,3.24236024605732,3.3029917869961,\"4\"\n3.34775228893159,4.47645569620253,3.23011361598648,3.28893295245903,\"4\"\n3.33191927978013,4.87556962025316,3.2178289560638,3.27487411792197,\"4\"\n3.31612796006636,5.2746835443038,3.20550260670344,3.2608152833849,\"4\"\n3.30038237050706,5.67379746835443,3.19313052718861,3.24675644884783,\"4\"\n3.28468696233347,6.07291139240506,3.18070826628806,3.23269761431077,\"4\"\n3.2690466234465,6.4720253164557,3.1682309361009,3.2186387797737,\"4\"\n3.253466698673,6.87113924050633,3.15569319180027,3.20457994523663,\"4\"\n3.23795300061811,7.27025316455696,3.14308922078102,3.19052111069957,\"4\"\n3.22251180670118,7.66936708860759,3.13041274562382,3.1764622761625,\"4\"\n3.20714983708151,8.06848101265823,3.11765704616936,3.16240344162543,\"4\"\n3.19187420749638,8.46759493670886,3.10481500668036,3.14834460708837,\"4\"\n3.1766923508021,8.86670886075949,3.0918791943005,3.1342857725513,\"4\"\n3.16161190156493,9.26582278481013,3.07884197446354,3.12022693801423,\"4\"\n3.14664053976576,9.66493670886076,3.06569566718858,3.10616810347717,\"4\"\n3.13178579288143,10.0640506329114,3.05243274499877,3.0921092689401,\"4\"\n3.11705480040083,10.463164556962,3.03904606840524,3.07805043440303,\"4\"\n3.10245405095277,10.8622784810127,3.02552914877917,3.06399159986597,\"4\"\n3.08798910885018,11.2613924050633,3.01187642180762,3.0499327653289,\"4\"\n3.07366435262998,11.6605063291139,2.99808350895369,3.03587393079183,\"4\"\n3.05948275141263,12.0596202531646,2.9841474410969,3.02181509625477,\"4\"\n3.04544570413025,12.4587341772152,2.97006681930515,3.0077562617177,\"4\"\n3.03155296119533,12.8578481012658,2.95584189316594,2.99369742718063,\"4\"\n3.0178026385978,13.2569620253165,2.94147454668934,2.97963859264357,\"4\"\n3.00419132261179,13.6560759493671,2.92696819360121,2.9655797581065,\"4\"\n2.99071425189693,14.0551898734177,2.91232759524194,2.95152092356943,\"4\"\n2.97736555528679,14.4543037974684,2.89755862277794,2.93746208903237,\"4\"\n2.96413851949677,14.853417721519,2.88266798949383,2.9234032544953,\"4\"\n2.95102586153903,15.2525316455696,2.86766297837744,2.90934441995823,\"4\"\n2.93801998481623,15.6516455696203,2.8525511860261,2.89528558542117,\"4\"\n2.92511320406673,16.0507594936709,2.83734029770147,2.8812267508841,\"4\"\n2.91229793092865,16.4498734177215,2.82203790176542,2.86716791634703,\"4\"\n2.8995668176697,16.8489873417722,2.80665134595023,2.85310908180997,\"4\"\n2.88691286095185,17.2481012658228,2.79118763359396,2.8390502472729,\"4\"\n2.87432947022736,17.6472151898734,2.77565335524431,2.82499141273584,\"4\"\n2.86181050669223,18.046329113924,2.7600546497053,2.81093257819877,\"4\"\n2.8493502990024,18.4454430379747,2.744397188321,2.7968737436617,\"4\"\n2.83694364155904,18.8445569620253,2.72868617669023,2.78281490912464,\"4\"\n2.82458578040245,19.2436708860759,2.71292636877269,2.76875607458757,\"4\"\n2.81227239085089,19.6427848101266,2.69712208925011,2.7546972400505,\"4\"\n2.79999955012749,20.0418987341772,2.68127726089938,2.74063840551343,\"4\"\n2.78776370741698,20.4410126582278,2.66539543453575,2.72657957097637,\"4\"\n2.7755616531202,20.8401265822785,2.64947981975841,2.7125207364393,\"4\"\n2.7633904885328,21.2392405063291,2.63353331527167,2.69846190190224,\"4\"\n2.75124759675695,21.6383544303797,2.61755853797339,2.68440306736517,\"4\"\n2.73913061534139,22.0374683544304,2.60155785031481,2.6703442328281,\"4\"\n2.72703741091832,22.436582278481,2.58553338566375,2.65628539829104,\"4\"\n2.71496605594528,22.8356962025316,2.56948707156266,2.64222656375397,\"4\"\n2.70291480755163,23.2348101265823,2.55342065088217,2.6281677292169,\"4\"\n2.69088208841826,23.6339240506329,2.53733570094141,2.61410889467983,\"4\"\n2.67886646957576,24.0330379746835,2.52123365070978,2.60005006014277,\"4\"\n2.66686665498268,24.4321518987342,2.50511579622873,2.5859912256057,\"4\"\n2.654881467735,24.8312658227848,2.48898331440227,2.57193239106864,\"4\"\n2.64290983775661,25.2303797468354,2.47283727530653,2.55787355653157,\"4\"\n2.63095079082514,25.6294936708861,2.45667865316386,2.5438147219945,\"4\"\n2.6190034387954,26.0286075949367,2.44050833611947,2.52975588745744,\"4\"\n2.60706697089274,26.4277215189873,2.424327134948,2.51569705292037,\"4\"\n2.5951406459594,26.826835443038,2.4081357908072,2.5016382183833,\"4\"\n2.58322378554785,27.2259493670886,2.39193498214462,2.48757938384624,\"4\"\n2.57131576776584,27.6250632911392,2.3757253308525,2.47352054930917,\"4\"\n2.55941602178789,28.0241772151899,2.35950740775631,2.4594617147721,\"4\"\n2.54752402295737,28.4232911392405,2.3432817375127,2.44540288023504,\"4\"\n2.5356392884117,28.8224050632911,2.32704880298424,2.43134404569797,\"4\"\n2.52376137317103,29.2215189873418,2.31080904915078,2.4172852111609,\"4\"\n2.51188986663767,29.6206329113924,2.29456288661,2.40322637662384,\"4\"\n2.50002438945968,30.019746835443,2.27831069471386,2.38916754208677,\"4\"\n2.48816459071738,30.4188607594937,2.26205282438203,2.3751087075497,\"4\"\n2.47631014539682,30.8179746835443,2.24578960062845,2.36104987301264,\"4\"\n2.46446075211794,31.2170886075949,2.22952132483319,2.34699103847557,\"4\"\n2.45261613108938,31.6162025316456,2.21324827678762,2.3329322039385,\"4\"\n2.44077602226504,32.0153164556962,2.19697071653784,2.31887336940144,\"4\"\n2.4289401836804,32.4144303797468,2.18068888604834,2.30481453486437,\"4\"\n2.41710838994928,32.8135443037975,2.16440301070532,2.2907557003273,\"4\"\n2.40528043090377,33.2126582278481,2.1481133006767,2.27669686579024,\"4\"\n2.39345611036227,33.6117721518987,2.13181995214406,2.26263803125317,\"4\"\n2.38163524501221,34.0108860759494,2.11552314841999,2.2485791967161,\"4\"\n2.36981766339546,34.41,2.09922306096261,2.23452036217904,\"4\"\n3.61104833862113,1.73,3.46514065011583,3.53809449436848,\"5\"\n3.59516802425868,2.08696202531646,3.45317683292625,3.52417242859246,\"5\"\n3.57931545759572,2.44392405063291,3.44118526803718,3.51025036281645,\"5\"\n3.56349303131257,2.80088607594937,3.4291635627683,3.49632829704044,\"5\"\n3.54770337701316,3.15784810126582,3.41710908551568,3.48240623126442,\"5\"\n3.53194938790294,3.51481012658228,3.40501894307388,3.46848416548841,\"5\"\n3.51623424228446,3.87177215189873,3.39288995714034,3.4545620997124,\"5\"\n3.50056142730168,4.22873417721519,3.38071864057108,3.44064003393638,\"5\"\n3.48493476209784,4.58569620253165,3.36850117422289,3.42671796816037,\"5\"\n3.46935841921296,4.9426582278481,3.35623338555575,3.41279590238435,\"5\"\n3.45383694263158,5.29962025316456,3.34391073058511,3.39887383660834,\"5\"\n3.4383752603998,5.65658227848101,3.33152828126486,3.38495177083233,\"5\"\n3.42297868918034,6.01354430379747,3.31908072093229,3.37102970505631,\"5\"\n3.40765292754319,6.37050632911392,3.30656235101741,3.3571076392803,\"5\"\n3.39240403426596,6.72746835443038,3.29396711274262,3.34318557350429,\"5\"\n3.37723838755121,7.08443037974684,3.28128862790533,3.32926350772827,\"5\"\n3.36216262101058,7.44139240506329,3.26852026289394,3.31534144195226,\"5\"\n3.34718353270628,7.79835443037975,3.25565521964621,3.30141937617624,\"5\"\n3.33230796468589,8.1553164556962,3.24268665611458,3.28749731040023,\"5\"\n3.31754265246479,8.51227848101266,3.22960783678364,3.27357524462422,\"5\"\n3.30289404686723,8.86924050632912,3.21641231082918,3.2596531788482,\"5\"\n3.28836811439694,9.22620253164557,3.20309411174744,3.24573111307219,\"5\"\n3.27397012646883,9.58316455696202,3.18964796812352,3.23180904729618,\"5\"\n3.25970445169887,9.94012658227848,3.17606951134146,3.21788698152016,\"5\"\n3.24557436812459,10.2970886075949,3.16235546336371,3.20396491574415,\"5\"\n3.2315819128218,10.6540506329114,3.14850378711447,3.19004284996814,\"5\"\n3.21772778430305,11.0110126582278,3.1345137840812,3.17612078419212,\"5\"\n3.20401130831207,11.3679746835443,3.12038612852014,3.16219871841611,\"5\"\n3.19043047084787,11.7249367088608,3.10612283443231,3.14827665264009,\"5\"\n3.17698201471622,12.0818987341772,3.09172715901194,3.13435458686408,\"5\"\n3.16366158910013,12.4388607594937,3.077203453076,3.12043252108807,\"5\"\n3.15046393682607,12.7958227848101,3.06255697379803,3.10651045531205,\"5\"\n3.13738310187269,13.1527848101266,3.04779367719939,3.09258838953604,\"5\"\n3.12441264021885,13.509746835443,3.0329200073012,3.07866632376003,\"5\"\n3.11154581977299,13.8667088607595,3.01794269619504,3.06474425798401,\"5\"\n3.09877579897883,14.223670886076,3.00286858543716,3.050822192208,\"5\"\n3.08609577785449,14.5806329113924,2.98770447500947,3.03690012643198,\"5\"\n3.07349911899352,14.9375949367089,2.97245700231842,3.02297806065597,\"5\"\n3.06097943902917,15.2945569620253,2.95713255073074,3.00905599487996,\"5\"\n3.04853067309826,15.6515189873418,2.94173718510963,2.99513392910394,\"5\"\n3.03614711600033,16.0084810126582,2.92627661065552,2.98121186332793,\"5\"\n3.02382344419952,16.3654430379747,2.91075615090431,2.96728979755192,\"5\"\n3.01155472276598,16.7224050632911,2.89518074078583,2.9533677317759,\"5\"\n2.99933640099113,17.0793670886076,2.87955493100865,2.93944566599989,\"5\"\n2.98716429988904,17.436329113924,2.86388290055871,2.92552360022387,\"5\"\n2.97503459422539,17.7932911392405,2.84816847467034,2.91160153444786,\"5\"\n2.96294379116412,18.150253164557,2.83241514617957,2.89767946867185,\"5\"\n2.95088870712956,18.5072151898734,2.81662609866211,2.88375740289583,\"5\"\n2.93886644406426,18.8641772151899,2.80080423017538,2.86983533711982,\"5\"\n2.92687436592341,19.2211392405063,2.7849521767642,2.85591327134381,\"5\"\n2.91491007597872,19.5781012658228,2.76907233515686,2.84199120556779,\"5\"\n2.90297139529977,19.9350632911392,2.75316688428379,2.82806913979178,\"5\"\n2.89105634262801,20.2920253164557,2.73723780540351,2.81414707401576,\"5\"\n2.87916311574726,20.6489873417722,2.72128690073225,2.80022500823975,\"5\"\n2.86729007437597,21.0059493670886,2.7053158105515,2.78630294246374,\"5\"\n2.85543572455335,21.3629113924051,2.6893260288221,2.77238087668772,\"5\"\n2.84359870445582,21.7198734177215,2.6733189173676,2.75845881091171,\"5\"\n2.8317777715598,22.076835443038,2.65729571871159,2.7445367451357,\"5\"\n2.81997179105465,22.4337974683544,2.64125756766471,2.73061467935968,\"5\"\n2.80817972540557,22.7907594936709,2.62520550176177,2.71669261358367,\"5\"\n2.79640062496594,23.1477215189873,2.60914047064937,2.70277054780765,\"5\"\n2.78463361954226,23.5046835443038,2.59306334452102,2.68884848203164,\"5\"\n2.77287791081951,23.8616455696203,2.57697492169174,2.67492641625563,\"5\"\n2.76113276556134,24.2186075949367,2.56087593539789,2.66100435047961,\"5\"\n2.749397509506,24.5755696202532,2.5447670599012,2.6470822847036,\"5\"\n2.73767152188598,24.9325316455696,2.52864891596919,2.63316021892759,\"5\"\n2.72595423050576,25.2894936708861,2.51252207579739,2.61923815315157,\"5\"\n2.71424510731871,25.6464556962025,2.4963870674324,2.60531608737556,\"5\"\n2.70254366445006,26.003417721519,2.48024437874903,2.59139402159954,\"5\"\n2.69084945061825,26.3603797468354,2.46409446102881,2.57747195582353,\"5\"\n2.67916204791235,26.7173417721519,2.44793773218268,2.56354989004752,\"5\"\n2.66748106888746,27.0743037974684,2.43177457965554,2.5496278242715,\"5\"\n2.65580615394431,27.4312658227848,2.41560536304667,2.53570575849549,\"5\"\n2.644136968963,27.7882278481013,2.39943041647595,2.52178369271948,\"5\"\n2.63247320316412,28.1451898734177,2.38325005072281,2.50786162694346,\"5\"\n2.62081456717326,28.5021518987342,2.36706455516164,2.49393956116745,\"5\"\n2.609160791268,28.8591139240506,2.35087419951487,2.48001749539143,\"5\"\n2.5975116237882,29.2160759493671,2.33467923544264,2.46609542961542,\"5\"\n2.58586682969306,29.5730379746835,2.31847989798575,2.45217336383941,\"5\"\n2.57422618924987,29.93,2.30227640687691,2.43825129806339,\"5\"\n3.23548749796158,5.19,3.07159173616988,3.15353961706573,\"6\"\n3.23038076589065,5.39189873417722,3.07011297723326,3.15024687156195,\"6\"\n3.22528604226717,5.59379746835443,3.06862220984917,3.14695412605817,\"6\"\n3.22020417520807,5.79569620253165,3.06711858590071,3.14366138055439,\"6\"\n3.21513608765899,5.99759493670886,3.06560118244223,3.14036863505061,\"6\"\n3.21008278487479,6.19949367088608,3.06406899421887,3.13707588954683,\"6\"\n3.20504536265178,6.40139240506329,3.06252092543432,3.13378314404305,\"6\"\n3.2000250163665,6.60329113924051,3.06095578071204,3.13049039853927,\"6\"\n3.19502305087029,6.80518987341772,3.05937225520069,3.12719765303549,\"6\"\n3.19004089127921,7.00708860759494,3.05776892378422,3.12390490753171,\"6\"\n3.18508009468269,7.20898734177215,3.05614422937317,3.12061216202793,\"6\"\n3.18014236277023,7.41088607594937,3.05449647027807,3.11731941652415,\"6\"\n3.17522955533988,7.61278481012658,3.05282378670086,3.11402667102037,\"6\"\n3.17034370460313,7.8146835443038,3.05112414643005,3.11073392551659,\"6\"\n3.16548703013317,8.01658227848101,3.04939532989246,3.10744118001281,\"6\"\n3.16066195421338,8.21848101265823,3.04763491480469,3.10414843450903,\"6\"\n3.15587111722572,8.42037974683544,3.04584026078478,3.10085568900525,\"6\"\n3.15111739256894,8.62227848101266,3.04400849443401,3.09756294350147,\"6\"\n3.1464039004107,8.82417721518987,3.04213649558469,3.09427019799769,\"6\"\n3.14173401935363,9.02607594936709,3.04022088563419,3.09097745249391,\"6\"\n3.13711139483397,9.2279746835443,3.03825801914629,3.08768470699013,\"6\"\n3.13253994278121,9.42987341772152,3.0362439801915,3.08439196148635,\"6\"\n3.12802384676489,9.63177215189873,3.03417458520026,3.08109921598257,\"6\"\n3.12356754657052,9.83367088607595,3.03204539438706,3.07780647047879,\"6\"\n3.11917571592727,10.0355696202532,3.02985173402276,3.07451372497501,\"6\"\n3.11485322702159,10.2374683544304,3.02758873192087,3.07122097947123,\"6\"\n3.11060509955646,10.4393670886076,3.02525136837845,3.06792823396745,\"6\"\n3.10643643254915,10.6412658227848,3.02283454437819,3.06463548846367,\"6\"\n3.10235231789189,10.843164556962,3.0203331680279,3.06134274295989,\"6\"\n3.0983577359873,11.0450632911392,3.01774225892493,3.05804999745611,\"6\"\n3.09445743551139,11.2469620253165,3.01505706839327,3.05475725195233,\"6\"\n3.09065580144474,11.4488607594937,3.01227321145237,3.05146450644855,\"6\"\n3.08695671772008,11.6507594936709,3.00938680416947,3.04817176094477,\"6\"\n3.08336343281188,11.8526582278481,3.00639459807011,3.04487901544099,\"6\"\n3.07987843791476,12.0545569620253,3.00329410195967,3.04158626993721,\"6\"\n3.07650336761807,12.2564556962025,3.0000836812488,3.03829352443343,\"6\"\n3.07323893191457,12.4583544303797,2.99676262594473,3.03500077892965,\"6\"\n3.07008488596659,12.660253164557,2.99333118088516,3.03170803342587,\"6\"\n3.06704004058013,12.8621518987342,2.98979053526406,3.02841528792209,\"6\"\n3.06410231235847,13.0640506329114,2.98614277247816,3.02512254241831,\"6\"\n3.06126880870502,13.2659493670886,2.98239078512405,3.02182979691453,\"6\"\n3.05853593985984,13.4678481012658,2.97853816296167,3.01853705141075,\"6\"\n3.0558995484108,13.669746835443,2.97458906340315,3.01524430590697,\"6\"\n3.05335504635132,13.8716455696203,2.97054807445507,3.01195156040319,\"6\"\n3.05089755060491,14.0735443037975,2.96642007919392,3.00865881489941,\"6\"\n3.04852200964805,14.2754430379747,2.96221012914322,3.00536606939563,\"6\"\n3.04622331600818,14.4773417721519,2.95792333177553,3.00207332389185,\"6\"\n3.04399640159635,14.6792405063291,2.9535647551798,2.99878057838807,\"6\"\n3.04183631476518,14.8811392405063,2.94913935100341,2.99548783288429,\"6\"\n3.0397382794973,15.0830379746835,2.94465189526373,2.99219508738051,\"6\"\n3.03769773817981,15.2849367088608,2.94010694557366,2.98890234187673,\"6\"\n3.03571038003772,15.486835443038,2.93550881270819,2.98560959637295,\"6\"\n3.03377215756347,15.6887341772152,2.93086154417488,2.98231685086918,\"6\"\n3.03187929328402,15.8906329113924,2.92616891744677,2.97902410536539,\"6\"\n3.03002827904266,16.0925316455696,2.92143444068057,2.97573135986161,\"6\"\n3.0282158697127,16.2944303797468,2.91666135900297,2.97243861435784,\"6\"\n3.02643907296258,16.4963291139241,2.91185266474553,2.96914586885406,\"6\"\n3.0246951363924,16.6982278481013,2.90701111030815,2.96585312335028,\"6\"\n3.02298153308543,16.9001265822785,2.90213922260756,2.9625603778465,\"6\"\n3.02129594637507,17.1020253164557,2.89723931831037,2.95926763234272,\"6\"\n3.01963625442332,17.3039240506329,2.89231351925455,2.95597488683894,\"6\"\n3.01800051504012,17.5058227848101,2.88736376763019,2.95268214133516,\"6\"\n3.01638695104005,17.7077215189873,2.8823918406227,2.94938939583138,\"6\"\n3.01479393633036,17.9096202531646,2.87739936432483,2.9460966503276,\"6\"\n3.0132199828464,18.1115189873418,2.87238782680123,2.94280390482382,\"6\"\n3.01166372839245,18.313417721519,2.86735859024762,2.93951115932004,\"6\"\n3.01012392540468,18.5153164556962,2.86231290222783,2.93621841381626,\"6\"\n3.00859943062329,18.7172151898734,2.85725190600167,2.93292566831248,\"6\"\n3.00708919564137,18.9191139240506,2.85217664997603,2.9296329228087,\"6\"\n3.00559225828526,19.1210126582278,2.84708809632457,2.92634017730492,\"6\"\n3.00410773477386,19.3229113924051,2.84198712882841,2.92304743180114,\"6\"\n3.00263481260081,19.5248101265823,2.83687455999391,2.91975468629736,\"6\"\n3.00117274408248,19.7267088607595,2.83175113750468,2.91646194079358,\"6\"\n2.99972084051596,19.9286075949367,2.82661755006363,2.9131691952898,\"6\"\n2.99827846689304,20.1305063291139,2.82147443267899,2.90987644978602,\"6\"\n2.99684503711948,20.3324050632911,2.816322371445,2.90658370428224,\"6\"\n2.9954200096921,20.5343037974684,2.81116190786481,2.90329095877846,\"6\"\n2.99400288378989,20.7362025316456,2.80599354275947,2.89999821327468,\"6\"\n2.9925931957387,20.9381012658228,2.8008177398031,2.8967054677709,\"6\"\n2.99119051581304,21.14,2.79563492872119,2.89341272226712,\"6\"\n3.61381300445658,3.53,3.34374054152168,3.47877677298913,\"7\"\n3.60169374391287,3.71898734177215,3.33848482174087,3.47008928282687,\"7\"\n3.5896371778878,3.9079746835443,3.33316640744144,3.46140179266462,\"7\"\n3.57764837850139,4.09696202531646,3.32778022650333,3.45271430250236,\"7\"\n3.56573286044338,4.28594936708861,3.32232076423683,3.44402681234011,\"7\"\n3.55389660968434,4.47493670886076,3.31678203467136,3.43533932217785,\"7\"\n3.54214610968193,4.66392405063291,3.31115755434926,3.4266518320156,\"7\"\n3.53048836330313,4.85291139240506,3.30544032040355,3.41796434185334,\"7\"\n3.51893090820334,5.04189873417721,3.29962279517883,3.40927685169108,\"7\"\n3.50748182287547,5.23088607594937,3.29369690018219,3.40058936152883,\"7\"\n3.49614972003832,5.41987341772152,3.28765402269483,3.39190187136657,\"7\"\n3.4849437235271,5.60886075949367,3.28148503888154,3.38321438120432,\"7\"\n3.473873424464,5.79784810126582,3.27518035762013,3.37452689104206,\"7\"\n3.46294881233555,5.98683544303798,3.26872998942407,3.36583940087981,\"7\"\n3.45218017683031,6.17582278481013,3.2621236446048,3.35715191071755,\"7\"\n3.44157797705652,6.36481012658228,3.25535086405408,3.3484644205553,\"7\"\n3.43115267621768,6.55379746835443,3.2484011845684,3.33977693039304,\"7\"\n3.42091454208158,6.74278481012658,3.24126433838,3.33108944023079,\"7\"\n3.4108734166371,6.93177215189873,3.23393048349996,3.32240195006853,\"7\"\n3.40103846203557,7.12075949367089,3.22639045777699,3.31371445990628,\"7\"\n3.39141789389553,7.30974683544304,3.21863604559252,3.30502696974402,\"7\"\n3.38201871674235,7.49873417721519,3.21066024242119,3.29633947958177,\"7\"\n3.37284647904108,7.68772151898734,3.20245749979794,3.28765198941951,\"7\"\n3.36390506624462,7.87670886075949,3.1940239322699,3.27896449925726,\"7\"\n3.35519654898531,8.06569620253165,3.18535746920469,3.270277009095,\"7\"\n3.34672109983562,8.2546835443038,3.17645793802987,3.26158951893275,\"7\"\n3.33847698629695,8.44367088607595,3.16732707124403,3.25290202877049,\"7\"\n3.33046064067136,8.6326582278481,3.15796843654512,3.24421453860824,\"7\"\n3.32266680035808,8.82164556962025,3.14838729653388,3.23552704844598,\"7\"\n3.31508870605476,9.01063291139241,3.13859041051269,3.22683955828373,\"7\"\n3.30771834122759,9.19962025316456,3.12858579501535,3.21815206812147,\"7\"\n3.30054669448446,9.38860759493671,3.11838246143397,3.20946457795921,\"7\"\n3.29356402707243,9.57759493670886,3.10799014852149,3.20077708779696,\"7\"\n3.28676013014771,9.76658227848101,3.0974190651217,3.1920895976347,\"7\"\n3.28012456003272,9.95556962025316,3.08667965491218,3.18340210747245,\"7\"\n3.27364684365249,10.1445569620253,3.07578239096789,3.17471461731019,\"7\"\n3.26731665012839,10.3335443037975,3.06473760416748,3.16602712714794,\"7\"\n3.26112392769948,10.5225316455696,3.05355534627189,3.15733963698568,\"7\"\n3.25505900754733,10.7115189873418,3.04224528609953,3.14865214682343,\"7\"\n3.2491126776971,10.9005063291139,3.03081663562525,3.13996465666117,\"7\"\n3.24327623104767,11.0894936708861,3.01927810195017,3.13127716649892,\"7\"\n3.23754149189572,11.2784810126582,3.0076378607776,3.12258967633666,\"7\"\n3.23190082522438,11.4674683544304,2.99590354712443,3.11390218617441,\"7\"\n3.22634713267452,11.6564556962025,2.98408225934979,3.10521469601215,\"7\"\n3.22087383862566,11.8454430379747,2.97218057307413,3.0965272058499,\"7\"\n3.21547486927149,12.0344303797468,2.9602045621038,3.08783971568764,\"7\"\n3.21014462704115,12.223417721519,2.94815982400962,3.07915222552539,\"7\"\n3.20487796222773,12.4124050632911,2.93605150849853,3.07046473536313,\"7\"\n3.1996701432567,12.6013924050633,2.92388434714505,3.06177724520088,\"7\"\n3.19451682666605,12.7903797468354,2.9116626834112,3.05308975503862,\"7\"\n3.18941402757435,12.9793670886076,2.89939050217839,3.04440226487637,\"7\"\n3.1843580911775,13.1683544303797,2.88707145825072,3.03571477471411,\"7\"\n3.17934566563073,13.3573417721519,2.87470890347298,3.02702728455186,\"7\"\n3.17437367653234,13.5463291139241,2.86230591224686,3.0183397943896,\"7\"\n3.16943930312079,13.7353164556962,2.8498653053339,3.00965230422734,\"7\"\n3.16453995622002,13.9243037974684,2.83738967191016,3.00096481406509,\"7\"\n3.15967325791388,14.1132911392405,2.82488138989179,2.99227732390283,\"7\"\n3.15483702289313,14.3022784810127,2.81234264458803,2.98358983374058,\"7\"\n3.15002924139434,14.4912658227848,2.79977544576231,2.97490234357832,\"7\"\n3.14524806363547,14.680253164557,2.78718164319667,2.96621485341607,\"7\"\n3.14049178564533,14.8692405063291,2.77456294086229,2.95752736325381,\"7\"\n3.13575883638168,15.0582278481013,2.76192090980144,2.94883987309156,\"7\"\n3.13104776603386,15.2472151898734,2.74925699982474,2.9401523829293,\"7\"\n3.12635723540937,15.4362025316456,2.73657255012472,2.93146489276705,\"7\"\n3.12168600630856,15.6251898734177,2.72386879890102,2.92277740260479,\"7\"\n3.11703293279787,15.8141772151899,2.7111468920872,2.91408991244254,\"7\"\n3.11239695329813,16.003164556962,2.69840789126243,2.90540242228028,\"7\"\n3.10777708341107,16.1921518987342,2.68565278082498,2.89671493211803,\"7\"\n3.10317240941351,16.3811392405063,2.67288247449803,2.88802744195577,\"7\"\n3.09858208235497,16.5701265822785,2.66009782123206,2.87933995179352,\"7\"\n3.09400531270027,16.7591139240506,2.64729961056225,2.87065246163126,\"7\"\n3.08944136546402,16.9481012658228,2.63448857747399,2.86196497146901,\"7\"\n3.08488955578926,17.1370886075949,2.62166540682424,2.85327748130675,\"7\"\n3.08034924492682,17.3260759493671,2.60883073736217,2.8445899911445,\"7\"\n3.0758198365766,17.5150632911392,2.59598516538788,2.83590250098224,\"7\"\n3.07130077355552,17.7040506329114,2.58312924808444,2.82721501081998,\"7\"\n3.06679153476061,17.8930379746835,2.57026350655485,2.81852752065773,\"7\"\n3.0622916323988,18.0820253164557,2.55738842859215,2.80984003049547,\"7\"\n3.0578006094579,18.2710126582278,2.54450447120854,2.80115254033322,\"7\"\n3.05331803739575,18.46,2.53161206294618,2.79246505017096,\"7\"\n3.80477748655657,2.47,3.5171261537263,3.66095182014144,\"8\"\n3.78798306410018,2.71037974683544,3.50810214021646,3.64804260215832,\"8\"\n3.77125902018529,2.95075949367089,3.49900774816511,3.6351333841752,\"8\"\n3.75461143896295,3.19113924050633,3.48983689342122,3.62222416619208,\"8\"\n3.73804698200852,3.43151898734177,3.48058291440941,3.60931494820897,\"8\"\n3.72157293193036,3.67189873417722,3.47123852852133,3.59640573022585,\"8\"\n3.70519723352578,3.91227848101266,3.46179579095968,3.58349651224273,\"8\"\n3.68892852993424,4.1526582278481,3.45224605858499,3.57058729425961,\"8\"\n3.67277619043605,4.39303797468354,3.44257996211694,3.5576780762765,\"8\"\n3.65675032563097,4.63341772151899,3.43278739095579,3.54476885829338,\"8\"\n3.64086178475238,4.87379746835443,3.42285749586814,3.53185964031026,\"8\"\n3.62512212891976,5.11417721518987,3.41277871573453,3.51895042232714,\"8\"\n3.60954357335316,5.35455696202532,3.40253883533489,3.50604120434402,\"8\"\n3.5941388911849,5.59493670886076,3.39212508153691,3.49313198636091,\"8\"\n3.57892127178919,5.8353164556962,3.38152426496639,3.48022276837779,\"8\"\n3.56390412783929,6.07569620253165,3.37072297295006,3.46731355039467,\"8\"\n3.54910084791463,6.31607594936709,3.35970781690848,3.45440433241155,\"8\"\n3.53452449563497,6.55645569620253,3.34846573322191,3.44149511442844,\"8\"\n3.52018746198228,6.79683544303798,3.33698433090835,3.42858589644532,\"8\"\n3.50610108430422,7.03721518987342,3.32525227262018,3.4156766784622,\"8\"\n3.49227525263623,7.27759493670886,3.31325966832193,3.40276746047908,\"8\"\n3.47871803016013,7.5179746835443,3.30099845483181,3.38985824249597,\"8\"\n3.46543531832437,7.75835443037975,3.28846273070133,3.37694902451285,\"8\"\n3.45243059701824,7.99873417721519,3.27564901604122,3.36403980652973,\"8\"\n3.43970476545938,8.23911392405063,3.26255641163384,3.35113058854661,\"8\"\n3.42725610036188,8.47949367088608,3.24918664076511,3.3382213705635,\"8\"\n3.41508033584525,8.71987341772152,3.2355439693155,3.32531215258038,\"8\"\n3.40317085660161,8.96025316455696,3.22163501259291,3.31240293459726,\"8\"\n3.39151898447686,9.20063291139241,3.20746844875143,3.29949371661414,\"8\"\n3.38011433080947,9.44101265822785,3.19305466645258,3.28658449863103,\"8\"\n3.36894518361499,9.68139240506329,3.17840537768082,3.27367528064791,\"8\"\n3.3579988999025,9.92177215189873,3.16353322542709,3.26076606266479,\"8\"\n3.34726227806002,10.1621518987342,3.14845141130333,3.24785684468167,\"8\"\n3.33672189188817,10.4025316455696,3.13317336150894,3.23494762669855,\"8\"\n3.32636437502556,10.6429113924051,3.11771244240531,3.22203840871544,\"8\"\n3.31617665104831,10.8832911392405,3.10208173041633,3.20912919073232,\"8\"\n3.30614610974843,11.123670886076,3.08629383574998,3.1962199727492,\"8\"\n3.29626073375213,11.3640506329114,3.07036077578004,3.18331075476608,\"8\"\n3.28650918180356,11.6044303797468,3.05429389176237,3.17040153678297,\"8\"\n3.27688083597557,11.8448101265823,3.03810380162413,3.15749231879985,\"8\"\n3.26736582010781,12.0851898734177,3.02180038152565,3.14458310081673,\"8\"\n3.25795499623349,12.3255696202532,3.00539276943374,3.13167388283361,\"8\"\n3.24863994490186,12.5659493670886,2.98888938479914,3.1187646648505,\"8\"\n3.23941293432964,12.8063291139241,2.97229795940511,3.10585544686738,\"8\"\n3.23026688234889,13.0467088607595,2.95562557541963,3.09294622888426,\"8\"\n3.22119531423672,13.2870886075949,2.93887870756556,3.08003701090114,\"8\"\n3.21219231875089,13.5274683544304,2.92206326708517,3.06712779291803,\"8\"\n3.20325250406365,13.7678481012658,2.90518464580616,3.05421857493491,\"8\"\n3.19437095478057,14.0082278481013,2.88824775912302,3.04130935695179,\"8\"\n3.18554319083591,14.2486075949367,2.87125708710143,3.02840013896867,\"8\"\n3.176765128757,14.4889873417722,2.85421671321411,3.01549092098556,\"8\"\n3.16803304556687,14.7293670886076,2.837130360438,3.00258170300244,\"8\"\n3.1593435454343,14.969746835443,2.82000142460434,2.98967248501932,\"8\"\n3.15069352906735,15.2101265822785,2.80283300500506,2.9767632670362,\"8\"\n3.14208016577005,15.4505063291139,2.78562793233612,2.96385404905308,\"8\"\n3.13350086803314,15.6908860759494,2.7683887941068,2.95094483106997,\"8\"\n3.12495326850039,15.9312658227848,2.75111795767331,2.93803561308685,\"8\"\n3.11643519913742,16.1716455696203,2.73381759107004,2.92512639510373,\"8\"\n3.10794467242521,16.4120253164557,2.71648968181602,2.91221717712061,\"8\"\n3.09947986440276,16.6524050632911,2.69913605387223,2.8993079591375,\"8\"\n3.09103909938987,16.8927848101266,2.68175838291889,2.88639874115438,\"8\"\n3.08262083623049,17.133164556962,2.66435821011203,2.87348952317126,\"8\"\n3.07422365590825,17.3735443037975,2.64693695446804,2.86058030518814,\"8\"\n3.06584625039703,17.6139240506329,2.62949592401302,2.84767108720503,\"8\"\n3.05748741262164,17.8543037974684,2.61203632582218,2.83476186922191,\"8\"\n3.04914602741474,18.0946835443038,2.59455927506284,2.82185265123879,\"8\"\n3.04082106336717,18.3350632911392,2.57706580314418,2.80894343325567,\"8\"\n3.03251156547906,18.5754430379747,2.55955686506605,2.79603421527256,\"8\"\n3.02421664852848,18.8158227848101,2.5420333460504,2.78312499728944,\"8\"\n3.01593549108286,19.0562025316456,2.52449606752978,2.77021577930632,\"8\"\n3.00766733008667,19.296582278481,2.50694579255974,2.7573065613232,\"8\"\n2.9994114559653,19.5369620253165,2.48938323071487,2.74439734334009,\"8\"\n2.99116720819216,19.7773417721519,2.47180904252177,2.73148812535697,\"8\"\n2.98293397127113,20.0177215189873,2.45422384347657,2.71857890737385,\"8\"\n2.97471117109206,20.2581012658228,2.4366282076894,2.70566968939073,\"8\"\n2.96649827162137,20.4984810126582,2.41902267119386,2.69276047140761,\"8\"\n2.95829477189385,20.7388607594937,2.40140773495514,2.6798512534245,\"8\"\n2.95010020327559,20.9792405063291,2.38378386760717,2.66694203544138,\"8\"\n2.94191412697099,21.2196202531646,2.36615150794553,2.65403281745826,\"8\"\n2.93373613174984,21.46,2.34851106720045,2.64112359947514,\"8\"\n3.58145445242358,2.96,3.32914380123721,3.45529912683039,\"24\"\n3.55686760588441,3.40316455696203,3.3105331164654,3.43370036117491,\"24\"\n3.53229554878792,3.84632911392405,3.29190764225092,3.41210159551942,\"24\"\n3.50773940638534,4.28949367088608,3.27326625334251,3.39050282986393,\"24\"\n3.48320041404423,4.7326582278481,3.25460771437265,3.36890406420844,\"24\"\n3.45867993002835,5.17582278481013,3.23593066707755,3.34730529855295,\"24\"\n3.43417944991341,5.61898734177215,3.21723361588151,3.32570653289746,\"24\"\n3.40970062284683,6.06215189873418,3.19851491163711,3.30410776724197,\"24\"\n3.38524526987963,6.5053164556962,3.17977273329334,3.28250900158649,\"24\"\n3.36081540461603,6.94848101265823,3.16100506724596,3.260910235931,\"24\"\n3.33641325644003,7.39164556962025,3.14220968411098,3.23931147027551,\"24\"\n3.3120412965838,7.83481012658228,3.12338411265624,3.21771270462002,\"24\"\n3.28770226729609,8.2779746835443,3.10452561063297,3.19611393896453,\"24\"\n3.26339921434253,8.72113924050633,3.08563113227556,3.17451517330904,\"24\"\n3.2391355230137,9.16430379746835,3.06669729229341,3.15291640765355,\"24\"\n3.21491495771874,9.60746835443038,3.04772032627739,3.13131764199807,\"24\"\n3.19074170508295,10.0506329113924,3.0286960476022,3.10971887634258,\"24\"\n3.16662042022566,10.4937974683544,3.00961980114851,3.08812011068709,\"24\"\n3.14255627553979,10.9369620253165,2.99048641452341,3.0665213450316,\"24\"\n3.1185550107925,11.3801265822785,2.97129014795972,3.04492257937611,\"24\"\n3.09462298268048,11.8232911392405,2.95202464476076,3.02332381372062,\"24\"\n3.07076721106439,12.2664556962025,2.93268288506588,3.00172504806513,\"24\"\n3.04699541795091,12.7096202531646,2.91325714686837,2.98012628240964,\"24\"\n3.02331605388558,13.1527848101266,2.89373897962273,2.95852751675416,\"24\"\n2.99973830481779,13.5959493670886,2.87411919737955,2.93692875109867,\"24\"\n2.97627207083851,14.0391139240506,2.85438790004784,2.91532998544318,\"24\"\n2.95292790673419,14.4822784810127,2.83453453284119,2.89373121978769,\"24\"\n2.92971691347431,14.9254430379747,2.81454799479009,2.8721324541322,\"24\"\n2.90665057015395,15.3686075949367,2.79441680679948,2.85053368847671,\"24\"\n2.88374049826572,15.8117721518987,2.77412934737673,2.82893492282122,\"24\"\n2.86099815516805,16.2549367088608,2.75367415916342,2.80733615716574,\"24\"\n2.83843446162396,16.6981012658228,2.73304032139653,2.78573739151025,\"24\"\n2.81605937899559,17.1412658227848,2.71221787271392,2.76413862585476,\"24\"\n2.79388146373895,17.5844303797468,2.69119825665959,2.74253986019927,\"24\"\n2.77190743775684,18.0275949367089,2.66997475133072,2.72094109454378,\"24\"\n2.7501418196945,18.4707594936709,2.64854283808209,2.69934232888829,\"24\"\n2.72858666139764,18.9139240506329,2.62690046506796,2.6777435632328,\"24\"\n2.70724142403153,19.3570886075949,2.6050481711231,2.65614479757731,\"24\"\n2.68610301086916,19.800253164557,2.58298905297449,2.63454603192183,\"24\"\n2.66516595219889,20.243417721519,2.56072858033379,2.61294726626634,\"24\"\n2.64442271734592,20.686582278481,2.53827428387578,2.59134850061085,\"24\"\n2.6238641142157,21.129746835443,2.51563535569502,2.56974973495536,\"24\"\n2.60347973074086,21.5729113924051,2.49282220785888,2.54815096929987,\"24\"\n2.58325837510419,22.0160759493671,2.46984603218458,2.52655220364438,\"24\"\n2.56318848035889,22.4592405063291,2.4467183956189,2.50495343798889,\"24\"\n2.54325845082203,22.9024050632911,2.42345089384478,2.48335467233341,\"24\"\n2.52345693935781,23.3455696202532,2.40005487399802,2.46175590667792,\"24\"\n2.50377305435776,23.7887341772152,2.3765412276871,2.44015714102243,\"24\"\n2.48419650197323,24.2318987341772,2.35292024876065,2.41855837536694,\"24\"\n2.46471767298403,24.6750632911392,2.32920154643888,2.39695960971145,\"24\"\n2.44532768514293,25.1182278481013,2.30539400296899,2.37536084405596,\"24\"\n2.42601839164784,25.5613924050633,2.28150576515311,2.35376207840047,\"24\"\n2.40678236524288,26.0045569620253,2.25754426024709,2.33216331274499,\"24\"\n2.387612865867,26.4477215189873,2.23351622831199,2.3105645470895,\"24\"\n2.36850379810759,26.8908860759494,2.20942776476043,2.28896578143401,\"24\"\n2.34944966318628,27.3340506329114,2.18528436837076,2.26736701577852,\"24\"\n2.33044550889856,27.7772151898734,2.1610909913475,2.24576825012303,\"24\"\n2.31148687987752,28.2203797468354,2.13685208905757,2.22416948446754,\"24\"\n2.29256976973926,28.6635443037975,2.11257166788485,2.20257071881205,\"24\"\n2.27369057606257,29.1067088607595,2.08825333025056,2.18097195315656,\"24\"\n2.25484605871875,29.5498734177215,2.06390031628341,2.15937318750108,\"24\"\n2.23603330176327,29.9930379746835,2.0395155419279,2.13777442184559,\"24\"\n2.21724967889614,30.4362025316456,2.01510163348405,2.1161756561901,\"24\"\n2.19849282236635,30.8793670886076,1.99066095870287,2.09457689053461,\"24\"\n2.17976059511701,31.3225316455696,1.96619565464123,2.07297812487912,\"24\"\n2.16105106592562,31.7656962025316,1.94170765252165,2.05137935922363,\"24\"\n2.14236248727632,32.2088607594937,1.91719869985997,2.02978059356814,\"24\"\n2.12369327570067,32.6520253164557,1.89267038012464,2.00818182791266,\"24\"\n2.10504199433247,33.0951898734177,1.86812413018187,1.98658306225717,\"24\"\n2.08640733743796,33.5383544303797,1.8435612557654,1.96498429660168,\"24\"\n2.06778811670151,33.9815189873418,1.81898294519087,1.94338553094619,\"24\"\n2.0491832490666,34.4246835443038,1.7943902815148,1.9217867652907,\"24\"\n2.03059174595213,34.8678481012658,1.76978425331829,1.90018799963521,\"24\"\n2.01201270368309,35.3110126582278,1.74516576427636,1.87858923397972,\"24\"\n1.99344529499245,35.7541772151899,1.72053564165602,1.85699046832424,\"24\"\n1.97488876146783,36.1973417721519,1.69589464386967,1.83539170266875,\"24\"\n1.95634240683096,36.6405063291139,1.67124346719556,1.81379293701326,\"24\"\n1.93780559095181,37.083670886076,1.64658275176372,1.79219417135777,\"24\"\n1.91927772451071,37.526835443038,1.62191308689386,1.77059540570228,\"24\"\n1.90075826423249,37.97,1.5972350158611,1.74899664004679,\"24\""
        },
        {
            "name": ".0/group_by1/model_prediction2",
            "source": ".0/group_by1/model_prediction2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.rad"
                    ]
                }
            ]
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"1\"\n\"2\"\n\"3\"\n\"4\"\n\"5\"\n\"6\"\n\"7\"\n\"8\"\n\"24\""
        },
        {
            "name": "scale/stroke",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"1\"\n\"2\"\n\"3\"\n\"4\"\n\"5\"\n\"6\"\n\"7\"\n\"8\"\n\"24\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-0.0820000000000003\n39.782"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n1.48149561638275\n4.0277624049065"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "stroke",
            "type": "ordinal",
            "domain": {
                "data": "scale/stroke",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "symbol",
            "properties": {
                "update": {
                    "size": {
                        "value": 50
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.lstat"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.log_medv"
                    },
                    "fill": {
                        "scale": "fill",
                        "field": "data.rad"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0"
                    }
                }
            },
            "from": {
                "data": ".0"
            }
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/model_prediction2"
            },
            "marks": [
                {
                    "type": "area",
                    "properties": {
                        "update": {
                            "fill": {
                                "value": "#333333"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.resp_upr_"
                            },
                            "fillOpacity": {
                                "value": 0.2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_lwr_"
                            },
                            "stroke": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/model_prediction2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/model_prediction2"
            },
            "marks": [
                {
                    "type": "line",
                    "properties": {
                        "update": {
                            "strokeWidth": {
                                "value": 2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_"
                            },
                            "stroke": {
                                "scale": "stroke",
                                "field": "data.rad"
                            },
                            "fill": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/model_prediction2"
                            }
                        }
                    }
                }
            ]
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "rad"
        },
        {
            "orient": "right",
            "stroke": "stroke",
            "title": "rad"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "lstat"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "log_medv"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id488841763").parseSpec(plot_id488841763_spec);
</script><!--/html_preserve-->
While we have some slight differences in the fits between `rad`
levels, most slopes are fairly similar and all show a fairly strong
negative linear relationship between the variables.

When `rad=6` we can see the slope mitigated, while the opposite occurs
when `rad=1`. As with other analyses for this dataset, these
differences should be tempered by the fact that these subsets have
sizes of $20$ and $24$, respectively. Accounting for an interaction
may slightly improve our model accuracy, but I'm not sure it'd be
worth the loss of simplicity.

b)  Plotting percent lower status (`lstat`) relative to the log of the median home (`log_medv`) with points coloured (filled) by the Charles River (`chas`). Add the linear fits and standard errors grouped by `chas`. Discuss the efficacy of the fits and whether or not there appears to be an interaction between `lstat` and `chas`.

```r
Boston %>% ggvis(~lstat, ~log_medv) %>%
    layer_points(fill = ~chas) %>%
    group_by(chas) %>%
    layer_model_predictions(model="lm", se=TRUE, stroke = ~chas)
```

```
## Guessing formula = log_medv ~ lstat
```

<!--html_preserve--><div id="plot_id348263735-container" class="ggvis-output-container">
<div id="plot_id348263735" class="ggvis-output"></div>
<div class="plot-gear-icon">
<nav class="ggvis-control">
<a class="ggvis-dropdown-toggle" title="Controls" onclick="return false;"></a>
<ul class="ggvis-dropdown">
<li>
Renderer: 
<a id="plot_id348263735_renderer_svg" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id348263735" data-renderer="svg">SVG</a>
 | 
<a id="plot_id348263735_renderer_canvas" class="ggvis-renderer-button" onclick="return false;" data-plot-id="plot_id348263735" data-renderer="canvas">Canvas</a>
</li>
<li>
<a id="plot_id348263735_download" class="ggvis-download" data-plot-id="plot_id348263735">Download</a>
</li>
</ul>
</nav>
</div>
</div>
<script type="text/javascript">
var plot_id348263735_spec = {
    "data": [
        {
            "name": ".0",
            "format": {
                "type": "csv",
                "parse": {
                    "lstat": "number",
                    "log_medv": "number"
                }
            },
            "values": "\"lstat\",\"log_medv\",\"chas\"\n4.98,3.17805383034795,\"Not on River\"\n9.14,3.07269331469012,\"Not on River\"\n4.03,3.54673968695281,\"Not on River\"\n2.94,3.50855589998265,\"Not on River\"\n5.33,3.58905911883173,\"Not on River\"\n5.21,3.35689712276558,\"Not on River\"\n12.43,3.13113691056019,\"Not on River\"\n19.15,3.29953372788566,\"Not on River\"\n29.93,2.80336038090653,\"Not on River\"\n17.1,2.9391619220656,\"Not on River\"\n20.45,2.70805020110221,\"Not on River\"\n13.27,2.9391619220656,\"Not on River\"\n15.71,3.07731226054641,\"Not on River\"\n8.26,3.01553490085017,\"Not on River\"\n10.26,2.90142159408275,\"Not on River\"\n8.47,2.99071973173045,\"Not on River\"\n6.58,3.13983261752775,\"Not on River\"\n14.67,2.86220088092947,\"Not on River\"\n11.69,3.00568260440716,\"Not on River\"\n11.28,2.90142159408275,\"Not on River\"\n21.02,2.61006979274201,\"Not on River\"\n13.83,2.97552956623647,\"Not on River\"\n18.72,2.72129542785223,\"Not on River\"\n19.88,2.67414864942653,\"Not on River\"\n16.3,2.74727091425549,\"Not on River\"\n16.51,2.63188884013665,\"Not on River\"\n14.81,2.8094026953625,\"Not on River\"\n17.28,2.69462718077007,\"Not on River\"\n12.8,2.91235066461494,\"Not on River\"\n11.98,3.04452243772342,\"Not on River\"\n22.6,2.54160199346455,\"Not on River\"\n13.04,2.67414864942653,\"Not on River\"\n27.71,2.58021682959233,\"Not on River\"\n18.35,2.57261223020711,\"Not on River\"\n20.34,2.60268968544438,\"Not on River\"\n9.68,2.9391619220656,\"Not on River\"\n11.41,2.99573227355399,\"Not on River\"\n8.77,3.04452243772342,\"Not on River\"\n10.13,3.20680324363393,\"Not on River\"\n4.32,3.42751468997953,\"Not on River\"\n1.98,3.55248682920838,\"Not on River\"\n4.84,3.28091121578765,\"Not on River\"\n5.81,3.23080439573347,\"Not on River\"\n7.44,3.20680324363393,\"Not on River\"\n9.55,3.05400118167797,\"Not on River\"\n10.21,2.96010509591084,\"Not on River\"\n14.15,2.99573227355399,\"Not on River\"\n18.8,2.8094026953625,\"Not on River\"\n30.81,2.66722820658195,\"Not on River\"\n16.2,2.96527306606928,\"Not on River\"\n13.45,2.98061863574394,\"Not on River\"\n9.43,3.02042488614436,\"Not on River\"\n5.28,3.2188758248682,\"Not on River\"\n8.43,3.15273602236366,\"Not on River\"\n14.8,2.9391619220656,\"Not on River\"\n4.81,3.56671182013973,\"Not on River\"\n5.77,3.20680324363393,\"Not on River\"\n3.95,3.45315712059287,\"Not on River\"\n6.86,3.14845336057165,\"Not on River\"\n9.22,2.97552956623647,\"Not on River\"\n13.15,2.92852352386054,\"Not on River\"\n14.44,2.77258872223978,\"Not on River\"\n6.73,3.10009228887823,\"Not on River\"\n9.5,3.2188758248682,\"Not on River\"\n8.05,3.49650756146648,\"Not on River\"\n4.67,3.15700042115011,\"Not on River\"\n10.24,2.96527306606928,\"Not on River\"\n8.1,3.09104245335832,\"Not on River\"\n13.09,2.85647020622048,\"Not on River\"\n8.79,3.03974915897077,\"Not on River\"\n6.72,3.18635263316264,\"Not on River\"\n9.88,3.07731226054641,\"Not on River\"\n5.52,3.1267605359604,\"Not on River\"\n7.54,3.15273602236366,\"Not on River\"\n6.78,3.18221184049661,\"Not on River\"\n8.94,3.06339092202781,\"Not on River\"\n11.97,2.99573227355399,\"Not on River\"\n10.27,3.03495298670727,\"Not on River\"\n12.34,3.05400118167797,\"Not on River\"\n9.1,3.01062088604774,\"Not on River\"\n5.29,3.3322045101752,\"Not on River\"\n7.22,3.17387845893747,\"Not on River\"\n6.72,3.21084365317094,\"Not on River\"\n7.51,3.13113691056019,\"Not on River\"\n9.62,3.17387845893747,\"Not on River\"\n6.53,3.28091121578765,\"Not on River\"\n12.86,3.11351530921037,\"Not on River\"\n8.44,3.10009228887823,\"Not on River\"\n5.5,3.16124671203156,\"Not on River\"\n5.7,3.35689712276558,\"Not on River\"\n8.81,3.11794990627824,\"Not on River\"\n8.2,3.09104245335832,\"Not on River\"\n8.16,3.13113691056019,\"Not on River\"\n6.21,3.2188758248682,\"Not on River\"\n10.59,3.02529107579554,\"Not on River\"\n6.65,3.34638914516716,\"Not on River\"\n11.34,3.06339092202781,\"Not on River\"\n4.21,3.65583960003574,\"Not on River\"\n3.57,3.7796338173824,\"Not on River\"\n6.19,3.50254987592244,\"Not on River\"\n9.42,3.31418600467253,\"Not on River\"\n7.67,3.27714473299218,\"Not on River\"\n10.63,2.92316158071916,\"Not on River\"\n13.44,2.96010509591084,\"Not on River\"\n12.33,3.00071981506503,\"Not on River\"\n16.47,2.9704144655697,\"Not on River\"\n18.66,2.9704144655697,\"Not on River\"\n14.09,3.01553490085017,\"Not on River\"\n12.27,2.98568193770049,\"Not on River\"\n15.55,2.96527306606928,\"Not on River\"\n13,3.07731226054641,\"Not on River\"\n10.16,3.1267605359604,\"Not on River\"\n16.21,2.9338568698359,\"Not on River\"\n17.09,2.92852352386054,\"Not on River\"\n10.45,2.91777073208428,\"Not on River\"\n15.76,2.90690105984738,\"Not on River\"\n12.04,3.05400118167797,\"Not on River\"\n10.3,2.95491027903374,\"Not on River\"\n15.37,3.01553490085017,\"Not on River\"\n13.61,2.96010509591084,\"Not on River\"\n14.37,3.09104245335832,\"Not on River\"\n14.27,3.01062088604774,\"Not on River\"\n17.93,3.02042488614436,\"Not on River\"\n25.41,2.85070650150373,\"Not on River\"\n17.58,2.9338568698359,\"Not on River\"\n14.81,3.06339092202781,\"Not on River\"\n27.26,2.75366071235426,\"Not on River\"\n17.19,2.78501124223834,\"Not on River\"\n15.39,2.89037175789616,\"Not on River\"\n18.34,2.66025953726586,\"Not on River\"\n12.6,2.95491027903374,\"Not on River\"\n12.26,2.97552956623647,\"Not on River\"\n11.12,3.13549421592915,\"Not on River\"\n15.03,2.91235066461494,\"Not on River\"\n17.31,2.74727091425549,\"Not on River\"\n16.96,2.89591193827178,\"Not on River\"\n16.9,2.85647020622048,\"Not on River\"\n14.59,2.83907846350861,\"Not on River\"\n21.32,2.58776403522771,\"Not on River\"\n18.46,2.87919845729804,\"Not on River\"\n24.16,2.63905732961526,\"Not on River\"\n34.41,2.66722820658195,\"Not on River\"\n26.82,2.59525470695687,\"On River\"\n26.42,2.74727091425549,\"Not on River\"\n29.29,2.46809953147162,\"Not on River\"\n27.8,2.62466859216316,\"Not on River\"\n16.65,2.74727091425549,\"Not on River\"\n29.53,2.68102152871429,\"Not on River\"\n28.32,2.87919845729804,\"Not on River\"\n21.45,2.73436750941958,\"Not on River\"\n14.1,3.06805293513362,\"Not on River\"\n13.28,2.97552956623647,\"Not on River\"\n12.12,2.72785282839839,\"On River\"\n15.79,2.96527306606928,\"Not on River\"\n15.12,2.83321334405622,\"On River\"\n15.02,2.74727091425549,\"On River\"\n16.14,2.57261223020711,\"Not on River\"\n4.59,3.72086249996699,\"Not on River\"\n6.43,3.1904763503465,\"Not on River\"\n7.39,3.14845336057165,\"Not on River\"\n5.5,3.29583686600433,\"On River\"\n1.73,3.91202300542815,\"Not on River\"\n1.92,3.91202300542815,\"On River\"\n3.32,3.91202300542815,\"On River\"\n11.64,3.12236492448736,\"Not on River\"\n9.81,3.2188758248682,\"Not on River\"\n3.7,3.91202300542815,\"Not on River\"\n12.14,3.16968558067743,\"Not on River\"\n11.1,3.16968558067743,\"Not on River\"\n11.32,3.10458667846607,\"Not on River\"\n14.43,2.85647020622048,\"Not on River\"\n12.03,2.94968833505258,\"Not on River\"\n14.69,3.13983261752775,\"Not on River\"\n9.04,3.16124671203156,\"Not on River\"\n9.64,3.11794990627824,\"Not on River\"\n5.33,3.38099467434464,\"Not on River\"\n10.11,3.14415227867226,\"Not on River\"\n6.29,3.20274644293832,\"Not on River\"\n6.92,3.39785848039664,\"Not on River\"\n5.04,3.6163087612791,\"Not on River\"\n7.56,3.68386691229039,\"Not on River\"\n9.45,3.58905911883173,\"Not on River\"\n4.82,3.63495111208838,\"Not on River\"\n5.68,3.48124008933569,\"Not on River\"\n13.98,3.27336401015227,\"Not on River\"\n13.15,3.38777436133001,\"Not on River\"\n4.45,3.91202300542815,\"Not on River\"\n6.68,3.46573590279973,\"Not on River\"\n4.56,3.39450839351136,\"Not on River\"\n5.39,3.55248682920838,\"Not on River\"\n5.1,3.61091791264422,\"Not on River\"\n4.69,3.41772668361337,\"Not on River\"\n2.87,3.5945687746427,\"Not on River\"\n5.03,3.43720781918519,\"Not on River\"\n4.38,3.37073817417745,\"Not on River\"\n2.97,3.91202300542815,\"Not on River\"\n4.08,3.5055573969864,\"Not on River\"\n8.61,3.41114771251532,\"Not on River\"\n6.62,3.54385368206368,\"Not on River\"\n4.56,3.55248682920838,\"Not on River\"\n4.45,3.49347265777133,\"Not on River\"\n7.43,3.18221184049661,\"Not on River\"\n3.11,3.74478708605223,\"Not on River\"\n3.81,3.88156379794344,\"Not on River\"\n2.88,3.91202300542815,\"Not on River\"\n10.87,3.11794990627824,\"Not on River\"\n10.97,3.19458313229916,\"Not on River\"\n18.06,3.11351530921037,\"Not on River\"\n14.66,3.19458313229916,\"On River\"\n23.09,2.99573227355399,\"On River\"\n17.27,3.07731226054641,\"On River\"\n23.98,2.96010509591084,\"On River\"\n16.03,3.10906095886099,\"On River\"\n9.38,3.3357695763397,\"Not on River\"\n29.55,3.16547504814109,\"Not on River\"\n9.47,3.2188758248682,\"Not on River\"\n13.51,3.14845336057165,\"On River\"\n9.69,3.35689712276558,\"Not on River\"\n17.92,3.06805293513362,\"On River\"\n10.5,3.13549421592915,\"On River\"\n9.71,3.2846635654062,\"On River\"\n21.46,3.07731226054641,\"On River\"\n9.93,3.31418600467253,\"On River\"\n7.6,3.40452517175483,\"Not on River\"\n4.14,3.80220813942094,\"Not on River\"\n4.63,3.91202300542815,\"Not on River\"\n3.13,3.62700405039585,\"Not on River\"\n6.36,3.45315712059287,\"Not on River\"\n3.92,3.84374416467485,\"Not on River\"\n3.76,3.44998754583159,\"Not on River\"\n11.65,3.1904763503465,\"Not on River\"\n5.25,3.45631668088323,\"Not on River\"\n2.47,3.73050112880476,\"Not on River\"\n3.95,3.87743156065853,\"Not on River\"\n8.05,3.36729582998647,\"On River\"\n10.88,3.17805383034795,\"Not on River\"\n9.54,3.22286784613774,\"On River\"\n4.73,3.44998754583159,\"Not on River\"\n6.36,3.16547504814109,\"Not on River\"\n7.37,3.14845336057165,\"Not on River\"\n11.38,3.09104245335832,\"Not on River\"\n12.4,3.00071981506503,\"Not on River\"\n11.22,3.10009228887823,\"Not on River\"\n5.19,3.16547504814109,\"Not on River\"\n12.5,2.86789890204411,\"Not on River\"\n18.46,2.91777073208428,\"Not on River\"\n9.16,3.1904763503465,\"Not on River\"\n10.15,3.02042488614436,\"Not on River\"\n9.52,3.19867311755068,\"Not on River\"\n6.56,3.26575941076705,\"Not on River\"\n5.9,3.19458313229916,\"Not on River\"\n3.59,3.21084365317094,\"Not on River\"\n3.53,3.38777436133001,\"Not on River\"\n3.54,3.75653810258775,\"Not on River\"\n6.57,3.08648663682246,\"Not on River\"\n9.25,3.03974915897077,\"Not on River\"\n3.11,3.78418963391826,\"Not on River\"\n5.12,3.91202300542815,\"Not on River\"\n7.79,3.58351893845611,\"Not on River\"\n6.9,3.40452517175483,\"Not on River\"\n9.59,3.52046080248897,\"Not on River\"\n7.26,3.7635229971097,\"Not on River\"\n5.91,3.8877303128591,\"Not on River\"\n11.25,3.43398720448515,\"Not on River\"\n8.1,3.59731226058845,\"Not on River\"\n10.45,3.1267605359604,\"Not on River\"\n14.79,3.42426265459315,\"Not on River\"\n7.44,3.91202300542815,\"Not on River\"\n3.16,3.77276093809464,\"Not on River\"\n13.65,3.03013370027132,\"On River\"\n13,3.04927304048202,\"Not on River\"\n6.59,3.22684399451738,\"Not on River\"\n7.73,3.19458313229916,\"Not on River\"\n6.58,3.56104608260405,\"On River\"\n3.53,3.47815842279828,\"On River\"\n2.98,3.46573590279973,\"Not on River\"\n6.05,3.50254987592244,\"On River\"\n4.16,3.49953328238302,\"On River\"\n7.19,3.37073817417745,\"Not on River\"\n4.85,3.55820113047182,\"Not on River\"\n3.76,3.8155121050473,\"Not on River\"\n4.59,3.56671182013973,\"Not on River\"\n3.01,3.8286413964891,\"On River\"\n3.16,3.91202300542815,\"On River\"\n7.85,3.47196645255036,\"Not on River\"\n8.23,3.09104245335832,\"Not on River\"\n12.93,3.00071981506503,\"Not on River\"\n7.14,3.14415227867226,\"Not on River\"\n7.6,3.10458667846607,\"Not on River\"\n9.51,3.21084365317094,\"Not on River\"\n3.33,3.3499040872746,\"Not on River\"\n3.56,3.61899332664977,\"Not on River\"\n4.7,3.32862668882732,\"Not on River\"\n8.58,3.17387845893747,\"Not on River\"\n10.4,3.07731226054641,\"Not on River\"\n6.27,3.35340671782581,\"Not on River\"\n7.39,3.29953372788566,\"Not on River\"\n15.84,3.01062088604774,\"Not on River\"\n4.97,3.11351530921037,\"Not on River\"\n4.74,3.36729582998647,\"Not on River\"\n6.07,3.21084365317094,\"Not on River\"\n9.5,3.09104245335832,\"Not on River\"\n8.67,3.27336401015227,\"Not on River\"\n4.86,3.49953328238302,\"Not on River\"\n6.93,3.58629286533884,\"Not on River\"\n8.93,3.34638914516716,\"Not on River\"\n6.47,3.50855589998265,\"Not on River\"\n7.53,3.33932197794407,\"Not on River\"\n4.54,3.1267605359604,\"Not on River\"\n9.97,3.01062088604774,\"Not on River\"\n12.64,2.77881927199042,\"Not on River\"\n5.98,3.09557760852371,\"Not on River\"\n11.72,2.96527306606928,\"Not on River\"\n7.9,3.07269331469012,\"Not on River\"\n9.28,3.16968558067743,\"Not on River\"\n11.5,2.78501124223834,\"Not on River\"\n18.33,2.87919845729804,\"Not on River\"\n15.94,2.98568193770049,\"Not on River\"\n10.36,3.13983261752775,\"Not on River\"\n12.73,3.04452243772342,\"Not on River\"\n7.2,3.16968558067743,\"Not on River\"\n6.87,3.13983261752775,\"Not on River\"\n7.7,3.01553490085017,\"Not on River\"\n11.74,2.91777073208428,\"Not on River\"\n6.12,3.2188758248682,\"Not on River\"\n5.08,3.20274644293832,\"Not on River\"\n6.15,3.13549421592915,\"Not on River\"\n12.79,3.10009228887823,\"Not on River\"\n9.97,2.96010509591084,\"Not on River\"\n7.34,3.11794990627824,\"Not on River\"\n9.09,2.98568193770049,\"Not on River\"\n12.43,2.83907846350861,\"Not on River\"\n7.83,2.96527306606928,\"Not on River\"\n5.68,3.10009228887823,\"Not on River\"\n6.75,3.03013370027132,\"Not on River\"\n8.01,3.04927304048202,\"Not on River\"\n9.8,2.9704144655697,\"Not on River\"\n10.56,2.91777073208428,\"Not on River\"\n8.51,3.02529107579554,\"Not on River\"\n9.74,2.94443897916644,\"Not on River\"\n9.29,2.92852352386054,\"Not on River\"\n5.49,3.48737507790321,\"Not on River\"\n8.65,2.80336038090653,\"Not on River\"\n7.18,3.17387845893747,\"Not on River\"\n4.61,3.44041809481544,\"Not on River\"\n10.53,2.86220088092947,\"Not on River\"\n12.67,2.84490938381941,\"Not on River\"\n6.36,3.13983261752775,\"Not on River\"\n5.99,3.19867311755068,\"Not on River\"\n5.89,3.28091121578765,\"Not on River\"\n5.98,3.13113691056019,\"Not on River\"\n5.49,3.18221184049661,\"Not on River\"\n7.79,2.92316158071916,\"Not on River\"\n4.5,3.40452517175483,\"Not on River\"\n8.05,2.90142159408275,\"Not on River\"\n5.57,3.02529107579554,\"Not on River\"\n17.6,2.87919845729804,\"On River\"\n13.27,3.07731226054641,\"On River\"\n11.48,3.12236492448736,\"On River\"\n12.67,3.11794990627824,\"Not on River\"\n7.79,3.2188758248682,\"Not on River\"\n14.19,2.99071973173045,\"Not on River\"\n10.19,3.03495298670727,\"Not on River\"\n14.64,2.82137888640921,\"On River\"\n5.29,3.08648663682246,\"On River\"\n7.12,3.31418600467253,\"Not on River\"\n14,3.08648663682246,\"Not on River\"\n13.33,3.13983261752775,\"Not on River\"\n3.26,3.91202300542815,\"Not on River\"\n3.73,3.91202300542815,\"On River\"\n2.96,3.91202300542815,\"On River\"\n9.53,3.91202300542815,\"Not on River\"\n8.88,3.91202300542815,\"On River\"\n34.77,2.62466859216316,\"Not on River\"\n37.97,2.62466859216316,\"Not on River\"\n13.44,2.70805020110221,\"Not on River\"\n23.24,2.63188884013665,\"Not on River\"\n21.24,2.58776403522771,\"Not on River\"\n23.69,2.57261223020711,\"Not on River\"\n21.78,2.32238772029023,\"Not on River\"\n17.21,2.34180580614733,\"Not on River\"\n21.08,2.3887627892351,\"Not on River\"\n23.6,2.42480272571829,\"Not on River\"\n24.56,2.50959926237837,\"Not on River\"\n30.63,2.17475172148416,\"Not on River\"\n30.81,1.97408102602201,\"Not on River\"\n28.28,2.35137525716348,\"Not on River\"\n31.99,2.00148000021012,\"Not on River\"\n30.62,2.32238772029023,\"Not on River\"\n20.85,2.4423470353692,\"Not on River\"\n17.11,2.71469474382088,\"Not on River\"\n18.76,3.14415227867226,\"Not on River\"\n25.68,2.27212588550934,\"Not on River\"\n15.17,2.62466859216316,\"Not on River\"\n16.35,2.54160199346455,\"Not on River\"\n17.12,2.57261223020711,\"Not on River\"\n19.37,2.52572864430826,\"Not on River\"\n19.92,2.14006616349627,\"Not on River\"\n30.59,1.6094379124341,\"Not on River\"\n29.97,1.84054963339749,\"Not on River\"\n26.77,1.7227665977411,\"Not on River\"\n20.32,1.97408102602201,\"Not on River\"\n20.31,2.4932054526027,\"Not on River\"\n19.77,2.11625551480255,\"Not on River\"\n27.38,2.14006616349627,\"Not on River\"\n22.98,1.6094379124341,\"Not on River\"\n23.34,2.47653840011748,\"Not on River\"\n12.13,3.32862668882732,\"Not on River\"\n26.4,2.84490938381941,\"Not on River\"\n19.78,3.31418600467253,\"Not on River\"\n10.11,2.70805020110221,\"Not on River\"\n21.22,2.84490938381941,\"Not on River\"\n34.37,2.88480071284671,\"Not on River\"\n20.08,2.79116510781272,\"Not on River\"\n36.98,1.94591014905531,\"Not on River\"\n29.05,1.97408102602201,\"Not on River\"\n25.79,2.01490302054226,\"Not on River\"\n26.64,2.34180580614733,\"Not on River\"\n20.62,2.17475172148416,\"Not on River\"\n22.74,2.12823170584927,\"Not on River\"\n15.02,2.81540871942271,\"Not on River\"\n15.7,2.65324196460721,\"Not on River\"\n14.1,3.03495298670727,\"Not on River\"\n23.29,2.59525470695687,\"Not on River\"\n17.16,2.45958884180371,\"Not on River\"\n24.39,2.11625551480255,\"Not on River\"\n15.69,2.32238772029023,\"Not on River\"\n14.52,2.3887627892351,\"Not on River\"\n21.52,2.39789527279837,\"Not on River\"\n24.08,2.2512917986065,\"Not on River\"\n17.64,2.67414864942653,\"Not on River\"\n19.69,2.64617479738412,\"Not on River\"\n12.03,2.77881927199042,\"Not on River\"\n16.22,2.66025953726586,\"Not on River\"\n15.17,2.45958884180371,\"Not on River\"\n23.27,2.59525470695687,\"Not on River\"\n18.05,2.26176309847379,\"Not on River\"\n26.45,2.16332302566054,\"Not on River\"\n34.02,2.12823170584927,\"Not on River\"\n22.88,2.54944517092557,\"Not on River\"\n22.11,2.35137525716348,\"Not on River\"\n19.52,2.83907846350861,\"Not on River\"\n16.59,2.91235066461494,\"Not on River\"\n18.85,2.73436750941958,\"Not on River\"\n23.79,2.37954613413017,\"Not on River\"\n23.98,2.46809953147162,\"Not on River\"\n17.79,2.70136121295141,\"Not on River\"\n16.44,2.53369681395743,\"Not on River\"\n18.13,2.64617479738412,\"Not on River\"\n19.31,2.56494935746154,\"Not on River\"\n17.44,2.59525470695687,\"Not on River\"\n17.73,2.72129542785223,\"Not on River\"\n17.27,2.77881927199042,\"Not on River\"\n16.74,2.87919845729804,\"Not on River\"\n18.71,2.70136121295141,\"Not on River\"\n18.13,2.64617479738412,\"Not on River\"\n19.01,2.54160199346455,\"Not on River\"\n16.94,2.60268968544438,\"Not on River\"\n16.23,2.70136121295141,\"Not on River\"\n14.7,2.99573227355399,\"Not on River\"\n16.42,2.79728133483015,\"Not on River\"\n14.65,2.87356463957978,\"Not on River\"\n13.99,2.9704144655697,\"Not on River\"\n10.29,3.00568260440716,\"Not on River\"\n13.22,3.06339092202781,\"Not on River\"\n14.13,2.99071973173045,\"Not on River\"\n17.15,2.94443897916644,\"Not on River\"\n21.32,2.94968833505258,\"Not on River\"\n18.13,2.94968833505258,\"Not on River\"\n14.76,3.00071981506503,\"Not on River\"\n16.29,2.99071973173045,\"Not on River\"\n12.87,2.97552956623647,\"Not on River\"\n14.36,3.14415227867226,\"Not on River\"\n11.66,3.39450839351136,\"Not on River\"\n18.14,2.62466859216316,\"Not on River\"\n24.1,2.58776403522771,\"Not on River\"\n18.68,2.81540871942271,\"Not on River\"\n24.91,2.484906649788,\"Not on River\"\n18.03,2.68102152871429,\"Not on River\"\n13.11,3.06339092202781,\"Not on River\"\n10.74,3.13549421592915,\"Not on River\"\n7.74,3.16547504814109,\"Not on River\"\n7.01,3.2188758248682,\"Not on River\"\n10.42,3.08190996979504,\"Not on River\"\n13.34,3.02529107579554,\"Not on River\"\n10.58,3.05400118167797,\"Not on River\"\n14.98,2.94968833505258,\"Not on River\"\n11.45,3.02529107579554,\"Not on River\"\n18.06,2.72129542785223,\"Not on River\"\n23.97,1.94591014905531,\"Not on River\"\n29.68,2.09186406167839,\"Not on River\"\n18.07,2.61006979274201,\"Not on River\"\n13.35,3.00071981506503,\"Not on River\"\n12.01,3.08190996979504,\"Not on River\"\n13.59,3.19867311755068,\"Not on River\"\n17.6,3.13983261752775,\"Not on River\"\n21.14,2.98061863574394,\"Not on River\"\n14.1,2.90690105984738,\"Not on River\"\n12.92,3.05400118167797,\"Not on River\"\n15.1,2.86220088092947,\"Not on River\"\n14.33,2.82137888640921,\"Not on River\"\n9.67,3.10906095886099,\"Not on River\"\n9.08,3.02529107579554,\"Not on River\"\n5.64,3.17387845893747,\"Not on River\"\n6.48,3.09104245335832,\"Not on River\"\n7.88,2.47653840011748,\"Not on River\""
        },
        {
            "name": ".0/group_by1/model_prediction2_flat",
            "format": {
                "type": "csv",
                "parse": {
                    "resp_upr_": "number",
                    "pred_": "number",
                    "resp_lwr_": "number",
                    "resp_": "number"
                }
            },
            "values": "\"resp_upr_\",\"pred_\",\"resp_lwr_\",\"resp_\",\"chas\"\n3.56047596155946,1.73,3.48112019808703,3.52079807982325,\"Not on River\"\n3.5383606872399,2.18873417721519,3.46131198022371,3.49983633373181,\"Not on River\"\n3.51626107549178,2.64746835443038,3.44148809978895,3.47887458764036,\"Not on River\"\n3.49417860062017,3.10620253164557,3.42164708247767,3.45791284154892,\"Not on River\"\n3.47211490120526,3.56493670886076,3.4017872897097,3.43695109545748,\"Not on River\"\n3.45007179843919,4.02367088607595,3.38190690029288,3.41598934936604,\"Not on River\"\n3.4280513156868,4.48240506329114,3.36200389086239,3.3950276032746,\"Not on River\"\n3.40605569892466,4.94113924050633,3.34207601544165,3.37406585718315,\"Not on River\"\n3.3840874374837,5.39987341772152,3.32212078469972,3.35310411109171,\"Not on River\"\n3.36214928421038,5.85860759493671,3.30213544579016,3.33214236500027,\"Not on River\"\n3.34024427375736,6.3173417721519,3.2821169640603,3.31118061890883,\"Not on River\"\n3.31837573720833,6.77607594936709,3.26206200842644,3.29021887281739,\"Not on River\"\n3.29654731063901,7.23481012658228,3.24196694281288,3.26925712672594,\"Not on River\"\n3.27476293454851,7.69354430379747,3.2218278267205,3.2482953806345,\"Not on River\"\n3.25302684043247,8.15227848101266,3.20164042865365,3.22733363454306,\"Not on River\"\n3.23134352023779,8.61101265822785,3.18140025666545,3.20637188845162,\"Not on River\"\n3.20971767423315,9.06974683544304,3.16110261048721,3.18541014236018,\"Not on River\"\n3.18815413321045,9.52848101265823,3.14074265932702,3.16444839626874,\"Not on River\"\n3.16665775219355,9.98721518987342,3.12031554816104,3.14348665017729,\"Not on River\"\n3.14523327522622,10.4459493670886,3.09981653294548,3.12252490408585,\"Not on River\"\n3.12388517442557,10.9046835443038,3.07924114156325,3.10156315799441,\"Not on River\"\n3.10261747108167,11.363417721519,3.05858535272427,3.08060141190297,\"Not on River\"\n3.08143355147862,11.8221518987342,3.03784578014443,3.05963966581153,\"Not on River\"\n3.06033599418861,12.2808860759494,3.01701984525156,3.03867791972008,\"Not on River\"\n3.03932642752352,12.7396202531646,2.99610591973377,3.01771617362864,\"Not on River\"\n3.01840543452581,13.1983544303797,2.97510342054859,2.9967544275372,\"Not on River\"\n2.99757251800034,13.6570886075949,2.95401284489118,2.97579268144576,\"Not on River\"\n2.97682613036528,14.1158227848101,2.93283574034335,2.95483093535432,\"Not on River\"\n2.95616376423752,14.5745569620253,2.91157461428823,2.93386918926287,\"Not on River\"\n2.93558209178023,15.0332911392405,2.89023279456263,2.91290744317143,\"Not on River\"\n2.91507713569216,15.4920253164557,2.86881425846782,2.89194569707999,\"Not on River\"\n2.89464445312835,15.9507594936709,2.84732344884875,2.87098395098855,\"Not on River\"\n2.87427931555047,16.4094936708861,2.82576509424374,2.85002220489711,\"Not on River\"\n2.85397687145867,16.8682278481013,2.80414404615266,2.82906045880567,\"Not on River\"\n2.83373228383187,17.3269620253165,2.78246514159658,2.80809871271422,\"Not on River\"\n2.8135408387535,17.7856962025316,2.76073309449207,2.78713696662278,\"Not on River\"\n2.79339802540611,18.2444303797468,2.73895241565657,2.76617522053134,\"Not on River\"\n2.77329959011131,18.703164556962,2.71712735876848,2.7452134744399,\"Not on River\"\n2.75324156843545,19.1618987341772,2.69526188826147,2.72425172834846,\"Not on River\"\n2.73322029982188,19.6206329113924,2.67335966469215,2.70328998225701,\"Not on River\"\n2.71323242904132,20.0793670886076,2.65142404328983,2.68232823616557,\"Not on River\"\n2.69327489823831,20.5381012658228,2.62945808190995,2.66136649007413,\"Not on River\"\n2.67334493269414,20.996835443038,2.60746455527124,2.64040474398269,\"Not on River\"\n2.65344002275553,21.4555696202532,2.58544597302696,2.61944299789125,\"Not on River\"\n2.6335579037695,21.9143037974684,2.56340459983011,2.59848125179981,\"Not on River\"\n2.61369653534993,22.3730379746835,2.5413424760668,2.57751950570836,\"Not on River\"\n2.59385408088958,22.8317721518987,2.51926143834427,2.55655775961692,\"Not on River\"\n2.57402888791399,23.2905063291139,2.49716313913697,2.53559601352548,\"Not on River\"\n2.55421946963834,23.7492405063291,2.47504906522973,2.51463426743404,\"Not on River\"\n2.53442448791909,24.2079746835443,2.4529205547661,2.4936725213426,\"Not on River\"\n2.51464273767424,24.6667088607595,2.43077881282807,2.47271077525115,\"Not on River\"\n2.49487313276725,25.1254430379747,2.40862492555218,2.45174902915971,\"Not on River\"\n2.47511469329895,25.5841772151899,2.38645987283759,2.43078728306827,\"Not on River\"\n2.45536653422182,26.0429113924051,2.36428453973184,2.40982553697683,\"Not on River\"\n2.43562785517497,26.5016455696203,2.34209972659581,2.38886379088539,\"Not on River\"\n2.41589793143223,26.9603797468354,2.31990615815566,2.36790204479394,\"Not on River\"\n2.39617610585556,27.4191139240506,2.29770449154944,2.3469402987025,\"Not on River\"\n2.3764617817504,27.8778481012658,2.27549532347172,2.32597855261106,\"Not on River\"\n2.35675441652588,28.336582278481,2.25327919651336,2.30501680651962,\"Not on River\"\n2.33705351607065,28.7953164556962,2.2310566047857,2.28405506042818,\"Not on River\"\n2.31735862976306,29.2540506329114,2.20882799891041,2.26309331433674,\"Not on River\"\n2.29766934604251,29.7127848101266,2.18659379044808,2.24213156824529,\"Not on River\"\n2.27798528847676,30.1715189873418,2.16435435583094,2.22116982215385,\"Not on River\"\n2.258306112267,30.630253164557,2.14211003985782,2.20020807606241,\"Not on River\"\n2.23863150113915,31.0889873417722,2.11986115880279,2.17924632997097,\"Not on River\"\n2.21896116457604,31.5477215189873,2.09760800318301,2.15828458387953,\"Not on River\"\n2.19929483535046,32.0064556962025,2.07535084022571,2.13732283778808,\"Not on River\"\n2.17963226732365,32.4651898734177,2.05308991606963,2.11636109169664,\"Not on River\"\n2.15997323347855,32.9239240506329,2.03082545773185,2.0953993456052,\"Not on River\"\n2.14031752416034,33.3826582278481,2.00855767486717,2.07443759951376,\"Not on River\"\n2.12066494550059,33.8413924050633,1.98628676134404,2.05347585342232,\"Not on River\"\n2.10101531800389,34.3001265822785,1.96401289665786,2.03251410733087,\"Not on River\"\n2.08136847527865,34.7588607594937,1.94173624720021,2.01155236123943,\"Not on River\"\n2.0617242628958,35.2175949367089,1.91945696740018,1.99059061514799,\"Not on River\"\n2.0420825373611,35.676329113924,1.897175200752,1.96962886905655,\"Not on River\"\n2.02244316518861,36.1350632911392,1.87489108074161,1.94866712296511,\"Not on River\"\n2.00280602206427,36.5937974683544,1.85260473168306,1.92770537687367,\"Not on River\"\n1.98317099208974,37.0525316455696,1.8303162694747,1.90674363078222,\"Not on River\"\n1.963537967098,37.5112658227848,1.80802580228356,1.88578188469078,\"Not on River\"\n1.94390684603308,37.97,1.7857334311656,1.86482013859934,\"Not on River\"\n3.84380374741524,1.92,3.56106968653756,3.7024367169764,\"On River\"\n3.82607474468546,2.23518987341772,3.54967788331425,3.68787631399986,\"On River\"\n3.80838419887832,2.55037974683544,3.5382476231683,3.67331591102331,\"On River\"\n3.79073484626097,2.86556962025316,3.52677616983257,3.65875550804677,\"On River\"\n3.77312964752525,3.18075949367089,3.5152605626152,3.64419510507023,\"On River\"\n3.75557180525224,3.49594936708861,3.50369759893513,3.62963470209368,\"On River\"\n3.7380647819079,3.81113924050633,3.49208381632638,3.61507429911714,\"On River\"\n3.72061231808482,4.12632911392405,3.48041547419638,3.6005138961406,\"On River\"\n3.70321845059437,4.44151898734177,3.46868853573374,3.58595349316405,\"On River\"\n3.68588752987796,4.75670886075949,3.45689865049706,3.57139309018751,\"On River\"\n3.66862423604261,5.07189873417721,3.44504113837932,3.55683268721096,\"On River\"\n3.65143359263433,5.38708860759494,3.43311097583451,3.54227228423442,\"On River\"\n3.63432097704354,5.70227848101266,3.42110278547221,3.52771188125788,\"On River\"\n3.61729212619629,6.01746835443038,3.40901083036638,3.51315147828133,\"On River\"\n3.60035313593305,6.3326582278481,3.39682901467653,3.49859107530479,\"On River\"\n3.58351045223213,6.64784810126582,3.38455089242436,3.48403067232825,\"On River\"\n3.56677085222455,6.96303797468354,3.37216968647886,3.4694702693517,\"On River\"\n3.55014141280973,7.27822784810126,3.35967831994059,3.45490986637516,\"On River\"\n3.53362946466628,7.59341772151899,3.34706946213095,3.44034946339862,\"On River\"\n3.51724252961776,7.90860759493671,3.33433559122639,3.42578906042207,\"On River\"\n3.50098823972291,8.22379746835443,3.32146907516815,3.41122865744553,\"On River\"\n3.48487423717168,8.53898734177215,3.3084622717663,3.39666825446899,\"On River\"\n3.46890805511973,8.85417721518987,3.29530764786516,3.38210785149244,\"On River\"\n3.45309698098792,9.16936708860759,3.28199791604387,3.3675474485159,\"On River\"\n3.43744790543244,9.48455696202532,3.26852618564627,3.35298704553936,\"On River\"\n3.4219671620299,9.79974683544304,3.25488612309572,3.33842664256281,\"On River\"\n3.40666036451694,10.1149367088608,3.2410721146556,3.32386623958627,\"On River\"\n3.39153224991162,10.4301265822785,3.22707942330783,3.30930583660973,\"On River\"\n3.37658653673673,10.7453164556962,3.21290433052963,3.29474543363318,\"On River\"\n3.36182580760769,11.0605063291139,3.19854425370559,3.28018503065664,\"On River\"\n3.34725142448353,11.3756962025316,3.18399783087666,3.26562462768009,\"On River\"\n3.33286348290839,11.6908860759494,3.16926496649871,3.25106422470355,\"On River\"\n3.31866080877368,12.0060759493671,3.15434683468033,3.23650382172701,\"On River\"\n3.30464099785388,12.3212658227848,3.13924583964704,3.22194341875046,\"On River\"\n3.29080049505614,12.6364556962025,3.1239655364917,3.20738301577392,\"On River\"\n3.27713470742967,12.9516455696203,3.10851051816508,3.19282261279738,\"On River\"\n3.26363814287005,13.266835443038,3.09288627677161,3.17826220982083,\"On River\"\n3.25030456533395,13.5820253164557,3.07709904835463,3.16370180684429,\"On River\"\n3.23712715727854,13.8972151898734,3.06115565045696,3.14914140386775,\"On River\"\n3.22409868081961,14.2124050632911,3.0450633209628,3.1345810008912,\"On River\"\n3.21121163051939,14.5275949367089,3.02882956530993,3.12002059791466,\"On River\"\n3.19845837248222,14.8427848101266,3.01246201739401,3.10546019493812,\"On River\"\n3.18583126628271,15.1579746835443,2.99596831764043,3.09089979196157,\"On River\"\n3.17332276796462,15.473164556962,2.97935601000544,3.07633938898503,\"On River\"\n3.16092551378968,15.7883544303797,2.96263245822729,3.06177898600849,\"On River\"\n3.14863238552,16.1035443037975,2.94580478054388,3.04721858303194,\"On River\"\n3.13643655877834,16.4187341772152,2.92887980133246,3.0326581800554,\"On River\"\n3.12433153648183,16.7339240506329,2.91186401767588,3.01809777707886,\"On River\"\n3.11231116954345,17.0491139240506,2.89476357866117,3.00353737410231,\"On River\"\n3.10036966704299,17.3643037974684,2.87758427520855,2.98897697112577,\"On River\"\n3.08850159794715,17.6794936708861,2.8603315383513,2.97441656814922,\"On River\"\n3.07670188625624,17.9946835443038,2.84301044408912,2.95985616517268,\"On River\"\n3.06496580121314,18.3098734177215,2.82562572317913,2.94529576219614,\"On River\"\n3.0532889439586,18.6250632911392,2.80818177448059,2.93073535921959,\"On River\"\n3.04166723177299,18.940253164557,2.79068268071311,2.91617495624305,\"On River\"\n3.03009688082263,19.2554430379747,2.77313222571039,2.90161455326651,\"On River\"\n3.01857438813208,19.5706329113924,2.75553391244784,2.88705415028996,\"On River\"\n3.00709651333667,19.8858227848101,2.73789098129017,2.87249374731342,\"On River\"\n2.99566026062932,20.2010126582278,2.72020642804443,2.85793334433688,\"On River\"\n2.98426286120202,20.5162025316456,2.70248302151865,2.84337294136033,\"On River\"\n2.97290175639062,20.8313924050633,2.68472332037696,2.82881253838379,\"On River\"\n2.96157458166,21.146582278481,2.66692968915449,2.81425213540725,\"On River\"\n2.95027915151087,21.4617721518987,2.64910431335053,2.7996917324307,\"On River\"\n2.93901344534749,21.7769620253165,2.63124921356082,2.78513132945416,\"On River\"\n2.92777559431432,22.0921518987342,2.61336625864092,2.77057092647762,\"On River\"\n2.9165638690867,22.4073417721519,2.59545717791545,2.75601052350107,\"On River\"\n2.90537666858518,22.7225316455696,2.57752357246388,2.74145012052453,\"On River\"\n2.8942125095719,23.0377215189873,2.55956692552407,2.72688971754799,\"On River\"\n2.88307001708121,23.3529113924051,2.54158861206167,2.71232931457144,\"On River\"\n2.8719479156328,23.6681012658228,2.523589907557,2.6977689115949,\"On River\"\n2.86084502117417,23.9832911392405,2.50557199606254,2.68320850861836,\"On River\"\n2.84976023369956,24.2984810126582,2.48753597758407,2.66864810564181,\"On River\"\n2.83869253049344,24.6136708860759,2.46948287483709,2.65408770266527,\"On River\"\n2.82764095994918,24.9288607594937,2.45141363942826,2.63952729968872,\"On River\"\n2.8166046359155,25.2440506329114,2.43332915750886,2.62496689671218,\"On River\"\n2.8055827325266,25.5592405063291,2.41523025494468,2.61040649373564,\"On River\"\n2.79457447947461,25.8744303797468,2.39711770204358,2.59584609075909,\"On River\"\n2.78357915768605,26.1896202531646,2.37899221787905,2.58128568778255,\"On River\"\n2.77259609536686,26.5048101265823,2.36085447424515,2.56672528480601,\"On River\"\n2.76162466438359,26.82,2.34270509927534,2.55216488182946,\"On River\""
        },
        {
            "name": ".0/group_by1/model_prediction2",
            "source": ".0/group_by1/model_prediction2_flat",
            "transform": [
                {
                    "type": "treefacet",
                    "keys": [
                        "data.chas"
                    ]
                }
            ]
        },
        {
            "name": "scale/fill",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/stroke",
            "format": {
                "type": "csv",
                "parse": {

                }
            },
            "values": "\"domain\"\n\"Not on River\"\n\"On River\""
        },
        {
            "name": "scale/x",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n-0.0820000000000003\n39.782"
        },
        {
            "name": "scale/y",
            "format": {
                "type": "csv",
                "parse": {
                    "domain": "number"
                }
            },
            "values": "\"domain\"\n1.4943086577844\n4.02715226007785"
        }
    ],
    "scales": [
        {
            "name": "fill",
            "type": "ordinal",
            "domain": {
                "data": "scale/fill",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "stroke",
            "type": "ordinal",
            "domain": {
                "data": "scale/stroke",
                "field": "data.domain"
            },
            "points": true,
            "sort": false,
            "range": "category10"
        },
        {
            "name": "x",
            "domain": {
                "data": "scale/x",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "width"
        },
        {
            "name": "y",
            "domain": {
                "data": "scale/y",
                "field": "data.domain"
            },
            "zero": false,
            "nice": false,
            "clamp": false,
            "range": "height"
        }
    ],
    "marks": [
        {
            "type": "symbol",
            "properties": {
                "update": {
                    "size": {
                        "value": 50
                    },
                    "x": {
                        "scale": "x",
                        "field": "data.lstat"
                    },
                    "y": {
                        "scale": "y",
                        "field": "data.log_medv"
                    },
                    "fill": {
                        "scale": "fill",
                        "field": "data.chas"
                    }
                },
                "ggvis": {
                    "data": {
                        "value": ".0"
                    }
                }
            },
            "from": {
                "data": ".0"
            }
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/model_prediction2"
            },
            "marks": [
                {
                    "type": "area",
                    "properties": {
                        "update": {
                            "fill": {
                                "value": "#333333"
                            },
                            "y2": {
                                "scale": "y",
                                "field": "data.resp_upr_"
                            },
                            "fillOpacity": {
                                "value": 0.2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_lwr_"
                            },
                            "stroke": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/model_prediction2"
                            }
                        }
                    }
                }
            ]
        },
        {
            "type": "group",
            "from": {
                "data": ".0/group_by1/model_prediction2"
            },
            "marks": [
                {
                    "type": "line",
                    "properties": {
                        "update": {
                            "strokeWidth": {
                                "value": 2
                            },
                            "x": {
                                "scale": "x",
                                "field": "data.pred_"
                            },
                            "y": {
                                "scale": "y",
                                "field": "data.resp_"
                            },
                            "stroke": {
                                "scale": "stroke",
                                "field": "data.chas"
                            },
                            "fill": {
                                "value": "transparent"
                            }
                        },
                        "ggvis": {
                            "data": {
                                "value": ".0/group_by1/model_prediction2"
                            }
                        }
                    }
                }
            ]
        }
    ],
    "legends": [
        {
            "orient": "right",
            "fill": "fill",
            "title": "chas"
        },
        {
            "orient": "right",
            "stroke": "stroke",
            "title": "chas"
        }
    ],
    "axes": [
        {
            "type": "x",
            "scale": "x",
            "orient": "bottom",
            "layer": "back",
            "grid": true,
            "title": "lstat"
        },
        {
            "type": "y",
            "scale": "y",
            "orient": "left",
            "layer": "back",
            "grid": true,
            "title": "log_medv"
        }
    ],
    "padding": null,
    "ggvis_opts": {
        "keep_aspect": false,
        "resizable": true,
        "padding": {

        },
        "duration": 250,
        "renderer": "svg",
        "hover_duration": 0,
        "width": 504,
        "height": 504
    },
    "handlers": null
}
;
ggvis.getPlot("plot_id348263735").parseSpec(plot_id348263735_spec);
</script><!--/html_preserve-->

Here we see almost perfectly parallel slopes.  While accounting for
`chas` in the model is probably a good idea, there is no significant
interaction.


