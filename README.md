# SLIDE RULE DSI XML PROJECT



```python
# XML exercise and solution
# Solutions to exercise of accessing nodes in XML tree structure

# reference: https://docs.python.org/2.7/library/xml.etree.elementtree.html
# data source: http://www.dbis.informatik.uni-goettingen.de/Mondial

# Data in 'data/mondial_database.xml', used, and refering to 
# https://docs.python.org/2.7/library/xml.etree.elementtree.html
```


```python
# 1. Find 10 countries with the lowest infant mortality rates
# Import the data by Reading the file from directory
from xml.etree import ElementTree as ET
tree = ET.parse('./data/mondial_database.xml')
root = tree.getroot()
# Empty array of low countries with low mortality eartes
low_mortality_countries = []
for country in root.findall('./country'):
    data = {
        'name': None,
        'infant_mortality': None
    }
    data['name'] = country.find('./name').text
    data['infant_mortality'] = country.findtext('./infant_mortality')
    if data['infant_mortality'] is not None:
        low_mortality_countries.append((float(data['infant_mortality']), data['name']))
        #print (len(low_mortality_countries))
        if len(low_mortality_countries) is 228:
            low_mortality_countries.sort()
            print(low_mortality_countries[:10])

```

    [(1.81, 'Monaco'), (2.13, 'Japan'), (2.48, 'Bermuda'), (2.48, 'Norway'), (2.53, 'Singapore'), (2.6, 'Sweden'), (2.63, 'Czech Republic'), (2.73, 'Hong Kong'), (3.13, 'Macao'), (3.15, 'Iceland')]
    


```python
# 2. Find 10 cities with the largest population
import operator
from xml.etree import ElementTree as ET
tree = ET.parse('./data/mondial_database.xml')
root = tree.getroot()
cities_largest_pop = []
for city in root.findall('./country/city'):
    data = {
        'name': None,
        'population': None
    }
    data['name'] = city.findtext('./name')
    data['population'] = city.findtext('./population[last()]')
    if data['population'] is not None and float(data['population']) > 0:
        cities_largest_pop.append((float(data['population']), data['name']))
        #print (len(cities_largest_pop))
        if len(cities_largest_pop) is 256:
            cities_largest_pop.sort()
            print(cities_largest_pop[-10:])
```

    [(3255288.0, 'Pyongyang'), (3403135.0, 'Busan'), (3939305.0, 'New Taipei'), (4123869.0, 'Al Iskandariyah'), (5076700.0, 'Singapore'), (5968384.0, 'Ho Chi Minh'), (7055071.0, 'Hong Kong'), (7506700.0, 'Bangkok'), (8471859.0, 'Al Qahirah'), (9708483.0, 'Seoul')]
    


```python
# 3. Find 10 ethnic groups with the largest overall populations 
# (sum of best/latest estimates over all countries)
# Import the data by Reading the file from directory
import operator
from xml.etree import ElementTree as ET
tree = ET.parse('./data/mondial_database.xml')
root = tree.getroot()
Ethnic_Grps = []
for country in root.findall('./country'):
    data = {
        'name': None,
        'population': None,
        'ethnicgroup': None
    }
    data['name'] = country.findtext('./name')
    population_list = int(country.findtext('./population[last()]'))
    for ethnicgroup in country.findall('./ethnicgroup[1]'):
        data['ethnicgroup']= list(ethnicgroup.itertext())
        ethnic = country.find('./ethnicgroup[1]')
        ethnicperc = float(ethnic.get('percentage'))/100
        data['population'] = population_list*ethnicperc
        if data['population'] is not None and data['ethnicgroup'] is not None:
            Ethnic_Grps.append((float(data['population']), data['name'], data['ethnicgroup']))
            #print (len(Ethnic_Grps))
            if len(Ethnic_Grps) is 197:
                Ethnic_Grps.sort()
                print(Ethnic_Grps[-10:])
```

    [(76078375.3, 'Vietnam', ['Viet/Kinh']), (108886717.794, 'Brazil', ['European']), (113456006.10000001, 'Indonesia', ['Javanese']), (114646210.938, 'Russia', ['Russian']), (126534212.00000001, 'Japan', ['Japanese']), (146776916.72, 'Bangladesh', ['Bengali']), (162651570.84, 'Nigeria', ['African']), (254958101.97759998, 'United States', ['European']), (302713744.25, 'India', ['Dravidian']), (1245058800.0, 'China', ['Han Chinese'])]
    


```python
# 4 a. Find name and country of the longest river
longest_river = {}
for river in root.iterfind('river'):
    name = river.get('id')
    #print (name)
    country = river.get('country')
    #print (country)
    length = river.find('./length')
    if length is None:
        pass
    else:
        #print (float(length.text))
        longest_river[name, country]=float(length.text)
sorted_longest_river = sorted(longest_river.items(), key=operator.itemgetter(1), reverse=True)[:1]
sorted_longest_river
```




    [(('river-Amazonas', 'CO BR PE'), 6448.0)]




```python
# 4 b. Find name and country of the largest lake
largest_lake = {}
for lake in root.iterfind('lake'):
    name = lake.get('id')
    #print (name)
    country = lake.get('country')
    #print (country)
    area = lake.find('./area')
    if area is None:
        pass
    else:
        #print (float(area.text))
        largest_lake[name, country]=float(area.text)
sorted_largest_lake = sorted(largest_lake.items(), key=operator.itemgetter(1), reverse=True)[:1]
sorted_largest_lake
```




    [(('lake-KaspischesMeer', 'R AZ KAZ IR TM'), 386400.0)]




```python
# 4 c. Find name and country of the airport at highest elevation
import operator
from xml.etree import ElementTree as ET
tree = ET.parse('./data/mondial_database.xml')
root = tree.getroot()
highest_elev_airport = {}
for airport in root.iterfind('airport'):
    name = airport.get('iatacode')
    #print (name)
    country = airport.get('country')
    #print (country)
    elevation = airport.findtext('./elevation')
    if elevation is None:
        pass
    elif elevation=='':
        pass
    else:
        #print (float(elevation.text))
        highest_elev_airport[name, country]=float(elevation)
sorted_highest_elev_airport = sorted(highest_elev_airport.items(), key=operator.itemgetter(1), reverse=True)[:1]
sorted_highest_elev_airport
```




    [(('LPB', 'BOL'), 4063.0)]




```python

```


```python

```