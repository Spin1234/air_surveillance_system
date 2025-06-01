# air_surveillance_system

### The purpose of this system is to detect any aircraft within a specified airspace range based on latitude and longitude

We have a latitude and longitude, now we want a bounding box of R km range around the base latitude and longitude. In order to calculate the bounding box we need four points, those four points are latitude minimum, latitude maximum, longitude minimum and longitude maximum. Here is the mathematical concepts and calculations to get those four points to get the bounding box:

---

### ✅ **Part 1: Why is 1 degree ≈ 111 km?**

The Earth's circumference is approximately **40,075 km** around the equator. A full circle is **360 degrees**, so:

$$
\frac{40,075 \text{ km}}{360^\circ} \approx 111.32 \text{ km per degree (latitude)}
$$

So:

* **1 degree of latitude** ≈ **111.32 km** anywhere on Earth.

#### 🔹 For Longitude:

Longitude lines converge at the poles, so the distance **1° of longitude** covers depends on your latitude:

$$
\text{Length of 1° of longitude} = 111.32 \times \cos(\text{latitude in radians})
$$

Example (at latitude 22.7°):

```python
import math
lat = 22.7
lon_distance = 111.32 * math.cos(math.radians(lat))
print(lon_distance)  # ≈ 102.6 km
```

---

### ✅ **Part 2: How to Get Bounding Box for N km Around a Point**

To approximate a circle of radius **R km** around a point (lat, lon), we calculate the bounding box:

#### Formula:

```python
degree_lat = R / 111.0
degree_lon = R / (111.32 * cos(latitude))
```

So:

```python
from math import cos, radians

def get_bounding_box(lat, lon, radius_km):
    delta_lat = radius_km / 111.0
    delta_lon = radius_km / (111.32 * cos(radians(lat)))

    return {
        "lamin": lat - delta_lat,
        "lamax": lat + delta_lat,
        "lomin": lon - delta_lon,
        "lomax": lon + delta_lon,
    }
```

#### Example (for 22.7093873, 88.1673183 and radius 15 km):

```python
bbox = get_bounding_box(22.7093873, 88.1673183, 15)
print(bbox)
```

Result (approx):

```python
{
 'lamin': 22.5747,
 'lamax': 22.8440,
 'lomin': 88.0274,
 'lomax': 88.3072
}
```

---

### ✅ Summary:

| Type      | Per Degree               |
| --------- | ------------------------ |
| Latitude  | \~111.32 km (constant)   |
| Longitude | \~111.32 × cos(latitude) |

**- By Sayak Pal**
