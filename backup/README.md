# DataCol
This repository is an updated version of the plots from DataCol (previously developed on [covid19-mx-viz](https://github.com/jballesterosc/covid19-mx-viz) and [mobility-covid19](https://github.com/jballesterosc/covid19-mx-viz))

## About the project

DataCol borns in the need to socialize understandable plots of the evolution of sanitary emergency of COVID-19 at a local level, focused only on Colima's context. 

The remanence of the website, attends to the importance of the existence of the data on a post-covid context, for any purpose necessary but mostly, accountability from citizens and authorities as well. 

# Notebooks
### **_df_builder_**
This notebook retrieves [Mario Romero's](https://github.com/mariorz) data's repository [covid-19-mx-time-series](https://github.com/mariorz/covid19-mx-time-series) and builds it's own `csv` file with new DataSet, filtered by state to simplify the Data Visualization.

The times-series selected for this project are:

| Indicator | Description | File |
|:------------------------------------------------|:-------------------------------------------------|:-------------------------------------------------|
| Confirmed cases | Confirmed by confirmation date | [covid19_confirmed_mx.csv](https://raw.githubusercontent.com/mariorz/covid19-mx-time-series/master/data/covid19_confirmed_mx.csv) |
| Suspects | Suspect cases by date of official publication | [covid19_suspects_mx.csv](https://raw.githubusercontent.com/mariorz/covid19-mx-time-series/master/data/covid19_suspects_mx.csv) |
| Hospitalized | Confirmed hospitalized by admission date | [hospitalized_confirmed_by_admission_date_mx.csv](https://raw.githubusercontent.com/mariorz/covid19-mx-time-series/master/data/full/by_hospital_state/hospitalized_confirmed_by_admission_date_mx.csv) |
| Deaths | Deaths confirmed by death date | [deaths_confirmed_by_death_date_mx.csv](https://raw.githubusercontent.com/mariorz/covid19-mx-time-series/master/data/full/by_hospital_state/deaths_confirmed_by_death_date_mx.csv) |

From this files, the notebook creates new columns with MA (through the `rolling` function) of 7 and 14 days. Also, calculates through `diff` function the daily cases depending of the indicator of interest. 

the output of this notebook contains the following columns: 
| column | description | 
|:--------------------|:-----------------|
| confirmed | confirmed cases by confirmation date |
| confirmed_daily | daily cases by confirmation date |
| confirmed_ma_7 | MA 7 days |
| confirmed_ma_14 | MA 14 days |
| suspects | daily suspects |
| suspects_ma_7 | MA  7 days |
| suspects_ma_14 | MA 14 days |
| hospitalized | hospitalized confirmed by admission date |
| hospitalized_daily | daily hospitalized by admission date |
| hospitalized_ma_7 | MA 7 days |
| hospitalized_ma_14 | MA 14 days |
| deaths | deaths confirmed by death date |
| deaths_daily | daily deaths confirmed by death date |
| deaths_ma_7 | MA 7 days |
| deaths_ma_14 | MA 14 days |


Finally, saves this new DataFrame on _[/data_output/colima_df.csv](https://github.com/jballesterosc/datacol/tree/main/code/data_output)_.

## html_plots
This notebook visualizes the data from the new DataSet without titles, following the _Style Guide_ build on [datacol_theme.py](https://github.com/jballesterosc/datacol/blob/main/code/datacol_theme.py) (find more [here](https://github.com/jballesterosc/datacol/tree/main/style_guide)), and saves them on `html` in the following directory: _[/plots/html](https://github.com/jballesterosc/datacol/tree/main/code/plots/html)_. 

Here an example: 

````python
suspects_7  = alt.Chart(df.reset_index()).mark_line(color="#63283c").encode(
    alt.X('index:T', title=" "),
    alt.Y('suspects_ma_7:Q', title=" "),
    tooltip=[alt.Tooltip('index:T', title="Fecha"), alt.Tooltip('suspects:Q', title="Casos sospechosos"), alt.Tooltip('suspects_ma_7:Q', title="Promedio movil de 7 días"), alt.Tooltip('suspects_ma_14:Q', title="Promedio movil de 14 días")]
)

suspects_14 = alt.Chart(df.reset_index()).mark_line(color="#45a887").encode(
    alt.X('index:T', title=" "),
    alt.Y('suspects_ma_14:Q', title=" "),
    tooltip=[alt.Tooltip('index:T', title="Fecha"), alt.Tooltip('suspects:Q', title="Casos sospechosos"), alt.Tooltip('suspects_ma_7:Q', title="Promedio movil de 7 días"), alt.Tooltip('suspects_ma_14:Q', title="Promedio movil de 14 días")]
)

suspects_concatenated = suspects_area + suspects_7 + suspects_14
suspects_concatenated.save("plots/html/suspects_concatenated.html")
suspects_concatenated

````

![hola](https://i.imgur.com/iEXG9bj.png). 

The html plots are intended to embed on the notion.so template to visualize Colima's data per indicator.

**_EN CONSTRUCCIÓN_**