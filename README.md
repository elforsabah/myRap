" After the WHILE loop, with all tours collected for the group
LOOP AT GROUP <group> ASSIGNING FIELD-SYMBOL(<member>).
  result = VALUE #( BASE result
                    FOR tour IN tours ( %cid_ref = <member>-%cid
                                        %tky     = CORRESPONDING #( tour )
                                        %param   = tour ) ).
ENDLOOP.
