> **Note:** This code has not been tested. Before using it, check that the syntax and logic are correct and that it runs in your environment. Paths, file names, and dependencies may need to be adjusted.

---

# PRACTICAL 2

---

## CSV to HORUS

```python
import pandas as pd

df = pd.read_csv('/content/country_code.csv', encoding='latin-1')
df = (
df.drop(['ISO-2-CODE','ISO-3-Code'], axis=1)
.rename(columns={'Country':'CountryName','ISO-M49':'CountryNumber'})
.set_index('CountryNumber')
.sort_values('CountryName', ascending=False)
)
df.to_csv('/content/Hourous.csv')
print('Done')
```

---

## XML to HORUS

```python
import pandas as pd
import xml.etree.ElementTree as ET

def xml2df(x):
r = ET.XML(x)
return pd.DataFrame([{c.tag: c.text for c in e} for e in r])

xml_raw = open('/content/Country_Code.xml').read()
df = (
xml2df(xml_raw)
.drop(['ISO-2-CODE', 'ISO-3-Code'], axis=1)
.rename(columns={'Country': 'CountryName', 'ISO-M49': 'CountryNumber'})
.set_index('CountryNumber')
.sort_values('CountryName', ascending=False)
)
df.to_csv('/content/HORUS-XML-Country.csv')
print('XML to HORUS - Done')
```

---

## JSON to HORUS

```python
import pandas as pd

df = (
pd.read_json('/content/Country_Code.json', orient='index', encoding='latin-1')
.drop(['ISO-2-CODE', 'ISO-3-Code'], axis=1)
.rename(columns={'Country': 'CountryName', 'ISO-M49': 'CountryNumber'})
.set_index('CountryNumber')
.sort_values('CountryName', ascending=False)
.reset_index()
)
df.to_csv('/content/HORUS-JSON-Country.csv', index=False)
print('JSON to HORUS - Done')
```

---

## Database to HORUS

```python
import pandas as pd
import sqlite3 as sq

con = sq.connect('/content/utility.db')
df = pd.read_sql_query('SELECT \* FROM Country_Code;', con)
df = (
df.drop(['ISO-2-CODE', 'ISO-3-Code'], axis=1)
.rename(columns={'Country': 'CountryName', 'ISO-M49': 'CountryNumber'})
.set_index('CountryNumber')
.sort_values('CountryName', ascending=False)
.reset_index()
)
df.to_csv('/content/HORUS-CSV-Country.csv', index=False)
print('Database to HORUS - Done')
```

---

## Image to HORUS

```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import imageio.v3 as iio

img = iio.imread('/content/Angus.jpg')
h, w, c = img.shape

x, y = np.meshgrid(np.arange(w), np.arange(h))
pixels = img.reshape(-1, c)

if c == 3:
pixels = np.hstack([pixels, np.full((h*w, 1), 255, np.uint8)])
elif c != 4:
raise ValueError(f"Unsupported channels: {c}")

data = np.column_stack([x.flatten(), y.flatten(), pixels])
df = pd.DataFrame(data, columns=['XAxis','YAxis','Red','Green','Blue','Alpha'])
df.index.name = 'ID'

plt.imshow(img)
plt.show()

df.to_csv('/content/HORUS-Picture.csv', index=False)
print('Picture to HORUS - Done')
```

---

# PRACTICAL 3

---

## Fixers Utilities

```python
import string
import datetime as dt

# 1 Removing leading or lagging spaces from a data entry

print('#1 Removing leading or lagging spaces from a data entry');
baddata = " Data Science with too many spaces is bad!!! "
print('>',baddata,'<')
cleandata=baddata.strip()
print('>',cleandata,'<')

# 2 Removing nonprintable characters from a data entry

print('#2 Removing nonprintable characters from a data entry')
printable = set(string.printable)
baddata = "Data\x00Science with\x02 funny characters is \x10bad!!!"
cleandata=''.join(filter(lambda x: x in string.printable,baddata))
print('Bad Data : ',baddata);
print('Clean Data : ',cleandata)

# 3 Reformatting data entry to match specific formatting criteria.

# Convert YYYY/MM/DD to DD Month YYYY

print('# 3 Reformatting data entry to match specific formatting criteria.')
baddate = dt.date(2019, 10, 31)
baddata=format(baddate,'%Y-%m-%d')
gooddate = dt.datetime.strptime(baddata,'%Y-%m-%d')
gooddata=format(gooddate,'%d %B %Y')
print('Bad Data : ',baddata)
print('Good Data : ',gooddata)
```

