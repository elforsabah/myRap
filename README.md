init: function () {
  sap.ui.core.UIComponent.prototype.init.apply(this, arguments);
  // Hide the FLP shell header when running in Launchpad
  if (sap.ushell && sap.ushell.Container) {
    try {
      /** @type {any} */
      var containerAny = sap.ushell.Container;
      var oRenderer = containerAny.getRenderer("fiori2");
      oRenderer.setHeaderVisibility(false, false); // (visible, animate)
    } catch (e) {
      // ignore if not in FLP
    }
  }
}
