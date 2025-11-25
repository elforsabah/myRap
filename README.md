" Read the created tours (use mapped keys from MODIFY, not 'keys')
  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
    ALL FIELDS WITH CORRESPONDING #( mapped_tour-tour )  " Map from mapped keys (has %tky implicitly via corresponding)
    RESULT DATA(tour_result).

  " Build result: Map keys to %tky using CORRESPONDING # instead of direct -%tky access
  result = VALUE #( FOR tour IN tour_result ( %tky = CORRESPONDING #( tour )  " This maps key fields (e.g., TourId) to %tky
                                      %param = tour ) ).  " Return full entity data in %param
