
# Хакатон SocialHack 
# Кейс: Прогнозирование потребления энергии 

#### Импорт библиотек


```python
import pandas as pd
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt
import xlrd
from sklearn import preprocessing
%matplotlib inline
```

#### Подгружаем файл со статистическими данными


```python
table = pd.read_excel('data/hack_4_50.xlsx')
table = table.replace(np.nan,0)
table = table.astype(np.float64)
table.columns
```




    Index(['Time', 'Year', 'Month', 'Day_of_week', 'Season', 'Temperature',
           'Number_Of_Guest_Nights', 'Total_hours_worked', 'Sales_new_vehicles',
           'Demand'],
          dtype='object')




```python
table
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Time</th>
      <th>Year</th>
      <th>Month</th>
      <th>Day_of_week</th>
      <th>Season</th>
      <th>Temperature</th>
      <th>Number_Of_Guest_Nights</th>
      <th>Total_hours_worked</th>
      <th>Sales_new_vehicles</th>
      <th>Demand</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>14.5</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.555</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>13.3</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.433</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>13.8</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.393</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>13.0</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.375</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>12.2</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.390</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>11.2</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.467</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>10.8</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.631</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>12.1</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.775</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>13.8</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.965</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>15.3</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.159</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>16.7</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.211</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>16.9</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.265</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>17.2</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.160</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>17.0</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.288</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>15.7</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.361</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>14.3</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.362</td>
    </tr>
    <tr>
      <th>16</th>
      <td>16.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>13.0</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.406</td>
    </tr>
    <tr>
      <th>17</th>
      <td>17.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>12.2</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.438</td>
    </tr>
    <tr>
      <th>18</th>
      <td>18.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>11.4</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.501</td>
    </tr>
    <tr>
      <th>19</th>
      <td>19.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>11.1</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.381</td>
    </tr>
    <tr>
      <th>20</th>
      <td>20.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>10.9</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.242</td>
    </tr>
    <tr>
      <th>21</th>
      <td>21.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>9.5</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>3.084</td>
    </tr>
    <tr>
      <th>22</th>
      <td>22.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>8.5</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.973</td>
    </tr>
    <tr>
      <th>23</th>
      <td>23.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>8.2</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.811</td>
    </tr>
    <tr>
      <th>24</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>8.4</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.613</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>8.2</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.492</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>8.1</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.431</td>
    </tr>
    <tr>
      <th>27</th>
      <td>3.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>8.4</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.403</td>
    </tr>
    <tr>
      <th>28</th>
      <td>4.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>8.8</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.390</td>
    </tr>
    <tr>
      <th>29</th>
      <td>5.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>8.8</td>
      <td>42178210.0</td>
      <td>1488.0</td>
      <td>324738.0</td>
      <td>2.432</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>26250</th>
      <td>18.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>10.2</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.520</td>
    </tr>
    <tr>
      <th>26251</th>
      <td>19.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>9.8</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.485</td>
    </tr>
    <tr>
      <th>26252</th>
      <td>20.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>8.7</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.423</td>
    </tr>
    <tr>
      <th>26253</th>
      <td>21.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>7.5</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.289</td>
    </tr>
    <tr>
      <th>26254</th>
      <td>22.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>7.5</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.085</td>
    </tr>
    <tr>
      <th>26255</th>
      <td>23.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>7.9</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.888</td>
    </tr>
    <tr>
      <th>26256</th>
      <td>0.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>8.0</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.686</td>
    </tr>
    <tr>
      <th>26257</th>
      <td>1.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>7.4</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.551</td>
    </tr>
    <tr>
      <th>26258</th>
      <td>2.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>7.8</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.506</td>
    </tr>
    <tr>
      <th>26259</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>7.6</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.483</td>
    </tr>
    <tr>
      <th>26260</th>
      <td>4.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>8.2</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.477</td>
    </tr>
    <tr>
      <th>26261</th>
      <td>5.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>8.3</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.494</td>
    </tr>
    <tr>
      <th>26262</th>
      <td>6.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>8.5</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.566</td>
    </tr>
    <tr>
      <th>26263</th>
      <td>7.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>8.1</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.714</td>
    </tr>
    <tr>
      <th>26264</th>
      <td>8.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>8.7</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.907</td>
    </tr>
    <tr>
      <th>26265</th>
      <td>9.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>8.5</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.987</td>
    </tr>
    <tr>
      <th>26266</th>
      <td>10.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>9.9</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.955</td>
    </tr>
    <tr>
      <th>26267</th>
      <td>11.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>11.6</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.835</td>
    </tr>
    <tr>
      <th>26268</th>
      <td>12.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>12.0</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.791</td>
    </tr>
    <tr>
      <th>26269</th>
      <td>13.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>12.0</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.746</td>
    </tr>
    <tr>
      <th>26270</th>
      <td>14.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>13.9</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.669</td>
    </tr>
    <tr>
      <th>26271</th>
      <td>15.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>14.7</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.648</td>
    </tr>
    <tr>
      <th>26272</th>
      <td>16.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>15.1</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.740</td>
    </tr>
    <tr>
      <th>26273</th>
      <td>17.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>13.9</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.942</td>
    </tr>
    <tr>
      <th>26274</th>
      <td>18.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>12.3</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.223</td>
    </tr>
    <tr>
      <th>26275</th>
      <td>19.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>12.9</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.271</td>
    </tr>
    <tr>
      <th>26276</th>
      <td>20.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>11.8</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.234</td>
    </tr>
    <tr>
      <th>26277</th>
      <td>21.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>10.7</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>3.130</td>
    </tr>
    <tr>
      <th>26278</th>
      <td>22.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>10.3</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.962</td>
    </tr>
    <tr>
      <th>26279</th>
      <td>23.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>9.2</td>
      <td>39820270.0</td>
      <td>1383.0</td>
      <td>640803.0</td>
      <td>2.766</td>
    </tr>
  </tbody>
</table>
<p>26280 rows × 10 columns</p>
</div>



#### Составим корреляционную матрицу для определения зависимостей между параметрами и значением спроса на электроэнергию


```python
correlation = table.corr()
plt.figure(figsize=(20,20))
sns.heatmap(correlation, vmax=1, square=True,annot=True,cmap='cubehelix',linewidths=2)
plt.title('Correlation between different features')
```




    Text(0.5, 1.0, 'Correlation between different features')




![png](output_7_1.png)


#### На графиках зависимостей параметров от величины спроса рассмотрим распределение их значений


```python
plt.figure(figsize=(40,60))
with sns.axes_style('white'):
    g = sns.factorplot("Time","Demand", data=table, aspect=2,
                        color='steelblue')
    #g.set_xticklabels(step=40)
