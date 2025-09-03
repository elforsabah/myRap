association [0..*] to ZI_WR_SALESITEM_CONTRACTVH as _SalesItems
    on _SalesItems.SalesOrder = $projection.Vbeln
