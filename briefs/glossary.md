# Field glossary supplied to the model

This text was injected verbatim between the persona and the spatial brief in every
prompt of the benchmark. Its identifier in the run metadata (`glossary_sha256` in
`benchmark/runs/**/*.jsonl`) is `fa7ee737a14a`, the first twelve hex digits of its
SHA-256. It is reproduced as Appendix B of the paper.

---

The brief contains the following fields.

- retrieval_method: how the area was defined. network_distance_m is the walking distance budget; block_depth_m is how far the catchment reaches off the streets to fill the blocks between them. The area is a network catchment along the streets, not a circle.
- location / point: the country, region, and city the point falls in, and its latitude and longitude.
- population: the number of residents living within the catchment, from GHS-POP, a modelled RESIDENTIAL population (2021 estimate) that counts people who live there, not daytime workers or visitors. In commercial or mixed-use areas this figure can be low even where the place is busy. Per-resident ratios such as poi_per_1000_residents are based on this residential figure.
- road_length_m: the total length of walkable streets reachable within the catchment, in metres.
- poi_total / poi_by_category: the count of points of interest (amenity, shop, and leisure features recorded in OpenStreetMap) found inside the catchment, broken down by category. The categories listed are those recorded within the catchment.
- indicators: pre-computed metrics. catchment_area_m2 is the area of the filled catchment. population_density_per_km2 is residents per square kilometre. building_count is the number of buildings (excluding building parts and features under 15 m2); building_footprint_m2 is the summed footprint of those buildings, whole; avg_building_footprint_m2 is their mean footprint. building_coverage_ratio is the built footprint CLIPPED to the catchment boundary divided by catchment area, so it can differ slightly from building_footprint_m2 divided by the area (which sums whole footprints of buildings centred inside). people_per_building is residents divided by buildings. road_density_m_per_km2 is road length per square kilometre. poi_per_1000_residents is points of interest divided by residents, times 1000 (for example, a value of 100 means 100 points of interest for every 1000 residents).
