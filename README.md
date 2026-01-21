" Set result (VBELN + messages)
result = VALUE #(
  (
    %param = VALUE #(
      techkey    = 'X'
      vbeln      = lv_vbeln
      _messages  = CORRESPONDING #(
                     lt_return
                     MAPPING techkey    = 'X'
                             msgid      = id
                             msgno      = number
                             msgty      = type
                             msgv1      = message_v1
                             msgv2      = message_v2
                             msgv3      = message_v3
                             msgv4      = message_v4
                   )
    )
  )
).