---

## Data Binning or Bucketing

```python
import numpy as np
import matplotlib.pyplot as plt
import scipy.stats as stats

np.random.seed(0)
mu, sigma = 90, 25
x = mu + sigma \* np.random.randn(5000)
num_bins = 25

fig, ax = plt.subplots()
n, bins, \_ = ax.hist(x, num_bins, density=True)
ax.plot(bins, stats.norm.pdf(bins, mu, sigma), '--')
ax.set_xlabel('Example Data')
ax.set_ylabel('Probability Density')
ax.set_title(f'Histogram {len(x)} entries into {num_bins} Bins: μ={mu}, σ={sigma}')

fig.tight_layout()
fig.savefig('/content/DU-Histogram.png')
plt.show()
```

---

## Averaging of Data

```python
import pandas as pd

df = pd.read_csv('/content/IP_DATA_CORE.csv', usecols=['Country','Place Name','Latitude','Longitude'],
encoding='latin-1', low_memory=False)
df = df.rename(columns={'Place Name': 'Place_Name'})[['Country','Place_Name','Latitude']]
print(df)

mean_data = df.groupby(['Country','Place_Name'])['Latitude'].mean()
print(mean_data)
```

---

## Outlier Detection

```python
import pandas as pd

df = pd.read_csv('/content/IP_DATA_CORE.csv',
usecols=['Country','Place Name','Latitude','Longitude'],
encoding='latin-1', low_memory=False
).rename(columns={'Place Name': 'Place_Name'})

df = df[df.Place_Name == 'London'][['Country','Place_Name','Latitude']]
print(df)

mean = df['Latitude'].mean()
std = df['Latitude'].std()

upper = mean + std
lower = mean - std

print("Outliers Higher:", upper)
print(df[df.Latitude > upper])

print("Outliers Lower:", lower)
print(df[df.Latitude < lower])

print("Not Outliers")
print(df[(df.Latitude >= lower) & (df.Latitude <= upper)])
```

---

## Logging

```python
import os, logging, uuid, shutil, time

companies = ['01-Vermeulen','02-Krennwallner','03-Hillman','04-Clark']
layers = ['01-Retrieve','02-Assess','03-Process','04-Transform','05-Organise','06-Report']
levels = ['debug','info','warning','error']

for comp in companies:
for layer in layers:
log_dir = f'/content/{comp}/{layer}/Logging'

        os.makedirs(log_dir, exist_ok=True)
        shutil.rmtree(log_dir, ignore_errors=True)
        time.sleep(1)
        os.makedirs(log_dir, exist_ok=True)

        log_file = f'{log_dir}/Logging_{uuid.uuid4()}.log'
        print('Set up:', log_file)

        logging.shutdown()
        logging.basicConfig(
            level=logging.DEBUG,
            format='%(asctime)s %(name)-12s %(levelname)-8s %(message)s',
            datefmt='%m-%d %H:%M',
            filename=log_file,
            filemode='w'
        )

        console = logging.StreamHandler()
        console.setLevel(logging.INFO)
        console.setFormatter(logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s'))
        logging.getLogger('').addHandler(console)

        logging.info('Practical Data Science is fun!')

        for lvl in levels:
            logger = logging.getLogger(f"App-{comp}-{layer}-{lvl}")
            getattr(logger, lvl)(f"Logged {lvl} message.")
```

---

# PRACTICAL 4

---

## Retrieve different attributes of data