plt.title('Зависимость спроса на энергию, в зависимости от времени суток(часа)')
plt.show()
```

    C:\Users\remem\.conda\envs\python3\lib\site-packages\seaborn\categorical.py:3666: UserWarning: The `factorplot` function has been renamed to `catplot`. The original name will be removed in a future release. Please update your code. Note that the default `kind` in `factorplot` (`'point'`) has changed `'strip'` in `catplot`.
      warnings.warn(msg)
    


    <Figure size 2880x4320 with 0 Axes>



![png](output_9_2.png)



```python
plt.figure(figsize=(15,10))
with sns.axes_style('white'):
    g = sns.lineplot("Temperature","Demand", data=table,
                        color='steelblue')
    #r = sns.scatterplot("Temperature","Demand", data=table,
                        #color='steelblue')
    #g.set_xticklabels(step=40)
plt.title('Зависимость спроса на энергию, в зависимости от температуры за окном')
plt.show()
```


![png](output_10_0.png)



```python
plt.figure(figsize=(15,15))
with sns.axes_style('white'):
    g = sns.scatterplot("Season","Demand", data=table,
                        color='steelblue')
    r = sns.lineplot("Season","Demand", data=table,
                        color='steelblue')
plt.title('Зависимость спроса на энергию, в зависимости от времени года')
plt.show()
```


![png](output_11_0.png)


#### Проведем нормализацию данных


```python
COLUMNS = ['Time', 'Year', 'Month', 'Day_of_week', 'Season', 'Temperature',
       'Number_Of_Guest_Nights', 'Total_hours_worked', 'Sales_new_vehicles']
