
# Customer segmentation with E-Commerce Data



## Context
Typically e-commerce datasets are proprietary and consequently hard to find among publicly available data. However, The UCI Machine Learning Repository has made this dataset containing actual transactions from 2010 and 2011. The dataset is maintained on their site, where it can be found by the title "Online Retail".

## Content
"This is a transnational data set which contains all the transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based and registered non-store online retail.The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers."

## Acknowledgements
Per the UCI Machine Learning Repository, this data was made available by Dr Daqing Chen, Director: Public Analytics group. chend '@' lsbu.ac.uk, School of Engineering, London South Bank University, London SE1 0AA, UK.

## Source

Dr. Daqing Chen, Course Director: MSc Data Science. chend '@' lsbu.ac.uk, School of Engineering, London South Bank University, London SE1 0AA, UK.


[Dataset](https://www.kaggle.com/carrie1/ecommerce-data/data#)

# Imports


```python
import plotly.express as px
import pandas as pd
import numpy as np
```

# Load the data


```python
e_comerce_data = pd.read_csv('data/data.csv', encoding='ISO-8859-1')
e_comerce_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>InvoiceNo</th>
      <th>StockCode</th>
      <th>Description</th>
      <th>Quantity</th>
      <th>InvoiceDate</th>
      <th>UnitPrice</th>
      <th>CustomerID</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>536365</td>
      <td>85123A</td>
      <td>WHITE HANGING HEART T-LIGHT HOLDER</td>
      <td>6</td>
      <td>12/1/2010 8:26</td>
      <td>2.55</td>
      <td>17850.0</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>1</th>
      <td>536365</td>
      <td>71053</td>
      <td>WHITE METAL LANTERN</td>
      <td>6</td>
      <td>12/1/2010 8:26</td>
      <td>3.39</td>
      <td>17850.0</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>2</th>
      <td>536365</td>
      <td>84406B</td>
      <td>CREAM CUPID HEARTS COAT HANGER</td>
      <td>8</td>
      <td>12/1/2010 8:26</td>
      <td>2.75</td>
      <td>17850.0</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>3</th>
      <td>536365</td>
      <td>84029G</td>
      <td>KNITTED UNION FLAG HOT WATER BOTTLE</td>
      <td>6</td>
      <td>12/1/2010 8:26</td>
      <td>3.39</td>
      <td>17850.0</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>4</th>
      <td>536365</td>
      <td>84029E</td>
      <td>RED WOOLLY HOTTIE WHITE HEART.</td>
      <td>6</td>
      <td>12/1/2010 8:26</td>
      <td>3.39</td>
      <td>17850.0</td>
      <td>United Kingdom</td>
    </tr>
  </tbody>
</table>
</div>



# Dataset description

| Variable Name | Type  | Description |
|---------------|-------|:------------|
|InvoiceNo      |Nominal|Invoice number. A 6-digit integral number uniquely assigned to each transaction. If this code starts with the letter 'c', it indicates a cancellation.|
|StockCode      |Nominal|Product (item) code. A 5-digit integral number uniquely assigned to each distinct product.|
|Description    |Nominal|Product (item) name.|
|Quantity       |Numeric|The quantities of each product (item) per transaction.|
|InvoiceDate    |Numeric|Invice date and time. The day and time when a transaction was generated.|
|UnitPrice      |Numeric|Unit price. Product price per unit in sterling (£).|
|CustomerID     |Nominal|Customer number. A 5-digit integral number uniquely assigned to each customer.|
|Country        |Nominal|Country name. The name of the country where a customer resides.|

# Helper functions


```python
def get_duplicates(dataframe):
    return dataframe[dataframe.duplicated()]
```

# Date preprocess


```python
print(f'Removed {len(get_duplicates(e_comerce_data))} duplicated entries')
e_comerce_data.drop_duplicates(inplace=True)
```

    Removed 5268 duplicated entries
    

# Data Structures


```python
class Customer:
    def __init__(self, customer_id, country):
        self.customer_id = customer_id
        self.country = country
        self.transactions = []
    def addTransaction(transaction
                       
        self.transactions.append(transaction)
                       
    def __str__(self):
        return f'{self.customer_id} : {self.country}'
    

                       
class Product:
    def __init__(self, price, code, description):
        self.price = price
        self.code = code
        self.description = description
        
    def __str__(self):
        return f'{self.description} : {self.code} : {self.price}£'
    
    
                       
class Transaction:
    def __init__(self, product, quantity, date, canceled):
        self.product = product
        self.quantity = quantity
        self.date = date
        self.canceled = canceled
                       
    def __str__(self):
        return f'{self.product} x {self.quantity} : {self.date} \
        : Status: {"Canceled" if self.canceled else "Successful"}'
    

                       
class ECommerce:
    def __init__(self):
        self.customers = []
        self.products = []
        self.transactions = []
                       
    def addCustomer(self, customer):
        self.customers.append(customer)
                       
    def addProduct(self, product):
        self.products.append(product)
                       
    def addTransaction(self, transaction):
        self.transactions.append(transaction)
                       
    def customer_exist(self, customer_id):
        return any(customer.customer_id==customer_id for customer in e_commerce.customers)
                       
    def product_exist(self, code):
        return any(product.code==code for product in e_commerce.products)
                       
    def is_canceled(self, invoice_nr):
        return True if invoice_nr[0]=='c' else False
                       
    def getCustomer(self, customer_id):
        for customer in e_commerce.customers:
            if customer.customer_id==customer_id:
                return customer
        raise NameError
                       
    def getProduct(self, code):
        for product in e_commerce.products:
            if product.code==code:
                return product
        raise NameError
                       
    def __str__(self):
        return f'Customers: {len(self.customers)}\nProducts: {len(self.products)}\
        \nTransactions: {len(self.transactions)}'
```

# Data initialization


```python
e_commerce = ECommerce()

def add_row_to_ecommerce(row):
    invoice_nr, code, description, quantity, date, price, customer_id, country = row.values
    if not e_commerce.customer_exist(customer_id):
        customer = Customer(customer_id, country)
        e_commerce.addCustomer(customer)
    else:
        customer =e_commerce.getCustomer(customer_id)
    if not e_commerce.product_exist(code):
        product = Product(price, code, description)
        e_commerce.addProduct(product)
    else:
        product = e_commerce.getProduct(code)
    is_canceled = e_commerce.is_canceled(invoice_nr)
    transaction = Transaction(product, quantity, date, is_canceled)
    e_commerce.addTransaction(transaction)

for row_idx in range(len(e_comerce_data)//100):
    add_row_to_ecommerce(e_comerce_data.iloc[row_idx])
    
print(e_commerce)
```

    Customers: 21175
    Products: 2908        
    Transactions: 54190
    

# Dataset analysis


```python
e_comerce_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 536641 entries, 0 to 541908
    Data columns (total 8 columns):
    InvoiceNo      536641 non-null object
    StockCode      536641 non-null object
    Description    535187 non-null object
    Quantity       536641 non-null int64
    InvoiceDate    536641 non-null object
    UnitPrice      536641 non-null float64
    CustomerID     401604 non-null float64
    Country        536641 non-null object
    dtypes: float64(2), int64(1), object(5)
    memory usage: 36.8+ MB
    


```python
e_comerce_data.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quantity</th>
      <th>UnitPrice</th>
      <th>CustomerID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>536641.000000</td>
      <td>536641.000000</td>
      <td>401604.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>9.620029</td>
      <td>4.632656</td>
      <td>15281.160818</td>
    </tr>
    <tr>
      <th>std</th>
      <td>219.130156</td>
      <td>97.233118</td>
      <td>1714.006089</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-80995.000000</td>
      <td>-11062.060000</td>
      <td>12346.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>1.250000</td>
      <td>13939.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.000000</td>
      <td>2.080000</td>
      <td>15145.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>10.000000</td>
      <td>4.130000</td>
      <td>16784.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>80995.000000</td>
      <td>38970.000000</td>
      <td>18287.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
e_comerce_data['spent'] = e_comerce_data.Quantity*e_comerce_data.UnitPrice
fig = px.choropleth(pd.DataFrame(np.array([e_comerce_data.groupby('Country')['spent'].mean().index,
                                 e_comerce_data.groupby('Country')['spent'].mean().values]).T,
                                 columns=['country', 'Average Spending']),
                    locations='country',
                    color='Average Spending',
                    locationmode='country names',
                    color_continuous_scale=px.colors.sequential.Plasma)
fig.show()
```


<div>
            <div id="85a043f5-1a5e-46fb-a538-c871340cdfe1" class="plotly-graph-div" style="height:600px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                if (document.getElementById("85a043f5-1a5e-46fb-a538-c871340cdfe1")) {
                    Plotly.newPlot(
                        '85a043f5-1a5e-46fb-a538-c871340cdfe1',
                        [{"coloraxis": "coloraxis", "geo": "geo", "hoverlabel": {"namelength": 0}, "hovertemplate": "country=%{location}<br>Average Spending=%{z}", "locationmode": "country names", "locations": ["Australia", "Austria", "Bahrain", "Belgium", "Brazil", "Canada", "Channel Islands", "Cyprus", "Czech Republic", "Denmark", "EIRE", "European Community", "Finland", "France", "Germany", "Greece", "Hong Kong", "Iceland", "Israel", "Italy", "Japan", "Lebanon", "Lithuania", "Malta", "Netherlands", "Norway", "Poland", "Portugal", "RSA", "Saudi Arabia", "Singapore", "Spain", "Sweden", "Switzerland", "USA", "United Arab Emirates", "United Kingdom", "Unspecified"], "name": "", "type": "choropleth", "z": [108.91078696343381, 25.322493765586024, 28.86315789473684, 19.773301111648127, 35.737500000000004, 24.280662251655635, 26.520990752972207, 21.04543371522095, 23.590666666666667, 48.24714652956299, 32.13506598240449, 21.17622950819672, 32.12480575539564, 23.102342817000352, 23.365977848101114, 32.26383561643835, 34.88816901408451, 23.681318681318665, 26.877448979591822, 21.034259028642595, 98.7168156424581, 37.64177777777778, 47.45885714285714, 19.728110236220473, 120.05969633066223, 32.378876611418086, 21.15290322580644, 19.40594039735099, 17.281206896551723, 13.116999999999999, 39.82703056768559, 21.659821993670874, 79.36097613882865, 28.266323971915792, 5.948178694158077, 27.97470588235295, 16.657410124415637, 10.726108597285059]}],
                        {"coloraxis": {"colorbar": {"title": {"text": "Average Spending"}}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "geo": {"center": {}, "domain": {"x": [0.0, 1.0], "y": [0.0, 1.0]}}, "height": 600, "legend": {"tracegroupgap": 0}, "margin": {"t": 60}, "template": {"data": {"bar": [{"error_x": {"color": "#2a3f5f"}, "error_y": {"color": "#2a3f5f"}, "marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "bar"}], "barpolar": [{"marker": {"line": {"color": "#E5ECF6", "width": 0.5}}, "type": "barpolar"}], "carpet": [{"aaxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "baxis": {"endlinecolor": "#2a3f5f", "gridcolor": "white", "linecolor": "white", "minorgridcolor": "white", "startlinecolor": "#2a3f5f"}, "type": "carpet"}], "choropleth": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "choropleth"}], "contour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "contour"}], "contourcarpet": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "contourcarpet"}], "heatmap": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmap"}], "heatmapgl": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "heatmapgl"}], "histogram": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "histogram"}], "histogram2d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2d"}], "histogram2dcontour": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "histogram2dcontour"}], "mesh3d": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "type": "mesh3d"}], "parcoords": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "parcoords"}], "pie": [{"automargin": true, "type": "pie"}], "scatter": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter"}], "scatter3d": [{"line": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatter3d"}], "scattercarpet": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattercarpet"}], "scattergeo": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergeo"}], "scattergl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattergl"}], "scattermapbox": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scattermapbox"}], "scatterpolar": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolar"}], "scatterpolargl": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterpolargl"}], "scatterternary": [{"marker": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "type": "scatterternary"}], "surface": [{"colorbar": {"outlinewidth": 0, "ticks": ""}, "colorscale": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "type": "surface"}], "table": [{"cells": {"fill": {"color": "#EBF0F8"}, "line": {"color": "white"}}, "header": {"fill": {"color": "#C8D4E3"}, "line": {"color": "white"}}, "type": "table"}]}, "layout": {"annotationdefaults": {"arrowcolor": "#2a3f5f", "arrowhead": 0, "arrowwidth": 1}, "coloraxis": {"colorbar": {"outlinewidth": 0, "ticks": ""}}, "colorscale": {"diverging": [[0, "#8e0152"], [0.1, "#c51b7d"], [0.2, "#de77ae"], [0.3, "#f1b6da"], [0.4, "#fde0ef"], [0.5, "#f7f7f7"], [0.6, "#e6f5d0"], [0.7, "#b8e186"], [0.8, "#7fbc41"], [0.9, "#4d9221"], [1, "#276419"]], "sequential": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]], "sequentialminus": [[0.0, "#0d0887"], [0.1111111111111111, "#46039f"], [0.2222222222222222, "#7201a8"], [0.3333333333333333, "#9c179e"], [0.4444444444444444, "#bd3786"], [0.5555555555555556, "#d8576b"], [0.6666666666666666, "#ed7953"], [0.7777777777777778, "#fb9f3a"], [0.8888888888888888, "#fdca26"], [1.0, "#f0f921"]]}, "colorway": ["#636efa", "#EF553B", "#00cc96", "#ab63fa", "#FFA15A", "#19d3f3", "#FF6692", "#B6E880", "#FF97FF", "#FECB52"], "font": {"color": "#2a3f5f"}, "geo": {"bgcolor": "white", "lakecolor": "white", "landcolor": "#E5ECF6", "showlakes": true, "showland": true, "subunitcolor": "white"}, "hoverlabel": {"align": "left"}, "hovermode": "closest", "mapbox": {"style": "light"}, "paper_bgcolor": "white", "plot_bgcolor": "#E5ECF6", "polar": {"angularaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "radialaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "scene": {"xaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "yaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}, "zaxis": {"backgroundcolor": "#E5ECF6", "gridcolor": "white", "gridwidth": 2, "linecolor": "white", "showbackground": true, "ticks": "", "zerolinecolor": "white"}}, "shapedefaults": {"line": {"color": "#2a3f5f"}}, "ternary": {"aaxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "baxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}, "bgcolor": "#E5ECF6", "caxis": {"gridcolor": "white", "linecolor": "white", "ticks": ""}}, "title": {"x": 0.05}, "xaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}, "yaxis": {"automargin": true, "gridcolor": "white", "linecolor": "white", "ticks": "", "title": {"standoff": 15}, "zerolinecolor": "white", "zerolinewidth": 2}}}},
                        {"responsive": true}
                    ).then(function(){
var gd = document.getElementById('85a043f5-1a5e-46fb-a538-c871340cdfe1');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});
// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {
    x.observe(notebookContainer, {childList: true});
}
// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {
    x.observe(outputEl, {childList: true});
}
                        })
                };
                });
            </script>
        </div>
