// Component.ts (init) or wherever you call it
init(): void {
  super.init?.();

  const ushell = (sap as any)?.ushell;
  const getRenderer = ushell?.Container?.getRenderer;

  if (typeof getRenderer === "function") {
    getRenderer.call(ushell.Container).setHeaderVisibility(false, false);
  }
}