```python
import os
import pandas as pd

print('Loading: /content/IP_DATA_ALL.csv')
df = pd.read_csv('/content/IP_DATA_ALL.csv', low_memory=False, encoding='latin-1')
os.makedirs('/content/02-Python', exist_ok=True)

print('Rows:', df.shape[0])
print('Columns:', df.shape[1])
print('### Raw Data Set #####################################')
for c in df.columns:
print(c, type(c))

print('### Fixed Data Set ###################################')
df_fix = df.copy()
df_fix.columns = [c.strip().replace(' ', '.') for c in df_fix.columns]
for c in df_fix.columns:
print(c, type(c))

print('Fixed Data Set with ID')
df_fix.index.name = 'RowID'
df_fix.to_csv('/content/02-Python/Retrieve_IP_DATA.csv', index=True, encoding='latin-1')
print('### Done!! ############################################')
```

---

## Loading IP_DATA_ALL

```python
import sys
import os
import pandas as pd

print('Loading: /content/IP_DATA_ALL.csv')
df = pd.read_csv('/content/IP_DATA_ALL.csv', low_memory=False, encoding='latin-1')
os.makedirs('/content/02-Python', exist_ok=True)

print('Rows:', df.shape[0])
print('Columns:', df.shape[1])
print('### Raw Data Set #####################################')
for c in df.columns:
print(c, type(c))

print('### Fixed Data Set ###################################')
df_fix = df.copy()
df_fix.columns = [c.strip().replace(' ', '.') for c in df_fix.columns]
for c in df_fix.columns:
print(c, type(c))

print('Fixed Data Set with ID')
df_fix.index.name = 'RowID'
df_fix.to_csv('/content/02-Python/Retrieve_IP_DATA.csv', index=True, encoding='latin-1')
print('### Done!! ############################################')
```

---

# PRACTICAL 5

---

## Perform error management on the given data using pandas package

```python
import sys
import os
import pandas as pd

os.makedirs('/content/01-Vermeulen/02-Assess/01-EDS/02-Python', exist_ok=True)

print('################################')
print('Working: /content/ using', sys.platform)
print('################################')
print('Loading: /content/Good-or-Bad.csv')
raw = pd.read_csv('/content/Good-or-Bad.csv')

print('################################')
print('## Raw Data Values')
print('################################')
print(raw)
print('Rows:', raw.shape[0])
print('Columns:', raw.shape[1])
print('################################')

raw.to_csv('/content/01-Vermeulen/02-Assess/01-EDS/02-Python/Good-or-Bad.csv', index=False)
test = raw.dropna(axis=1, how='all')

print('################################')
print('## Test Data Values')
print('################################')
print(test)
print('Rows:', test.shape[0])
print('Columns:', test.shape[1])
print('################################')

test.to_csv('/content/01-Vermeulen/02-Assess/01-EDS/02-Python/Good-or-Bad-01.csv', index=False)
print('################################')
print('### Done!! #####################')
print('################################')
```

---

## Write a python/R program to build acyclic graph

```python
import networkx as nx
import matplotlib.pyplot as plt
import sys
import os
import pandas as pd

print('Working: /content/ using', sys.platform)
print('Loading: /content/Retrieve_Router_Location.csv')
df = pd.read_csv('/content/Retrieve_Router_Location.csv', encoding='latin-1')
print(df)
print('Rows:', df.shape[0])

G1, G2 = nx.DiGraph(), nx.DiGraph()
for \_, row in df.iterrows():
G1.add_node(row['Country'])
G2.add_node(f"{row['Place_Name']}-{row['Country']}")

for g in [G1, G2]:
nodes = list(g.nodes())
for n1 in nodes:
for n2 in nodes:
if n1 != n2:
g.add_edge(n1, n2)

os.makedirs('/content/01-Vermeulen/02-Assess/01-EDS/02-Python', exist_ok=True)

nx.draw(G1, pos=nx.spectral_layout(G1),
node_color='r', edge_color='g',
with_labels=True, node_size=8000, font_size=12)
plt.savefig('/content/01-Vermeulen/02-Assess/01-EDS/02-Python/Assess-DAG-Company-Country.png')
plt.show()

nx.draw(G2, pos=nx.spectral_layout(G2),
node_color='r', edge_color='b',
with_labels=True, node_size=8000, font_size=12)
plt.savefig('/content/01-Vermeulen/02-Assess/01-EDS/02-Python/Assess-DAG-Company-Country-Place.png')
plt.show()
```

