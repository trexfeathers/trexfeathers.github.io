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
