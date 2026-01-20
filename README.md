@EndUserText.label: 'addDocLink Payload'
define root abstract entity Z_A_SO_ADD_DOCLINK
{
  key SalesOrder : vbeln_va;

  MimeType       : abap.char(40);    // PDF: MimeType needed :contentReference[oaicite:10]{index=10}
  FileName       : abap.char(128);   // like Slug filename in PDF :contentReference[oaicite:11]{index=11}

  Url            : abap.char(255);   // we store link (URL)
  Title          : abap.char(80);
}