---

## Write python/R program to pick the content for BillBoards from the given data

```python
import sys
import os
import sqlite3 as sq
import pandas as pd

print('################################')
print('Working: /content/ using', sys.platform)
print('################################')

os.makedirs('/content/02-Krennwallner/02-Assess/SQLite', exist_ok=True)
conn = sq.connect('/content/02-Krennwallner/02-Assess/SQLite/krennwallner.db')

print('Loading: /content/Retrieve_DE_Billboard_Locations.csv')
bill = pd.read_csv('/content/Retrieve_DE_Billboard_Locations.csv', encoding='latin-1', low_memory=False).drop_duplicates()
print('Columns:', bill.columns.tolist())
bill.to_sql('Assess_BillboardData', conn, if_exists='replace')
print(bill.head(), '\nRows:', bill.shape[0])

print('Loading: /content/Retrieve_Online_Visitor.csv')
vis = pd.read_csv('/content/Retrieve_Online_Visitor.csv', encoding='latin-1', low_memory=False)
vis = vis.drop_duplicates()
vis = vis[vis.Country == 'DE']
vis.to_sql('Assess_VisitorData', conn, if_exists='replace')
print(vis.head(), '\nRows:', vis.shape[0])

sql = """
SELECT DISTINCT
A.Country AS BillboardCountry,
A.Place_Name AS BillboardPlaceName,
A.Latitude AS BillboardLatitude,
A.Longitude AS BillboardLongitude,
B.Country AS VisitorCountry,
B.Place_Name AS VisitorPlaceName,
B.Latitude AS VisitorLatitude,
B.Longitude AS VisitorLongitude,
(B.Last_IP_Number - B.First_IP_Number) _ 365.25 _ 24 \* 12 AS VisitorYearRate
FROM Assess_BillboardData A
JOIN Assess_VisitorData B
ON A.Country = B.Country
AND A.Place_Name = B.Place_Name;
"""
bv = pd.read_sql_query(sql, conn)
bv.to_sql('Assess_BillboardVistorsData', conn, if_exists='replace')
print(bv.head(), '\nRows:', bv.shape[0])

os.makedirs('/content/02-Krennwallner/02-Assess/01-EDS/02-Python', exist_ok=True)
bv.to_csv('/content/02-Krennwallner/02-Assess/01-EDS/02-Python/Assess-DE-Billboard-Visitor.csv', index=False)

print('################################')
print('Saved: /content/02-Krennwallner/02-Assess/01-EDS/02-Python/Assess-DE-Billboard-Visitor.csv')
print('################################')
print('### Done!! ############################################')
```

---

## Placeholder / TODO sections (Practical 5)

- Write a python/R program to generate GML file from given csv file
- Write python/R program to plan location of warehouse from the given data
- Write python/R program using data science via clustering to determine new warehouse using the given data
- Using the given data Write python/R program to plan the shipping routers from best-fit international logistics
- Write python/R program to delete the best packing option to ship in container from the given data
- Write python program to create delivery route using the given data
- Write python program to create simple forex trading planner from the given data
- Write python program to process the balance sheet to ensure the only good data is processing
- Write python program to generate payroll from the given data

---

# PRACTICAL 6

## Build the time hub, links and satellites

*(No code or notes yet)*

---

# PRACTICAL 7

## Transforming data

*(No code or notes yet)*

---

# PRACTICAL 8

## Organizing data

*(No code or notes yet)*

---

# PRACTICAL 9

## Generating data

*(No code or notes yet)*

---

# PRACTICAL 10

## Data visualisation using Power BI

*(No code or notes yet)*