table_2 = table[COLUMNS]
```


```python
min_max_scaler = preprocessing.MinMaxScaler()
np_scaled = min_max_scaler.fit_transform(table_2)
df_normalized = pd.DataFrame(np_scaled)
df_normalized['9']=table['Demand']
```


```python
df_normalized
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.441463</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.555</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.043478</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.412195</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.433</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.086957</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.424390</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.393</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.130435</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.404878</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.375</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.173913</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.385366</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.390</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.217391</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.360976</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.467</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0.260870</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.351220</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.631</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0.304348</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.382927</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.775</td>
    </tr>
    <tr>
      <th>8</th>
      <td>0.347826</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.424390</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.965</td>
    </tr>
    <tr>
      <th>9</th>
      <td>0.391304</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.460976</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.159</td>
    </tr>
    <tr>
      <th>10</th>
      <td>0.434783</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.495122</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.211</td>
    </tr>
    <tr>
      <th>11</th>
      <td>0.478261</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.500000</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.265</td>
    </tr>
    <tr>
      <th>12</th>
      <td>0.521739</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.507317</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.160</td>
    </tr>
    <tr>
      <th>13</th>
      <td>0.565217</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.502439</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.288</td>
    </tr>
    <tr>
      <th>14</th>
      <td>0.608696</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.470732</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.361</td>
    </tr>
    <tr>
      <th>15</th>
      <td>0.652174</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.436585</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.362</td>
    </tr>
    <tr>
      <th>16</th>
      <td>0.695652</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.404878</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.406</td>
    </tr>
    <tr>
      <th>17</th>
      <td>0.739130</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.385366</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.438</td>
    </tr>
    <tr>
      <th>18</th>
      <td>0.782609</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.365854</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.501</td>
    </tr>
    <tr>
      <th>19</th>
      <td>0.826087</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.358537</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.381</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.869565</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.353659</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.242</td>
    </tr>
    <tr>
      <th>21</th>
      <td>0.913043</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.319512</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>3.084</td>
    </tr>
    <tr>
      <th>22</th>
      <td>0.956522</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.295122</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.973</td>
    </tr>
    <tr>
      <th>23</th>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>0.833333</td>
      <td>0.333333</td>
      <td>0.287805</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.811</td>
    </tr>
    <tr>
      <th>24</th>
      <td>0.000000</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.292683</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.613</td>
    </tr>
    <tr>
      <th>25</th>
      <td>0.043478</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.287805</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.492</td>
    </tr>
    <tr>
      <th>26</th>
      <td>0.086957</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.285366</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.431</td>
    </tr>
    <tr>
      <th>27</th>
      <td>0.130435</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.292683</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.403</td>
    </tr>
    <tr>
      <th>28</th>
      <td>0.173913</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.302439</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.390</td>
    </tr>
    <tr>
      <th>29</th>
      <td>0.217391</td>
      <td>0.0</td>
      <td>0.272727</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.302439</td>
      <td>0.306226</td>
      <td>0.989305</td>
      <td>0.483919</td>
      <td>2.432</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>26250</th>
      <td>0.782609</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.336585</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.520</td>
    </tr>
    <tr>
      <th>26251</th>
      <td>0.826087</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.326829</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.485</td>
    </tr>
    <tr>
      <th>26252</th>
      <td>0.869565</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.300000</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.423</td>
    </tr>
    <tr>
      <th>26253</th>
      <td>0.913043</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.270732</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.289</td>
    </tr>
    <tr>
      <th>26254</th>
      <td>0.956522</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.270732</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.085</td>
    </tr>
    <tr>
      <th>26255</th>
      <td>1.000000</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>1.000000</td>
      <td>0.333333</td>
      <td>0.280488</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.888</td>
    </tr>
    <tr>
      <th>26256</th>
      <td>0.000000</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.282927</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.686</td>
    </tr>
    <tr>
      <th>26257</th>
      <td>0.043478</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.268293</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.551</td>
    </tr>
    <tr>
      <th>26258</th>
      <td>0.086957</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.278049</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.506</td>
    </tr>
    <tr>
      <th>26259</th>
      <td>0.130435</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.273171</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.483</td>
    </tr>
    <tr>
      <th>26260</th>
      <td>0.173913</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.287805</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.477</td>
    </tr>
    <tr>
      <th>26261</th>
      <td>0.217391</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.290244</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.494</td>
    </tr>
    <tr>
      <th>26262</th>
      <td>0.260870</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.295122</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.566</td>
    </tr>
    <tr>
      <th>26263</th>
      <td>0.304348</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.285366</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.714</td>
    </tr>
    <tr>
      <th>26264</th>
      <td>0.347826</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.300000</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.907</td>
    </tr>
    <tr>
      <th>26265</th>
      <td>0.391304</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.295122</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.987</td>
    </tr>
    <tr>
      <th>26266</th>
      <td>0.434783</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.329268</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.955</td>
    </tr>
    <tr>
      <th>26267</th>
      <td>0.478261</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.370732</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.835</td>
    </tr>
    <tr>
      <th>26268</th>
      <td>0.521739</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.380488</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.791</td>
    </tr>
    <tr>
      <th>26269</th>
      <td>0.565217</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.380488</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.746</td>
    </tr>
    <tr>
      <th>26270</th>
      <td>0.608696</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.426829</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.669</td>
    </tr>
    <tr>
      <th>26271</th>
      <td>0.652174</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.446341</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.648</td>
    </tr>
    <tr>
      <th>26272</th>
      <td>0.695652</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.456098</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.740</td>
    </tr>
    <tr>
      <th>26273</th>
      <td>0.739130</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.426829</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.942</td>
    </tr>
    <tr>
      <th>26274</th>
      <td>0.782609</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.387805</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.223</td>
    </tr>
    <tr>
      <th>26275</th>
      <td>0.826087</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.402439</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.271</td>
    </tr>
    <tr>
      <th>26276</th>
      <td>0.869565</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.375610</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.234</td>
    </tr>
    <tr>
      <th>26277</th>
      <td>0.913043</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.348780</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>3.130</td>
    </tr>
    <tr>
      <th>26278</th>
      <td>0.956522</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.339024</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.962</td>
    </tr>
    <tr>
      <th>26279</th>
      <td>1.000000</td>
      <td>1.0</td>
      <td>0.181818</td>
      <td>0.000000</td>
      <td>0.333333</td>
      <td>0.312195</td>
      <td>0.194329</td>
      <td>0.427807</td>
      <td>0.960125</td>
      <td>2.766</td>
    </tr>
  </tbody>
