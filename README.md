method GET_V_ZZWASTE_DEF.
  data(lr_current) = me->collection_wrapper->get_current( ).
  break-point.   " inspect lr_current
  ...
lr_current->get_property_as_string( 'VBELN' )
lr_current->get_property_as_string( 'POSNR' )
lr_current->get_property_as_string( 'ZZCONTASSIGNMENT_ID' )
