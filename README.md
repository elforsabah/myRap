_messages = VALUE #(
  FOR r IN lt_return
  ( TechKey = 'X'
    Msgid   = r-id
    Msgno   = r-number
    Msgty   = r-type
    Msgv1   = r-message_v1
    Msgv2   = r-message_v2
    Msgv3   = r-message_v3
    Msgv4   = r-message_v4 )
).