</table>
<p>26280 rows × 10 columns</p>
</div>



#### Разобьем выборку на тренировочную и валидационную


```python
from sklearn.model_selection import train_test_split
train , test = train_test_split(table, test_size = 0.2, random_state=42)
```


```python
train.to_csv('train_4.csv',index=False)
test.to_csv('test_4.csv',index=False)
```

# !Attention
# Обучение модели проводилось на онлайн-платформе GoogleColab
## Код ниже для воспроизведения непосредственно там

#### Подгружаем нашу выборку


```python
from google.colab import files
uploaded = files.upload()
!ls
```

#### Импортируем модули


```python
import numpy as np #модуль для численных манипуляций с большим объемом данных
import pandas as pd #модуль для работы с таблицами
from sklearn.ensemble import GradientBoostingRegressor #модуль для градиентного бустинга
from sklearn.metrics import mean_squared_error #для подсчета значения MAE - абсолютной ошибки
import seaborn as sns #для отрисовки графиков
from math import sqrt
import matplotlib.pyplot as plt
def rmse(y_true, y_pred):
    return sqrt(mean_squared_error(y_true, y_pred))
!pip install catboost
%matplotlib inline
```

 #### Переводим файлы в таблицы pandas


```python
train = pd.read_csv('train_4.csv')
test = pd.read_csv('test_4.csv')
train = train.replace(np.nan,0)
test = test.replace(np.nan, 0)
```

#### Разделяем валидационную и тренировочную выборки для проведения обучения


```python
test.columns
```


```python
COLUMNS = ['Time', 'Year', 'Month', 'Day_of_week', 'Season', 'Temperature',
       'Number_Of_Guest_Nights', 'Total_hours_worked', 'Sales_new_vehicles']
           
y_train = train['Demand'].values
X_train = train[COLUMNS].values
X_test = test[COLUMNS].values
y_test = test['Demand'].values
```

#### Запускаем обучение модели


```python
from catboost import Pool, CatBoostRegressor
train_pool = Pool(X_train, y_train)#, cat_features=[0,2,5])
test_pool = Pool(X_test)
model = CatBoostRegressor(iterations=8000, depth=8, learning_rate=0.3, loss_function='RMSE')
#train the model
model.fit(train_pool,plot = True)

```

#### Спрогнозируем спрос на электроэнергию на тестовой выборке, подсчитаем ошибку по метрике RMSE


```python
preds = model.predict(test_pool)
sqrt(mean_squared_error(y_test, preds))
```


```python
output_data = pd.DataFrame({'Real_Demand': [],'Predict_Demand': [], 'Temperature': []})
output_data['Real_Demand'] = preds
output_data['Predict_Demand'] = y_test
output_data['Temperature'] = test['Temperature']
output_data
```


#### Построим графики зависимости спрогнозированного и реального спроса на энергию от температуры


```python
plt.figure(figsize=(65,15))
sns.set(style="whitegrid")
# RGB:

sns.lineplot(output_data['Temperature'],output_data['Real_Demand'], palette='tab10',color='green', linewidth=4).get_children()[0].set_color('w')
# RGBA:
sns.lineplot(output_data['Temperature'],output_data['Predict_Demand'], palette='tab10',color='red', linewidth=4).get_children()[1].set_color('w')
plt.title('Зависимость спроса на энергию, в зависимости от температуры, ')
plt.show()
```


```python

