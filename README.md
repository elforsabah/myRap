_onObjectMatched: function () {
  const oModel = this.getView().getModel();

  // Bind to the entity set
  const oListBinding = oModel.bindList("/ZI_WR_WEIGHBRIDGE");

  // Create a new (draft) context — do NOT pass unknown props or computed keys
  // SalesDocument is computed, IsActiveEntity is handled by RAP draft, so pass {}
  const oNewContext = oListBinding.create({});

  // Make the new context the view context so your controls bind to it
  this.getView().setBindingContext(oNewContext);

  // Request only properties that exist in metadata
  // (SalesDocument is computed; others are present in the entity type)
  oNewContext.requestProperty([
    "SalesDocument",
    "IsActiveEntity",
    "SalesDocumentType",
    "SDDocumentCategory"
  ]).catch(function () {
    // swallow; optional: add your message handling here
  });
}
