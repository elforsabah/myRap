  result = VALUE #( (
      %param = VALUE /siec/sd_lto_damex_abstract(
        batchid        = parameters[ 1 ]-%param-batchid
        mandant        = parameters[ 1 ]-%param-mandant
        sales_document = parameters[ 1 ]-%param-sales_document
        soap_result    = lv_response
      )
    ) ).
