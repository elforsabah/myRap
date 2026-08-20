In ABAP Development Tools öffnen
Datendefinition /PLCE/R_PDSERVICEEXTWR [RI4, 440]
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'PD Service Extension (WR)'
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDServiceExtWR
  as select from /plce/tpdsrvwr
  association        to parent /PLCE/R_PDService     as _Service                 on $projection.ServiceUUID = _Service.ServiceUUID
  association [0..1] to /PLCE/R_MDMaterial           as _Material                on _Material.Material = $projection.Material
  association [0..1] to /PLCE/R_MDTransptPackaggT    as _ContainerTypeAtLocation on _ContainerTypeAtLocation.TransportPackagingType = $projection.ContainerTypeAtLocation
  association [0..1] to /PLCE/R_MDTransptPackaggT    as _ContainerTypeNew        on _ContainerTypeNew.TransportPackagingType = $projection.ContainerTypeNew
  association [0..1] to /PLCE/R_PDFunctionalLocation as _PlantLocation           on _PlantLocation.FunctionalLocation = $projection.PlantLocation
  association [0..1] to /PLCE/R_PDFunctionalLocation as _ContainerSourceLocation on _ContainerSourceLocation.FunctionalLocation = $projection.ContainerSourceLocation
  association [0..1] to /PLCE/R_PDFunctionalLocation as _ContainerFinalLocation  on _ContainerFinalLocation.FunctionalLocation = $projection.ContainerSourceLocation
 
  association [0..*] to /PLCE/P_PDServiceFrequencyT  as _ServiceFrequencyText    on _ServiceFrequencyText.ServiceFrequency = $projection.ServiceFrequency
 
{
  key service_uuid                               as ServiceUUID,
      @EndUserText.label: 'Material'
      material                                   as Material,
      @EndUserText.label: 'Material Weight'
      material_weight                            as MaterialWeight,
      material_weight_unit                       as MaterialWeightUnit,
      @EndUserText.label: 'Material Plant'
      plant_location                             as PlantLocation,
      @EndUserText.label: 'Container Source Location'
      container_source_location                  as ContainerSourceLocation,
      @EndUserText.label: 'Container Final Destination'
      container_final_location                   as ContainerFinalLocation,
 
      @EndUserText.label: 'Container Type (at Location)'
      containertype_atloc                        as ContainerTypeAtLocation,
      @EndUserText.label: 'Container Count (at Location)'
      container_atloc_count                      as ContainerAtLocationCount,
      @EndUserText.label: 'Container ID (at Location)'
      container_atloc_tidnr                      as ContainerAtLocationTidnr,
 
      @EndUserText.label: 'Container Type (New)'
      containertype_new                          as ContainerTypeNew,
      @EndUserText.label: 'Container Count (New)'
      container_new_count                        as ContainerNewCount,
      @EndUserText.label: 'Container ID (New)'
      container_new_tidnr                        as ContainerNewTidnr,
      @EndUserText.label: 'EWC Code'
      ewc_code                                   as EwcCode,
      service_freq                               as ServiceFrequency,
 
      _Service,
      _Material,
      _ContainerTypeAtLocation,
      _ContainerTypeNew,
      _PlantLocation,
      _ContainerSourceLocation,
      _ContainerFinalLocation,
      _ServiceFrequencyText
}
