# Iconclass-for-liturgical-furnishings

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>
PREFIX crm: <http://www.cidoc-crm.org/cidoc-crm/>

SELECT DISTINCT ?value ?label ?location ?region (COUNT(?value) AS ?occorrenze)
WHERE {
  ?subject crm:P128_carries/crm:P138_represents ?value .
  ?value skos:prefLabel ?label FILTER (lang(?label)="it") .
  OPTIONAL {
    ?subject crm:P53_has_former_or_current_location/crm:P89_falls_within ?location .
  }
  OPTIONAL {
    ?subject
      (^<https://sacred_space.com/custom/includes_level_c>)?/
      (^<https://sacred_space.com/custom/includes>|crm:P46i_forms_part_of)*/ 
      crm:P53_has_former_or_current_location/crm:P89_falls_within+ ?region .
    ?region crm:P2_has_type <https://sacred_space.com/resource/hertziana/type/region> .
  }
}
GROUP BY ?value ?label ?location ?region
ORDER BY ?value
