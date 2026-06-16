managed implementation in class /plce/bp_r_pdfunctionallocatio unique;
strict ( 2 );
with draft;
extensible {
  with determinations on modify;
  with validations on save;
}

define behavior for /PLCE/R_PDFunctionalLocation alias PDFunctionalLocation
persistent table /plce/tpdfunclg
draft table /plce/dpdfunclg
lock master total etag LastChangedAt
authorization master ( global )
etag master LocalLastChangedAt
extensible
{
  create;
  update;
  delete;

  field ( suppress ) AddressId, IsGeocoded, DistrictName, AddressIsPersonAddress, AddresseeName1, AddresseeName2, CityName, Country, StreetName, PostalCode, HouseNumber, HouseNumberSupplementText, FullAddress;
  field ( mandatory : create, readonly : update ) FunctionalLocation;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;

  association _FuctionalLocationText { create; with draft; }

  action ( features : instance, precheck ) geoCode result [0..*] $self;
  static action geoCodeAll result [0..*] $self;
  action ( features : instance) geoCodeManual parameter /PLCE/D_PDGeocodeManual result [1] $self;

//  static action createFunctionalLocation parameter /PLCE/D_MDFunctionalLocationP;

  draft action Edit;
  draft action Activate;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare extensible
  {

  }

  mapping for /plce/tpdfunclg corresponding extensible
  {
    FunctionalLocation = functional_location;
    FunctionalLocationMDCategory = functional_location_category;
//    AddressId = address_id;
    Latitude = latitude;
    Longitude = longitude;
    Altitude = altitude;
    LastGeocode = last_geocode;
    IsManualGeocode = is_manual_geocode;
    GeocodePrecision = geocode_precision;

    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

define behavior for /PLCE/R_PDFunctionalLocationT alias PDFunctionalLocationText
persistent table /plce/tpdfunclgt
draft table /plce/dpdfunclgt
lock dependent by _FunctionalLocation
authorization dependent by _FunctionalLocation
{
  update;
  delete;

  field ( mandatory : create ) Language;
  field ( readonly : update ) FunctionalLocation, Language;

  association _FunctionalLocation { with draft; }

  mapping for /plce/tpdfunclgt corresponding
  {
    FunctionalLocation = functional_location;
    FunctionalLocationDescription = description;
    Language = spras;
  }
}
