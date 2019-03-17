# Getting Raw Data

You can access all of our database using two methods:

## Using [re:dash](https://redash.io)

This is an online SQL console in which you can execute complex SQL queries and fetch specific data.

How do I use it?
- Open [Hasadna organisation in re:dash](https://data.obudget.org) in your browser
- Log in using any Google account you have (click on 'Log in with Google' - it will open an account chooser)
- Click on 'Create Query'
- Select 'obudget-next' in the source selection on the left (below the query's title)
- Type your query in the big text editing box
- Click 'Execute'
- When the results are to your liking, you can:
  - Save the Query and share the link with others
  - Create a visualisation (also can be shared)
  - Download the results in CSV or XLS formats


## Using our flat files

This method allows you to get an entire dataset for bulk operations.

How do I use it?
- Open our [raw dataset store](https://next.obudget.org/datapackages/) in your browser
- Select a dataset that you'd like to download (if you're not sure which to choose, use the timestamps and filesizes as cues, or simply ask someone from the Project's team for directions)
- Locate the `datapackage.json` file, and write down the URL for it (e.g. `https://next.obudget.org/datapackages/entities/all/datapackage.json`)
- You can then use the datapackage Python library to load the data or download it:

```python
import datapackage

URL='https://next.obudget.org/datapackages/entities/all/datapackage.json'

p=datapackage.Package(URL)
print(p.resources)
# [<datapackage.resource.Resource object at 0x10e54eef0>]

# You can download this file if you want:
print(p.resources[0].source)
# 'https://next.obudget.org/datapackages/entities/all/data/entities.csv'

# Or you can iterate directly on the data:
it = p.resources[0].iter(keyed=True)
next(it)
# {'details': {'city': 'ירושלים', 'house_number': '3', 'kind_he': 'משרד ממשלתי', 'street': 'הנשיא', 'zipcode': '9218801'}, 'id': '500100011', 'kind': 'government_office', 'kind_he': 'משרד ממשלתי', 'name': 'משרד נשיא המדינה', 'name_en': None}

```
