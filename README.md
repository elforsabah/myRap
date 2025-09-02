// Keys must be readonly in strict managed draft
  field ( numbering : managed
          readonly
          semantic  : uuid ) SessionUUID;  " <-- RAW16 UUID key

  field ( readonly ) IsActiveEntity, HasActiveEntity, HasDraftEntity;
