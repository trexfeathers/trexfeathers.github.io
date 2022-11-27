## How I made across-Devon postbox orienteering

### Why?

It is always possible to plan your own course on an A-Z or Ordnance Survey 
map, using the honesty system to check course completion; but we all know it's
much more enjoyable when someone hands us a specialised map, with a 
prepared course, and we get the reassurance of visiting actual controls on 
our run.

Technology makes this authentic experience increasingly accessible so that
low-key events can now have some of the quality previously reserved for
high-calibre events:

- Mapping - [OpenOrienteeringMap](https://oomap.co.uk/) can automatically 
generate orienteering-style maps for urban areas.
- Course laying - [MapRun](https://maprunners.weebly.com/) instantly confirms
that competitors have visited a control.
- Course setting - **read on!**

Removing the need for specific course setting would bring the authentic 
experience to even the most informal orienteering - like individual training 
or challenges between friends - and also make more frequent orienteering
possible without upping the resources needed.

### How?

Create a geographically massive score event in MapRun, with the
[Start Anywhere](https://maprunners.weebly.com/start-anywhere.html) feature
enabled, so that it would take many runs to experience the whole area - an 
orienteer could enjoy this single course many times before getting bored.

Of course, there are likely to be only a few start 
locations that will yield high scores, but that's only one of the ways to 
enjoy such a course (see the [Ideas](postbox_o.md#ideas) on the main page).

### Positioning controls

Don't want to be positioning manually, as that defeats the reduced-effort
philosophy. Need to use some existing location data...

#### OpenStreetMap

[OpenSteetMap](https://www.openstreetmap.org/) includes precise point locations 
for many municipal objects 
such as postboxes, bus stops and benches. The large international community 
means it is relatively easy to query OSM for this data, such as
[this example for all postboxes in Devon](https://overpass-turbo.eu/s/1ogb).

#### Postboxes

Postboxes have long been used as controls in street orienteering, which is why
OpenOrienteeringMap includes an option for automatically creating controls 
at every postbox location within the mapped area
([read more](https://blog.oomap.co.uk/2015/01/oom-2-3-automatic-postbox-additions/)). This is ideal 
because it means orienteers can use OpenOrienteeringMap to both produce a 
map of their chosen sub-area, and also populate it with the controls.

OpenOrienteeringMap's postbox data comes from
[Matthew Somerville's postbox finder](https://postboxes.dracos.co.uk/), 
which itself feeds from OpenStreetMap, but also other sources which 
unfortunately means occasional discrepancies with the information I'm fetching
via OpenStreetMap queries.

#### Alternative features

E.g. bus stops. Don't get the convenience of OpenOrienteeringMap's automation,
but orienteers would still have several other options for producing their map:

* Produce the base map from OpenOrienteeringMap, then manually mark the control
locations by cross-checking the original OpenStreetMap information.
* Print from OpenStreetMap, admittedly a less usable format for fast navigation.
* Navigate directly from the MapRun app's screen.

### Creating the MapRun

MapRun needs a KML file listing the controls and their coordinates. While a KML 
file can be downloaded from OpenStreetMap quering services (such as
[Overpass](https://overpass-turbo.eu/)), this isn't enough since the controls
need to be numbered using a `<name>` element, and that isn't present.

So I used the below script to extract what I needed from an Overpass query 
output, and place it in a correctly formatted KML file, including `S1` and `F1`
controls (positioned according to 
[the Start Anywhere notes](https://maprunners.weebly.com/start-anywhere.html)):

<details>
<summary>Expand for Python script</summary>
<pre>

```python
from copy import deepcopy
from urllib import parse as url_parse
from xml.etree import ElementTree

import requests

# 57538 = Devon.
# Use https://www.openstreetmap.org/relation/57538 to search for desired area.
relation_id = 57538

query_lines = [
    f'area(id:36000{relation_id})',
    'node["amenity"="post_box"](area)',
    'out',
    '',
]
query = url_parse.quote(";".join(query_lines))
url = "https://overpass-api.de/api/interpreter?data=" + query
response = requests.get(url)

source_xml = ElementTree.fromstring(response.content)

output_kml = ElementTree.Element(
    "kml",
    attrib=dict(xmlns="http://earth.google.com/kml/2.0")
)
output_content = ElementTree.SubElement(output_kml, "Document")

postboxes = source_xml.findall("node")
for control_number, control_details in enumerate(postboxes, start=1):
    placemark = ElementTree.SubElement(output_content, "Placemark")

    name_ = ElementTree.SubElement(placemark, "name")
    name_.text = str(control_number)

    point = ElementTree.SubElement(placemark, "Point")

    lon = control_details.attrib["lon"]
    lat = control_details.attrib["lat"]
    coordinates = ElementTree.SubElement(point, "coordinates")
    coordinates.text = f"{lon},{lat}"

# Need arbitrary start-finish controls, even for the start-anywhere format.
#  Create by duplicating first control.
for new_name, position in [("F1", len(output_content)), ("S1", 0)]:
    new_control = deepcopy(output_content[0])
    name_element = new_control.find("name")
    name_element.text = new_name
    output_content.insert(position, new_control)

ElementTree.ElementTree(output_kml).write(f"postboxes_in_{relation_id}.kml")

```
</pre>

</details>
<br/>

Then it was a case of following the [MapRun course creation
guidelines](https://maprunners.weebly.com/step-by-step-guide.html), uploading
the just-created KML file. I **did not** provide a KMZ 
background file, as this is optional. Without one, MapRun just uses 
OpenStreetMap as a background. Specific settings:

- [Queensland score format](https://maprunners.weebly.com/scoring-schemes.html)
- 60 minute time limit
- [Enable Start Anywhere](https://maprunners.weebly.com/start-anywhere.html)
